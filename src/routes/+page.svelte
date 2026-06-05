<script>
  import { onMount } from 'svelte';

  let scrollY = 0;
  let bgOffset = 0;
  const speed = 0.3;

  function handleScroll() {
    const mainEl = document.querySelector('main');
    if (mainEl) {
      scrollY = mainEl.scrollTop;
      bgOffset = -scrollY * speed;
      const bg = document.querySelector('.global-bg');
      if (bg) {
        bg.style.transform = `translateY(${bgOffset}px)`;
      }
    }
  }

  onMount(() => {
    const mainEl = document.querySelector('main');
    if (mainEl) {
      mainEl.addEventListener('scroll', handleScroll);
      handleScroll();
      return () => mainEl.removeEventListener('scroll', handleScroll);
    }
  });
</script>

<main>
  <div class="global-bg" style="height: 200%; background-image: url('/background.jpg');"></div>
  <div class="global-overlay"></div> 

  <header>
    <div class="logo">✦ AI GENERALIST</div>
    <nav>
      <a href="#about">about</a>
      <a href="#project1">project 1</a>
      <a href="#project2">project 2</a>
    </nav>
  </header>

  <!-- СЕКЦИЯ 0: Обо мне -->
  <section id="about" class="snap-section parallax-section">
    <div class="content">
      <h1>Привет, я - <span class="accent">димон)</span></h1>
      <div class="bio">
        <!-- поемнять) -->
        <p>Интегрирую AI в бизнес-процессы</p>
        <p>Мои технологии: <span class="accent">SvelteKit / Python / PostgreSQL / AI</span></p>
        <p>
            Ниже представлены два проекта, которые максимально подходят под профиль вакансии - построены для решения 
            <span class="accent">настоящих</span> задач.
        </p> 
      </div>
      <div class="scroll-hint">↓ scroll</div>
    </div>
  </section>

    <!-- ПРОЕКТ 1 -->
    <section id="project1" class="snap-section">
    <div class="content project-layout">
        <div class="project-info">
        <h2> > <span class="accent">Система оптимизации работы службы поддержки</span></h2>
        <p>
          Система разработана в рамках хакатона EnigmaHack. Backend написан на <span class="accent">
          FastAPI</span>, в качестве базы данных выбран <span class="accent">PostgreSQL</span>.
          <span class="accent">AI-модуль</span> извлекает ключевую информацию из email и
          использует документацию продукции для формирования ответа. Сформированные ответы сохраняются в 
          <span class="accent">Redis</span> и читаются телеграм-ботом для автоматического уведомления сотрудников в 
          телеграм-канале. Фронтенд (<span class="accent">SvelteKit</span>) позволяет сотрудникам рассматривать
          ответы AI-модуля, редактировать их и закрывать тикеты - система сама отправляет ответ на почту через SMTP-сервер.
        </p>
        <div class="tech">
            <span>SvelteKit</span> <span>Redis</span> <span>aiogram</span> <span>OpenRouter</span>
        </div>
        <div class="arch">
            <!-- чо тут -->
            🧠 LLM → 📦 API → 💾 DB → ⚡ Frontend
        </div>
        <div class="links">
            <a href="#" class="btn">GitHub →</a>
        </div>
        </div>
        <div class="project-image">
        <div class="img-placeholder" style="background-image: url('/project1.png');"></div>
        </div>
    </div>
    </section>

    <!-- ПРОЕКТ 2 -->
    <section id="project2" class="snap-section">
    <div class="content project-layout">
        <div class="project-info">
        <h2> > <span class="accent">Startflow</span></h2>
        <p>
          Платформа помогает новым членам команды легче адаптироваться и ориентироваться в проекте.
          Каждый проект содержит описание, список задач и колонок (отображаются в виде канбан-доски).
          Добавление новых участников возможно по ссылке-приглашению. ИИ (локально развернутый LLM Vikhr)
          помогает в генерации отчетов и построении графиков.
        </p>
        <div class="tech">
            <span>FastAPI</span> <span>llama.cpp</span> <span>Docker</span> <span>PostgreSQL</span>
        </div>
        <div class="arch">
            <!-- что тут -->
            🔍 Web → 🤖 LLM (JSON) → 🗄️ DB → 🎨 SPA
        </div>
        <div class="links">
            <a href="#" class="btn">GitHub →</a>
        </div>
        </div>
        <div class="project-image">
        <div class="img-placeholder" style="background-image: url('/project2.png');"></div>
        </div>
    </div>
    </section>

  <footer class="snap-section">
    <div class="content footer-content">
      <p>📫 Контакты</p>
      <div class="contacts">
        <a href="mailto:vorobyev.dmitriy.a@yandex.ru">email</a>
        <a href="https://github.com/unisom0rphic">github</a>
        <a href="https://t.me/@wdrnr">telegram</a>
      </div>
    </div>
  </footer>
