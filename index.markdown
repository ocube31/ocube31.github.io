---
layout: default
---

<div id="floating-stars"></div>

<script>
const container = document.getElementById("floating-stars");

/* ⭐ PC / 모바일 개수 분기 */
const starCount = window.innerWidth < 768 ? 10 : 40;

for (let i = 0; i < starCount; i++) {
  const star = document.createElement("img");
  star.src = "{{ '/assets/star.png' | relative_url }}";
  star.className = "moving-star";

  const size = Math.random() * 30 + 30;
  star.style.width = size + "px";
  star.style.height = size + "px";

  star.style.left = Math.random() * 100 + "vw";
  star.style.top = Math.random() * 100 + "vh";

  container.appendChild(star);
}
</script>

<div id="intro">
  ˏ 𓏧 𓏲 𓏲 𓏲 𓋒𓏲 𓏲 𓏲 𓏲 𓏧 ˎ<br>
  ‧̍̊˙· 𓆝.° ｡˚𓆛˚｡ °.𓆞 ·˙‧̍̊
</div>

<div class="fixed-logo">
  ˏ 𓏧 𓏲 𓏲 𓏲 𓋒𓏲 𓏲 𓏲 𓏲 𓏧 ˎ<br>
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

<style>
body {
  margin: 0;
  background: #f5efe6;
  font-family: "Apple SD Gothic Neo", "Pretendard", sans-serif;
}

.archive-title {
  margin-top: 180px;
  text-align: center;
  font-size: 20px;
  letter-spacing: 3px;
  font-family: "Cormorant Garamond", serif;
  opacity: 0.7;
}



.post-grid {
  max-width: 860px;
  margin: 50px auto 100px;
  padding: 0 20px;
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 22px;
  position: relative;
  z-index: 2;
}

.post-card {
  display: block;
  padding: 22px 20px;
  text-decoration: none;
  color: #2f2f2f;
  background: rgba(255,255,255,0.28);
  border: 1px solid rgba(0,0,0,0.06);
  border-radius: 16px;
  backdrop-filter: blur(2px);
  transition: transform 0.25s ease, opacity 0.25s ease, border-color 0.25s ease;
}

.post-card:hover {
  transform: translateY(-3px);
  opacity: 0.88;
  border-color: rgba(0,0,0,0.12);
}

.post-card-date {
  font-size: 11px;
  opacity: 0.45;
  margin-bottom: 10px;
  letter-spacing: 1px;
}

.post-card-title {
  font-size: 22px;
  line-height: 1.3;
  margin-bottom: 10px;
  font-family: "Cormorant Garamond", serif;
}

.post-card-excerpt {
  font-size: 13px;
  line-height: 1.8;
  opacity: 0.7;
}

.post-card-thumb {
  width: 100%;
  height: 140px;
  overflow: hidden;
  border-radius: 12px;
  margin-bottom: 12px;
}

.post-card-thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;  
}

@media (max-width: 768px) {
  .post-grid {
    grid-template-columns: 1fr;
    gap: 14px;
    margin-top: 28px;
  }

  .post-card {
    padding: 18px 16px;
    border-radius: 14px;
  }

  .post-card-title {
    font-size: 20px;
  }
}

.category-menu {
  text-align: center;
  margin-top: 20px;
  margin-bottom: 20px;  
  font-size: 12px;
  letter-spacing: 2px;
}

.category-menu a {
  margin: 0 10px;
  text-decoration: none;
  color: #555;
  opacity: 0.6;
}

.category-menu a:hover {
  opacity: 1;
}  

.site-title,
.page-heading,
.home h1 {
  display: none !important;
}

#floating-stars {
  position: fixed;
  inset: 0;
  overflow: hidden;
  z-index: 0;
  opacity: 0;
  transition: opacity 0.8s ease;
  pointer-events: none;
}

.moving-star {
  position: absolute;
  pointer-events: auto;
  will-change: transform, filter;
  transition: filter 0.2s ease;
  touch-action: manipulation;
}

#intro {
  position: fixed;
  inset: 0;
  z-index: 9999;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  font-size: 14px;
  line-height: 1.8;
  letter-spacing: 2px;
  opacity: 0;
  pointer-events: none;
  background: #f5efe6;
  font-family:
    "Times New Roman",
    "Apple SD Gothic Neo",
    "Noto Sans Symbols",
    serif;
  animation: introFlow 2.8s ease forwards;
}

@keyframes introFlow {
  0% {
    opacity: 0;
    transform: scale(0.6) translateY(10px);
  }
  20% {
    opacity: 1;
    transform: scale(1);
  }
  40% {
    transform: scale(1) rotate(-0.4deg);
  }
  60% {
    transform: scale(1.02) rotate(0.4deg);
  }
  80% {
    opacity: 1;
  }
  100% {
    opacity: 0;
    transform: scale(1.1) translateY(-10px);
  }
}

.fixed-logo {
  position: fixed;
  top: 90px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 20;
  text-align: center;
  line-height: 1.8;
  letter-spacing: 2px;
  opacity: 0.88;
  font-size: 20px;
  white-space: nowrap;
  pointer-events: none;
  font-family:
    "Cormorant Garamond",
    "Times New Roman",
    serif;
  animation: logoFloat 4.8s ease-in-out infinite;
  transform-origin: center center;
  filter: drop-shadow(0 4px 10px rgba(0,0,0,0.08));
}

@media (max-width: 768px) {
  .fixed-logo {
    top: 72px;
    font-size: 15px;
    line-height: 1.7;
    letter-spacing: 1px;
  }
}

