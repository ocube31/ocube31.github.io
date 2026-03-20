---
layout: default
---

<div id="floating-stars"></div>

<div id="intro">
  ˏ 𓏧 𓏲 𓏲 𓏲 𓋒𓏲 𓏲 𓏲 𓏲 𓏧 ˎ<br>
  ‧̍̊˙· 𓆝.° ｡˚𓆛˚｡ °.𓆞 ·˙‧̍̊
</div>

<div class="fixed-logo">
  ˏ 𓏧 𓏲 𓏲 𓏲 𓏲 𓋒𓏲 𓏲 𓏲 𓏲 𓏧 ˎ<br>
  ‧̍̊˙· 𓆝.° ｡˚𓆛˚｡ °.𓆞 ·˙‧̍̊
</div>

<h2 class="archive-title">0ryu archive</h2>

<div class="category-menu">
  <a href="/">all</a>
  <a href="/category/diary">diary</a>
  <a href="/category/archive">archive</a>
  <a href="/category/food">food</a>
</div>

<div class="post-grid">
  {% for post in site.posts %}
    <a class="post-card" href="{{ post.url }}">
      {% if post.image %}
        <div class="post-card-thumb">
          <img src="{{ post.image }}" alt="">
        </div>
      {% endif %}

      <div class="post-card-date">{{ post.date | date: "%Y.%m.%d" }}</div>
      <div class="post-card-title">{{ post.title }}</div>

      {% if post.excerpt %}
        <div class="post-card-excerpt">
          {{ post.excerpt | strip_html | truncate: 80 }}
        </div>
      {% endif %}
    </a>
  {% endfor %}
</div>

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;500;600&display=swap" rel="stylesheet">

<script>
document.addEventListener("DOMContentLoaded", function () {
  const container = document.getElementById("floating-stars");
  if (!container) return;

  container.innerHTML = "";

  const isMobile = window.innerWidth < 768;
  const COUNT = isMobile ? 10 : 40;
  const state = [];
  const mouse = { x: -9999, y: -9999, active: false };

  const STAR_SRC = "{{ '/assets/star.png' | relative_url }}";
  const CLICK_SOUND_SRC = "{{ '/assets/click.mp3' | relative_url }}";

  const clickSound = new Audio(CLICK_SOUND_SRC);
  clickSound.preload = "auto";
  clickSound.volume = 0.35;

  function randomBetween(min, max) {
    return Math.random() * (max - min) + min;
  }

  function playClickSound() {
    try {
      clickSound.currentTime = 0;
      clickSound.play().catch(() => {});
    } catch (e) {}
  }

  function applyMouseRepel(s) {
    if (!mouse.active || isMobile) return;

    const cx = s.x + s.size / 2;
    const cy = s.y + s.size / 2;
    const dx = cx - mouse.x;
    const dy = cy - mouse.y;
    const dist = Math.sqrt(dx * dx + dy * dy);
    const repelRadius = 110;

    if (dist > 0 && dist < repelRadius) {
      const force = (repelRadius - dist) / repelRadius;
      s.vx += (dx / dist) * force * 0.035;
      s.vy += (dy / dist) * force * 0.035;
    }
  }

  function applyGlow(s) {
    if (!mouse.active) {
      s.el.style.filter = "";
      return;
    }

    const cx = s.x + s.size / 2;
    const cy = s.y + s.size / 2;
    const dx = cx - mouse.x;
    const dy = cy - mouse.y;
    const dist = Math.sqrt(dx * dx + dy * dy);
    const glowRadius = isMobile ? 90 : 150;

    if (dist < glowRadius) {
      const intensity = (glowRadius - dist) / glowRadius;
      s.el.style.filter = `
        drop-shadow(0 0 ${16 * intensity}px rgba(120, 200, 255, ${0.6 * intensity}))
        brightness(${1 + intensity * 0.45})
      `;
    } else {
      s.el.style.filter = "";
    }
  }

  function burstStars(x, y) {
    for (let i = 0; i < 7; i++) {
      const spark = document.createElement("img");
      spark.src = STAR_SRC;
      spark.className = "moving-star";
      spark.style.width = "14px";
      spark.style.height = "14px";
      spark.style.pointerEvents = "none";
      spark.style.left = "0px";
      spark.style.top = "0px";
      container.appendChild(spark);

      const angle = Math.random() * Math.PI * 2;
      const distance = 36 + Math.random() * 28;
      const dx = Math.cos(angle) * distance;
      const dy = Math.sin(angle) * distance;

      spark.animate(
        [
          { transform: `translate(${x}px, ${y}px) scale(0.8)`, opacity: 0.9 },
          { transform: `translate(${x + dx}px, ${y + dy}px) scale(0.1)`, opacity: 0 }
        ],
        {
          duration: 480,
          easing: "ease-out",
          fill: "forwards"
        }
      );

      setTimeout(() => spark.remove(), 520);
    }
  }

  for (let i = 0; i < COUNT; i++) {
    const star = document.createElement("img");
    star.src = STAR_SRC;
    star.className = "moving-star";
    container.appendChild(star);

    const size = isMobile
      ? randomBetween(28, 44)
      : randomBetween(20, 42);

    const baseSpeed = isMobile
      ? randomBetween(0.14, 0.32)
      : randomBetween(0.22, 0.55);

    const speedFactor = (44 - Math.min(size, 44)) / 20;

    const starData = {
      el: star,
      x: randomBetween(0, window.innerWidth - size),
      y: randomBetween(0, window.innerHeight - size),
      vx: baseSpeed * speedFactor * (Math.random() > 0.5 ? 1 : -1),
      vy: baseSpeed * speedFactor * (Math.random() > 0.5 ? 1 : -1),
      size
    };

    star.style.width = size + "px";
    star.style.height = size + "px";

    function clickBurst() {
      playClickSound();

      const rect = star.getBoundingClientRect();
      burstStars(rect.left + rect.width / 2, rect.top + rect.height / 2);

      star.animate(
        [
          { transform: `translate(${starData.x}px, ${starData.y}px) scale(1)` },
          { transform: `translate(${starData.x}px, ${starData.y}px) scale(1.35)` },
          { transform: `translate(${starData.x}px, ${starData.y}px) scale(1)` }
        ],
        { duration: 180 }
      );
    }

    star.addEventListener("click", clickBurst);
    star.addEventListener("touchstart", clickBurst, { passive: true });

    state.push(starData);
  }

  function animate() {
    const w = window.innerWidth;
    const h = window.innerHeight;

    for (const s of state) {
      applyMouseRepel(s);
      applyGlow(s);

      s.x += s.vx;
      s.y += s.vy;

      if (s.x <= 0 || s.x >= w - s.size) s.vx *= -1;
      if (s.y <= 0 || s.y >= h - s.size) s.vy *= -1;

      s.el.style.transform = `translate(${s.x}px, ${s.y}px)`;
    }

    requestAnimationFrame(animate);
  }

  animate();

  window.addEventListener("mousemove", (e) => {
    mouse.x = e.clientX;
    mouse.y = e.clientY;
    mouse.active = true;
  });

  window.addEventListener("mouseleave", () => {
    mouse.active = false;
  });

  setTimeout(() => {
    const intro = document.getElementById("intro");
    const stars = document.getElementById("floating-stars");

    if (intro) intro.remove();
    if (stars) stars.style.opacity = "1";
  }, 2800);
});
</script>