</main>

<style>
    :global(html), :global(body) {
        overflow: hidden;
        height: 100%;
    }

    main {
        height: 100vh;
        overflow-y: auto;
        scroll-snap-type: y mandatory;
        position: relative;
        background: transparent;
    }


    .global-bg {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 120%; 
        background-size: cover;
        background-position: center;
        background-repeat: no-repeat;
        will-change: transform;
        z-index: 0;
        transform: translateY(0);
    }
    .global-overlay {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.7); 
        z-index: 1;
        pointer-events: none;
    }
  

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

  h1 {
    font-size: 3.5rem;
    font-weight: 600;
    letter-spacing: -0.02em;
    margin-bottom: 1rem;
  }
  .accent {
    color: #b8b8ff;
  }
  .bio {
    max-width: 600px;
    font-size: 1.1rem;
    line-height: 1.6;
  }
  .bio p {
    margin-top: 1rem;
  }
  .scroll-hint {
    margin-top: 3rem;
    font-size: 0.75rem;
    opacity: 0.5;
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
  .btn, .btn-outline {
    padding: 0.5rem 1.2rem;
    border-radius: 30px;
    text-decoration: none;
    font-weight: 500;
    transition: 0.2s;
    font-size: 0.9rem;
  }
  .btn {
    background: white;
    color: black;
  }
  .btn-outline {
    border: 1px solid white;
    color: white;
    background: transparent;
  }
  .btn:hover {
    background: #ddd;
  }
  .btn-outline:hover {
    background: rgba(255,255,255,0.1);
  }

  header {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1.2rem 2rem;
    background: rgba(10,10,10,0.7);
    backdrop-filter: blur(8px);
    z-index: 3;
    border-bottom: 1px solid rgba(255,255,255,0.05);
  }
  .logo {
    font-weight: 600;
    letter-spacing: 1px;
    font-size: 1.1rem;
  }
  nav a {
    color: #ccc;
    text-decoration: none;
    margin-left: 1.8rem;
    font-size: 0.9rem;
    transition: 0.2s;
  }
  nav a:hover {
    color: white;
  }

  footer {
    text-align: center;
    min-height: 60vh;
  }
  .footer-content {
    background: rgba(0,0,0,0.5);
    backdrop-filter: blur(4px);
    border-radius: 2rem;
    padding: 2rem;
  }
  .contacts {
    display: flex;
    justify-content: center;
    gap: 2rem;
    margin: 1rem 0;
  }
  .contacts a {
    color: #b8b8ff;
    text-decoration: none;
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
    }

    .img-placeholder {
    width: 100%;
    aspect-ratio: 16 / 9;
    background-size: cover;
    background-position: center;
    border-radius: 1rem;
    border: 1px solid rgba(255,255,255,0.15);
    box-shadow: 0 20px 35px -10px rgba(0,0,0,0.5);
    transition: transform 0.2s ease;
    }

    .img-placeholder:hover {
    transform: scale(1.02);
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
    .img-placeholder {
        aspect-ratio: 16 / 9;
    }
    }
</style>