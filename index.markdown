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

<div class="archive-calendar">
  <div class="calendar-header">
  <button type="button" id="mini-prev-month">‹</button>
  <span id="mini-calendar-month"></span>
  <button type="button" id="mini-next-month">›</button>
</div>

  <div class="calendar-weekdays">
    <span>S</span>
    <span>M</span>
    <span>T</span>
    <span>W</span>
    <span>T</span>
    <span>F</span>
    <span>S</span>
  </div>

  <div class="calendar-grid" id="mini-calendar-grid"></div>
</div>

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@400;500;600&display=swap" rel="stylesheet">

<script>
document.addEventListener("DOMContentLoaded", function () {
  const postsByDate = {
    {% for post in site.posts %}
      "{{ post.date | date: '%Y-%m-%d' }}": {
        url: "{{ post.url }}",
        title: {{ post.title | jsonify }},
        image: "{{ post.image }}"
      }{% unless forloop.last %},{% endunless %}
    {% endfor %}
  };

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
      s.vx += (dx / dist) * force * 0.015;
      s.vy += (dy / dist) * force * 0.015;
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

  const mobileTopSafe = 140;
  const mobileSidePad = 18;
  const mobileBottomPad = 24;

  for (let i = 0; i < COUNT; i++) {
    const star = document.createElement("img");
    star.src = STAR_SRC;
    star.className = "moving-star";
    star.alt = "";
    container.appendChild(star);

    const size = isMobile
      ? randomBetween(24, 38)
      : randomBetween(20, 42);

    const baseSpeed = isMobile
      ? randomBetween(0.06, 0.18)
      : randomBetween(0.04, 0.10);

    const speedFactor = Math.pow((44 - Math.min(size, 44)) / 20, 1.6);

    const starData = {
      el: star,
      x: isMobile
        ? randomBetween(
            mobileSidePad,
            Math.max(mobileSidePad, window.innerWidth - size - mobileSidePad)
          )
        : randomBetween(0, window.innerWidth - size),
      y: isMobile
        ? randomBetween(
            mobileTopSafe,
            Math.max(mobileTopSafe, window.innerHeight - size - mobileBottomPad)
          )
        : randomBetween(0, window.innerHeight - size),
      vx: baseSpeed * speedFactor * (Math.random() > 0.5 ? 1 : -1),
      vy: baseSpeed * speedFactor * (Math.random() > 0.5 ? 1 : -1),
      size,
      angle: randomBetween(0, 360),
      spin: randomBetween(-0.05, 0.05)
    };

    if (isMobile) {
      for (const other of state) {
        const dx = starData.x - other.x;
        const dy = starData.y - other.y;
        const dist = Math.sqrt(dx * dx + dy * dy);

        if (dist < 70) {
          starData.x = randomBetween(
            mobileSidePad,
            Math.max(mobileSidePad, window.innerWidth - size - mobileSidePad)
          );
          starData.y = randomBetween(
            mobileTopSafe,
            Math.max(mobileTopSafe, window.innerHeight - size - mobileBottomPad)
          );
        }
      }
    }

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

      s.x += s.vx * (isMobile ? 0.45 : 0.75);
      s.y += s.vy * (isMobile ? 0.45 : 0.75);

      s.vx *= isMobile ? 0.88 : 0.94;
      s.vy *= isMobile ? 0.88 : 0.94;

      const minSpeed = isMobile ? 0.08 : 0.03;
      if (Math.abs(s.vx) < minSpeed) s.vx = minSpeed * (s.vx < 0 ? -1 : 1);
      if (Math.abs(s.vy) < minSpeed) s.vy = minSpeed * (s.vy < 0 ? -1 : 1);

      if (s.x <= 0) {
        s.x = 0;
        s.vx *= -1;
      } else if (s.x >= w - s.size) {
        s.x = w - s.size;
        s.vx *= -1;
      }

      if (s.y <= 0) {
        s.y = 0;
        s.vy *= -1;
      } else if (s.y >= h - s.size) {
        s.y = h - s.size;
        s.vy *= -1;
      }

      s.angle += s.spin;
      s.el.style.transform = `translate(${s.x}px, ${s.y}px) rotate(${s.angle}deg)`;
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
      logo.style.opacity = Math.max(0, 0.88 - scrollY / 300);
      logo.style.transition = "opacity 0.3s ease";
    }
  });

  const now = new Date().toLocaleString("en-US", {
    timeZone: "Asia/Seoul"
  });
  const today = new Date(now);
  const date = today.getDate();

  let miniCurrent = new Date();

  function renderMiniCalendar(dateObj) {
    const year = dateObj.getFullYear();
    const month = dateObj.getMonth();

    const calendarMonth = document.getElementById("mini-calendar-month");
    const calendarGrid = document.getElementById("mini-calendar-grid");

    if (!calendarMonth || !calendarGrid) return;

    calendarGrid.innerHTML = "";
    calendarMonth.textContent = `${year}.${month + 1}`;

    const firstDay = new Date(year, month, 1).getDay();
    const lastDate = new Date(year, month + 1, 0).getDate();

    for (let i = 0; i < firstDay; i++) {
      const empty = document.createElement("span");
      empty.className = "calendar-day empty";
      calendarGrid.appendChild(empty);
    }

    for (let i = 1; i <= lastDate; i++) {
      const dayEl = document.createElement("a");
      dayEl.className = "calendar-day";

      const monthStr = String(month + 1).padStart(2, "0");
      const dayStr = String(i).padStart(2, "0");
      const dateKey = `${year}-${monthStr}-${dayStr}`;

      const dayNumber = document.createElement("span");
      dayNumber.className = "day-number";
      dayNumber.textContent = i;
      dayEl.appendChild(dayNumber);

      if (postsByDate[dateKey]) {
        dayEl.href = postsByDate[dateKey].url;
        dayEl.classList.add("has-post");
      } else {
        dayEl.href = "/calendar/";
        dayEl.classList.add("no-post");
      }

      if (
        i === date &&
        month === today.getMonth() &&
        year === today.getFullYear()
      ) {
        dayEl.classList.add("today");
      }

      calendarGrid.appendChild(dayEl);
    }
  }

  renderMiniCalendar(miniCurrent);

  document.getElementById("mini-prev-month").addEventListener("click", () => {
    miniCurrent.setMonth(miniCurrent.getMonth() - 1);
    renderMiniCalendar(miniCurrent);
  });

  document.getElementById("mini-next-month").addEventListener("click", () => {
    miniCurrent.setMonth(miniCurrent.getMonth() + 1);
    renderMiniCalendar(miniCurrent);
  });

  setTimeout(() => {
    const intro = document.getElementById("intro");
    const stars = document.getElementById("floating-stars");

    if (intro) intro.remove();
    if (stars) stars.style.opacity = "1";
  }, 2800);
});
</script>

<script>
window.addEventListener('load', function() {
  document.body.classList.add('loaded');
});
</script>
