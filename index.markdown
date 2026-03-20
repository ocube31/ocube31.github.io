---
layout: default
---

<div id="floating-stars"></div>

<script>
document.addEventListener("DOMContentLoaded", () => {
  const container = document.getElementById("floating-stars");
  if (!container) return;

  container.innerHTML = "";

  const isMobile = window.innerWidth < 768;
  const starCount = isMobile ? 10 : 40;
  const starSrc = "{{ '/assets/star.png' | relative_url }}";

  const stars = [];
  let mouseX = -9999;
  let mouseY = -9999;

  const pad = 12;
  const vw = () => window.innerWidth;
  const vh = () => window.innerHeight;

  function rand(min, max) {
    return Math.random() * (max - min) + min;
  }

  for (let i = 0; i < starCount; i++) {
    const star = document.createElement("img");
    star.src = starSrc;
    star.className = "moving-star";
    star.alt = "";

    const size = isMobile ? rand(26, 42) : rand(34, 62);
    const x = rand(0, Math.max(0, vw() - size));
    const y = rand(0, Math.max(0, vh() - size));

    const speedBase = isMobile ? 0.18 : 0.28;
    const vx = (Math.random() > 0.5 ? 1 : -1) * rand(speedBase * 0.6, speedBase * 1.3);
    const vy = (Math.random() > 0.5 ? 1 : -1) * rand(speedBase * 0.6, speedBase * 1.3);

    star.style.width = size + "px";
    star.style.height = size + "px";
    star.style.left = x + "px";
    star.style.top = y + "px";

    container.appendChild(star);

    stars.push({
      el: star,
      x,
      y,
      vx,
      vy,
      size
    });
  }

  window.addEventListener("mousemove", (e) => {
    mouseX = e.clientX;
    mouseY = e.clientY;
  });

  window.addEventListener("mouseleave", () => {
    mouseX = -9999;
    mouseY = -9999;
  });

  window.addEventListener("resize", () => {
    const w = vw();
    const h = vh();
    for (const s of stars) {
      s.x = Math.min(Math.max(0, s.x), Math.max(0, w - s.size));
      s.y = Math.min(Math.max(0, s.y), Math.max(0, h - s.size));
    }
  });

  function animate() {
    const w = vw();
    const h = vh();

    for (const s of stars) {
      const cx = s.x + s.size / 2;
      const cy = s.y + s.size / 2;

      const dx = cx - mouseX;
      const dy = cy - mouseY;
      const dist = Math.sqrt(dx * dx + dy * dy);

      const avoidRadius = isMobile ? 0 : 110;

      if (!isMobile && dist < avoidRadius && dist > 0.01) {
        const push = (avoidRadius - dist) / avoidRadius;
        s.vx += (dx / dist) * push * 0.18;
        s.vy += (dy / dist) * push * 0.18;
      }

      const maxSpeed = isMobile ? 0.45 : 0.9;
      s.vx = Math.max(-maxSpeed, Math.min(maxSpeed, s.vx));
      s.vy = Math.max(-maxSpeed, Math.min(maxSpeed, s.vy));

      s.x += s.vx;
      s.y += s.vy;

      if (s.x <= pad) {
        s.x = pad;
        s.vx *= -1;
      } else if (s.x >= w - s.size - pad) {
        s.x = w - s.size - pad;
        s.vx *= -1;
      }

      if (s.y <= pad) {
        s.y = pad;
        s.vy *= -1;
      } else if (s.y >= h - s.size - pad) {
        s.y = h - s.size - pad;
        s.vy *= -1;
      }

      s.el.style.transform = `translate(${s.x}px, ${s.y}px)`;
    }

    requestAnimationFrame(animate);
  }

  requestAnimationFrame(animate);
});
</script>

<div id="intro">
  ˏ 𓏧 𓏲 𓏲 𓏲 𓋒𓏲 𓏲 𓏲 𓏲 𓏧 ˎ <br>
  ⊹˚₊‧ 𓆟 ₊˚⊹ 𓆝 ₊˚⊹ 𓆞 ‧₊˚⊹
</div>

<div class="fixed-logo">
  ˏ 𓏧 𓏲 𓏲 𓏲 𓋒𓏲 𓏲 𓏲 𓏲 𓏧 ˎ <br>
  ⊹˚₊‧ 𓆟 ₊˚⊹ 𓆝 ₊˚⊹ 𓆞 ‧₊˚⊹
</div>

<h2 class="archive-title">oryu archive</h2>

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
