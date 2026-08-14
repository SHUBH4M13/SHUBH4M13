<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Shubham Karna — Dashboard</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=JetBrains+Mono:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg-0:#08090c;
    --bg-1:#0d0f14;
    --surface:#12151c;
    --surface-2:#171a22;
    --border:rgba(255,255,255,0.08);
    --border-hi:rgba(255,255,255,0.16);
    --text-0:#f2f3f6;
    --text-1:#a7adbd;
    --text-2:#686f81;
    --blue:#5b8cff;
    --blue-soft:#5b8cff33;
    --violet:#9d6bff;
    --violet-soft:#9d6bff33;
    --grad: linear-gradient(135deg,var(--blue),var(--violet));
    --mono: 'JetBrains Mono', ui-monospace, monospace;
    --sans: 'Inter', -apple-system, sans-serif;
    --radius: 16px;
    --radius-sm: 10px;
  }
  *{box-sizing:border-box; margin:0; padding:0;}
  html{scroll-behavior:smooth;}
  body{
    background:
      radial-gradient(ellipse 900px 500px at 15% -10%, #5b8cff17, transparent 60%),
      radial-gradient(ellipse 800px 500px at 90% 10%, #9d6bff14, transparent 60%),
      var(--bg-0);
    color:var(--text-0);
    font-family:var(--sans);
    line-height:1.5;
    min-height:100vh;
  }
  .bg-grid{
    position:fixed; inset:0; z-index:-1; pointer-events:none;
    background-image:
      linear-gradient(rgba(255,255,255,0.025) 1px, transparent 1px),
      linear-gradient(90deg, rgba(255,255,255,0.025) 1px, transparent 1px);
    background-size:64px 64px;
    mask-image: radial-gradient(ellipse 80% 60% at 50% 0%, black 20%, transparent 75%);
  }
  ::selection{ background:var(--blue-soft); color:#fff; }
  a{ color:inherit; text-decoration:none; }
  .wrap{ max-width:1080px; margin:0 auto; padding:0 24px; }

  /* NAV */
  nav{
    position:sticky; top:0; z-index:50;
    backdrop-filter: blur(16px) saturate(140%);
    background:rgba(9,10,14,0.65);
    border-bottom:1px solid var(--border);
  }
  nav .inner{
    display:flex; align-items:center; justify-content:space-between;
    padding:14px 24px; max-width:1080px; margin:0 auto;
  }
  .brand{ display:flex; align-items:center; gap:10px; font-family:var(--mono); font-weight:600; font-size:14px; letter-spacing:0.02em; }
  .brand .dot{ width:8px; height:8px; border-radius:50%; background:var(--grad); box-shadow:0 0 12px var(--blue); animation:pulse 2.4s ease-in-out infinite; }
  @keyframes pulse{ 0%,100%{opacity:1;} 50%{opacity:0.4;} }
  .nav-links{ display:flex; align-items:center; gap:28px; font-size:13.5px; color:var(--text-1); }
  .nav-links a{ transition:color .2s ease; }
  .nav-links a:hover{ color:var(--text-0); }
  .kbd-hint{
    display:flex; align-items:center; gap:6px; font-family:var(--mono); font-size:12px; color:var(--text-2);
    border:1px solid var(--border); padding:5px 9px; border-radius:8px; cursor:pointer; transition:all .2s ease;
  }
  .kbd-hint:hover{ border-color:var(--border-hi); color:var(--text-1); }
  @media (max-width:760px){ .nav-links{ display:none; } }

  /* COMMAND PALETTE */
  .cmdk-overlay{
    position:fixed; inset:0; background:rgba(5,6,9,0.7); backdrop-filter:blur(4px);
    z-index:100; display:none; align-items:flex-start; justify-content:center; padding-top:14vh;
  }
  .cmdk-overlay.open{ display:flex; }
  .cmdk-box{
    width:92%; max-width:520px; background:var(--surface); border:1px solid var(--border-hi);
    border-radius:var(--radius); box-shadow:0 20px 60px rgba(0,0,0,0.5); overflow:hidden;
    animation:cmdkIn .16s ease;
  }
  @keyframes cmdkIn{ from{ opacity:0; transform:translateY(-8px) scale(.98);} to{opacity:1; transform:translateY(0) scale(1);} }
  .cmdk-input{
    width:100%; background:transparent; border:none; outline:none; color:var(--text-0);
    font-family:var(--mono); font-size:14px; padding:16px 18px; border-bottom:1px solid var(--border);
  }
  .cmdk-input::placeholder{ color:var(--text-2); }
  .cmdk-list{ padding:8px; }
  .cmdk-item{
    display:flex; align-items:center; justify-content:space-between; padding:10px 12px; border-radius:var(--radius-sm);
    font-size:13.5px; color:var(--text-1); cursor:pointer; transition:all .12s ease;
  }
  .cmdk-item:hover, .cmdk-item.active{ background:var(--surface-2); color:var(--text-0); }
  .cmdk-item span.tag{ font-family:var(--mono); font-size:11px; color:var(--text-2); }

  /* HERO */
  .hero{ padding:88px 0 56px; }
  .terminal{
    font-family:var(--mono); font-size:13.5px; color:var(--text-2);
    background:var(--surface); border:1px solid var(--border); border-radius:var(--radius-sm);
    padding:14px 18px; display:inline-block; margin-bottom:28px;
  }
  .terminal .prompt{ color:var(--blue); }
  .terminal .caret{ display:inline-block; width:7px; height:15px; background:var(--text-1); margin-left:2px; vertical-align:middle; animation:blink 1s step-end infinite; }
  @keyframes blink{ 50%{opacity:0;} }
  .hero h1{
    font-size:clamp(36px,6vw,58px); font-weight:800; letter-spacing:-0.03em; line-height:1.05;
    margin-bottom:16px;
  }
  .hero h1 .grad{ background:var(--grad); -webkit-background-clip:text; background-clip:text; color:transparent; }
  .hero p.sub{ font-size:17px; color:var(--text-1); max-width:560px; margin-bottom:32px; }
  .hero-actions{ display:flex; flex-wrap:wrap; gap:12px; margin-bottom:48px; }
  .btn{
    font-family:var(--sans); font-weight:600; font-size:13.5px; padding:11px 20px; border-radius:10px;
    display:inline-flex; align-items:center; gap:8px; transition:all .2s ease; border:1px solid transparent;
  }
  .btn-primary{ background:var(--text-0); color:#0a0b0e; }
  .btn-primary:hover{ transform:translateY(-2px); box-shadow:0 8px 24px rgba(255,255,255,0.12); }
  .btn-ghost{ border-color:var(--border); color:var(--text-1); }
  .btn-ghost:hover{ border-color:var(--border-hi); color:var(--text-0); transform:translateY(-2px); }

  .stat-row{ display:grid; grid-template-columns:repeat(4,1fr); gap:1px; background:var(--border); border:1px solid var(--border); border-radius:var(--radius); overflow:hidden; }
  .stat-cell{ background:var(--bg-1); padding:18px 20px; }
  .stat-cell .num{ font-family:var(--mono); font-size:24px; font-weight:700; }
  .stat-cell .lbl{ font-size:12px; color:var(--text-2); margin-top:4px; }
  @media (max-width:640px){ .stat-row{ grid-template-columns:repeat(2,1fr); } }

  /* SECTION HEADERS */
  section{ padding:56px 0; border-top:1px solid var(--border); }
  .section-head{ display:flex; align-items:baseline; justify-content:space-between; margin-bottom:28px; }
  .section-head h2{ font-size:22px; font-weight:700; letter-spacing:-0.01em; }
  .section-head .eyebrow{ font-family:var(--mono); font-size:11.5px; color:var(--blue); letter-spacing:0.08em; text-transform:uppercase; display:block; margin-bottom:6px; }
  .section-head .meta{ font-family:var(--mono); font-size:12px; color:var(--text-2); }

  /* CARDS / GLOW */
  .card{
    position:relative; background:var(--surface); border:1px solid var(--border); border-radius:var(--radius);
    padding:22px; overflow:hidden; transition:border-color .25s ease, transform .25s ease;
  }
  .card::before{
    content:""; position:absolute; inset:0; border-radius:inherit; padding:1px;
    background:radial-gradient(220px circle at var(--mx,50%) var(--my,50%), var(--blue-soft), transparent 60%);
    opacity:0; transition:opacity .3s ease; pointer-events:none;
  }
  .card:hover::before{ opacity:1; }
  .card:hover{ border-color:var(--border-hi); transform:translateY(-3px); }

  .bento{ display:grid; grid-template-columns:1.3fr 1fr; gap:16px; }
  .bento .span2{ grid-column:1/-1; }
  @media (max-width:760px){ .bento{ grid-template-columns:1fr; } }
  .bento img{ width:100%; border-radius:10px; display:block; }
  .card h3{ font-size:14px; font-weight:600; margin-bottom:14px; color:var(--text-1); }

  .about-card p{ color:var(--text-1); font-size:14.5px; margin-bottom:10px; }
  .about-card .kv{ display:flex; gap:8px; font-family:var(--mono); font-size:13px; margin-top:14px; }
  .about-card .kv b{ color:var(--text-2); font-weight:500; min-width:74px; }

  /* TECH STACK */
  .tech-group{ margin-bottom:20px; }
  .tech-group h4{ font-family:var(--mono); font-size:11.5px; color:var(--text-2); text-transform:uppercase; letter-spacing:0.08em; margin-bottom:12px; }
  .pill-row{ display:flex; flex-wrap:wrap; gap:8px; }
  .pill{
    display:inline-flex; align-items:center; gap:7px; font-size:12.5px; font-weight:500; color:var(--text-1);
    background:var(--surface-2); border:1px solid var(--border); padding:7px 12px; border-radius:999px;
    transition:all .2s ease;
  }
  .pill:hover{ border-color:var(--border-hi); color:var(--text-0); transform:translateY(-2px); }
  .pill .sw{ width:7px; height:7px; border-radius:50%; }

  /* TIMELINE */
  .timeline-item{ display:grid; grid-template-columns:120px 1fr; gap:20px; padding:18px 0; border-bottom:1px solid var(--border); }
  .timeline-item:last-child{ border-bottom:none; }
  .timeline-item .period{ font-family:var(--mono); font-size:12px; color:var(--text-2); padding-top:2px; }
  .timeline-item h4{ font-size:15.5px; font-weight:600; margin-bottom:4px; }
  .timeline-item .org{ color:var(--blue); font-size:13px; font-weight:500; margin-bottom:8px; }
  .timeline-item ul{ list-style:none; color:var(--text-1); font-size:13.5px; }
  .timeline-item li{ position:relative; padding-left:16px; margin-bottom:5px; }
  .timeline-item li::before{ content:"—"; position:absolute; left:0; color:var(--text-2); }

  /* PROJECTS */
  .proj-grid{ display:grid; grid-template-columns:1fr 1fr; gap:16px; }
  @media (max-width:700px){ .proj-grid{ grid-template-columns:1fr; } }
  .proj-card .top{ display:flex; align-items:center; justify-content:space-between; margin-bottom:12px; }
  .proj-card .top .icon{ width:34px; height:34px; border-radius:9px; background:var(--grad); display:flex; align-items:center; justify-content:center; font-family:var(--mono); font-weight:700; font-size:14px; color:#0a0b0e; }
  .proj-card .arrow{ color:var(--text-2); transition:transform .2s ease, color .2s ease; }
  .proj-card:hover .arrow{ transform:translate(3px,-3px); color:var(--text-0); }
  .proj-card h3{ font-size:16px; font-weight:700; color:var(--text-0); margin-bottom:6px; }
  .proj-card p{ font-size:13.5px; color:var(--text-1); margin-bottom:14px; min-height:40px; }
  .proj-card .tags{ display:flex; flex-wrap:wrap; gap:6px; }
  .proj-card .tags span{ font-family:var(--mono); font-size:10.5px; color:var(--text-2); border:1px solid var(--border); padding:3px 8px; border-radius:6px; }

  /* EDUCATION */
  .edu-card{ display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:16px; }
  .edu-card .left h4{ font-size:16px; font-weight:700; margin-bottom:4px; }
  .edu-card .left p{ color:var(--text-1); font-size:13.5px; }
  .edu-card .score{ text-align:right; }
  .edu-card .score .num{ font-family:var(--mono); font-size:26px; font-weight:700; background:var(--grad); -webkit-background-clip:text; background-clip:text; color:transparent; }
  .edu-card .score .lbl{ font-size:11px; color:var(--text-2); font-family:var(--mono); }

  /* FOOTER */
  footer{ padding:48px 0 64px; border-top:1px solid var(--border); }
  .footer-grid{ display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:20px; }
  .social-row{ display:flex; gap:10px; }
  .social-btn{
    width:40px; height:40px; border-radius:10px; border:1px solid var(--border); display:flex; align-items:center; justify-content:center;
    color:var(--text-1); transition:all .2s ease;
  }
  .social-btn:hover{ border-color:var(--border-hi); color:var(--text-0); transform:translateY(-3px); background:var(--surface-2); }
  footer .sig{ font-family:var(--mono); font-size:12px; color:var(--text-2); }

  @media (prefers-reduced-motion: reduce){
    *{ animation:none !important; transition:none !important; scroll-behavior:auto !important; }
  }
</style>
</head>
<body>
<div class="bg-grid"></div>

<nav>
  <div class="inner">
    <div class="brand"><span class="dot"></span> shubham<span style="color:var(--text-2)">.dev</span></div>
    <div class="nav-links">
      <a href="#stack">Stack</a>
      <a href="#activity">Activity</a>
      <a href="#work">Experience</a>
      <a href="#projects">Projects</a>
      <a href="#contact">Contact</a>
    </div>
    <div class="kbd-hint" id="cmdkTrigger">Search <span style="opacity:.6">⌘K</span></div>
  </div>
</nav>

<div class="cmdk-overlay" id="cmdkOverlay">
  <div class="cmdk-box">
    <input class="cmdk-input" id="cmdkInput" placeholder="Jump to a section or link…" autocomplete="off">
    <div class="cmdk-list" id="cmdkList"></div>
  </div>
</div>

<main class="wrap">

  <section class="hero" style="border-top:none;">
    <div class="terminal">
      <span class="prompt">shubham@portfolio</span>:~$ whoami<span class="caret"></span>
    </div>
    <h1>Building backend systems<br>with <span class="grad">Java &amp; Spring Boot.</span></h1>
    <p class="sub">B.Tech Computer Science undergrad at MIT ADT University, Pune. 350+ LeetCode problems solved, two shipped full-stack projects, and an eye for clean architecture. Currently hunting for a Java internship / fresher role.</p>
    <div class="hero-actions">
      <a class="btn btn-primary" href="https://github.com/SHUBH4M13" target="_blank" rel="noopener">View GitHub →</a>
      <a class="btn btn-ghost" href="https://linkedin.com/in/shubh4mkarna" target="_blank" rel="noopener">LinkedIn</a>
      <a class="btn btn-ghost" href="mailto:skarna230@gmail.com">Email me</a>
    </div>
    <div class="stat-row">
      <div class="stat-cell"><div class="num">350+</div><div class="lbl">LeetCode solved</div></div>
      <div class="stat-cell"><div class="num">8.34</div><div class="lbl">CGPA / 10</div></div>
      <div class="stat-cell"><div class="num">2</div><div class="lbl">Certifications</div></div>
      <div class="stat-cell"><div class="num">3+</div><div class="lbl">Shipped projects</div></div>
    </div>
  </section>

  <section id="activity">
    <div class="section-head">
      <div><span class="eyebrow">Live</span><h2>GitHub activity</h2></div>
      <span class="meta">github.com/SHUBH4M13</span>
    </div>
    <div class="bento">
      <div class="card"><h3>Overview</h3><img src="https://github-readme-stats.vercel.app/api?username=SHUBH4M13&show_icons=true&theme=tokyonight&include_all_commits=true&count_private=true&hide_border=true&bg_color=00000000&title_color=5b8cff&icon_color=9d6bff&text_color=a7adbd" alt="GitHub stats"></div>
      <div class="card"><h3>Top languages</h3><img src="https://github-readme-stats.vercel.app/api/top-langs/?username=SHUBH4M13&layout=compact&langs_count=7&theme=tokyonight&hide_border=true&bg_color=00000000&title_color=5b8cff&text_color=a7adbd" alt="Top languages"></div>
      <div class="card span2"><h3>Contribution streak</h3><img src="https://github-readme-streak-stats.herokuapp.com/?user=SHUBH4M13&theme=tokyonight&hide_border=true&background=00000000&stroke=9d6bff&ring=5b8cff&fire=5b8cff&currStreakLabel=f2f3f6" alt="Streak stats"></div>
    </div>
  </section>

  <section id="stack">
    <div class="section-head">
      <div><span class="eyebrow">Toolbox</span><h2>Languages &amp; technologies</h2></div>
    </div>
    <div class="card">
      <div class="tech-group">
        <h4>Languages</h4>
        <div class="pill-row">
          <span class="pill"><span class="sw" style="background:#5b8cff"></span>Java</span>
          <span class="pill"><span class="sw" style="background:#5b8cff"></span>C++</span>
          <span class="pill"><span class="sw" style="background:#5b8cff"></span>C</span>
          <span class="pill"><span class="sw" style="background:#f7df1e"></span>JavaScript</span>
        </div>
      </div>
      <div class="tech-group">
        <h4>Backend</h4>
        <div class="pill-row">
          <span class="pill"><span class="sw" style="background:#6db33f"></span>Spring Boot</span>
          <span class="pill"><span class="sw" style="background:#339933"></span>Node.js</span>
          <span class="pill"><span class="sw" style="background:#9d6bff"></span>Express.js</span>
        </div>
      </div>
      <div class="tech-group">
        <h4>Frontend</h4>
        <div class="pill-row">
          <span class="pill"><span class="sw" style="background:#61dafb"></span>React</span>
          <span class="pill"><span class="sw" style="background:#06b6d4"></span>Tailwind CSS</span>
          <span class="pill"><span class="sw" style="background:#0055ff"></span>Framer Motion</span>
        </div>
      </div>
      <div class="tech-group">
        <h4>Data &amp; tools</h4>
        <div class="pill-row">
          <span class="pill"><span class="sw" style="background:#47a248"></span>MongoDB</span>
          <span class="pill"><span class="sw" style="background:#4479a1"></span>MySQL</span>
          <span class="pill"><span class="sw" style="background:#f05032"></span>Git</span>
          <span class="pill"><span class="sw" style="background:#2496ed"></span>Docker</span>
          <span class="pill"><span class="sw" style="background:#ff6c37"></span>Postman</span>
          <span class="pill"><span class="sw" style="background:#a7adbd"></span>Vercel</span>
        </div>
      </div>
    </div>
  </section>

  <section id="work">
    <div class="section-head">
      <div><span class="eyebrow">Track record</span><h2>Experience</h2></div>
    </div>
    <div class="card">
      <div class="timeline-item">
        <div class="period">Jun — Jul<br>2025</div>
        <div>
          <h4>React.js Intern</h4>
          <div class="org">LearnCraft Engineering</div>
          <ul>
            <li>Built responsive frontend for Shivmala Infra projects</li>
            <li>Implemented Framer Motion animations and lazy loading for performance</li>
            <li>Deployed on Vercel with a custom domain via BigRock</li>
          </ul>
        </div>
      </div>
    </div>
  </section>

  <section id="projects">
    <div class="section-head">
      <div><span class="eyebrow">Featured</span><h2>Pinned projects</h2></div>
    </div>
    <div class="proj-grid">
      <div class="card proj-card">
        <div class="top"><div class="icon">SS</div><span class="arrow">↗</span></div>
        <h3>ShelfSync</h3>
        <p>A library management system built with Java and Spring Boot, covering cataloguing, lending, and member workflows end to end.</p>
        <div class="tags"><span>Java</span><span>Spring Boot</span><span>MySQL</span></div>
      </div>
      <div class="card proj-card">
        <div class="top"><div class="icon">PX</div><span class="arrow">↗</span></div>
        <h3>PrepX</h3>
        <p>A defence exam preparation platform delivering structured practice content and progress tracking for aspirants.</p>
        <div class="tags"><span>Java</span><span>Spring Boot</span><span>React</span></div>
      </div>
    </div>
  </section>

  <section>
    <div class="section-head">
      <div><span class="eyebrow">Academics</span><h2>Education</h2></div>
    </div>
    <div class="card edu-card">
      <div class="left">
        <h4>B.Tech — Computer Science</h4>
        <p>MIT ADT University, Pune · Aug 2023 – Present</p>
      </div>
      <div class="score"><div class="num">8.34</div><div class="lbl">CGPA / 10.0</div></div>
    </div>
  </section>

  <footer id="contact">
    <div class="footer-grid">
      <div>
        <div class="brand" style="margin-bottom:6px;"><span class="dot"></span> shubham<span style="color:var(--text-2)">.dev</span></div>
        <div class="sig">Keep building. Keep learning. Keep shipping.</div>
      </div>
      <div class="social-row">
        <a class="social-btn" href="https://linkedin.com/in/shubh4mkarna" target="_blank" rel="noopener" title="LinkedIn">in</a>
        <a class="social-btn" href="https://github.com/SHUBH4M13" target="_blank" rel="noopener" title="GitHub">gh</a>
        <a class="social-btn" href="https://leetcode.com/u/Shubh4m13/" target="_blank" rel="noopener" title="LeetCode">lc</a>
        <a class="social-btn" href="mailto:skarna230@gmail.com" title="Email">@</a>
      </div>
    </div>
  </footer>

</main>

<script>
  // Cursor-follow glow on cards
  document.querySelectorAll('.card').forEach(card=>{
    card.addEventListener('mousemove', e=>{
      const r = card.getBoundingClientRect();
      card.style.setProperty('--mx', (e.clientX - r.left) + 'px');
      card.style.setProperty('--my', (e.clientY - r.top) + 'px');
    });
  });

  // Command palette
  const links = [
    {label:'Go to Activity', tag:'section', href:'#activity'},
    {label:'Go to Stack', tag:'section', href:'#stack'},
    {label:'Go to Experience', tag:'section', href:'#work'},
    {label:'Go to Projects', tag:'section', href:'#projects'},
    {label:'Go to Contact', tag:'section', href:'#contact'},
    {label:'Open GitHub profile', tag:'link', href:'https://github.com/SHUBH4M13'},
    {label:'Open LinkedIn', tag:'link', href:'https://linkedin.com/in/shubh4mkarna'},
    {label:'Open LeetCode', tag:'link', href:'https://leetcode.com/u/Shubh4m13/'},
    {label:'Send email', tag:'mailto', href:'mailto:skarna230@gmail.com'},
  ];
  const overlay = document.getElementById('cmdkOverlay');
  const input = document.getElementById('cmdkInput');
  const list = document.getElementById('cmdkList');

  function renderList(filter=''){
    list.innerHTML = '';
    links.filter(l=>l.label.toLowerCase().includes(filter.toLowerCase())).forEach((l,i)=>{
      const item = document.createElement('div');
      item.className = 'cmdk-item' + (i===0 ? ' active':'');
      item.innerHTML = `<span>${l.label}</span><span class="tag">${l.tag}</span>`;
      item.onclick = ()=>{ navigate(l.href); };
      list.appendChild(item);
    });
  }
  function navigate(href){
    closeCmdk();
    if(href.startsWith('#')){ document.querySelector(href)?.scrollIntoView({behavior:'smooth'}); }
    else { window.open(href, '_blank'); }
  }
  function openCmdk(){ overlay.classList.add('open'); input.value=''; renderList(); setTimeout(()=>input.focus(),10); }
  function closeCmdk(){ overlay.classList.remove('open'); }

  document.getElementById('cmdkTrigger').addEventListener('click', openCmdk);
  overlay.addEventListener('click', e=>{ if(e.target === overlay) closeCmdk(); });
  input.addEventListener('input', ()=> renderList(input.value));
  document.addEventListener('keydown', e=>{
    if((e.metaKey || e.ctrlKey) && e.key.toLowerCase()==='k'){ e.preventDefault(); overlay.classList.contains('open') ? closeCmdk() : openCmdk(); }
    if(e.key === 'Escape') closeCmdk();
  });
</script>
</body>
</html>
