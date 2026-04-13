<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>VAULT | Premium Digital Agency</title>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Mono:wght@300;400;600&display=swap');

        /* ==========================================
           1. CSS VARIABLES (DARK MODE DEFAULT)
           ========================================== */
        :root {
            --bg-color: #1a0a02; 
            --bg-gradient: radial-gradient(circle at 50% 50%, #3b1b06 0%, #0a0401 100%);
            --text-main: #f4eadd; 
            --text-muted: #a68a6b; 
            --accent: #d48a42; 
            --layer-bg: rgba(255, 255, 255, 0.03);
            
            /* Glassmorphism Variables */
            --glass-bg: rgba(20, 10, 5, 0.4);
            --glass-border: rgba(255, 255, 255, 0.08);
            --glass-blur: blur(12px);
            
            --card-bg: rgba(17, 17, 17, 0.6);
            --card-text: #fff;
            --card-shadow: rgba(0,0,0,0.5);
            --nav-dot: rgba(255,255,255,0.1);
        }

        /* LIGHT MODE VARIABLES */
        [data-theme="light"] {
            --bg-color: #f4eadd; 
            --bg-gradient: radial-gradient(circle at 50% 50%, #ffffff 0%, #e8dccb 100%);
            --text-main: #2b1404; 
            --text-muted: #735942; 
            --accent: #c06c1a; 
            --layer-bg: rgba(0, 0, 0, 0.04);
            
            /* Glassmorphism Variables (Light) */
            --glass-bg: rgba(255, 255, 255, 0.4);
            --glass-border: rgba(0, 0, 0, 0.08);
            
            --card-bg: rgba(255, 255, 255, 0.4);
            --card-text: #1a0a02;
            --card-shadow: rgba(0,0,0,0.1);
            --nav-dot: rgba(0,0,0,0.15);
        }

        body, html {
            margin: 0; padding: 0;
            background-color: var(--bg-color);
            background-image: var(--bg-gradient);
            color: var(--text-main);
            font-family: 'DM Mono', monospace;
            overflow-x: hidden;
            transition: background 0.5s ease, color 0.5s ease;
        }

        /* ==========================================
           0. SMOOTH GLASS LOADER SCREEN
           ========================================== */
        #loader {
            position: fixed; inset: 0; 
            background: rgba(10, 4, 1, 0.5); /* Semi-transparent dark base */
            backdrop-filter: blur(40px); /* Massive blur */
            -webkit-backdrop-filter: blur(40px);
            z-index: 9999;
            display: flex; align-items: center; justify-content: center;
            transition: opacity 1s cubic-bezier(0.4, 0, 0.2, 1), backdrop-filter 1s ease;
        }
        
        .loader-content { width: 300px; text-align: center; }
        .loader-logo { font-family: 'Bebas Neue', sans-serif; font-size: 4rem; letter-spacing: 12px; color: #fff; margin-bottom: 20px; text-shadow: 0 0 20px rgba(212, 138, 66, 0.5); }
        .loader-bar-container { width: 100%; height: 2px; background: rgba(255,255,255,0.1); position: relative; margin-bottom: 15px; overflow: hidden; border-radius: 2px;}
        #loader-bar { height: 100%; width: 0%; background: #d48a42; transition: width 0.1s linear; box-shadow: 0 0 10px #d48a42;}
        #loader-text { font-family: 'DM Mono', monospace; font-size: 0.7rem; color: #fff; letter-spacing: 3px; text-transform: uppercase; opacity: 0.7;}

        /* ==========================================
           2. TOP NAVIGATION BAR (GLASS)
           ========================================== */
        #top-bar {
            position: fixed; top: 0; left: 0; width: 100%;
            display: flex; justify-content: space-between; align-items: center;
            padding: 20px 40px; box-sizing: border-box;
            z-index: 1000; 
            background: var(--glass-bg);
            backdrop-filter: var(--glass-blur);
            -webkit-backdrop-filter: var(--glass-blur);
            border-bottom: 1px solid var(--glass-border);
            transition: background 0.5s ease;
        }

        .logo { font-family: 'Bebas Neue', sans-serif; font-size: 2.5rem; letter-spacing: 4px; color: var(--text-main); }
        .nav-links { display: flex; gap: 30px; align-items: center; }
        .nav-links a { color: var(--text-main); text-decoration: none; font-size: 0.85rem; text-transform: uppercase; letter-spacing: 2px; transition: color 0.3s; opacity: 0.8;}
        .nav-links a:hover { color: var(--accent); opacity: 1;}

        /* GLASS THEME TOGGLE BUTTON */
        #theme-toggle {
            background: var(--glass-bg); 
            border: 1px solid var(--glass-border); 
            color: var(--text-main);
            padding: 8px 16px; border-radius: 20px; font-family: 'DM Mono', monospace;
            font-size: 0.75rem; text-transform: uppercase; letter-spacing: 1px; cursor: pointer;
            backdrop-filter: var(--glass-blur); -webkit-backdrop-filter: var(--glass-blur);
            transition: all 0.3s ease;
        }
        #theme-toggle:hover { border-color: var(--accent); color: var(--accent); background: rgba(255,255,255,0.05);}

        /* ==========================================
           3. 3D TYPOGRAPHY SCENE
           ========================================== */
        h2 { font-family: 'Bebas Neue', sans-serif; font-size: 8rem; line-height: 0.85; margin: 0 0 1.5rem 0; letter-spacing: 2px; }
        p { margin: 0; }
        .layer-tag { color: var(--text-muted); font-size: 0.85rem; margin-bottom: 1.5rem; text-transform: uppercase; letter-spacing: 2px; }
        .layer-line { color: var(--text-muted); font-size: 1rem; line-height: 1.6; }

        #fixed-ui { position: fixed; inset: 0; width: 100vw; height: 100vh; z-index: 10; will-change: transform; pointer-events: none; }
        
        #hud { position: absolute; top: 100px; right: 40px; text-align: right; font-size: 0.75rem; color: var(--text-muted); }
        .hud-title { display: inline-block; margin-bottom: 10px; letter-spacing: 2px; line-height: 1.4; }
        .progress-bar { display: inline-block; width: 2px; height: 50px; background: var(--glass-border); margin-left: 20px; vertical-align: top; }
        .progress-fill { width: 100%; height: 0%; background: var(--accent); transition: height 0.1s linear; }
        .hud-label { display: block; margin-top: 10px; letter-spacing: 2px; }

        /* GLASS NAV DOTS */
        #panel_nav { position: absolute; left: 40px; top: 50%; transform: translateY(-50%); display: flex; flex-direction: column; gap: 15px; pointer-events: auto; }
        .nav-dot { 
            width: 10px; height: 10px; border-radius: 50%; 
            background: var(--glass-bg); border: 1px solid var(--glass-border); 
            cursor: pointer; padding: 0; transition: all 0.3s ease;
            backdrop-filter: var(--glass-blur); -webkit-backdrop-filter: var(--glass-blur);
        }
        .nav-dot.is-active { background: var(--accent); border-color: var(--accent); transform: scale(1.3); box-shadow: 0 0 10px var(--accent);}

        .viewport { position: absolute; inset: 0; perspective: 1200px; overflow: hidden; pointer-events: none;}
        .world { position: absolute; top: 50%; left: 50%; transform-style: preserve-3d; will-change: transform; }
        .item { position: absolute; left: -50vw; top: -50vh; width: 100vw; height: 100vh; display: flex; align-items: center; justify-content: center; transform-origin: center center; opacity: var(--item-opacity, 1); transition: opacity 0.1s; }
        .panel-inner { width: 100%; padding: 0 10%; position: relative; z-index: 10; box-sizing: border-box;}
        
        .is-center { text-align: center; } .is-right { text-align: right; }
        .align-center { margin: 0 auto; } .align-right { margin-left: auto; }
        .panel-copy { max-width: 650px; }
        .layer-bg { position: absolute; font-family: 'Bebas Neue', sans-serif; font-size: 28vw; color: var(--layer-bg); z-index: -1; left: 5%; top: 50%; transform: translateY(-50%); white-space: nowrap; transition: color 0.5s ease; }
        .scroll-hint { margin-top: 3rem; color: var(--text-muted); font-size: 0.8rem; letter-spacing: 2px; text-transform: uppercase; }

        #proxy { width: 100%; position: relative; z-index: -1; }
        
        /* ==========================================
           4. FINAL NORMAL PAGE (Services, Contact, Footer)
           ========================================== */
        #final-page { position: relative; width: 100%; min-height: 100vh; z-index: 1; padding-bottom: 50px; }
        #shader-bg { position: fixed; inset: 0; z-index: -1; opacity: 0; transition: opacity 0.5s ease; pointer-events: none; }

        .final-content { position: relative; z-index: 2; padding: 120px 10% 50px 10%; max-width: 1300px; margin: 0 auto; pointer-events: auto; }
        .final-content h1 { font-family: 'Bebas Neue', sans-serif; font-size: 5rem; margin-bottom: 3rem; text-align: center;}

        /* FROSTED SERVICE CARDS */
        .cards { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 2rem; place-items: center; margin-bottom: 6rem; }

        .previewCard {
            --gutter: 1.5rem; --brightness: 0.85; --saturation: 1.5; --frostRadius: 1.5rem;
            padding: var(--gutter); border-radius: var(--gutter); aspect-ratio: 4 / 3; width: 100%; max-width: 32rem;
            display: grid; justify-content: start; align-content: end; position: relative;
            background: var(--card-bg); overflow: hidden; cursor: pointer;
            transition: transform 250ms ease-in-out, background 0.5s ease, box-shadow 0.5s ease;
            box-shadow: 0 10px 30px var(--card-shadow); border: 1px solid var(--glass-border);
        }
        
        .previewCard img.backdrop { position: absolute; inset: 0; object-fit: cover; pointer-events: none; user-select: none; width: 100%; height: 100%; transition: transform 500ms ease-in-out; opacity: 0.8; }
        .previewCard svg, .previewCard .icon-plus { transition: transform 250ms ease-in-out; }
        .previewCard:hover { transform: translateY(-5px); border-color: var(--accent);}
        .previewCard:hover img.backdrop { transform: scale(1.1); }
        .previewCard:hover svg, .previewCard:hover .icon-plus { transform: scale(1.5) rotate(90deg); color: var(--accent); }
        
        .previewCard::after {
            content: ""; position: absolute; inset: -0.5rem; z-index: 1; pointer-events: none;
            backdrop-filter: blur(var(--frostRadius)) saturate(var(--saturation)) brightness(var(--brightness));
            -webkit-backdrop-filter: blur(var(--frostRadius)) saturate(var(--saturation)) brightness(var(--brightness));
            background: linear-gradient(to bottom, rgba(255, 255, 255, 0) 0%, rgba(255, 255, 255, 0.1) 100%);
            mask-image: linear-gradient(to bottom, transparent 0%, rgba(0, 0, 0, 0.05) 40%, rgba(0, 0, 0, 0.6) 55%, black 85%);
            -webkit-mask-image: linear-gradient(to bottom, transparent 0%, rgba(0, 0, 0, 0.05) 40%, rgba(0, 0, 0, 0.6) 55%, black 85%);
        }

        .previewCard .content { position: relative; z-index: 2; display: grid; bottom: 0; justify-content: start; align-content: end; gap: 0.5rem; color: var(--card-text); transition: color 0.5s ease; }
        .previewCard .title { font-size: 1.4rem; margin: 0; text-shadow: 0 2px 4px rgba(0,0,0,0.5); }
        [data-theme="light"] .previewCard .title { text-shadow: none; font-weight: bold; }
        .previewCard .category { text-transform: uppercase; font-weight: 600; letter-spacing: 0.1rem; opacity: 0.8; font-size: 0.75rem; color: var(--accent);}
        .previewCard .description { font-size: 0.9rem; line-height: 1.35; display: grid; grid-template-columns: 1fr auto; place-content: start; gap: 0.5rem; opacity: 0.9; }
        .previewCard .description p { margin: 0; padding-right: 10px; }
        .previewCard .description .icon-plus { transform: translateY(-0.1rem); font-size: 1.5rem; display: grid; place-items: center; width: 2rem; height: 2rem; font-weight: 300; }

        /* ==========================================
           GLASS CONTACT FORM SECTION
           ========================================== */
        .contact-container {
            max-width: 600px; margin: 0 auto; padding: 3rem;
            background: var(--glass-bg); border: 1px solid var(--glass-border);
            border-radius: 1.5rem; box-shadow: 0 10px 40px var(--card-shadow);
            backdrop-filter: var(--glass-blur); -webkit-backdrop-filter: var(--glass-blur);
        }

        .contact-container h3 { font-family: 'Bebas Neue', sans-serif; font-size: 2.5rem; margin-top: 0; margin-bottom: 0.5rem; color: var(--card-text); letter-spacing: 2px;}
        .contact-container p { color: var(--text-main); font-size: 0.9rem; margin-bottom: 2rem; line-height: 1.5; opacity: 0.8;}

        .contact-form { display: flex; flex-direction: column; gap: 1.5rem; }
        
        /* Frosted Inputs */
        .contact-form input, .contact-form textarea, .contact-form select {
            width: 100%; box-sizing: border-box; padding: 15px 20px;
            background: var(--glass-bg); border: 1px solid var(--glass-border);
            color: var(--card-text); font-family: 'DM Mono', monospace; font-size: 0.9rem;
            border-radius: 8px; outline: none; transition: border-color 0.3s ease, background 0.3s ease;
            backdrop-filter: blur(8px); -webkit-backdrop-filter: blur(8px);
        }
        
        .contact-form select option { background: var(--bg-color); color: var(--text-main); }
        
        .contact-form input:focus, .contact-form textarea:focus, .contact-form select:focus { 
            border-color: var(--accent); background: rgba(255,255,255,0.1);
        }
        .contact-form textarea { resize: vertical; min-height: 120px; }
        
        /* Frosted Submit Button */
        .contact-form button {
            background: rgba(212, 138, 66, 0.2); border: 1px solid var(--accent);
            color: var(--card-text); padding: 15px;
            font-family: 'DM Mono', monospace; font-size: 1rem; font-weight: bold;
            text-transform: uppercase; letter-spacing: 2px; border-radius: 8px;
            cursor: pointer; transition: all 0.3s ease;
            backdrop-filter: blur(10px); -webkit-backdrop-filter: blur(10px);
        }
        .contact-form button:hover { background: var(--accent); color: #fff; transform: translateY(-2px); box-shadow: 0 5px 15px rgba(212, 138, 66, 0.4);}

        /* FOOTER SECTION */
        .site-footer {
            border-top: 1px solid var(--glass-border); margin-top: 6rem; padding-top: 3rem;
            display: flex; flex-direction: column; align-items: center; gap: 2rem;
        }

        .footer-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); width: 100%; gap: 2rem; text-align: left; }
        .footer-col h4 { font-family: 'Bebas Neue', sans-serif; font-size: 1.5rem; letter-spacing: 2px; color: var(--card-text); margin-bottom: 1rem; margin-top: 0;}
        .footer-col ul { list-style: none; padding: 0; margin: 0; display: flex; flex-direction: column; gap: 0.8rem; }
        .footer-col ul li a { color: var(--text-muted); text-decoration: none; font-size: 0.85rem; transition: color 0.3s ease; }
        .footer-col ul li a:hover { color: var(--accent); }

        .footer-bottom {
            width: 100%; border-top: 1px solid var(--glass-border); padding-top: 2rem;
            display: flex; justify-content: space-between; align-items: center;
            font-size: 0.75rem; color: var(--text-muted); flex-wrap: wrap; gap: 1rem;
        }

    </style>
