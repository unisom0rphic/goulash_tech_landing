<script>
  let { title, description, image } = $props();
  let isFullscreen = $state(false);

  function openFullscreen() {
    isFullscreen = true;
    document.body.style.overflow = 'hidden';
  }

  function closeFullscreen() {
    isFullscreen = false;
    document.body.style.overflow = '';
  }

  function onKeydown(e) {
    if (e.key === 'Escape') closeFullscreen();
  }
</script>

<svelte:window on:keydown={onKeydown} />

<section class="snap-section architecture">
  <div class="content arch-layout">
    <div class="arch-info">
      <h2 class="accent">{title}</h2>
      <div class="description">
        {@html description}
      </div>
    </div>
    <div class="arch-image">
        <button class="image-btn" onclick={openFullscreen} aria-label="Открыть изображение на весь экран">
            <img
                src={image}
                alt="Схема архитектуры"
                class="arch-img"
            />
        </button>
    </div>
  </div>
</section>

<!-- Полноэкранный оверлей -->
{#if isFullscreen}
  <div class="fullscreen-overlay" onclick={closeFullscreen} onkeydown={(e) => e.key === 'Escape' && closeFullscreen()} role="dialog" aria-modal="true" tabindex="-1">
    <button class="close-btn" onclick={closeFullscreen} aria-label="Закрыть" type="button">✕</button>
    <img src={image} alt="Схема архитектуры" class="fullscreen-img" />
  </div>
{/if}

<style>
  .snap-section {
    scroll-snap-align: start;
    min-height: 100vh;
    position: relative;
    z-index: 2;
    display: flex;
    align-items: center;
    justify-content: center;
    background: transparent;
  }
  .content {
    position: relative;
    z-index: 2;
    max-width: 1100px;
    backdrop-filter: blur(4px);
    border-radius: 1rem;
    padding: 2rem;
    margin: 0 2rem;
    background: rgba(0,0,0,0.35);
  }
  :global(.accent) {
    color: #b8b8ff;
  }
  h2 {
    font-size: 2rem;
    margin-bottom: 1.5rem;
  }
  .arch-layout {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 3rem;
    align-items: center;
  }
  .arch-image {
    cursor: pointer;
  }
  .arch-img {
    width: 100%;
    border-radius: 1rem;
    border: 1px solid rgba(255,255,255,0.1);
    box-shadow: 0 20px 35px -10px rgba(0,0,0,0.5);
    transition: transform 0.2s ease;
    display: block; /* убирает возможные отступы */
  }
  .arch-img:hover {
    transform: scale(1.03);
  }

  /* Фуллскрин */
  .fullscreen-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: rgba(0,0,0,0.9);
    backdrop-filter: blur(8px);
    z-index: 1000;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
    box-sizing: border-box;
  }
  .fullscreen-img {
    max-width: 90vw;
    max-height: 90vh;
    object-fit: contain;
    border-radius: 1rem;
    box-shadow: 0 0 40px rgba(0,0,0,0.5);
  }
  .close-btn {
    position: absolute;
    top: 1.5rem;
    right: 1.5rem;
    background: rgba(255,255,255,0.1);
    border: none;
    color: white;
    font-size: 2rem;
    width: 3rem;
    height: 3rem;
    border-radius: 50%;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background 0.2s;
    z-index: 1001;
  }
  .close-btn:hover {
    background: rgba(255,255,255,0.3);
  }
    .image-btn {
    display: block;
    width: 100%;
    padding: 0;
    border: none;
    background: none;
    cursor: pointer;
    }

  @media (max-width: 800px) {
    .arch-layout {
      grid-template-columns: 1fr;
    }
  }
</style>