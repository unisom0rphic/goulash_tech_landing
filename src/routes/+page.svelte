<script>
  import { onMount } from 'svelte';
  import ProjectSection from '$lib/components/ProjectSection.svelte';
  import ArchitectureSection from '$lib/components/ArchitectureSection.svelte';
	import SpawningBalls from '$lib/components/SpawningBalls.svelte';
  import { base } from '$app/paths';

  let scrollY = 0;
  let bgOffset = 0;
  const speed = 0.1;

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

  const projects = [
    {
      title: 'Система оптимизации работы службы поддержки',
      description: `
      <p>
        Система разработана в рамках хакатона <span class="accent">EnigmaHack</span>. Цель - оптимизировать и облегчить
        работу службы поддержки компании, занимающейся <span class="accent">разработкой и
        поддержкой газоаналитического оборудования</span>. Основные проблемы сотрудников: 
        чтение электронных писем, ручное заполнение таблиц, долгий поиск релевантной информации в документации.
        Языковая модель способна <span class="accent">в разы</span> ускорить рутину и 
        сэкономить <span class="accent">часы</span> монотонной работы.
      </p>
      `,
      tech: ['SvelteKit', 'Redis', 'aiogram', 'OpenRouter', 'FastAPI'],
      quote: `Планировал архитектуру, разрабатывал фронтенд и AI-модуль`,
      github: 'https://github.com/unisom0rphic/enigma',
      image: `${base}/enigma.png`,
      architecture: {
        title: 'Архитектура',
        description: `
        <p>
          Бэкенд написан на <span class="accent">FastAPI</span>, в качестве 
          базы данных выбран <span class="accent">PostgreSQL</span>.
          Поступающие письма автоматически парсятся благодаря IMAP-серверу.
          <span class="accent">AI-модуль</span> извлекает ключевую информацию 
          из email и использует документацию продукции на сайте компании для формирования ответа. 
          Полученная документация кэшируется в <span class="accent">FAISS</span> для семантического поиска.
          При помощи <span class="accent">NER</span> извлекается вся конфиденциальная информация (телефон, email), а письмо
          обрабатывается <span class="accent">LLM</span> через API <span class="accent">(OpenRouter),</span> после чего изъятая информация подставляется обратно
          в тело письма.
          Сформированные ответы сохраняются в <span class="accent">Redis</span> и 
          читаются <span class="accent">телеграм-ботом</span> для автоматического 
          уведомления сотрудников в телеграм-канале. Сотрудник техподдержки при 
          необходимости вносит модификации и принимает окончательное 
          решение. Письма отправляются клиенту после 
          закрытия тикета через <span class="accent">SMTP</span> без надобности покидать платформу.
       </p> 
        `,
        image: `${base}/arch-enigma.png`
      }
    },
    {
      title: 'Startflow',
      description: `
        <p>
          Платформа помогает новым членам команды легче 
          <span class="accent">адаптироваться</span> и ориентироваться в проекте. 
          Каждый проект содержит описание, список задач и колонок 
          (отображаются в виде канбан-доски). Добавление новых участников 
          возможно по <span class="accent">ссылке-приглашению.</span> 
          <span class="accent">ИИ</span> (локально развернутый LLM 
          <span class="accent">Vikhr</span>) помогает в генерации отчетов и построении графиков.
        </p>
      `,
      tech: ['FastAPI', 'llama.cpp', 'Docker', 'PostgreSQL', 'SvelteKit'],
      quote: `<p>
        Занимался планированием архитектуры, фронтендом, интеграцием с бэкендом, 
        исправлением возникших багов и добавлением ручек, формулировкой ТЗ для членов команды
        бэкенда.
        </p>`,
      github: 'https://github.com/50sohight/StartFlow',
      image: `${base}/startflow.png`,
      architecture: {
        title: 'Архитектура',
        description: `
        <p>
          Бэкенд написан на <span class="accent">FastAPI, Pydantic и SQLAlchemy.</span>
          Миграции схемы БД (<span class="accent">PostgreSQL</span>) отслеживает <span class="accent">Alembic</span>.
          Проекты поддерживают полный CRUD функционал + генерацию отчетов и графиков с помощью ИИ,
          динамическую загрузку шаблонов при создании.
          Фронтенд - многостраничное приложение с SSR и гидратацией (для производительности).
        </p>
        `,
        image:`${base}/arch-startflow.png` 
      }
    }
  ];
</script>

<main>
  <div class="global-bg" style="height: 200%; background-image: url('{base}/background.jpg');"></div>
  <div class="global-overlay"></div>

  <header>
    <div class="logo">✦ AI GENERALIST</div>
    <nav>
      <a href="#about">About</a>
      <a href="#система-оптимизации-работы-службы-поддержки">EnigmaHack</a>
      <a href="#startflow">Startflow</a>
      <a href="#контакты">Contacts</a>
    </nav>
  </header>

  <section id="about" class="snap-section parallax-section">
    <div class="content">
      <h1>Привет, я - <span class="accent">димон)</span></h1>
      <div class="bio">
        <p>
            Ниже представлены два проекта, которые максимально подходят под профиль вакансии - построены для решения 
            <span class="accent">настоящих</span> задач.
        </p> 
      </div>
      <div class="scroll-hint">↓ scroll</div>
    </div>
  </section>

  {#each projects as project (project.title)}
    <ProjectSection
      title={project.title}
      description={project.description}
      tech={project.tech}
      quote={project.quote}
      github={project.github}
      image={project.image}
    />
    <ArchitectureSection
      title={project.architecture.title}
      description={project.architecture.description}
      image={project.architecture.image}
    />
  {/each}

  <footer class="snap-section">
    <div class="footer-wrapper">
      <SpawningBalls />
      <div class="footer-content">
        <p>📫 Контакты</p>
        <div class="contacts" id="контакты">
          <a href="mailto:vorobyev.dmitriy.a@yandex.ru">email</a>
          <a href="https://github.com/unisom0rphic">github</a>
          <a href="https://t.me/@wdrnr">telegram</a>
        </div>
      </div>
    </div>
  </footer>
</main>

<style>
  /* шарики) */
  footer {
      min-height: 60vh;
      position: relative;
    }
    .footer-wrapper {
      position: relative;
      width: 100%;
      height: 60vh;
      overflow: hidden;
    }
    .footer-content {
      position: absolute;
      bottom: 2rem;
      left: 50%;
      transform: translateX(-50%);
      z-index: 3; /* выше шаров и аватарки */
      background: rgba(0, 0, 0, 0.5);
      backdrop-filter: blur(4px);
      border-radius: 2rem;
      padding: 1rem 2rem;
      text-align: center;
    }
    .contacts {
      display: flex;
      justify-content: center;
      gap: 2rem;
      margin-top: 0.5rem;
    }
    .contacts a {
      color: #b8b8ff;
      text-decoration: none;
    }

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
    will-change: transform;
    z-index: 0;
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

  header {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1.2rem 2rem;
    background: rgba(10, 10, 10, 0.7);
    backdrop-filter: blur(8px);
    z-index: 3;
    border-bottom: 1px solid rgba(255, 255, 255, 0.05);
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
    background: rgba(0, 0, 0, 0.5);
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
</style>