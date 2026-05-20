<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>AGS Creative Studio</title>
  <link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;0,900;1,400;1,700&family=DM+Sans:wght@300;400;500&display=swap" rel="stylesheet"/>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg: #0a0a08;
      --cream: #f0ead6;
      --gold: #c9a84c;
      --gold-light: #e8c96e;
      --muted: #4a4a42;
      --white: #ffffff;
    }

    html { scroll-behavior: smooth; }

    body {
      background: var(--bg);
      color: var(--cream);
      font-family: 'DM Sans', sans-serif;
      font-weight: 300;
      overflow-x: hidden;
      cursor: none;
    }

    /* Custom cursor */
    .cursor {
      position: fixed;
      width: 12px; height: 12px;
      background: var(--gold);
      border-radius: 50%;
      pointer-events: none;
      z-index: 9999;
      transform: translate(-50%, -50%);
      transition: transform 0.1s, width 0.3s, height 0.3s, background 0.3s;
      mix-blend-mode: difference;
    }
    .cursor-ring {
      position: fixed;
      width: 36px; height: 36px;
      border: 1px solid var(--gold);
      border-radius: 50%;
      pointer-events: none;
      z-index: 9998;
      transform: translate(-50%, -50%);
      transition: transform 0.18s ease, width 0.3s, height 0.3s, opacity 0.3s;
      opacity: 0.6;
    }

    /* Noise texture overlay */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
      pointer-events: none;
      z-index: 9000;
      opacity: 0.35;
    }

    /* NAV */
    nav {
      position: fixed;
      top: 0; left: 0; right: 0;
      z-index: 100;
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 28px 60px;
      mix-blend-mode: normal;
    }
    .nav-logo {
      font-family: 'Playfair Display', serif;
      font-size: 1.2rem;
      font-weight: 700;
      letter-spacing: 0.15em;
      color: var(--cream);
      text-transform: uppercase;
    }
    .nav-logo span { color: var(--gold); }
    .nav-links { display: flex; gap: 36px; list-style: none; }
    .nav-links a {
      color: var(--cream);
      text-decoration: none;
      font-size: 0.78rem;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      opacity: 0.7;
      transition: opacity 0.3s, color 0.3s;
    }
    .nav-links a:hover { opacity: 1; color: var(--gold); }

    /* HERO */
    .hero {
      min-height: 100vh;
      display: grid;
      grid-template-columns: 1fr 1fr;
      align-items: center;
      padding: 120px 60px 80px;
      position: relative;
      overflow: hidden;
    }
    .hero-bg-text {
      position: absolute;
      right: -30px;
      top: 50%;
      transform: translateY(-50%) rotate(90deg);
      font-family: 'Playfair Display', serif;
      font-size: clamp(80px, 14vw, 200px);
      font-weight: 900;
      color: transparent;
      -webkit-text-stroke: 1px rgba(201,168,76,0.08);
      white-space: nowrap;
      pointer-events: none;
      user-select: none;
    }
    .hero-left { position: relative; z-index: 2; }
    .hero-eyebrow {
      font-size: 0.72rem;
      letter-spacing: 0.3em;
      text-transform: uppercase;
      color: var(--gold);
      margin-bottom: 28px;
      opacity: 0;
      animation: fadeUp 0.8s 0.2s forwards;
    }
    .hero-title {
      font-family: 'Playfair Display', serif;
      font-size: clamp(52px, 6vw, 90px);
      font-weight: 900;
      line-height: 1.0;
      margin-bottom: 32px;
      opacity: 0;
      animation: fadeUp 0.8s 0.4s forwards;
    }
    .hero-title em {
      font-style: italic;
      color: var(--gold);
    }
    .hero-desc {
      font-size: 1rem;
      line-height: 1.8;
      max-width: 420px;
      color: rgba(240,234,214,0.65);
      margin-bottom: 48px;
      opacity: 0;
      animation: fadeUp 0.8s 0.6s forwards;
    }
    .btn-group { display: flex; gap: 18px; opacity: 0; animation: fadeUp 0.8s 0.8s forwards; }
    .btn-primary {
      background: var(--gold);
      color: var(--bg);
      padding: 14px 36px;
      font-size: 0.8rem;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      text-decoration: none;
      font-weight: 500;
      transition: background 0.3s, transform 0.2s;
    }
    .btn-primary:hover { background: var(--gold-light); transform: translateY(-2px); }
    .btn-outline {
      border: 1px solid rgba(240,234,214,0.3);
      color: var(--cream);
      padding: 14px 36px;
      font-size: 0.8rem;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      text-decoration: none;
      font-weight: 400;
      transition: border-color 0.3s, transform 0.2s;
    }
    .btn-outline:hover { border-color: var(--gold); color: var(--gold); transform: translateY(-2px); }

    .hero-right {
      position: relative;
      z-index: 2;
      display: flex;
      justify-content: center;
      align-items: center;
      opacity: 0;
      animation: fadeIn 1.2s 0.5s forwards;
    }
    .hero-visual {
      width: 420px;
      height: 520px;
      position: relative;
    }
    .hero-card {
      position: absolute;
      border: 1px solid rgba(201,168,76,0.25);
      background: rgba(201,168,76,0.04);
      backdrop-filter: blur(4px);
    }
    .hero-card:nth-child(1) {
      width: 300px; height: 380px;
      top: 0; left: 50%;
      transform: translateX(-50%);
    }
    .hero-card:nth-child(2) {
      width: 180px; height: 220px;
      bottom: 0; left: 0;
      border-color: rgba(240,234,214,0.1);
    }
    .hero-card:nth-child(3) {
      width: 120px; height: 120px;
      bottom: 60px; right: 0;
      border-color: rgba(201,168,76,0.4);
      display: flex; align-items: center; justify-content: center;
    }
    .hero-card-label {
      position: absolute;
      bottom: 18px; left: 18px;
      font-size: 0.65rem;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      color: var(--gold);
      opacity: 0.7;
    }
    .circle-logo {
      width: 70px; height: 70px;
      border-radius: 50%;
      border: 1px solid var(--gold);
      display: flex; align-items: center; justify-content: center;
      font-family: 'Playfair Display', serif;
      font-size: 1.3rem;
      font-weight: 900;
      color: var(--gold);
    }
    .floating-tag {
      position: absolute;
      top: 30px; right: -20px;
      background: var(--gold);
      color: var(--bg);
      font-size: 0.65rem;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      padding: 8px 16px;
      font-weight: 500;
    }

    /* MARQUEE */
    .marquee-wrap {
      background: var(--gold);
      overflow: hidden;
      padding: 14px 0;
      white-space: nowrap;
    }
    .marquee-track {
      display: inline-flex;
      animation: marquee 18s linear infinite;
    }
    .marquee-item {
      font-family: 'Playfair Display', serif;
      font-size: 0.85rem;
      font-style: italic;
      color: var(--bg);
      letter-spacing: 0.08em;
      margin-right: 60px;
    }
    .marquee-dot { margin-right: 60px; color: var(--bg); opacity: 0.4; }

    /* SERVICES */
    #services {
      padding: 120px 60px;
      position: relative;
    }
    .section-header {
      display: flex;
      justify-content: space-between;
      align-items: flex-end;
      margin-bottom: 80px;
    }
    .section-label {
      font-size: 0.7rem;
      letter-spacing: 0.3em;
      text-transform: uppercase;
      color: var(--gold);
      margin-bottom: 16px;
    }
    .section-title {
      font-family: 'Playfair Display', serif;
      font-size: clamp(36px, 4vw, 58px);
      font-weight: 900;
      line-height: 1.1;
    }
    .section-title em { font-style: italic; color: var(--gold); }
    .section-sub {
      max-width: 300px;
      font-size: 0.88rem;
      line-height: 1.7;
      color: rgba(240,234,214,0.5);
      text-align: right;
    }

    .services-grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 2px;
    }
    .service-item {
      padding: 52px 40px;
      border: 1px solid rgba(201,168,76,0.1);
      position: relative;
      overflow: hidden;
      transition: background 0.4s;
      cursor: none;
    }
    .service-item::before {
      content: '';
      position: absolute;
      inset: 0;
      background: linear-gradient(135deg, rgba(201,168,76,0.06) 0%, transparent 60%);
      opacity: 0;
      transition: opacity 0.4s;
    }
    .service-item:hover::before { opacity: 1; }
    .service-item:hover { background: rgba(201,168,76,0.04); }
    .service-num {
      font-family: 'Playfair Display', serif;
      font-size: 3rem;
      font-weight: 900;
      color: transparent;
      -webkit-text-stroke: 1px rgba(201,168,76,0.2);
      line-height: 1;
      margin-bottom: 24px;
      transition: -webkit-text-stroke-color 0.4s;
    }
    .service-item:hover .service-num { -webkit-text-stroke-color: rgba(201,168,76,0.5); }
    .service-name {
      font-family: 'Playfair Display', serif;
      font-size: 1.5rem;
      font-weight: 700;
      margin-bottom: 16px;
      color: var(--cream);
    }
    .service-desc {
      font-size: 0.85rem;
      line-height: 1.8;
      color: rgba(240,234,214,0.5);
    }
    .service-arrow {
      position: absolute;
      bottom: 28px; right: 28px;
      font-size: 1.2rem;
      color: var(--gold);
      opacity: 0;
      transform: translate(-8px, 8px);
      transition: opacity 0.3s, transform 0.3s;
    }
    .service-item:hover .service-arrow { opacity: 1; transform: translate(0, 0); }

    /* ABOUT */
    #about {
      padding: 120px 60px;
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 100px;
      align-items: center;
      background: rgba(201,168,76,0.03);
      border-top: 1px solid rgba(201,168,76,0.1);
      border-bottom: 1px solid rgba(201,168,76,0.1);
    }
    .about-visual {
      position: relative;
    }
    .about-box-main {
      width: 100%;
      height: 460px;
      border: 1px solid rgba(201,168,76,0.2);
      background: linear-gradient(160deg, rgba(201,168,76,0.06) 0%, transparent 70%);
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .about-ags-big {
      font-family: 'Playfair Display', serif;
      font-size: 8rem;
      font-weight: 900;
      color: transparent;
      -webkit-text-stroke: 1px rgba(201,168,76,0.15);
      font-style: italic;
    }
    .about-badge {
      position: absolute;
      bottom: -24px;
      right: -24px;
      background: var(--gold);
      color: var(--bg);
      padding: 28px;
      text-align: center;
    }
    .about-badge-num {
      font-family: 'Playfair Display', serif;
      font-size: 2.4rem;
      font-weight: 900;
      line-height: 1;
    }
    .about-badge-txt {
      font-size: 0.65rem;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      margin-top: 4px;
    }
    .about-content {}
    .about-content .section-label { margin-bottom: 16px; }
    .about-title {
      font-family: 'Playfair Display', serif;
      font-size: clamp(32px, 3.5vw, 52px);
      font-weight: 900;
      line-height: 1.1;
      margin-bottom: 28px;
    }
    .about-title em { font-style: italic; color: var(--gold); }
    .about-text {
      font-size: 0.95rem;
      line-height: 1.9;
      color: rgba(240,234,214,0.6);
      margin-bottom: 20px;
    }
    .about-values {
      display: flex;
      flex-direction: column;
      gap: 12px;
      margin-top: 36px;
    }
    .about-value {
      display: flex;
      align-items: center;
      gap: 14px;
      font-size: 0.85rem;
      color: rgba(240,234,214,0.7);
    }
    .about-value::before {
      content: '';
      width: 32px; height: 1px;
      background: var(--gold);
      flex-shrink: 0;
    }

    /* CONTACT */
    #contact {
      padding: 120px 60px;
      text-align: center;
      position: relative;
      overflow: hidden;
    }
    #contact::before {
      content: 'AGS';
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-family: 'Playfair Display', serif;
      font-size: clamp(120px, 20vw, 300px);
      font-weight: 900;
      font-style: italic;
      color: transparent;
      -webkit-text-stroke: 1px rgba(201,168,76,0.04);
      pointer-events: none;
      white-space: nowrap;
    }
    .contact-label { font-size: 0.7rem; letter-spacing: 0.3em; text-transform: uppercase; color: var(--gold); margin-bottom: 24px; }
    .contact-title {
      font-family: 'Playfair Display', serif;
      font-size: clamp(42px, 6vw, 82px);
      font-weight: 900;
      line-height: 1.05;
      margin-bottom: 24px;
      position: relative;
    }
    .contact-title em { font-style: italic; color: var(--gold); }
    .contact-sub {
      font-size: 0.95rem;
      color: rgba(240,234,214,0.5);
      margin-bottom: 52px;
      max-width: 500px;
      margin-left: auto; margin-right: auto;
      line-height: 1.7;
    }
    .contact-cta { display: flex; justify-content: center; gap: 18px; flex-wrap: wrap; position: relative; }
    .contact-info {
      margin-top: 64px;
      display: flex;
      justify-content: center;
      gap: 60px;
      color: rgba(240,234,214,0.4);
      font-size: 0.8rem;
      letter-spacing: 0.1em;
      position: relative;
    }
    .contact-info a { color: inherit; text-decoration: none; transition: color 0.3s; }
    .contact-info a:hover { color: var(--gold); }

    /* FOOTER */
    footer {
      padding: 36px 60px;
      border-top: 1px solid rgba(201,168,76,0.1);
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 0.75rem;
      color: rgba(240,234,214,0.3);
      letter-spacing: 0.08em;
    }
    .footer-logo {
      font-family: 'Playfair Display', serif;
      font-weight: 700;
      color: rgba(240,234,214,0.5);
    }
    .footer-logo span { color: var(--gold); }

    /* DIVIDER LINE */
    .gold-line {
      width: 60px; height: 1px;
      background: var(--gold);
      margin: 24px 0;
    }

    /* ANIMATIONS */
    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(30px); }
      to { opacity: 1; transform: translateY(0); }
    }
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
    @keyframes marquee {
      from { transform: translateX(0); }
      to { transform: translateX(-50%); }
    }
    @keyframes float {
      0%, 100% { transform: translateY(0px); }
      50% { transform: translateY(-12px); }
    }
    .floating-tag { animation: float 4s ease-in-out infinite; }

    /* Scroll reveal */
    .reveal {
      opacity: 0;
      transform: translateY(40px);
      transition: opacity 0.8s ease, transform 0.8s ease;
    }
    .reveal.visible {
      opacity: 1;
      transform: translateY(0);
    }

    @media (max-width: 900px) {
      nav { padding: 24px 28px; }
      .nav-links { display: none; }
      .hero { grid-template-columns: 1fr; padding: 100px 28px 60px; }
      .hero-right { display: none; }
      .hero-bg-text { display: none; }
      #services { padding: 80px 28px; }
      .services-grid { grid-template-columns: 1fr; }
      .section-header { flex-direction: column; align-items: flex-start; gap: 16px; }
      .section-sub { text-align: left; }
      #about { grid-template-columns: 1fr; padding: 80px 28px; gap: 60px; }
      #contact { padding: 80px 28px; }
      footer { padding: 28px; flex-direction: column; gap: 12px; text-align: center; }
    }
  </style>
