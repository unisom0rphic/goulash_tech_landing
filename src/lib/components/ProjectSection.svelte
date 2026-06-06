<script>
  let { title, description, tech, arch, github, image } = $props();
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

<section id={title.toLowerCase().replace(/\s+/g, '-')} class="snap-section">
  <div class="content project-layout">
    <div class="project-info">
      <h2> > <span class="accent">{title}</span></h2>
      <div class="description">
        {@html description}
      </div>
      <div class="tech">
        {#each tech as t}
          <span>{t}</span>
        {/each}
      </div>
      <div class="arch">
        {arch}
      </div>
      <div class="links">
        <a href={github} class="btn">GitHub →</a>
      </div>
    </div>
    <div class="project-image">
        <button class="image-btn" onclick={openFullscreen} aria-label="Открыть изображение на весь экран">
        <img
            src={image}
            alt="Скриншот проекта"
            class="project-img"
        />
        </button>
    </div>
  </div>
</section>

<!-- Полноэкранный оверлей -->
{#if isFullscreen}
  <div class="fullscreen-overlay" onclick={closeFullscreen} onkeydown={(e) => e.key === 'Escape' && closeFullscreen()} role="dialog" aria-modal="true" tabindex="-1">
    <button class="close-btn" onclick={closeFullscreen} aria-label="Закрыть" type="button">✕</button>
    <img src={image} alt="Скриншот проекта" class="fullscreen-img" />
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
  }
  :global(.accent) {
    color: #b8b8ff;
  }
  h2 {
    font-size: 2.2rem;
    font-weight: 600;
    margin-bottom: 1rem;
  }
  .tech {
    display: flex;
    flex-wrap: wrap;
    gap: 0.6rem;
    margin: 1.5rem 0;
  }
  .tech span {
    background: rgba(255,255,255,0.07);
    padding: 0.25rem 1rem;
    border-radius: 30px;
    font-size: 0.8rem;
    font-weight: 500;
  }
  .arch {
    background: rgba(0,0,0,0.6);
    padding: 0.8rem 1rem;
    border-left: 3px solid #b8b8ff;
    font-family: monospace;
    font-size: 0.9rem;
    margin: 1.5rem 0;
  }
  .links {
    display: flex;
    gap: 1rem;
    margin-top: 1.5rem;
  }
  .btn {
    padding: 0.5rem 1.2rem;
    border-radius: 30px;
    background: white;
    color: black;
    text-decoration: none;
    font-weight: 500;
    transition: 0.2s;
  }
  .btn:hover {
    background: #ddd;
  }
  .project-layout {
    display: flex;
    gap: 3rem;
    align-items: center;
    flex-wrap: wrap;
  }
  .project-info {
    flex: 1.2;
    min-width: 250px;
  }
  .project-image {
    flex: 0.8;
    min-width: 200px;
    cursor: pointer;
  }
  .project-img {
    width: 120%;
    aspect-ratio: 16 / 9;
    object-fit: cover;
    border-radius: 1rem;
    border: 1px solid rgba(255,255,255,0.15);
    box-shadow: 0 20px 35px -10px rgba(0,0,0,0.5);
    transition: transform 0.2s ease;
    display: block;
  }
  .project-img:hover {
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
    .project-layout {
      flex-direction: column;
      gap: 1.5rem;
    }
    .project-image {
      width: 100%;
      order: -1;
    }
  }
</style>