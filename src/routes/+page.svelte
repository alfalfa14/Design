<script>
  import { onMount } from 'svelte';
  import { gsap } from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';

  let lang = $state('zh');
  let menuOpen = $state(false);
  let activeProject = $state(null);
  let hoveredProject = $state(null);
  let activeAcademic = $state(null);

  const t = {
    en: {
      nav: ['About', 'Projects', 'Experience', 'Resume', 'Contact'],
      navIds: ['about','projects','experience','resume','contact'],
      heroGreeting: "Hello, I'm",
      heroName: 'Shangyi Zhou',
      heroSub: 'UI/UX Designer · Product Designer · Visual Designer · Project Manager',
      heroDesc: "M.A. Digital Media & XR @ UChicago (Dean's Scholarship) · B.A. ICAM @ UCSD",
      heroCta: 'View My Work',
      aboutTitle: 'About Me',
      aboutText: "I'm a designer and researcher bridging technology, art, and user experience. With a background spanning interdisciplinary computing, extended reality, and art history, I craft intuitive interfaces and meaningful interactions that put people first.",
      aboutText2: "Currently pursuing my Master's at the University of Chicago with a 4.0 GPA, I bring hands-on experience in UI/UX design, HCI research, Arduino prototyping, and agile product development.",
      skillsTitle: 'Skills',
      projectsTitle: 'Projects',
      expTitle: 'Experience',
      resumeTitle: 'Resume',
      resumeDesc: 'Download my full resume to learn more about my experience and skills.',
      resumeBtn: 'Download Resume',
      contactTitle: 'Get In Touch',
      contactDesc: "I'm currently looking for UI/UX, Product Design, Creative Visual Design, and Project Management internships. Let's connect!",
      portfolioLink: 'View Portfolio on ZCOOL',
      close: '✕ CLOSE',
      campusTitle: 'Campus',
      resumeBtnZh: 'Download CV (中文)',
      resumeBtnEn: 'Download Resume (EN)',
      academicTitle: 'Academic & Course Projects',
    },
    zh: {
      nav: ['关于我', '项目', '经历', '简历', '联系'],
      navIds: ['about','projects','experience','resume','contact'],
      heroGreeting: '你好，我是',
      heroName: '周尚嶷',
      heroSub: '交互设计师 · 产品设计师 · 视觉创意设计师 · 项目经理',
      heroDesc: '芝加哥大学数字媒体与扩展现实硕士 · 院长奖学金',
      heroCta: '查看作品',
      aboutTitle: '关于我',
      aboutText: '我是一名设计师与研究者，在科技、艺术与用户体验之间架桥铺路。我的背景横跨跨学科计算机、扩展现实与艺术史，致力于打造以人为本的直觉界面与有意义的交互体验。',
      aboutText2: '目前就读于芝加哥大学，GPA 4.0，拥有 UI/UX 设计、HCI 研究、Arduino 原型开发及敏捷产品开发的实战经验，正在寻找产品/交互/UIUX 设计及项目管理方向的实习机会。',
      skillsTitle: '技能',
      projectsTitle: '项目展示',
      expTitle: '实习经历',
      resumeTitle: '简历',
      resumeDesc: '下载我的完整简历，了解更多关于我的经历和技能。',
      resumeBtn: '下载简历',
      contactTitle: '联系我',
      contactDesc: '我正在寻找 UI/UX、产品设计、视觉创意设计、和项目管理方向的实习机会，欢迎联系！',
      portfolioLink: '在站酷查看作品集',
      close: '✕ 关闭',
      campusTitle: '校园经历',
      resumeBtnZh: '下载简历（中文版）',
      resumeBtnEn: 'Download Resume (EN)',
      academicTitle: '课程与学校项目',
    }
  };

  const projects = [
    {
      id: 'footnow',
      titleEn: 'FOOTNOW',
      titleZh: 'FOOTNOW',
      tagEn: 'APP Design · UX · Product Design',
      tagZh: 'APP设计 · 用户体验 · 产品设计',
      descEn: 'A smart soccer cleat app helping hobbyist players get game feedback, strengthen team bonds, and find nearby matches. Full UX research, personas, wireframes, and hi-fi prototypes.',
      descZh: '一款配合智能足球鞋的App，帮助业余球员获取比赛数据反馈、加强团队凝聚力，并轻松发现附近球友。包含完整用研、用户画像、线框图与高保真原型。',
      cover: '/images/footnow/5.jpg',
      imagesEn: ['/images/footnow/1.jpg','/images/footnow/2.jpg','/images/footnow/3.jpg','/images/footnow/4.jpg','/images/footnow/5.jpg','/images/footnow/6.jpg'],
      imagesZh: ['/images/footnow/C1.jpg','/images/footnow/C2.jpg','/images/footnow/C3.jpg','/images/footnow/C4.jpg','/images/footnow/C5.jpg','/images/footnow/C6.jpg'],
      color: '#c8a45a',
      bg: '#0e0b04',
      tools: ['Figma', 'Adobe Photoshop', 'UX Research', 'Prototyping', 'Product Design'],
    },
    {
      id: 'trackpack',
      titleEn: 'TrackPack',
      titleZh: 'TrackPack',
      tagEn: 'Product Design · APP Design · UX',
      tagZh: '产品设计 · APP设计 · 用户体验',
      descEn: 'An AI-powered smart suitcase and companion app using cameras and weight sensors to track belongings in real time, notifying users of missing items before departure.',
      descZh: '搭载AI摄像头与电子秤的智能行李箱及配套App，实时追踪物品进出与重量变化，在出发前提醒用户防止遗漏。',
      cover: '/images/trackpack/11.jpg',
      imagesEn: ['/images/trackpack/7.jpg','/images/trackpack/8.jpg','/images/trackpack/9.jpg','/images/trackpack/10.jpg','/images/trackpack/11.jpg','/images/trackpack/12.jpg'],
      imagesZh: ['/images/trackpack/C7.jpg','/images/trackpack/C8.jpg','/images/trackpack/C9.jpg','/images/trackpack/C10.jpg','/images/trackpack/C11.jpg','/images/trackpack/C12.jpg'],
      color: '#e87d9b',
      bg: '#0e0408',
      tools: ['Figma', 'Adobe Photoshop', 'Product Design', 'User Research', 'Prototyping'],
    },
    {
      id: 'happyhour',
      titleEn: 'HappyHour',
      titleZh: 'HappyHour',
      tagEn: 'VR Design · User Experience',
      tagZh: 'VR设计 · 用户体验',
      descEn: 'A VR app encouraging moderate drinking while delivering an authentic bar atmosphere and social interactions — with user journey maps, personas, wireframes, and hi-fi VR UI.',
      descZh: '一款鼓励适度饮酒的虚拟现实App，提供真实感十足的酒吧体验与社交互动，含用户行为路径图、用户画像与高保真VR界面。',
      cover: '/images/happyhour/16.jpg',
      imagesEn: ['/images/happyhour/13.jpg','/images/happyhour/14.jpg','/images/happyhour/15.jpg','/images/happyhour/16.jpg','/images/happyhour/17.jpg'],
      imagesZh: ['/images/happyhour/C13.jpg','/images/happyhour/C14.jpg','/images/happyhour/C15.jpg','/images/happyhour/C16.jpg','/images/happyhour/C17.jpg'],
      color: '#e26f50',
      bg: '#0e0604',
      tools: ['VR Design', 'Adobe Photoshop', 'Figma', 'UX Research', 'User Journey Map'],
    },
    {
      id: 'lanterns',
      titleEn: 'Lanterns But Light & Reunion',
      titleZh: 'Lanterns But Light & Reunion',
      tagEn: 'Interactive Art · Arduino · P5.JS',
      tagZh: '互动艺术 · Arduino · P5.JS',
      descEn: 'Two interactive artworks exploring cultural discontinuity — a physical Arduino light installation and a P5.js generative particle visualization.',
      descZh: '两件探讨文化断裂性与延续性的互动艺术作品——Arduino实体灯光装置与P5.js粒子可视化生成艺术。',
      cover: '/images/lanterns/22.jpg',
      imagesEn: ['/images/lanterns/18.jpg','/images/lanterns/19.jpg','/images/lanterns/20.jpg','/images/lanterns/21.jpg','/images/lanterns/22.jpg','/images/lanterns/23.jpg'],
      imagesZh: ['/images/lanterns/C18-1.jpg','/images/lanterns/C19-1.jpg','/images/lanterns/C20-1.jpg','/images/lanterns/C21-1.jpg','/images/lanterns/C22-1.jpg','/images/lanterns/C23-1.jpg'],
      color: '#8fc44a',
      bg: '#050a02',
      tools: ['Arduino', 'P5.js', 'Interactive Art', 'Cultural Research'],
    },
  ];

  const academicProjects = [
    {
      id: 'ugarit',
      titleEn: 'Ugarit Tablet Inventory',
      titleZh: 'Ugarit 泥板文物数据库',
      tagEn: 'Svelte · OCHRE SDK · MapLibre · ShadCN',
      tagZh: 'Svelte · OCHRE SDK · MapLibre · ShadCN',
      descEn: 'A Svelte app querying the OCHRE archaeological database to display cuneiform tablet inventory from the kingdom of Ugarit — with an interactive MapLibre map, filterable ShadCN table, and dynamic UUID-based routes. Deployed on Vercel.',
      descZh: '基于 OCHRE 考古数据库开发的 Svelte 应用，展示乌加里特王国楔形文字泥板文物清单，含 MapLibre 交互地图、可筛选数据表格与动态路由，部署于 Vercel。',
      cover: '/images/Academic/ugarit-1.png',
      images: ['/images/Academic/ugarit-1.png', '/images/Academic/ugarit-2.png', '/images/Academic/ugarit-3.png'],
      link: 'https://ugarit-app.vercel.app',
      tools: ['Front-End Development', 'Web Design', 'HTML', 'CSS', 'Javescript', 'Svelte', 'OCHRE SDK', 'MapLibre', 'ShadCN', 'Tailwind'],
      color: '#7ea8c4',
    },
    {
      id: 'met',
      titleEn: 'Met Museum Explorer',
      titleZh: '大都会博物馆艺术探索',
      tagEn: 'Svelte 5 · TypeScript · Met API · Tailwind 4',
      tagZh: 'Svelte 5 · TypeScript · Met API · Tailwind 4',
      descEn: 'A Svelte 5 + TypeScript app that calls the Metropolitan Museum of Art API to let users explore fish and bird artworks. Features dynamic routing, random artwork selection, and nested detail pages. Deployed on Vercel.',
      descZh: '使用 Svelte 5 与 TypeScript 调用大都会博物馆 API，让用户探索馆藏鱼类与鸟类主题艺术品，含动态路由、随机展示与嵌套详情页，部署于 Vercel。',
      cover: '/images/Academic/met-1.png',
      images: ['/images/Academic/met-1.png', '/images/Academic/met-2.png', '/images/Academic/met-3.png'],
      link: 'https://met-museum-app-uiux.vercel.app',
      tools: ['Front-End Development', 'Web Design', 'HTML', 'CSS', 'Javescript', 'Svelte', 'TypeScript', 'Tailwind', 'Met API'],
      color: '#c4a45a',
    },
    {
      id: 'brave',
      titleEn: 'Brave Buddies',
      titleZh: 'Brave Buddies',
      tagEn: 'Unity · C# · 2D Platformer · Multiplayer',
      tagZh: 'Unity · C# · 2D横版游戏 · 双人合作',
      descEn: 'A co-op 2D platformer built in Unity where two players — Wilo and Gabby — fight through forests and caves to rescue enslaved villagers. Features custom sprite animations, enemy AI, tilemap level design, and multiplayer controls.',
      descZh: '使用 Unity 开发的双人合作 2D 横版闯关游戏，玩家扮演 Wilo 与 Gabby 穿越森林与洞穴，击败恶魔救援村民，含自定义精灵动画、敌人 AI、Tilemap 关卡设计与双人控制系统。',
      cover: '/images/Academic/brave-1.png',
      images: ['/images/Academic/brave-1.png', '/images/Academic/brave-2.png', '/images/Academic/brave-3.png', '/images/Academic/brave-4.png'],
      link: 'https://kikoli0620.itch.io/brave-buddies',
      tools: ['Unity', 'C#', 'Figma', 'Adobe Photoshop', 'Tilemap', 'Sprite Animation'],
      color: '#a07ed4',
    },
    {
      id: 'spider',
      titleEn: 'Arduino Spider Robot',
      titleZh: 'Arduino 六足蜘蛛机器人',
      tagEn: 'Arduino · 3D Printing · Fusion 360 · PCB',
      tagZh: 'Arduino · 3D打印 · Fusion 360 · PCB焊接',
      descEn: 'A six-legged walking robot built from scratch — designed body parts in Fusion 360, printed with 3D printer, hand-soldered a custom PCB with ESP32, and programmed servo gait sequences in Arduino. A full hardware-to-firmware pipeline.',
      descZh: '从零构建的六足行走机器人——在 Fusion 360 中建模机身结构，3D 打印各关节部件，手工焊接含 ESP32 的自定义 PCB，并用 Arduino 编程舵机步态序列，完整覆盖硬件到固件的全流程。',
      cover: '/images/Academic/spider-3.jpg',
      images: ['/images/Academic/spider-5.jpg', '/images/Academic/spider-1.jpg', '/images/Academic/spider-2.jpg', '/images/Academic/spider-4.jpg', '/images/Academic/spider-3.jpg'],
      link: null,
      tools: ['Arduino', 'Fusion 360', '3D Printing', 'ESP32', 'PCB Design'],
      color: '#6ec49a',
    },
  ];

  const experiences = [
    {
      companyEn: 'Lifang International Digital Technology Co., Ltd.',
      companyZh: '力方国际数字科技有限公司',
      roleEn: 'UI Design Intern',
      roleZh: 'UI 设计实习生',
      period: '2025.07 – 2025.09',
      locationEn: 'Chengdu',
      locationZh: '成都',
      bulletsEn: [
        'Led UI/UX design for digital cultural interactive products; delivered 30+ lo-fi and hi-fi prototypes and component designs, driving core feature pages from concept to implementation.',
        'Built 20+ components and information architectures in Figma to enhance interface consistency and support efficient front-end development, improving design-development collaboration efficiency by 30%',
        'Participated in requirements reviews and version iterations under agile development; collaborated with PM and 20+ developers to optimize user experience and usability.',
      ],
      bulletsZh: [
        '负责数字文化互动产品 UI/UX 设计，完成 30+ 低保真/高保真原型及组件设计，推动核心功能页面设计落地。',
        '使用 Figma构建20+组件与信息架构，增强界面一致性并支持前端开发高效实现，提升设计开发协作效率30%。',
        '在敏捷开发模式下参与需求评审与版本迭代，协同 PM 及20+开发成员优化用户体验与可用性。',
      ],
    },
    {
      companyEn: 'USC – Prof. Aisling Kelliher "Memory Research"',
      companyZh: '南加州大学 Aisling Kelliher 教授 Memory Research 项目',
      roleEn: 'Research Assistant',
      roleZh: '项目研究员',
      period: '2024.06 – 2024.07',
      locationEn: 'Los Angeles',
      locationZh: '洛杉矶',
      bulletsEn: [
        'Designed AI interaction systems and optimized user experience; built human-computer interaction frameworks centered on emotion recognition and memory management, completing prototype design and interaction logic.',
        'Analyzed 100+ cases and user behaviors; proposed 10+ product optimization recommendations to drive experience improvements for the AI interaction system.',
      ],
      bulletsZh: [
        '设计AI交互系统与优化用户体验，围绕情感识别、记忆管理构建人机交互框架，完成原型设计与交互逻辑。',
        '·基于100+条案例与用户行为分析，提出 10+ 产品优化建议，推动 AI 交互系统体验改进。',
      ],
    },
    {
      companyEn: 'Nanjing Museum – Exhibition Department',
      companyZh: '南京博物院陈列部',
      roleEn: 'Intern',
      roleZh: '实习生',
      period: '2023.08 – 2023.09',
      locationEn: 'Nanjing',
      locationZh: '南京',
      bulletsEn: [
        'Participated in 3 major exhibition projects: the 90th Anniversary Institutional Exhibition, the William Morris Special Exhibition, and the Jiangsu Province Top Ten Fine Exhibitions review.',
        'Organized 1,000+ archival materials, manually translated 20+ Chinese/English documents, assisted with design and layout, coordinated artifact packaging and transportation, and collaborated with designers and multiple institutions.',
        'Hosted 10+ industry expert judges, arranged review venues, and participated in guided presentations; strengthened project execution and collaborative communication skills within a high-standard team environment.',
      ],
      bulletsZh: [
        '参与3 场大型展览项目：九十周年院展、威廉·莫里斯特展及江苏省十大精品展评审。',
        '整理资料1000+、人工翻译中英文文稿20+、协助设计与排版、包装运输文物、配合设计师与多方机构推进工作。',
        '接待业内专家评委10+，布置评审会场并参与学习讲解，在高标准团队氛围中提升项目执行力与协作沟通能力。',
      ],
    },
  ];

  const campus = [
    {
      companyEn: 'UCSD Envision Lab',
      companyZh: '加州大学圣地亚哥分校 Envision Lab',
      roleEn: 'Lab Assistant',
      roleZh: '实验室助理',
      period: '2024.01 – 2025.06',
      locationEn: 'San Diego',
      locationZh: '圣地亚哥',
      bulletsEn: [
        'Supported 300+ students in design and prototyping practice; guided problem decomposition and solution building around user needs, facilitating cross-disciplinary collaboration between engineering and visual design.',
        'Provided hands-on guidance for 3D printing, laser cutting, soldering, and weaving equipment to support prototype iteration and interaction implementation, strengthening design expression and execution capabilities.',
      ],
      bulletsZh: [
        '支持 300+ 学生开展设计与原型实践，围绕用户需求引导问题拆解与方案构建，促进工程与视觉设计跨学科协作。',
        '提供 3D 打印、激光切割、焊接与编织等设备指导，支持原型迭代与交互实现，强化设计表达与落地能力。',
      ],
    },
  ];

  const skills = ['Figma', 'UI/UX Design', 'Interaction Design', 'Prototyping', 'Design Systems', 'User Research', 'HCI', 'XR/VR', 'HTML/CSS', 'JavaScript', 'Svelte', 'Python', 'Arduino', 'P5.js', 'Blender', 'Adobe PS', 'Illustrator', 'InDesign', 'Unreal Engine', 'Unity'];

  onMount(() => {
    gsap.registerPlugin(ScrollTrigger);
    gsap.from('.hero-greeting', { opacity: 0, y: 60, duration: 1, delay: 0.2, ease: 'power3.out' });
    gsap.from('.hero-name', { opacity: 0, y: 80, duration: 1.2, delay: 0.5, ease: 'power3.out' });
    gsap.from('.hero-sub', { opacity: 0, y: 40, duration: 1, delay: 0.9, ease: 'power3.out' });
    gsap.from('.hero-desc', { opacity: 0, y: 30, duration: 1, delay: 1.1, ease: 'power3.out' });
    gsap.from('.hero-cta', { opacity: 0, y: 30, duration: 1, delay: 1.3, ease: 'power3.out' });
    gsap.from('.hero-image', { opacity: 0, x: 80, duration: 1.4, delay: 0.4, ease: 'power3.out' });

    gsap.utils.toArray('.fade-up').forEach(el => {
      gsap.from(el, {
        scrollTrigger: { trigger: el, start: 'top 85%' },
        opacity: 0, y: 50, duration: 0.9, ease: 'power3.out'
      });
    });

    gsap.utils.toArray('.skill-tag').forEach((el, i) => {
      gsap.from(el, {
        scrollTrigger: { trigger: el, start: 'top 90%' },
        opacity: 0, y: 20, duration: 0.5, delay: i * 0.04, ease: 'power2.out'
      });
    });

    gsap.utils.toArray('.exp-item').forEach((el, i) => {
      gsap.from(el, {
        scrollTrigger: { trigger: el, start: 'top 85%' },
        opacity: 0, x: -40, duration: 0.9, delay: i * 0.15, ease: 'power3.out'
      });
    });

    const cursor = document.querySelector('.cursor');
    const cursorDot = document.querySelector('.cursor-dot');
    if (cursor && cursorDot) {
      document.addEventListener('mousemove', (e) => {
        gsap.to(cursor, { x: e.clientX, y: e.clientY, duration: 0.4, ease: 'power2.out' });
        gsap.to(cursorDot, { x: e.clientX, y: e.clientY, duration: 0.1 });
      });
    }
  });

  function scrollTo(id) {
    document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' });
    menuOpen = false;
  }

  function openProject(p) {
    activeProject = p;
    document.body.style.overflow = 'hidden';
  }

  function closeProject() {
    activeProject = null;
    document.body.style.overflow = '';
  }

  function openAcademic(p) {
    activeAcademic = p;
    document.body.style.overflow = 'hidden';
  }

  function closeAcademic() {
    activeAcademic = null;
    document.body.style.overflow = '';
  }

  function toggleLang() {
    lang = lang === 'en' ? 'zh' : 'en';
  }

  function currentImages(project) {
    return lang === 'zh' ? project.imagesZh : project.imagesEn;
  }

  const email = 'shz059' + '@' + 'uchicago.edu';