</head>
<body>

  <div class="cursor" id="cursor"></div>
  <div class="cursor-ring" id="cursorRing"></div>

  <!-- NAV -->
  <nav>
    <div class="nav-logo">AGS <span>·</span> Creative</div>
    <ul class="nav-links">
      <li><a href="#services">Services</a></li>
      <li><a href="#about">About</a></li>
      <li><a href="#contact">Contact</a></li>
    </ul>
  </nav>

  <!-- HERO -->
  <section class="hero">
    <div class="hero-bg-text">Creative</div>
    <div class="hero-left">
      <p class="hero-eyebrow">AGS Creative Studio</p>
      <h1 class="hero-title">Where Vision<br>Becomes <em>Art</em></h1>
      <p class="hero-desc">We craft visual identities, powerful imagery, and unforgettable designs that make your brand impossible to ignore.</p>
      <div class="btn-group">
        <a href="#services" class="btn-primary">Our Services</a>
        <a href="#contact" class="btn-outline">Get In Touch</a>
      </div>
    </div>
    <div class="hero-right">
      <div class="hero-visual">
        <div class="hero-card">
          <div class="hero-card-label">Photography</div>
        </div>
        <div class="hero-card">
          <div class="hero-card-label">Design</div>
        </div>
        <div class="hero-card">
          <div class="circle-logo">A</div>
        </div>
        <div class="floating-tag">Est. Studio</div>
      </div>
    </div>
  </section>

  <!-- MARQUEE -->
  <div class="marquee-wrap">
    <div class="marquee-track">
      <span class="marquee-item">Photo Editing</span><span class="marquee-dot">✦</span>
      <span class="marquee-item">Recording</span><span class="marquee-dot">✦</span>
      <span class="marquee-item">Photo Shoots</span><span class="marquee-dot">✦</span>
      <span class="marquee-item">Poster Design</span><span class="marquee-dot">✦</span>
      <span class="marquee-item">Logo Design</span><span class="marquee-dot">✦</span>
      <span class="marquee-item">Creative Direction</span><span class="marquee-dot">✦</span>
      <span class="marquee-item">Photo Editing</span><span class="marquee-dot">✦</span>
      <span class="marquee-item">Recording</span><span class="marquee-dot">✦</span>
      <span class="marquee-item">Photo Shoots</span><span class="marquee-dot">✦</span>
      <span class="marquee-item">Poster Design</span><span class="marquee-dot">✦</span>
      <span class="marquee-item">Logo Design</span><span class="marquee-dot">✦</span>
      <span class="marquee-item">Creative Direction</span><span class="marquee-dot">✦</span>
    </div>
  </div>

  <!-- SERVICES -->
  <section id="services">
    <div class="section-header reveal">
      <div>
        <p class="section-label">What We Do</p>
        <h2 class="section-title">Our <em>Services</em></h2>
      </div>
      <p class="section-sub">Every service crafted with passion, precision, and a relentless eye for detail.</p>
    </div>

    <div class="services-grid">
      <div class="service-item reveal">
        <div class="service-num">01</div>
        <h3 class="service-name">Photo Editing</h3>
        <p class="service-desc">Professional retouching, color grading, and enhancement that transforms raw shots into polished masterpieces.</p>
        <div class="service-arrow">↗</div>
      </div>
      <div class="service-item reveal" style="transition-delay:0.1s">
        <div class="service-num">02</div>
        <h3 class="service-name">Recording</h3>
        <p class="service-desc">High-quality audio recording sessions with professional equipment and expert sound engineering.</p>
        <div class="service-arrow">↗</div>
      </div>
      <div class="service-item reveal" style="transition-delay:0.2s">
        <div class="service-num">03</div>
        <h3 class="service-name">Photo Shoot</h3>
        <p class="service-desc">Styled, directed photography sessions — portraits, products, events — captured with artistic intent.</p>
        <div class="service-arrow">↗</div>
      </div>
      <div class="service-item reveal" style="transition-de