@keyframes logoFloat {
  0% {
    transform: translateX(-50%) translateY(0px) rotate(0deg);
  }
  25% {
    transform: translateX(-50%) translateY(-2px) rotate(-0.4deg);
  }
  50% {
    transform: translateX(-50%) translateY(1px) rotate(0.3deg);
  }
  75% {
    transform: translateX(-50%) translateY(-1px) rotate(0.2deg);
  }
  100% {
    transform: translateX(-50%) translateY(0px) rotate(0deg);
  }
}
</style>

<script>
document.addEventListener("DOMContentLoaded", function () {
  const container = document.getElementById("floating-stars");
  const COUNT = 43;
  const state = [];
  const mouse = { x: -9999, y: -9999, active: false };
  const clickSound = new Audio("assets/click.mp3");

  function randomBetween(min, max) {
    return Math.random() * (max - min) + min;
  }

  function applyMouseRepel(s) {
    if (!mouse.active) return;

    const cx = s.x + s.size / 2;
    const cy = s.y + s.size / 2;
    const dx = cx - mouse.x;
    const dy = cy - mouse.y;
    const dist = Math.sqrt(dx * dx + dy * dy);
    const repelRadius = 100;

    if (dist > 0 && dist < repelRadius) {
      const force = (repelRadius - dist) / repelRadius;
      s.vx += (dx / dist) * force * 0.02;
      s.vy += (dy / dist) * force * 0.02;
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
    const glowRadius = 150;

    if (dist < glowRadius) {
      const intensity = (glowRadius - dist) / glowRadius;
      s.el.style.filter = `
        drop-shadow(0 0 ${20 * intensity}px rgba(100,180,255,${intensity}))
        brightness(${1 + intensity})
      `;
    } else {
      s.el.style.filter = "";
    }
  }

  function burstStars(x, y) {
    for (let i = 0; i < 6; i++) {
      const spark = document.createElement("img");
      spark.src = "/assets/star.png";
      spark.className = "moving-star";
      spark.style.width = "14px";
      spark.style.height = "14px";
      spark.style.position = "absolute";
      spark.style.pointerEvents = "none";
      spark.style.transform = `translate(${x}px, ${y}px) scale(0.6)`;
      container.appendChild(spark);

      const angle = Math.random() * Math.PI * 2;
      const distance = 40 + Math.random() * 30;
      const dx = Math.cos(angle) * distance;
      const dy = Math.sin(angle) * distance;

      spark.animate(
        [
          {
            transform: `translate(${x}px, ${y}px) scale(0.6)`,
            opacity: 0.7
          },
          {
            transform: `translate(${x + dx}px, ${y + dy}px) scale(0.2)`,
            opacity: 0
          }
        ],
        {
          duration: 500,
          easing: "ease-out",
          fill: "forwards"
        }
      );

      setTimeout(() => {
        spark.remove();
      }, 500);
    }
  }

  for (let i = 0; i < COUNT; i++) {
    const star = document.createElement("img");
    star.src = "/assets/star.png";
    star.className = "moving-star";
    container.appendChild(star);

    star.addEventListener("click", () => {
      clickSound.currentTime = 0;
      clickSound.play().catch(() => {});

      const originalTransform = star.style.transform;

      const rect = star.getBoundingClientRect();
      const burstX = rect.left + rect.width / 2;
      const burstY = rect.top + rect.height / 2;
      burstStars(burstX, burstY);

      star.style.transform = originalTransform + " scale(1.5)";
      star.style.filter = "brightness(2)";

      setTimeout(() => {
        star.style.transform = originalTransform;
        star.style.filter = "";
      }, 150);
    });

    const size = window.innerWidth < 768
      ? randomBetween(28, 48)
      : randomBetween(20, 40);

    const baseSpeed = randomBetween(0.02, 0.08);
    const speedFactor = (40 - Math.min(size, 40)) / 20;

    state.push({
      el: star,
      x: randomBetween(0, window.innerWidth - size),
      y: randomBetween(0, window.innerHeight - size),
      vx: baseSpeed * speedFactor * (Math.random() > 0.5 ? 1 : -1),
      vy: baseSpeed * speedFactor * (Math.random() > 0.5 ? 1 : -1),
      size
    });

    star.style.width = size + "px";
    star.style.height = size + "px";
  }

  function animate() {
    const w = window.innerWidth;
    const h = window.innerHeight;

    for (const s of state) {
      applyMouseRepel(s);
      applyGlow(s);

      s.x += s.vx * 1;
      s.y += s.vy * 1;

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

  window.addEventListener("touchstart", (e) => {
    const touch = e.touches[0];
    if (!touch) return;
    mouse.x = touch.clientX;
    mouse.y = touch.clientY;
    mouse.active = true;
  }, { passive: true });

  window.addEventListener("touchmove", (e) => {
    const touch = e.touches[0];
    if (!touch) return;
    mouse.x = touch.clientX;
    mouse.y = touch.clientY;
    mouse.active = true;
  }, { passive: true });

  window.addEventListener("touchend", () => {
    mouse.active = false;
  });

  window.addEventListener("scroll", () => {
    const logo = document.querySelector(".fixed-logo");
    const scrollY = window.scrollY;

    if (logo) {
      logo.style.opacity = 1 - scrollY / 300;
      logo.style.transition = "opacity 0.3s ease";
    }
  });

  setTimeout(() => {
  const intro = document.getElementById("intro");
  const stars = document.getElementById("floating-stars");

  if (intro) intro.remove();
  if (stars) stars.style.opacity = "1";
}, 2800);
});
</script>