</head>
<body>

    <div id="loader">
        <div class="loader-content">
            <div class="loader-logo">VAULT</div>
            <div class="loader-bar-container"><div id="loader-bar"></div></div>
            <div id="loader-text">INITIALIZING SECURE PROTOCOLS... <span id="loader-percent">0%</span></div>
        </div>
    </div>

    <header id="top-bar">
        <div class="logo">VAULT</div>
        <div class="nav-links">
            <a href="#" onclick="window.scrollTo({top: 0, behavior: 'smooth'}); return false;">Foundation</a>
            <a href="#" onclick="window.scrollTo({top: 5000, behavior: 'smooth'}); return false;">Identity</a>
            <a href="#services-section" onclick="window.scrollTo({top: document.body.scrollHeight - 1600, behavior: 'smooth'}); return false;">Services</a>
            <a href="#contact-section" onclick="window.scrollTo({top: document.body.scrollHeight, behavior: 'smooth'}); return false;">Contact</a>
            <button id="theme-toggle">☀️ Light</button>
        </div>
    </header>

    <div id="fixed-ui">
        <div id="hud">
            <span class="hud-title">DIGITAL<br>ASSET<br>MANAGEMENT</span>
            <div class="progress-bar"><div id="progress_fill" class="progress-fill"></div></div>
            <span id="hud_label" class="hud-label">FOUNDATION</span>
        </div>

        <nav id="panel_nav" aria-label="Panels">
            <button class="nav-dot is-active" data-index="0" title="Foundation"></button>
            <button class="nav-dot" data-index="1" title="Security"></button>
            <button class="nav-dot" data-index="2" title="Identity"></button>
            <button class="nav-dot" data-index="3" title="Expansion"></button>
            <button class="nav-dot" data-index="4" title="Protection"></button>
        </nav>

        <div class="viewport">
            <div class="world" id="world">
                <div class="item layer-panel" data-label="FOUNDATION"><div class="panel-inner"><span class="layer-bg">VAULT</span><div class="panel-copy"><p class="layer-tag">Layer I · Foundation · ↓ Scroll to initiate</p><h2>SECURE.<br>ELEVATE.</h2><p class="layer-line">Protecting your digital assets.<br>Building your legacy.</p><div class="scroll-hint"><span>↓</span> scroll down to engage</div></div></div></div>
                <div class="item layer-panel" data-label="SECURITY"><div class="panel-inner is-right"><span class="layer-bg">RECOVER</span><div class="panel-copy align-right"><p class="layer-tag">Layer II · Security</p><h2>RECOVER.<br>RESTORE.</h2><p class="layer-line">Official recovery guidance.<br>Securing what is rightfully yours.</p></div></div></div>
                <div class="item layer-panel" data-label="IDENTITY"><div class="panel-inner is-center"><span class="layer-bg">IDENTITY</span><div class="panel-copy align-center"><p class="layer-tag">Layer III · Identity</p><h2>AUTHORITY.<br>VERIFIED.</h2><p class="layer-line">Premium username branding.<br>Official verification support.</p></div></div></div>
                <div class="item layer-panel" data-label="EXPANSION"><div class="panel-inner is-right"><span class="layer-bg">GROWTH</span><div class="panel-copy align-right"><p class="layer-tag">Layer IV · Expansion</p><h2>EXPANSION.<br>AMPLIFIED.</h2><p class="layer-line">Targeted audience strategies.<br>High-impact content creation.</p></div></div></div>
                <div class="item layer-panel" data-label="PROTECTION"><div class="panel-inner is-center"><span class="layer-bg">SEALED</span><div class="panel-copy align-center"><p class="layer-tag">Layer V · Protection</p><h2>THE VAULT.<br>SEALED.</h2><p class="layer-line">Complete brand protection.<br>Bulletproof digital compliance.</p><div class="scroll-hint" style="margin-top: 4rem;"><span>↓</span> Keep Scrolling to Access Services</div></div></div></div>
            </div>
        </div>
    </div>

    <div class="scroll-proxy" id="proxy"></div>

    <section id="final-page">
        <div id="shader-bg"></div>
        
        <div class="final-content" id="services-section">
            <h1>OUR SERVICES</h1>
            
            <div class="cards">
                <div class="previewCard"><img class="backdrop" src="https://images.unsplash.com/photo-1550751827-4bd374c3f58b?q=80&w=600&auto=format&fit=crop" alt="Security"><div class="content"><span class="category">Security</span><h3 class="title">Account Recovery</h3><div class="description"><p>Secure support & official recovery guidance.</p><span class="icon-plus">+</span></div></div></div>
                <div class="previewCard"><img class="backdrop" src="https://images.unsplash.com/photo-1616469829581-73993eb86b02?q=80&w=600&auto=format&fit=crop" alt="Branding"><div class="content"><span class="category">Identity</span><h3 class="title">Username Branding</h3><div class="description"><p>Consultation & availability research.</p><span class="icon-plus">+</span></div></div></div>
                <div class="previewCard"><img class="backdrop" src="https://images.unsplash.com/photo-1611162617474-5b21e879e113?q=80&w=600&auto=format&fit=crop" alt="Setup"><div class="content"><span class="category">Presence</span><h3 class="title">Profile Setup</h3><div class="description"><p>Optimization for IG, TikTok, Snap & Telegram.</p><span class="icon-plus">+</span></div></div></div>
                <div class="previewCard"><img class="backdrop" src="https://images.unsplash.com/photo-1460925895917-afdab827c52f?q=80&w=600&auto=format&fit=crop" alt="Growth"><div class="content"><span class="category">Growth</span><h3 class="title">Organic Strategies</h3><div class="description"><p>Audience engagement & targeted growth plans.</p><span class="icon-plus">+</span></div></div></div>
                <div class="previewCard"><img class="backdrop" src="https://images.unsplash.com/photo-1563986768609-322da13575f3?q=80&w=600&auto=format&fit=crop" alt="Verification"><div class="content"><span class="category">Authority</span><h3 class="title">Verification Support</h3><div class="description"><p>Eligibility checks & application guidance.</p><span class="icon-plus">+</span></div></div></div>
                <div class="previewCard"><img class="backdrop" src="https://images.unsplash.com/photo-1536240478700-b869070f9279?q=80&w=600&auto=format&fit=crop" alt="Content"><div class="content"><span class="category">Media</span><h3 class="title">Content Studio</h3><div class="description"><p>Reel editing & creation for maximum reach.</p><span class="icon-plus">+</span></div></div></div>
                <div class="previewCard"><img class="backdrop" src="https://images.unsplash.com/photo-1551288049-bebda4e38f71?q=80&w=600&auto=format&fit=crop" alt="Audits"><div class="content"><span class="category">Analytics</span><h3 class="title">Profile Audits</h3><div class="description"><p>Deep-dive performance improvement strategies.</p><span class="icon-plus">+</span></div></div></div>
                <div class="previewCard"><img class="backdrop" src="https://images.unsplash.com/photo-1550745165-9bc0b252726f?q=80&w=600&auto=format&fit=crop" alt="Legacy"><div class="content"><span class="category">Strategy</span><h3 class="title">Aged Accounts</h3><div class="description"><p>Consulting for branding & history insights.</p><span class="icon-plus">+</span></div></div></div>
                <div class="previewCard"><img class="backdrop" src="https://images.unsplash.com/photo-1573164713988-8665fc963095?q=80&w=600&auto=format&fit=crop" alt="Community"><div class="content"><span class="category">Engagement</span><h3 class="title">Community Management</h3><div class="description"><p>Professional DM handling & fan interaction.</p><span class="icon-plus">+</span></div></div></div>
                <div class="previewCard"><img class="backdrop" src="https://images.unsplash.com/photo-1611162616305-c69b3fa7fbe0?q=80&w=600&auto=format&fit=crop" alt="Influencer"><div class="content"><span class="category">Networking</span><h3 class="title">Influencer Marketing</h3><div class="description"><p>Collaboration & partnership support.</p><span class="icon-plus">+</span></div></div></div>
                <div class="previewCard"><img class="backdrop" src="https://images.unsplash.com/photo-1563013544-824ae1b704d3?q=80&w=600&auto=format&fit=crop" alt="Protection"><div class="content"><span class="category">Legal</span><h3 class="title">Brand Protection</h3><div class="description"><p>Copyright education & fake account reporting.</p><span class="icon-plus">+</span></div></div></div>
                <div class="previewCard"><img class="backdrop" src="https://images.unsplash.com/photo-1450101499163-c8848c66cb85?q=80&w=600&auto=format&fit=crop" alt="Compliance"><div class="content"><span class="category">Safety</span><h3 class="title">Policy & Compliance</h3><div class="description"><p>2FA setup, safe digital exchange & strike avoidance.</p><span class="icon-plus">+</span></div></div></div>
            </div>

            <div class="contact-container" id="contact-section">
                <h3>INITIATE TRANSMISSION</h3>
                <p>Ready to secure and elevate your digital presence? Submit your inquiry below and our specialists will establish contact.</p>
                
                <form class="contact-form" action="https://formspree.io/f/YOUR_FORMSPREE_ENDPOINT" method="POST">
                    <input type="text" name="name" placeholder="Full Name / Brand Name" required>
                    <input type="email" name="email" placeholder="Secure Email Address" required>
                    <select name="service" required>
                        <option value="" disabled selected>Select Service of Interest...</option>
                        <option value="recovery">Account Recovery & Security</option>
                        <option value="branding">Username Branding</option>
                        <option value="verification">Verification Support</option>
                        <option value="growth">Organic Growth & Content</option>
                        <option value="other">Other Inquiry</option>
                    </select>
                    <textarea name="message" placeholder="Describe your current situation or goals..." required></textarea>
                    <button type="submit">Send Transmission</button>
                </form>
            </div>

            <footer class="site-footer">
                <div class="footer-grid">
                    <div class="footer-col">
                        <h4>VAULT</h4>
                        <p style="color: var(--text-muted); font-size: 0.85rem; line-height: 1.5; margin-top: 0;">
                            Premium digital asset management, brand protection, and strategic growth for elite creators and businesses.
                        </p>
                    </div>
                    
                    <div class="footer-col">
                        <h4 style="font-size: 1.2rem;">Quick Links</h4>
                        <ul>
                            <li><a href="#" onclick="window.scrollTo({top: 0, behavior: 'smooth'}); return false;">Return to Origin</a></li>
                            <li><a href="#services-section">View Services</a></li>
                            <li><a href="#contact-section">Contact Us</a></li>
                        </ul>
                    </div>

                    <div class="footer-col">
                        <h4 style="font-size: 1.2rem;">Legal & Policy</h4>
                        <ul>
                            <li><a href="#terms">Terms & Conditions</a></li>
                            <li><a href="#privacy">Privacy Policy</a></li>
                            <li><a href="#cookies">Cookie Policy</a></li>
                            <li><a href="#disclaimer">Legal Disclaimer</a></li>
                        </ul>
                    </div>
                </div>

                <div class="footer-bottom">
                    <div>&copy; 2026 VAULT Digital Management. All rights reserved.</div>
                    <div style="display: flex; gap: 15px;">
                        <a href="#" style="color: var(--text-muted); text-decoration: none; transition: color 0.3s;" onmouseover="this.style.color='var(--accent)'" onmouseout="this.style.color='var(--text-muted)'">Instagram</a>
                        <a href="#" style="color: var(--text-muted); text-decoration: none; transition: color 0.3s;" onmouseover="this.style.color='var(--accent)'" onmouseout="this.style.color='var(--text-muted)'">Twitter/X</a>
                        <a href="#" style="color: var(--text-muted); text-decoration: none; transition: color 0.3s;" onmouseover="this.style.color='var(--accent)'" onmouseout="this.style.color='var(--text-muted)'">LinkedIn</a>
                    </div>
                </div>
            </footer>

        </div>
    </section>

    <script>
        // --- 0. SMOOTH GLASS LOADER LOGIC ---
        document.addEventListener("DOMContentLoaded", () => {
            let loadProgress = 0;
            const loaderBar = document.getElementById('loader-bar');
            const loaderPercent = document.getElementById('loader-percent');
            const loader = document.getElementById('loader');

            document.body.style.overflow = 'hidden';

            const interval = setInterval(() => {
                loadProgress += Math.floor(Math.random() * 15) + 5;
                if (loadProgress >= 100) {
                    loadProgress = 100;
                    clearInterval(interval);
                    
                    // The smooth glass melt-away effect
                    setTimeout(() => {
                        loader.style.opacity = '0';
                        loader.style.backdropFilter = 'blur(0px)';
                        loader.style.webkitBackdropFilter = 'blur(0px)';
                        
                        setTimeout(() => {
                            loader.style.visibility = 'hidden';
                            document.body.style.overflow = ''; 
                        }, 1000); // Wait for the transition to finish before hiding
                    }, 500);
                }
                loaderBar.style.width = loadProgress + '%';
                loaderPercent.innerText = loadProgress + '%';
            }, 120);
        });

        // --- 1. THEME TOGGLE LOGIC ---
        const themeBtn = document.getElementById('theme-toggle');
        let isDarkMode = true;

        themeBtn.addEventListener('click', () => {
            isDarkMode = !isDarkMode;
            if (isDarkMode) {
                document.body.removeAttribute('data-theme');
                themeBtn.innerText = '☀️ Light';
                updateShaderColors('#1a0000', '#8a0a00', '#ff4d00', '#ffea00'); 
            } else {
                document.body.setAttribute('data-theme', 'light');
                themeBtn.innerText = '🌙 Dark';
                updateShaderColors('#ffffff', '#e0f7fa', '#b2ebf2', '#4dd0e1'); 
            }
        });

        // --- 2. SCROLL & 3D LOGIC ---
        const world = document.getElementById('world');
        const proxy = document.getElementById('proxy');
        const fixedUI = document.getElementById('fixed-ui');
        const shaderBg = document.getElementById('shader-bg');
        const panels = document.querySelectorAll('.layer-panel');
        const navDots = document.querySelectorAll('.nav-dot');
        const hudLabel = document.getElementById('hud_label');
        const progressFill = document.getElementById('progress_fill');

        const zStep = 2500; 
        panels.forEach((panel, index) => { panel.style.transform = `translateZ(-${index * zStep}px)`; });

        const maxScrollDepth = (panels.length - 1) * zStep;
        proxy.style.height = `${maxScrollDepth + window.innerHeight}px`;

        window.addEventListener('scroll', () => {
            const scrollY = window.scrollY;
            
            if (scrollY <= maxScrollDepth) {
                world.style.transform = `translateZ(${scrollY}px)`;
                fixedUI.style.transform = `translateY(0px)`;
                shaderBg.style.opacity = '0'; 
                
                let progress = (scrollY / maxScrollDepth) * 100;
                progressFill.style.height = `${Math.min(100, Math.max(0, progress))}%`;

                let activeIndex = Math.round(scrollY / zStep);
                activeIndex = Math.min(panels.length - 1, Math.max(0, activeIndex));

                navDots.forEach(dot => dot.classList.remove('is-active'));
                navDots[activeIndex].classList.add('is-active');
                hudLabel.innerText = panels[activeIndex].getAttribute('data-label');

                panels.forEach((panel, index) => {
                    if (scrollY - (index * zStep) > 500) {
                        panel.style.setProperty('--item-opacity', '0');
                    } else {
                        panel.style.setProperty('--item-opacity', '1');
                    }
                });
            } else {
                world.style.transform = `translateZ(${maxScrollDepth}px)`;
                const overflow = scrollY - maxScrollDepth;
                fixedUI.style.transform = `translateY(-${overflow}px)`; 
                shaderBg.style.opacity = '1'; 
            }
        });

        navDots.forEach(dot => {
            dot.addEventListener('click', (e) => {
                const index = e.target.getAttribute('data-index');
                window.scrollTo({ top: index * zStep, behavior: 'smooth' });
            });
        });

        // --- 3. THREE.JS FLUID SHADER LOGIC ---
        const scene = new THREE.Scene();
        const camera = new THREE.OrthographicCamera(-1, 1, 1, -1, 0.1, 10);
        camera.position.z = 1;

        const renderer = new THREE.WebGLRenderer({ antialias: true, alpha: true });
        renderer.setSize(window.innerWidth, window.innerHeight);
        renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
        shaderBg.appendChild(renderer.domElement); 

        const vertexShader = `varying vec2 vUv; void main() { vUv = uv; gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0); }`;

        const fragmentShader = `
            varying vec2 vUv; uniform float uTime; uniform vec2 uResolution;
            uniform vec3 uColor1; uniform vec3 uColor2; uniform vec3 uColor3; uniform vec3 uColor4;
            uniform vec3 uLightPos; uniform float uDepth; uniform float uSpeed; uniform float uNoiseScale;
            uniform float uWarpAmount; uniform float uFoldFrequency; uniform float uAngle;
            uniform float uConnections; uniform float uShadowWidth;

            vec3 mod289(vec3 x) { return x - floor(x * (1.0 / 289.0)) * 289.0; }
            vec4 mod289(vec4 x) { return x - floor(x * (1.0 / 289.0)) * 289.0; }
            vec4 permute(vec4 x) { return mod289(((x*34.0)+1.0)*x); }
            vec4 taylorInvSqrt(vec4 r) { return 1.79284291400159 - 0.85373472095314 * r; }

            float snoise(vec3 v) {
                const vec2 C = vec2(1.0/6.0, 1.0/3.0); const vec4 D = vec4(0.0, 0.5, 1.0, 2.0);
                vec3 i = floor(v + dot(v, C.yyy)); vec3 x0 = v - i + dot(i, C.xxx);
                vec3 g = step(x0.yzx, x0.xyz); vec3 l = 1.0 - g;
                vec3 i1 = min(g.xyz, l.zxy); vec3 i2 = max(g.xyz, l.zxy);
                vec3 x1 = x0 - i1 + C.xxx; vec3 x2 = x0 - i2 + C.yyy; vec3 x3 = x0 - D.yyy;
                i = mod289(i); vec4 p = permute(permute(permute(i.z + vec4(0.0, i1.z, i2.z, 1.0)) + i.y + vec4(0.0, i1.y, i2.y, 1.0)) + i.x + vec4(0.0, i1.x, i2.x, 1.0));
                float n_ = 0.142857142857; vec3 ns = n_ * D.wyz - D.xzx;
                vec4 j = p - 49.0 * floor(p * ns.z * ns.z);
                vec4 x_ = floor(j * ns.z); vec4 y_ = floor(j - 7.0 * x_);
                vec4 x = x_ * ns.x + ns.yyyy; vec4 y = y_ * ns.x + ns.yyyy;
                vec4 h = 1.0 - abs(x) - abs(y); vec4 b0 = vec4(x.xy, y.xy); vec4 b1 = vec4(x.zw, y.zw);
                vec4 s0 = floor(b0) * 2.0 + 1.0; vec4 s1 = floor(b1) * 2.0 + 1.0;
                vec4 sh = -step(h, vec4(0.0));
                vec4 a0 = b0.xzyw + s0.xzyw * sh.xxyy; vec4 a1 = b1.xzyw + s1.xzyw * sh.zzww;
                vec3 p0 = vec3(a0.xy, h.x); vec3 p1 = vec3(a0.zw, h.y); vec3 p2 = vec3(a1.xy, h.z); vec3 p3 = vec3(a1.zw, h.w);
                vec4 norm = taylorInvSqrt(vec4(dot(p0,p0), dot(p1,p1), dot(p2, p2), dot(p3,p3)));
                p0 *= norm.x; p1 *= norm.y; p2 *= norm.z; p3 *= norm.w;
                vec4 m = max(0.5 - vec4(dot(x0,x0), dot(x1,x1), dot(x2,x2), dot(x3,x3)), 0.0);
                return 105.0 * dot(m*m, vec4(dot(p0,x0), dot(p1,x1), dot(p2,x2), dot(p3,x3)));
            }

            float getSurface(vec2 p) {
                float c = cos(uAngle), s = sin(uAngle); mat2 rot = mat2(c, -s, s, c); vec2 rp = rot * p;
                float n1 = snoise(vec3(rp * uNoiseScale * 0.25, uTime * uSpeed * 0.7));
                float n2 = snoise(vec3(rp * uNoiseScale * 0.25 + vec2(21.4, 15.2), uTime * uSpeed * 0.9));
                float trig1 = sin(rp.x * uNoiseScale * 0.5 + uTime * uSpeed) * 0.3;
                float trig2 = cos(rp.y * uNoiseScale * 0.5 - uTime * uSpeed) * 0.3;
                vec2 flow = vec2(n1 + trig1, n2 + trig2); vec2 wp = rp + flow * (uWarpAmount * 0.12);
                float freq = uFoldFrequency * 0.5; float phase = sin(wp.y * freq + flow.y * 2.0) * uConnections;
                float mainWave = sin(wp.x * freq + phase * uWarpAmount * 0.3);
                float n3 = snoise(vec3(wp * 0.5, uTime * uSpeed * 0.5));
                return (mainWave * 0.85 + n3 * 0.15) * 0.5;
            }

            void main() {
                vec2 uv = gl_FragCoord.xy / uResolution.xy; vec2 p = uv * 2.0 - 1.0; p.x *= uResolution.x / uResolution.y;
                vec2 e = vec2(0.09, 0.0);
                float dx = (getSurface(p + e.xy) - getSurface(p - e.xy)) / (2.0 * e.x);
                float dy = (getSurface(p + e.yx) - getSurface(p - e.yx)) / (2.0 * e.x);
                float safeDepth = max(uDepth, 0.02);
                vec3 normal = normalize(vec3(-dx, -dy, safeDepth)); vec3 lightDir = normalize(uLightPos);
                float diffuse = dot(normal, lightDir) * 0.5 + 0.5;
                float t = clamp(diffuse + getSurface(p) * 0.04, 0.0, 1.0);
                t = t * t * (3.0 - 2.0 * t);
                vec3 color = mix(uColor1, uColor2, smoothstep(0.0, uShadowWidth + 0.15, t));
                color = mix(color, uColor3, smoothstep(uShadowWidth + 0.05, 0.65, t));
                color = mix(color, uColor4, smoothstep(0.55, 1.05, t));
                float grain = fract(sin(dot(uv.xy, vec2(12.9898,78.233))) * 43758.5453); color += (grain - 0.5) * 0.03;
                gl_FragColor = vec4(color, 1.0);
            }
        `;

        const material = new THREE.ShaderMaterial({
            vertexShader, fragmentShader,
            uniforms: {
                uTime: { value: 0 }, uResolution: { value: new THREE.Vector2(window.innerWidth, window.innerHeight) },
                uColor1: { value: new THREE.Color('#1a0000') }, uColor2: { value: new THREE.Color('#8a0a00') },
                uColor3: { value: new THREE.Color('#ff4d00') }, uColor4: { value: new THREE.Color('#ffea00') },
                uDepth: { value: 0.04 }, uLightPos: { value: new THREE.Vector3(0.968, -0.36, 1.0) },
                uSpeed: { value: 0.1148 }, uNoiseScale: { value: 0.714 }, uWarpAmount: { value: 4.0 },
                uFoldFrequency: { value: 1.865 }, uAngle: { value: 1.08699 }, uConnections: { value: 0.8715 },
                uShadowWidth: { value: 0.01 }
            }
        });

        scene.add(new THREE.Mesh(new THREE.PlaneGeometry(2, 2), material));

        function updateShaderColors(c1, c2, c3, c4) {
            material.uniforms.uColor1.value.set(c1);
            material.uniforms.uColor2.value.set(c2);
            material.uniforms.uColor3.value.set(c3);
            material.uniforms.uColor4.value.set(c4);
        }

        window.addEventListener('resize', () => {
            renderer.setSize(window.innerWidth, window.innerHeight);
            material.uniforms.uResolution.value.set(window.innerWidth, window.innerHeight);
        });

        const clock = new THREE.Clock();
        function animate() { requestAnimationFrame(animate); material.uniforms.uTime.value = clock.getElapsedTime(); renderer.render(scene, camera); }
        animate();
    </script>
</body>
</html>