</script>

<div class="cursor"></div>
<div class="cursor-dot"></div>

<!-- NAV -->
<nav class="fixed top-0 left-0 right-0 z-50 flex items-center justify-between px-8 py-5 nav-bar">
  <button onclick={()=>scrollTo('hero')} class="sy-logo">SY</button>
  <div class="hidden md:flex items-center gap-8">
    {#each t[lang].nav as item, i}
      <button onclick={()=>scrollTo(t[lang].navIds[i])} class="nav-link">{item}</button>
    {/each}
    <button onclick={toggleLang} class="lang-btn">{lang === 'en' ? '中文' : 'EN'}</button>
  </div>
  <button onclick={()=>menuOpen=!menuOpen} class="md:hidden flex flex-col gap-1.5 p-2">
    <div class="w-6 h-px bg-white transition-all" style="transform:{menuOpen?'rotate(45deg) translateY(6px)':'none'}"></div>
    <div class="w-6 h-px bg-white" style="opacity:{menuOpen?0:1}"></div>
    <div class="w-6 h-px bg-white transition-all" style="transform:{menuOpen?'rotate(-45deg) translateY(-6px)':'none'}"></div>
  </button>
</nav>

{#if menuOpen}
  <div class="fixed inset-0 z-40 bg-black flex flex-col items-center justify-center gap-8">
    {#each t[lang].nav as item, i}
      <button onclick={()=>scrollTo(t[lang].navIds[i])} class="text-white text-4xl tracking-widest uppercase" style="font-family:'Bebas Neue',sans-serif">{item}</button>
    {/each}
    <button onclick={()=>{toggleLang();menuOpen=false;}} class="text-white/50 text-base mt-4" style="font-family:'Space Mono',monospace">{lang==='en'?'切换中文':'Switch to EN'}</button>
  </div>
{/if}

<!-- HERO -->
<section id="hero" class="relative min-h-screen flex items-center overflow-hidden" style="background:#0a0a0a">
  <div class="noise-overlay"></div>
  <div class="vert-line" style="right:33.333%"></div>
  <div class="vert-line" style="left:33.333%"></div>

  <div class="relative z-10 w-full max-w-7xl mx-auto px-8 md:px-16 pt-32 pb-20 grid md:grid-cols-2 gap-12 items-center">
    <div>
      <p class="hero-greeting mono-sm mb-4" style="color:rgba(255,255,255,0.35)">{t[lang].heroGreeting}</p>
      <h1 class="hero-name display-font text-7xl md:text-9xl leading-none mb-6" style="color:white">{t[lang].heroName}</h1>
      <p class="hero-sub text-lg md:text-xl font-light mb-4 leading-relaxed" style="color:rgba(255,255,255,0.65)">{t[lang].heroSub}</p>
      <p class="hero-desc mono-sm tracking-wide mb-10" style="color:rgba(255,255,255,0.3)">{t[lang].heroDesc}</p>
      <button onclick={()=>scrollTo('projects')} class="hero-cta cta-btn">
        {t[lang].heroCta} <span class="arrow">→</span>
      </button>
    </div>
    <div class="hero-image relative flex justify-center">
      <img src="/images/photos/photo1.jpeg" alt="Shangyi Zhou" class="hero-photo w-full max-w-xs object-cover" style="aspect-ratio:3/4" />
      <div class="badge">
        <p class="mono-sm" style="color:rgba(255,255,255,0.45)">UChicago · UCSD</p>
      </div>
    </div>
  </div>

  <div class="scroll-hint">
    <div class="scroll-line"></div>
    <p class="mono-sm" style="color:rgba(255,255,255,0.22)">SCROLL</p>
  </div>
</section>

<!-- ABOUT -->
<section id="about" style="background:#111;padding:8rem 2rem">
  <div class="max-w-7xl mx-auto grid md:grid-cols-2 gap-20 items-center">
    <div class="relative" style="padding-bottom:2.5rem;padding-right:2.5rem">
      <img src="/images/photos/photo2.jpeg" alt="Machu Picchu" class="fade-up w-full object-cover" style="aspect-ratio:1/1" />
      <img src="/images/photos/photo3.jpeg" alt="Envision Lab" class="fade-up absolute object-cover" style="width:10rem;height:10rem;bottom:0;right:0;border:4px solid #111" />
    </div>
    <div>
      <p class="fade-up mono-sm mb-4" style="color:rgba(255,255,255,0.28)">01 / {t[lang].aboutTitle}</p>
      <h2 class="fade-up display-font text-5xl md:text-6xl mb-8" style="color:white">{t[lang].aboutTitle}</h2>
      <p class="fade-up text-lg leading-relaxed mb-6" style="color:rgba(255,255,255,0.58)">{t[lang].aboutText}</p>
      <p class="fade-up text-lg leading-relaxed mb-12" style="color:rgba(255,255,255,0.58)">{t[lang].aboutText2}</p>
      <p class="fade-up mono-sm mb-6" style="color:rgba(255,255,255,0.28)">{t[lang].skillsTitle}</p>
      <div class="flex flex-wrap gap-2">
        {#each skills as skill}
          <span class="skill-tag mono-sm px-3 py-1.5 border transition-all cursor-default" style="color:rgba(255,255,255,0.5);border-color:rgba(255,255,255,0.1)">{skill}</span>
        {/each}
      </div>
    </div>
  </div>
</section>

<!-- PROJECTS — large hover rows -->
<section id="projects" style="background:#0a0a0a;padding:8rem 2rem 4rem">
  <div class="max-w-7xl mx-auto">
    <p class="fade-up mono-sm mb-4" style="color:rgba(255,255,255,0.28)">02 / {t[lang].projectsTitle}</p>
    <h2 class="fade-up display-font text-5xl md:text-6xl mb-16" style="color:white">{t[lang].projectsTitle}</h2>

    <div style="border-top:1px solid rgba(255,255,255,0.07)">
      {#each projects as project, i}
        <div
          class="proj-row"
          onmouseenter={() => hoveredProject = project.id}
          onmouseleave={() => hoveredProject = null}
          onclick={() => openProject(project)}
          role="button"
          tabindex="0"
          onkeydown={(e) => e.key==='Enter' && openProject(project)}
          style="
            border-bottom:1px solid rgba(255,255,255,0.07);
            cursor:pointer;
            overflow:hidden;
            transition: background 0.5s ease, padding 0.5s ease;
            background:{hoveredProject===project.id ? project.color+'14' : 'transparent'};
            padding:{hoveredProject===project.id ? '2.5rem 1rem' : '1.8rem 1rem'};
          "
        >
          <div style="display:flex;align-items:center;gap:2rem;flex-wrap:wrap">
            <span class="mono-sm" style="color:rgba(255,255,255,0.22);min-width:2rem">0{i+1}</span>
            <h3 class="display-font" style="
              font-size:clamp(2.2rem,4.5vw,4rem);
              color:{hoveredProject===project.id ? project.color : 'white'};
              transition:color 0.35s ease;
              line-height:1;
              flex-shrink:0;
            ">{lang==='en' ? project.titleEn : project.titleZh}</h3>
            <span class="mono-sm" style="color:rgba(255,255,255,0.32);flex:1">{lang==='en' ? project.tagEn : project.tagZh}</span>
            <span style="
              font-size:1.4rem;
              color:{hoveredProject===project.id ? project.color : 'rgba(255,255,255,0.18)'};
              transition:color 0.3s,transform 0.3s;
              transform:{hoveredProject===project.id ? 'translate(3px,-3px)' : 'none'};
              display:inline-block;
            ">↗</span>
          </div>

          <div style="
            display:grid;
            grid-template-columns:1fr 1fr;
            gap:2rem;
            margin-top:{hoveredProject===project.id ? '2rem' : '0'};
            max-height:{hoveredProject===project.id ? '36rem' : '0'};
            opacity:{hoveredProject===project.id ? 1 : 0};
            overflow:hidden;
            transition:max-height 0.55s cubic-bezier(0.4,0,0.2,1), opacity 0.4s ease, margin-top 0.4s ease;
          ">
            <div style="overflow:hidden;border-radius:2px">
              <img src={project.cover} alt={project.titleEn}
                style="width:100%;height:100%;object-fit:cover;max-height:32rem;transition:transform 0.6s ease;transform:{hoveredProject===project.id?'scale(1.03)':'scale(1)'}" />
            </div>
            <div style="display:flex;flex-direction:column;justify-content:center;gap:1.5rem;padding:1rem 0">
              <p style="font-size:0.95rem;line-height:1.75;color:rgba(255,255,255,0.58)">{lang==='en' ? project.descEn : project.descZh}</p>
              <div style="display:flex;flex-wrap:wrap;gap:0.5rem">
                {#each project.tools as tool}
                  <span class="mono-sm px-3 py-1" style="border:1px solid {project.color}55;color:{project.color}">{tool}</span>
                {/each}
              </div>
              <p class="mono-sm" style="color:{project.color};letter-spacing:0.15em">
                {lang==='en' ? '↗ CLICK TO VIEW FULL PROJECT' : '↗ 点击查看完整项目'}
              </p>
            </div>
          </div>
        </div>
      {/each}
    </div>
  </div>
</section>

<!-- ACADEMIC PROJECTS -->
<section style="background:#0a0a0a;padding:0 2rem 8rem">
  <div class="max-w-7xl mx-auto">

    <!-- 次级标题 -->
    <div style="display:flex;align-items:center;gap:1rem;margin-bottom:2rem;padding-top:1rem">
      <div style="width:1.5rem;height:1px;background:rgba(255,255,255,0.15)"></div>
      <p class="mono-sm" style="color:rgba(255,255,255,0.22)">
        {lang==='en' ? '↳ Academic & Course Projects' : '↳ 课程与学校项目'}
      </p>
    </div>

    <!-- 四格卡片 grid -->
    <div style="display:grid;grid-template-columns:repeat(4,1fr);gap:1px;background:rgba(255,255,255,0.06)" class="academic-grid">
      {#each academicProjects as proj}
        <div
          class="academic-card"
          onclick={() => openAcademic(proj)}
          role="button"
          tabindex="0"
          onkeydown={(e) => e.key==='Enter' && openAcademic(proj)}
        >
          <!-- Cover -->
          <div class="academic-cover">
            <img src={proj.cover} alt={proj.titleEn} class="academic-img" />
            <!-- Color accent bar -->
            <div style="position:absolute;bottom:0;left:0;right:0;height:2px;background:{proj.color};opacity:0.6"></div>
          </div>

          <!-- Info -->
          <div style="padding:1.25rem 1.5rem 1.5rem;display:flex;flex-direction:column;gap:0.6rem;flex:1">
            <p class="mono-sm" style="color:{proj.color};letter-spacing:0.1em;font-size:0.65rem">
              {lang==='en' ? proj.tagEn : proj.tagZh}
            </p>
            <h3 style="font-family:'Bebas Neue',sans-serif;font-size:1.3rem;color:white;line-height:1.1">
              {lang==='en' ? proj.titleEn : proj.titleZh}
            </h3>
            <p style="font-size:0.78rem;line-height:1.65;color:rgba(255,255,255,0.35)">
              {lang==='en'
                ? (proj.descEn.length > 110 ? proj.descEn.slice(0,110)+'…' : proj.descEn)
                : (proj.descZh.length > 55 ? proj.descZh.slice(0,55)+'…' : proj.descZh)}
            </p>
            <div style="display:flex;justify-content:space-between;align-items:center;margin-top:auto;padding-top:0.75rem;border-top:1px solid rgba(255,255,255,0.05)">
              <div style="display:flex;gap:0.4rem;flex-wrap:wrap">
                {#each proj.tools.slice(0,3) as tool, ti}
                  <span class="mono-sm" style="color:rgba(255,255,255,0.22);font-size:0.62rem">{tool}{ti < Math.min(proj.tools.length,3)-1 ? ' ·' : ''}</span>
                {/each}
              </div>
              <span style="font-size:0.8rem;color:{proj.color};opacity:0.65">↗</span>
            </div>
          </div>
        </div>
      {/each}
    </div>
  </div>
</section>

<!-- EXPERIENCE -->
<section id="experience" style="background:#111;padding:8rem 2rem">
  <div class="max-w-7xl mx-auto">
    <p class="fade-up mono-sm mb-4" style="color:rgba(255,255,255,0.28)">03 / {t[lang].expTitle}</p>
    <h2 class="fade-up display-font text-5xl md:text-6xl mb-20" style="color:white">{t[lang].expTitle}</h2>
    <div>
      {#each experiences as exp}
        <div class="exp-item" style="border-top:1px solid rgba(255,255,255,0.07);padding:3rem 0;display:grid;grid-template-columns:1fr 2fr;gap:3rem">
          <div>
            <p class="mono-sm mb-2" style="color:rgba(255,255,255,0.28)">{exp.period}</p>
            <p class="mono-sm" style="color:rgba(255,255,255,0.22)">{lang==='en'?exp.locationEn:exp.locationZh}</p>
          </div>
          <div>
            <h3 class="display-font text-2xl mb-1" style="color:white">{lang==='en'?exp.roleEn:exp.roleZh}</h3>
            <p class="mono-sm mb-6" style="color:rgba(255,255,255,0.32)">{lang==='en'?exp.companyEn:exp.companyZh}</p>
            <ul style="display:flex;flex-direction:column;gap:0.75rem">
              {#each (lang==='en'?exp.bulletsEn:exp.bulletsZh) as bullet}
                <li style="display:flex;gap:0.75rem;font-size:0.875rem;line-height:1.65;color:rgba(255,255,255,0.52)">
                  <span style="color:rgba(255,255,255,0.18);flex-shrink:0;margin-top:0.1rem">—</span>{bullet}
                </li>
              {/each}
            </ul>
          </div>
        </div>
      {/each}
      <div style="border-top:1px solid rgba(255,255,255,0.07)"></div>
    </div>
  </div>
</section>

<!-- CAMPUS -->
<section style="background:#0a0a0a;padding:0 2rem 8rem">
  <div class="max-w-7xl mx-auto">
    <p class="fade-up mono-sm mb-12" style="color:rgba(255,255,255,0.28)">{t[lang].campusTitle}</p>
    <div>
      {#each campus as exp}
        <div class="exp-item" style="border-top:1px solid rgba(255,255,255,0.07);padding:3rem 0;display:grid;grid-template-columns:1fr 2fr;gap:3rem">
          <div>
            <p class="mono-sm mb-2" style="color:rgba(255,255,255,0.28)">{exp.period}</p>
            <p class="mono-sm" style="color:rgba(255,255,255,0.22)">{lang==='en'?exp.locationEn:exp.locationZh}</p>
          </div>
          <div>
            <h3 class="display-font text-2xl mb-1" style="color:white">{lang==='en'?exp.roleEn:exp.roleZh}</h3>
            <p class="mono-sm mb-6" style="color:rgba(255,255,255,0.32)">{lang==='en'?exp.companyEn:exp.companyZh}</p>
            <ul style="display:flex;flex-direction:column;gap:0.75rem">
              {#each (lang==='en'?exp.bulletsEn:exp.bulletsZh) as bullet}
                <li style="display:flex;gap:0.75rem;font-size:0.875rem;line-height:1.65;color:rgba(255,255,255,0.52)">
                  <span style="color:rgba(255,255,255,0.18);flex-shrink:0;margin-top:0.1rem">—</span>{bullet}
                </li>
              {/each}
            </ul>
          </div>
        </div>
      {/each}
      <div style="border-top:1px solid rgba(255,255,255,0.07)"></div>
    </div>
  </div>
</section>

<!-- RESUME -->
<section id="resume" style="background:#111;padding:8rem 2rem">
  <div class="max-w-7xl mx-auto text-center">
    <p class="fade-up mono-sm mb-4" style="color:rgba(255,255,255,0.28)">04 / {t[lang].resumeTitle}</p>
    <h2 class="fade-up display-font text-5xl md:text-6xl mb-8" style="color:white">{t[lang].resumeTitle}</h2>
    <p class="fade-up text-lg mb-12 max-w-xl mx-auto" style="color:rgba(255,255,255,0.38)">{t[lang].resumeDesc}</p>
    <div class="fade-up flex flex-col sm:flex-row gap-4 justify-center">
      <a href="/resume-zh.pdf" download="周尚嶷简历-中文版2026.pdf"
        class="inline-flex items-center gap-3 bg-white text-black px-8 py-4 text-sm font-bold tracking-widest uppercase hover:bg-white/85 transition-all">
        {t[lang].resumeBtnZh} ↓
      </a>
      <a href="/resume-en.pdf" target="_blank"
        class="inline-flex items-center gap-3 border border-white/30 text-white px-8 py-4 text-sm font-bold tracking-widest uppercase hover:bg-white hover:text-black transition-all">
        {t[lang].resumeBtnEn} ↓
      </a>
    </div>
  </div>
</section>

<!-- CONTACT -->
<section id="contact" style="background:#111;padding:8rem 2rem">
  <div class="max-w-7xl mx-auto">
    <p class="fade-up mono-sm mb-4" style="color:rgba(255,255,255,0.28)">05 / {t[lang].contactTitle}</p>
    <h2 class="fade-up display-font text-5xl md:text-6xl mb-8" style="color:white">{t[lang].contactTitle}</h2>
    <p class="fade-up text-lg mb-12 max-w-2xl" style="color:rgba(255,255,255,0.38)">{t[lang].contactDesc}</p>
    <div class="fade-up flex flex-col sm:flex-row gap-6">
      <a href="mailto:{email}" class="inline-flex items-center gap-3 border border-white/20 text-white px-8 py-4 mono-sm tracking-widest hover:bg-white hover:text-black transition-all">
        ✉ {email}
      </a>
      <a href="https://www.zcool.com.cn/work/ZNzMyNzAyMjA=.html" target="_blank" class="inline-flex items-center gap-3 border border-white/20 px-8 py-4 mono-sm tracking-widest hover:border-white hover:text-white transition-all" style="color:rgba(255,255,255,0.45)">
        {t[lang].portfolioLink} ↗
      </a>
    </div>
  </div>
</section>

<footer style="background:#0a0a0a;border-top:1px solid rgba(255,255,255,0.05);padding:2rem">
  <div class="max-w-7xl mx-auto flex flex-col md:flex-row justify-between items-center gap-4">
    <p class="mono-sm" style="color:rgba(255,255,255,0.18)">© 2025 Shangyi Zhou · 周尚嶷</p>
    <p class="mono-sm" style="color:rgba(255,255,255,0.18)">{email}</p>
  </div>
</footer>

<!-- PROJECT MODAL (main) -->
{#if activeProject}
  <div class="modal-overlay" style="background:{activeProject.bg}">
    <div class="modal-topbar">
      <div style="display:flex;align-items:center;gap:1.5rem">
        <p class="mono-sm" style="color:{activeProject.color};letter-spacing:0.2em;text-transform:uppercase">
          {lang==='en' ? activeProject.tagEn : activeProject.tagZh}
        </p>
      </div>
      <div style="display:flex;align-items:center;gap:1rem">
        <button onclick={toggleLang} class="lang-btn-modal" style="border-color:{activeProject.color}50;color:{activeProject.color}">
          {lang==='en' ? '中文' : 'EN'}
        </button>
        <button onclick={closeProject} class="lang-btn-modal" style="border-color:{activeProject.color}50;color:{activeProject.color}">
          {t[lang].close}
        </button>
      </div>
    </div>

    <div style="max-width:72rem;margin:0 auto;padding:2rem 2rem 6rem">
      <h2 class="display-font" style="font-size:clamp(3rem,7vw,5.5rem);color:white;line-height:1;margin-bottom:1.5rem">
        {lang==='en' ? activeProject.titleEn : activeProject.titleZh}
      </h2>
      <p style="color:rgba(255,255,255,0.55);font-size:1.05rem;line-height:1.75;margin-bottom:2rem;max-width:42rem">
        {lang==='en' ? activeProject.descEn : activeProject.descZh}
      </p>
      <div style="display:flex;flex-wrap:wrap;gap:0.5rem;margin-bottom:3rem">
        {#each activeProject.tools as tool}
          <span class="mono-sm px-4 py-1.5" style="border:1px solid {activeProject.color}50;color:{activeProject.color}">{tool}</span>
        {/each}
      </div>
      <div style="display:flex;flex-direction:column;gap:0.5rem">
        {#each currentImages(activeProject) as img}
          <img src={img} alt="" style="width:100%;display:block;object-fit:contain" />
        {/each}
      </div>
    </div>
  </div>
{/if}

<!-- ACADEMIC MODAL -->
{#if activeAcademic}
  <div class="modal-overlay" style="background:#0d0d0d">
    <div class="modal-topbar">
      <p class="mono-sm" style="color:{activeAcademic.color};letter-spacing:0.2em;text-transform:uppercase">
        {lang==='en' ? activeAcademic.tagEn : activeAcademic.tagZh}
      </p>
      <div style="display:flex;align-items:center;gap:1rem">
        <button onclick={toggleLang} class="lang-btn-modal" style="border-color:rgba(255,255,255,0.2);color:rgba(255,255,255,0.45)">
          {lang==='en' ? '中文' : 'EN'}
        </button>
        <button onclick={closeAcademic} class="lang-btn-modal" style="border-color:rgba(255,255,255,0.15);color:rgba(255,255,255,0.45)">
          {t[lang].close}
        </button>
      </div>
    </div>

    <div style="max-width:72rem;margin:0 auto;padding:2rem 2rem 6rem">
      <!-- Two-column header: info left, button right -->
      <div style="display:flex;gap:3rem;align-items:flex-start;margin-bottom:3rem;flex-wrap:wrap">
        <!-- Left: title + desc + tags -->
        <div style="flex:1;min-width:0">
          <h2 class="display-font" style="font-size:clamp(2.5rem,6vw,4.5rem);color:white;line-height:1;margin-bottom:1rem">
            {lang==='en' ? activeAcademic.titleEn : activeAcademic.titleZh}
          </h2>
          <p style="color:rgba(255,255,255,0.5);font-size:1rem;line-height:1.8;margin-bottom:1.5rem">
            {lang==='en' ? activeAcademic.descEn : activeAcademic.descZh}
          </p>
          <div style="display:flex;flex-wrap:wrap;gap:0.5rem">
            {#each activeAcademic.tools as tool}
              <span class="mono-sm px-4 py-1.5" style="border:1px solid {activeAcademic.color}40;color:{activeAcademic.color}">{tool}</span>
            {/each}
          </div>
        </div>
        <!-- Right: visit button -->
        {#if activeAcademic.link}
          <div style="flex-shrink:0;display:flex;align-items:center">
            <a
              href={activeAcademic.link}
              target="_blank"
              style="
                display:inline-flex;
                align-items:center;
                gap:0.75rem;
                border:1px solid {activeAcademic.color};
                color:{activeAcademic.color};
                padding:1rem 2rem;
                font-family:'Space Mono',monospace;
                font-size:0.75rem;
                letter-spacing:0.18em;
                text-transform:uppercase;
                text-decoration:none;
                transition:background 0.25s, color 0.25s;
                white-space:nowrap;
              "
              onmouseenter={(e) => {
                e.currentTarget.style.background = activeAcademic.color;
                e.currentTarget.style.color = '#000';
              }}
              onmouseleave={(e) => {
                e.currentTarget.style.background = 'transparent';
                e.currentTarget.style.color = activeAcademic.color;
              }}
            >
              {lang==='en' ? 'VISIT LIVE SITE' : '访问网站'} ↗
            </a>
          </div>
        {/if}
      </div>
      <div style="display:flex;flex-direction:column;gap:0.5rem">
        {#each activeAcademic.images as img}
          <img src={img} alt="" style="width:100%;display:block;object-fit:contain" />
        {/each}
      </div>
    </div>
  </div>
{/if}

<style>
  @import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Space+Mono:wght@400;700&display=swap');

  :global(html) { scroll-behavior: smooth; }
  :global(body) { background: #0a0a0a; overflow-x: hidden; }

  .display-font { font-family: 'Bebas Neue', sans-serif; }
  .mono-sm { font-family: 'Space Mono', monospace; font-size: 0.72rem; letter-spacing: 0.12em; }

  .nav-bar {
    background: linear-gradient(to bottom, rgba(10,10,10,0.92), transparent);
    backdrop-filter: blur(8px);
  }
  .sy-logo { font-family:'Bebas Neue',sans-serif; font-size:1.25rem; color:white; letter-spacing:0.15em; }
  .nav-link { font-family:'Space Mono',monospace; font-size:0.72rem; color:white; letter-spacing:0.18em; text-transform:uppercase; transition:opacity 0.2s; }
  .nav-link:hover { opacity: 0.45; }
  .lang-btn { font-family:'Space Mono',monospace; font-size:0.72rem; color:white; border:1px solid rgba(255,255,255,0.35); padding:0.25rem 0.85rem; border-radius:999px; transition:all 0.2s; }
  .lang-btn:hover { background:white; color:black; }

  .noise-overlay {
    position:absolute; inset:0; opacity:0.038;
    background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='200'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4'/%3E%3C/filter%3E%3Crect width='200' height='200' filter='url(%23n)'/%3E%3C/svg%3E");
    background-size:200px;
  }
  .vert-line { position:absolute; top:0; width:1px; height:100%; background:rgba(255,255,255,0.04); }

  .hero-photo { transition: filter 0.8s ease; }
  .hero-photo:hover { filter: sepia(0.5) saturate(2.5) hue-rotate(280deg) brightness(1.1); }

  .badge { position:absolute; bottom:1rem; right:1rem; padding:0.4rem 1rem; border:1px solid rgba(255,255,255,0.1); background:rgba(0,0,0,0.55); backdrop-filter:blur(6px); }

  .cta-btn { display:inline-flex; align-items:center; gap:0.75rem; background:white; color:black; padding:1rem 2rem; font-size:0.78rem; font-weight:700; letter-spacing:0.2em; text-transform:uppercase; transition:background 0.2s; }
  .cta-btn:hover { background:rgba(255,255,255,0.88); }
  .arrow { display:inline-block; transition:transform 0.3s; }
  .cta-btn:hover .arrow { transform:translateX(6px); }

  .scroll-hint { position:absolute; bottom:2rem; left:50%; transform:translateX(-50%); display:flex; flex-direction:column; align-items:center; gap:0.5rem; }
  .scroll-line { width:1px; height:4rem; background:linear-gradient(to bottom, transparent, rgba(255,255,255,0.28)); animation:pulse 2s ease-in-out infinite; }
  @keyframes pulse { 0%,100%{opacity:0.4} 50%{opacity:1} }

  .proj-row:focus { outline:none; }
  .proj-row:focus-visible { outline:2px solid rgba(255,255,255,0.25); }

  /* Academic cards */
  .academic-grid {
    border: 1px solid rgba(255,255,255,0.06);
  }

  .academic-card {
    background: #0a0a0a;
    cursor: pointer;
    display: flex;
    flex-direction: column;
    transition: background 0.3s ease;
    outline: none;
  }
  .academic-card:hover { background: #111; }
  .academic-card:focus-visible { outline: 1px solid rgba(255,255,255,0.2); }

  .academic-cover {
    position: relative;
    overflow: hidden;
    aspect-ratio: 16 / 10;
    background: #111;
  }
  .academic-img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
    transition: transform 0.5s ease, filter 0.5s ease;
    filter: brightness(0.35) saturate(0.4);
  }
  .academic-card:hover .academic-img {
    transform: scale(1.04);
    filter: brightness(1) saturate(1);
  }

  /* Modal */
  .modal-overlay {
    position:fixed; inset:0; z-index:100; overflow-y:auto;
    animation: modalIn 0.35s cubic-bezier(0.4,0,0.2,1);
  }
  @keyframes modalIn {
    from { opacity:0; transform:translateY(30px); }
    to { opacity:1; transform:translateY(0); }
  }

  .modal-topbar {
    position:sticky; top:0; z-index:10;
    display:flex; justify-content:space-between; align-items:center;
    padding:1.25rem 2rem;
    background:rgba(0,0,0,0.6);
    backdrop-filter:blur(12px);
    border-bottom:1px solid rgba(255,255,255,0.06);
    max-width:100%;
  }

  .lang-btn-modal {
    font-family:'Space Mono',monospace;
    font-size:0.7rem;
    letter-spacing:0.15em;
    padding:0.35rem 0.9rem;
    border:1px solid;
    background:transparent;
    cursor:pointer;
    transition:opacity 0.2s;
  }
  .lang-btn-modal:hover { opacity:0.6; }

  /* Cursor */
  .cursor { position:fixed; width:36px; height:36px; border:1px solid rgba(255,255,255,0.4); border-radius:50%; pointer-events:none; z-index:9999; top:0; left:0; transform:translate(-50%,-50%); display:none; transition:width 0.3s,height 0.3s; }
  .cursor-dot { position:fixed; width:4px; height:4px; background:white; border-radius:50%; pointer-events:none; z-index:9999; top:0; left:0; transform:translate(-50%,-50%); display:none; }
  @media (pointer:fine) { .cursor { display:block; } .cursor-dot { display:block; } }

  /* Tablet: academic grid two columns */
  @media (max-width: 900px) {
    .academic-grid {
      grid-template-columns: repeat(2, 1fr) !important;
    }
  }
  /* Mobile: academic grid single column */
  @media (max-width: 640px) {
    .academic-grid {
      grid-template-columns: 1fr !important;
    }
  }
</style>