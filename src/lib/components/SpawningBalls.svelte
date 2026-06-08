<script>
  import { onMount } from 'svelte';

  let container = $state(null);
  let containerWidth = $state(0);
  let containerHeight = $state(0);
  let balls = $state([]);
  let draggedBall = $state(null);
  let dragOffsetX = 0;
  let dragOffsetY = 0;
  let lastPointerX = 0;
  let lastPointerY = 0;
  let pointerSpeedX = 0;
  let pointerSpeedY = 0;
  let lastPointerTime = 0;

  const COUNT = 13;
  const GRAVITY = 0.5;
  const BOUNCE = -0.6;
  const FRICTION = 0.99;

  function spawnBalls() {
    if (!container || containerWidth <= 0) return;
    balls = Array.from({ length: COUNT }, (_, i) => ({
      id: i,
      x: Math.random() * containerWidth,
      y: -60 - Math.random() * 150, // стартуют над верхом
      vx: (Math.random() - 0.5) * 2,
      vy: Math.random() * 2, // небольшой толчок вниз
      radius: 35 + Math.random() * 25,
    }));
  }

  function onAvatarClick() {
    spawnBalls();
  }

  let animationFrameId;

  function animate(timestamp) {
    if (!container) {
      animationFrameId = requestAnimationFrame(animate);
      return;
    }

    if (draggedBall) {
      if (lastPointerTime) {
        const dt = timestamp - lastPointerTime;
        if (dt > 0) {
          // нормируем скорость к 16мс (60fps) для стабильности
          const factor = 16 / dt;
          pointerSpeedX = (lastPointerX - draggedBall.x + dragOffsetX) * factor;
          pointerSpeedY = (lastPointerY - draggedBall.y + dragOffsetY) * factor;
        }
      }
      lastPointerX = draggedBall.x - dragOffsetX;
      lastPointerY = draggedBall.y - dragOffsetY;
      lastPointerTime = timestamp;
    } else {
      pointerSpeedX = 0;
      pointerSpeedY = 0;
      lastPointerTime = 0;
    }

    for (let ball of balls) {
      if (ball === draggedBall) continue;
      ball.vy += GRAVITY;
      ball.vx *= FRICTION;
      ball.vy *= FRICTION;
      ball.x += ball.vx;
      ball.y += ball.vy;

      if (ball.x - ball.radius < 0) {
        ball.x = ball.radius;
        ball.vx *= BOUNCE;
      }
      if (ball.x + ball.radius > containerWidth) {
        ball.x = containerWidth - ball.radius;
        ball.vx *= BOUNCE;
      }
      if (ball.y - ball.radius < 0) {
        ball.y = ball.radius;
        ball.vy *= BOUNCE;
      }
      if (ball.y + ball.radius > containerHeight) {
        ball.y = containerHeight - ball.radius;
        ball.vy *= BOUNCE;
      }
    }

    balls = balls; // триггерим реактивность
    animationFrameId = requestAnimationFrame(animate);
  }

  function pointerDownOnBall(ball, e) {
    e.preventDefault();
    draggedBall = ball;
    const rect = container.getBoundingClientRect();
    dragOffsetX = e.clientX - rect.left - ball.x + ball.radius; 
    dragOffsetY = e.clientY - rect.top - ball.y + ball.radius;
    container.setPointerCapture(e.pointerId);
    lastPointerTime = 0;
  }

  function pointerMove(e) {
    if (!draggedBall || !container) return;
    const rect = container.getBoundingClientRect();
    const px = e.clientX - rect.left;
    const py = e.clientY - rect.top;
    draggedBall.x = px - dragOffsetX;
    draggedBall.y = py - dragOffsetY;
    lastPointerX = px;
    lastPointerY = py;
    lastPointerTime = performance.now();
  }

  function pointerUp(e) {
    if (draggedBall) {
      if (lastPointerTime) {
        draggedBall.vx = pointerSpeedX;
        draggedBall.vy = pointerSpeedY;
      } else {
        draggedBall.vx = 0;
        draggedBall.vy = 0;
      }
      draggedBall = null;
      container.releasePointerCapture(e.pointerId);
    }
  }

  onMount(() => {
    const resize = () => {
      containerWidth = container.clientWidth;
      containerHeight = container.clientHeight;
    };
    resize();
    window.addEventListener('resize', resize);
    animationFrameId = requestAnimationFrame(animate);
    return () => {
      cancelAnimationFrame(animationFrameId);
      window.removeEventListener('resize', resize);
    };
  });
</script>

<div
  bind:this={container}
  class="balls-container"
  onpointermove={pointerMove}
  onpointerup={pointerUp}
  onpointercancel={pointerUp}
  role="application"
  aria-label="Интерактивные фото-шарики"
>
  <!-- Аватарка-кнопка -->
  <button class="avatar-btn" onclick={onAvatarClick} type="button">
    <img src="/avatar.jpg" alt="Моё фото" class="avatar-img" />
  </button>

  {#each balls as ball (ball.id)}
    <div
      class="photo-ball"
      style="left: {ball.x - ball.radius}px; top: {ball.y - ball.radius}px; width: {ball.radius * 2}px; height: {ball.radius * 2}px;"
      onpointerdown={(e) => pointerDownOnBall(ball, e)}
      role="button"
      tabindex="0"
      aria-label="Перетащи меня"
    >
      <img src="/photos/photo{ball.id + 1}.jpg" alt="Фото {ball.id + 1}" draggable="false" />
    </div>
  {/each}
</div>

<style>
  .balls-container {
    position: relative;
    width: 100%;
    height: 100%;
    overflow: hidden;
    touch-action: none;
    background: transparent;
  }
  .avatar-btn {
    position: absolute;
    top: 40%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 2;
    background: none;
    border: none;
    cursor: pointer;
    padding: 0;
    border-radius: 50%;
    transition: transform 0.2s;
  }
  .avatar-btn:hover {
    transform: translate(-50%, -50%) scale(1.1);
  }
  .avatar-img {
    width: 150px;
    height: 150px;
    border-radius: 50%;
    object-fit: cover;
    border: 3px solid rgba(255,255,255,0.3);
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
  }
  .photo-ball {
    position: absolute;
    border-radius: 50%;
    overflow: hidden;
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
    cursor: grab;
    user-select: none;
    touch-action: none;
    z-index: 1;
  }
  .photo-ball:active {
    cursor: grabbing;
  }
  .photo-ball img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    pointer-events: none;
    display: block;
  }
</style>