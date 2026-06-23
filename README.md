<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>HR // SOC Threat Console — Tier 1·2·3 Preview</title>
<style>
  :root{
    --bg:#04080f; --panel:#0a1929; --border:#0e3a5c;
    --text:#e6f1ff; --muted:#5fa8d3; --cyan:#00b4d8; --neon:#64ffda; --alert:#ff4d6d; --ok:#7ee787; --amber:#ffb703;
  }
  *{box-sizing:border-box;}
  body{margin:0;background:var(--bg);color:var(--text);font-family:"JetBrains Mono",ui-monospace,Consolas,monospace;line-height:1.6;overflow-x:hidden;}
  body::before{content:"";position:fixed;inset:0;z-index:0;pointer-events:none;
    background-image:linear-gradient(rgba(14,58,92,.25) 1px,transparent 1px),linear-gradient(90deg,rgba(14,58,92,.25) 1px,transparent 1px);
    background-size:42px 42px;mask-image:radial-gradient(ellipse 80% 60% at 50% 25%,#000 40%,transparent 100%);animation:drift 16s linear infinite;}
  @keyframes drift{from{background-position:0 0,0 0}to{background-position:42px 42px,42px 42px}}
  body::after{content:"";position:fixed;inset:0;z-index:50;pointer-events:none;background:repeating-linear-gradient(transparent 0 2px,rgba(0,0,0,.18) 3px 4px);mix-blend-mode:overlay;opacity:.4;}
  .note{position:relative;z-index:2;text-align:center;font-size:12px;color:var(--muted);padding:9px;border-bottom:1px solid var(--border);background:#06101e;}
  .wrap{position:relative;z-index:2;max-width:1040px;margin:0 auto;padding:26px 18px 90px;}
  .hero{text-align:center;}.hero img{max-width:100%;}
  .frame{border:1px solid var(--border);border-radius:14px;overflow:hidden;box-shadow:0 0 40px rgba(0,180,216,.18),inset 0 0 60px rgba(0,180,216,.05);}
  .typing{margin:18px auto 6px;color:var(--neon);font-size:15px;min-height:24px;text-shadow:0 0 8px rgba(100,255,218,.6);}
  .cursor{display:inline-block;width:9px;height:16px;background:var(--neon);margin-left:2px;animation:blink 1s steps(1) infinite;vertical-align:middle;box-shadow:0 0 8px var(--neon);}
  @keyframes blink{50%{opacity:0}}
  .links{margin-top:16px;display:flex;gap:10px;justify-content:center;flex-wrap:wrap;}
  .chip{border:1px solid var(--border);background:var(--panel);color:var(--neon);padding:8px 16px;border-radius:8px;font-size:12px;font-weight:600;letter-spacing:1px;text-decoration:none;transition:.2s;}
  .chip:hover{border-color:var(--neon);box-shadow:0 0 14px rgba(100,255,218,.4);transform:translateY(-2px);}
  .chip.yt{color:var(--alert);}.chip.tx{color:var(--cyan);}
  h2{font-size:15px;letter-spacing:2px;color:var(--neon);margin:40px 0 14px;text-shadow:0 0 8px rgba(100,255,218,.4);}
  h2 .br{color:var(--muted);} h2 .sub{color:var(--muted);font-size:12px;letter-spacing:1px;}
  hr{border:none;border-top:1px dashed var(--border);margin:30px 0;opacity:.6;}
  .term{background:#06101e;border:1px solid var(--border);border-radius:10px;padding:16px 18px;font-size:13px;line-height:1.8;box-shadow:inset 0 0 30px rgba(0,180,216,.06);}
  .term .ok{color:var(--ok);}.term .go{color:var(--cyan);}.term .pr{color:var(--neon);}.term .dim{color:var(--muted);}
  blockquote{margin:16px 0;padding:2px 16px;border-left:3px solid var(--cyan);color:#bcdcee;font-family:-apple-system,sans-serif;}
  blockquote b{color:var(--neon);}
  /* TIER MATRIX */
  .matrix{display:grid;grid-template-columns:repeat(3,1fr);gap:14px;}
  .tier{background:var(--panel);border:1px solid var(--border);border-radius:12px;padding:0;overflow:hidden;transition:.25s;}
  .tier:hover{border-color:var(--neon);box-shadow:0 0 22px rgba(100,255,218,.22);transform:translateY(-3px);}
  .tier .head{padding:12px 16px;font-size:14px;font-weight:700;letter-spacing:1px;border-bottom:1px solid var(--border);}
  .t1 .head{color:var(--ok);background:linear-gradient(90deg,rgba(126,231,135,.08),transparent);}
  .t2 .head{color:var(--cyan);background:linear-gradient(90deg,rgba(0,180,216,.1),transparent);}
  .t3 .head{color:var(--alert);background:linear-gradient(90deg,rgba(255,77,109,.1),transparent);}
  .tier .body{padding:14px 16px;}
  .meta{font-size:11px;color:var(--muted);margin-bottom:10px;}
  .meta b{color:var(--amber);}
  .lbl{font-size:10px;letter-spacing:1px;color:var(--cyan);margin:12px 0 6px;}
  .tier ul{margin:0;padding-left:16px;}
  .tier li{font-size:11.5px;color:#bcdcee;font-family:-apple-system,sans-serif;margin:4px 0;}
  .toolrow{margin-top:6px;}
  .tool{display:inline-block;background:#06101e;border:1px solid var(--border);color:#9fc7e0;font-size:10px;padding:3px 7px;border-radius:5px;margin:2px;}
  .cert{margin-top:10px;font-size:11px;color:var(--neon);}
  /* AI flow */
  .aiflow{background:#06101e;border:1px solid var(--border);border-radius:10px;padding:18px;font-size:13px;line-height:2;color:#bcdcee;}
  .aiflow .h{color:var(--neon);} .aiflow .a{color:var(--cyan);} .aiflow .o{color:var(--ok);}
  /* arsenal */
  .arsenal h3{font-size:12px;color:var(--cyan);letter-spacing:1px;margin:18px 0 8px;}
  .arsenal img{margin:3px;}
  /* ops */
  .ops{display:grid;grid-template-columns:1fr 1fr;gap:14px;}
  .op{background:var(--panel);border:1px solid var(--border);border-radius:10px;padding:16px;transition:.2s;}
  .op:hover{border-color:var(--neon);box-shadow:0 0 18px rgba(100,255,218,.2);}
  .op .tag{font-size:10px;color:var(--bg);background:var(--neon);padding:2px 7px;border-radius:4px;font-weight:700;}
  .op h4{margin:8px 0 6px;color:var(--neon);font-size:13px;letter-spacing:1px;}
  .op .bar{font-size:12px;color:var(--cyan);margin-bottom:8px;}
  .op p{font-size:12px;color:#bcdcee;font-family:-apple-system,sans-serif;margin:0 0 12px;}
  .op a{color:var(--neon);font-size:12px;text-decoration:none;border:1px solid var(--border);padding:5px 12px;border-radius:6px;}
  .op a:hover{border-color:var(--neon);box-shadow:0 0 12px rgba(100,255,218,.35);}
  .stats{display:flex;gap:14px;flex-wrap:wrap;justify-content:center;}
  .stat{background:var(--panel);border:1px solid var(--border);border-radius:10px;padding:18px 22px;min-width:250px;}
  .stat .t{color:var(--neon);font-size:13px;margin-bottom:12px;letter-spacing:1px;}
  .stat .r{display:flex;justify-content:space-between;font-size:12px;margin:6px 0;}
  .stat .r span:first-child{color:var(--cyan);}
  .ghost{text-align:center;color:var(--muted);font-size:11px;font-style:italic;margin-top:10px;}
  .footer{text-align:center;margin-top:40px;}
  .footer .q{color:var(--neon);font-size:14px;text-shadow:0 0 8px rgba(100,255,218,.4);}
  @media(max-width:780px){.matrix,.ops{grid-template-columns:1fr;}}
</style>
</head>
<body>
<div class="note">⚡ FULL FUTURISTIC PREVIEW — now covering SOC Tier 1·2·3 (2026 AI-augmented). Glow/scanlines are HTML-only; the cinematic animated SVG hero, typing line, neon badges & widgets all survive on GitHub.</div>
<div class="wrap">
  <div class="hero">
    <div class="frame">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 340" width="1200" height="340" font-family="'JetBrains Mono','Courier New',monospace">
  <defs>
    <linearGradient id="bg" x1="0" y1="0" x2="1" y2="1">
      <stop offset="0%" stop-color="#02060d"/>
      <stop offset="55%" stop-color="#061325"/>
      <stop offset="100%" stop-color="#08203a"/>
    </linearGradient>
    <radialGradient id="halo" cx="72%" cy="45%" r="55%">
      <stop offset="0%" stop-color="#00b4d8" stop-opacity="0.30"/>
      <stop offset="100%" stop-color="#00b4d8" stop-opacity="0"/>
    </radialGradient>
    <filter id="glow" x="-30%" y="-30%" width="160%" height="160%">
      <feGaussianBlur stdDeviation="2.4" result="b"/>
      <feMerge><feMergeNode in="b"/><feMergeNode in="SourceGraphic"/></feMerge>
    </filter>
    <clipPath id="screen"><rect x="40" y="58" width="470" height="224" rx="10"/></clipPath>
  </defs>

  <!-- base -->
  <rect width="1200" height="340" fill="url(#bg)"/>
  <rect width="1200" height="340" fill="url(#halo)"/>

  <!-- ===== MATRIX CODE RAIN (right field) ===== -->
  <g font-size="15" font-family="monospace" opacity="0.55">
    <g fill="#15a06a">
      <text x="560" y="0"><tspan x="560" dy="16">1</tspan><tspan x="560" dy="16">0</tspan><tspan x="560" dy="16">1</tspan><tspan x="560" dy="16">1</tspan><tspan x="560" dy="16">0</tspan><tspan x="560" dy="16">0</tspan><tspan x="560" dy="16">1</tspan><tspan x="560" dy="16">0</tspan><tspan x="560" dy="16">1</tspan><tspan x="560" dy="16">1</tspan><tspan x="560" dy="16">0</tspan><tspan x="560" dy="16" fill="#b6ffd9">1</tspan><animateTransform attributeName="transform" type="translate" from="0 -200" to="0 360" dur="5.5s" begin="0s" repeatCount="indefinite"/></text>
      <text x="610" y="0"><tspan x="610" dy="16">0</tspan><tspan x="610" dy="16">1</tspan><tspan x="610" dy="16">0</tspan><tspan x="610" dy="16">0</tspan><tspan x="610" dy="16">1</tspan><tspan x="610" dy="16">1</tspan><tspan x="610" dy="16">0</tspan><tspan x="610" dy="16">1</tspan><tspan x="610" dy="16">0</tspan><tspan x="610" dy="16" fill="#b6ffd9">0</tspan><animateTransform attributeName="transform" type="translate" from="0 -160" to="0 360" dur="7s" begin="1.2s" repeatCount="indefinite"/></text>
      <text x="660" y="0"><tspan x="660" dy="16">1</tspan><tspan x="660" dy="16">1</tspan><tspan x="660" dy="16">0</tspan><tspan x="660" dy="16">1</tspan><tspan x="660" dy="16">0</tspan><tspan x="660" dy="16">1</tspan><tspan x="660" dy="16">1</tspan><tspan x="660" dy="16">0</tspan><tspan x="660" dy="16">0</tspan><tspan x="660" dy="16">1</tspan><tspan x="660" dy="16">1</tspan><tspan x="660" dy="16">0</tspan><tspan x="660" dy="16" fill="#b6ffd9">1</tspan><animateTransform attributeName="transform" type="translate" from="0 -240" to="0 360" dur="6.2s" begin="0.5s" repeatCount="indefinite"/></text>
    </g>
    <g fill="#0e7c9e">
      <text x="1040" y="0"><tspan x="1040" dy="16">0</tspan><tspan x="1040" dy="16">1</tspan><tspan x="1040" dy="16">1</tspan><tspan x="1040" dy="16">0</tspan><tspan x="1040" dy="16">1</tspan><tspan x="1040" dy="16">0</tspan><tspan x="1040" dy="16">0</tspan><tspan x="1040" dy="16">1</tspan><tspan x="1040" dy="16">1</tspan><tspan x="1040" dy="16" fill="#9be8ff">0</tspan><animateTransform attributeName="transform" type="translate" from="0 -180" to="0 360" dur="6.6s" begin="0.9s" repeatCount="indefinite"/></text>
      <text x="1095" y="0"><tspan x="1095" dy="16">1</tspan><tspan x="1095" dy="16">0</tspan><tspan x="1095" dy="16">0</tspan><tspan x="1095" dy="16">1</tspan><tspan x="1095" dy="16">1</tspan><tspan x="1095" dy="16">0</tspan><tspan x="1095" dy="16">1</tspan><tspan x="1095" dy="16">0</tspan><tspan x="1095" dy="16">1</tspan><tspan x="1095" dy="16">1</tspan><tspan x="1095" dy="16" fill="#9be8ff">0</tspan><animateTransform attributeName="transform" type="translate" from="0 -220" to="0 360" dur="5.8s" begin="2s" repeatCount="indefinite"/></text>
      <text x="1150" y="0"><tspan x="1150" dy="16">0</tspan><tspan x="1150" dy="16">0</tspan><tspan x="1150" dy="16">1</tspan><tspan x="1150" dy="16">0</tspan><tspan x="1150" dy="16">1</tspan><tspan x="1150" dy="16">1</tspan><tspan x="1150" dy="16">0</tspan><tspan x="1150" dy="16">0</tspan><tspan x="1150" dy="16" fill="#9be8ff">1</tspan><animateTransform attributeName="transform" type="translate" from="0 -140" to="0 360" dur="7.4s" begin="1.6s" repeatCount="indefinite"/></text>
    </g>
  </g>

  <!-- faint grid -->
  <g stroke="#0e3a5c" stroke-width="1" opacity="0.35">
    <line x1="0" y1="170" x2="1200" y2="170"/>
    <line x1="540" y1="0" x2="540" y2="340"/>
    <line x1="900" y1="0" x2="900" y2="340"/>
  </g>

  <!-- corner HUD brackets -->
  <g stroke="#64ffda" stroke-width="2" fill="none" opacity="0.8">
    <path d="M22 22 h38 M22 22 v38"/>
    <path d="M1178 22 h-38 M1178 22 v38"/>
    <path d="M22 318 h38 M22 318 v-38"/>
    <path d="M1178 318 h-38 M1178 318 v-38"/>
  </g>

  <!-- ===== TERMINAL: breach -> defend -> secure ===== -->
  <g>
    <rect x="40" y="58" width="470" height="224" rx="10" fill="#04101e" stroke="#0e3a5c" stroke-width="1.5"/>
    <rect x="40" y="58" width="470" height="26" rx="10" fill="#0a1c30"/>
    <circle cx="58" cy="71" r="4.5" fill="#ff5f57"/>
    <circle cx="74" cy="71" r="4.5" fill="#febc2e"/>
    <circle cx="90" cy="71" r="4.5" fill="#28c840"/>
    <text x="250" y="75" text-anchor="middle" fill="#3d7ea6" font-size="11" letter-spacing="1">hr-soc — live monitor</text>

    <g clip-path="url(#screen)" font-size="13.5">
      <!-- each line cascades in, holds, then the whole sequence loops (10s) -->
      <text x="58" y="108" fill="#64ffda"><tspan fill="#7ee787">root@hr-soc</tspan>:~# ./monitor --live<animate attributeName="opacity" values="0;1;1;1;1;1;1;0" keyTimes="0;0.05;0.1;0.85;0.9;0.93;0.97;1" dur="10s" repeatCount="indefinite"/></text>
      <text x="58" y="132" fill="#ff4d6d">[!] ANOMALY DETECTED :: 10.0.4.17<animate attributeName="opacity" values="0;0;1;1;1;1;0;0" keyTimes="0;0.12;0.17;0.85;0.9;0.93;0.96;1" dur="10s" repeatCount="indefinite"/></text>
      <text x="58" y="156" fill="#9fc7e0">[*] correlating events across SIEM...<animate attributeName="opacity" values="0;0;1;1;1;1;0;0" keyTimes="0;0.24;0.29;0.85;0.9;0.93;0.96;1" dur="10s" repeatCount="indefinite"/></text>
      <text x="58" y="180" fill="#ff4d6d" font-weight="700">[!] BREACH CONFIRMED — lateral movement<animate attributeName="opacity" values="0;0;1;1;0.4;1;0;0" keyTimes="0;0.36;0.41;0.55;0.6;0.85;0.95;1" dur="10s" repeatCount="indefinite"/></text>
      <text x="58" y="204" fill="#ffb703">[&gt;] isolating host... blocking C2 beacon...<animate attributeName="opacity" values="0;0;1;1;1;1;0;0" keyTimes="0;0.5;0.55;0.85;0.9;0.93;0.96;1" dur="10s" repeatCount="indefinite"/></text>
      <text x="58" y="228" fill="#9fc7e0">[*] pushing detection rule -> Sentinel<animate attributeName="opacity" values="0;0;1;1;1;1;0;0" keyTimes="0;0.62;0.67;0.85;0.9;0.93;0.96;1" dur="10s" repeatCount="indefinite"/></text>
      <text x="58" y="256" fill="#7ee787" font-weight="700" filter="url(#glow)">[+] THREAT NEUTRALIZED :: SYSTEM SECURE<animate attributeName="opacity" values="0;0;0;1;0.5;1;0.6;1" keyTimes="0;0.74;0.78;0.8;0.83;0.86;0.92;1" dur="10s" repeatCount="indefinite"/></text>
      <!-- blinking cursor -->
      <rect x="58" y="270" width="8" height="14" fill="#64ffda"><animate attributeName="opacity" values="1;0;1" dur="1s" repeatCount="indefinite"/></rect>
    </g>
  </g>

  <!-- ===== GLITCH TITLE ===== -->
  <g transform="translate(870,128)" text-anchor="middle">
    <text x="2" y="2" fill="#ff00d4" font-size="50" font-weight="700" letter-spacing="3" opacity="0">HR
      <animate attributeName="opacity" values="0;0.7;0;0.5;0;0" keyTimes="0;0.05;0.1;0.16;0.22;1" dur="3.5s" repeatCount="indefinite"/>
      <animateTransform attributeName="transform" type="translate" values="-3 1; 2 -1; -2 0; 0 0" dur="3.5s" repeatCount="indefinite"/>
    </text>
    <text x="-2" y="-1" fill="#00f0ff" font-size="50" font-weight="700" letter-spacing="3" opacity="0">HR
      <animate attributeName="opacity" values="0;0.7;0;0.5;0;0" keyTimes="0;0.06;0.12;0.18;0.24;1" dur="3.5s" begin="0.1s" repeatCount="indefinite"/>
      <animateTransform attributeName="transform" type="translate" values="3 -1; -2 1; 2 0; 0 0" dur="3.5s" begin="0.1s" repeatCount="indefinite"/>
    </text>
    <text x="0" y="0" fill="#e6f1ff" font-size="50" font-weight="700" letter-spacing="3" filter="url(#glow)">HR</text>
  </g>
  <text x="870" y="172" text-anchor="middle" fill="#64ffda" font-size="26" font-weight="600" letter-spacing="7" filter="url(#glow)">AI SOC ANALYST</text>
  <text x="870" y="200" text-anchor="middle" fill="#5fa8d3" font-size="13" letter-spacing="3">TIER 1 &#8594; 2 &#8594; 3 &#160;//&#160; DETECT &#183; HUNT &#183; SECURE</text>

  <!-- ===== PADLOCK: red unlocked -> green locked ===== -->
  <g transform="translate(870,236)">
    <!-- shackle -->
    <path d="M-14 4 v-8 a14 14 0 0 1 28 0 v8" fill="none" stroke="#ff4d6d" stroke-width="5" stroke-linecap="round">
      <animate attributeName="stroke" values="#ff4d6d;#ff4d6d;#7ee787;#7ee787" keyTimes="0;0.55;0.62;1" dur="10s" repeatCount="indefinite"/>
      <animate attributeName="d" values="M-14 4 v-14 a14 14 0 0 1 28 0 v14; M-14 4 v-14 a14 14 0 0 1 28 0 v14; M-14 4 v-8 a14 14 0 0 1 28 0 v8" keyTimes="0;0.6;0.66" dur="10s" repeatCount="indefinite"/>
    </path>
    <!-- body -->
    <rect x="-20" y="2" width="40" height="30" rx="5" fill="#0a1c30" stroke="#ff4d6d" stroke-width="2.5">
      <animate attributeName="stroke" values="#ff4d6d;#ff4d6d;#7ee787;#7ee787" keyTimes="0;0.55;0.62;1" dur="10s" repeatCount="indefinite"/>
    </rect>
    <circle cx="0" cy="14" r="4" fill="#ff4d6d">
      <animate attributeName="fill" values="#ff4d6d;#ff4d6d;#7ee787;#7ee787" keyTimes="0;0.55;0.62;1" dur="10s" repeatCount="indefinite"/>
    </circle>
    <rect x="-1.5" y="16" width="3" height="9" fill="#ff4d6d">
      <animate attributeName="fill" values="#ff4d6d;#ff4d6d;#7ee787;#7ee787" keyTimes="0;0.55;0.62;1" dur="10s" repeatCount="indefinite"/>
    </rect>
  </g>

  <!-- ===== STATUS BAR ===== -->
  <rect x="540" y="298" width="660" height="0.5" fill="#0e3a5c"/>
  <text x="870" y="320" text-anchor="middle" fill="#3d7ea6" font-size="12" letter-spacing="2">
    <tspan fill="#7ee787">&#9679;</tspan> HUNT MODE: ACTIVE &#160;|&#160; THREATS BLOCKED: 1,337 &#160;|&#160; CALM MIND, RELENTLESS HUNT
  </text>

  <!-- ===== SCANNING BEAM ===== -->
  <rect x="0" y="0" width="3" height="340" fill="#64ffda" opacity="0.5">
    <animate attributeName="x" values="540;1180;540" dur="7s" repeatCount="indefinite"/>
    <animate attributeName="opacity" values="0.08;0.55;0.08" dur="7s" repeatCount="indefinite"/>
  </rect>
</svg>

    </div>
    <div class="typing"><span id="tw"></span><span class="cursor"></span></div>
    <div class="links">
      <a class="chip" href="#">▸ LINKEDIN</a>
      <a class="chip yt" href="#">▸ HRTECHVERSE</a>
      <a class="chip tx" href="#">▸ TRANSMIT</a>
      <a class="chip" href="#">◉ VISITORS · 1.2k</a>
    </div>
  </div>

  <h2><span class="br">┌──[</span> ./whoami <span class="br">]</span></h2>
  <div class="term">
    <span class="pr">visitor@hr-soc:~$</span> boot --profile=operator<br/>
    <span class="ok">&nbsp;[ OK ]</span> Computer Engineer + PG Diploma, Cybersecurity<br/>
    <span class="ok">&nbsp;[ OK ]</span> role .............. SOC Analyst <span class="dim">// full-spectrum Tier 1 → 3</span><br/>
    <span class="ok">&nbsp;[ OK ]</span> edge .............. Agentic AI for security operations<br/>
    <span class="ok">&nbsp;[ OK ]</span> lab ............... multi-VM home SOC <span class="dim">// Wazuh · Splunk · Sentinel</span><br/>
    <span class="ok">&nbsp;[ OK ]</span> location .......... New Brunswick, CA<br/>
    <span class="go">&nbsp;[ ►► ]</span> mission ........... detect first. automate everything. hunt relentlessly.
  </div>

  <h2><span class="br">┌──[</span> ./soc_operations_matrix <span class="br">]</span> <span class="sub">// Tier 1 · 2 · 3 — 2026 AI-Augmented</span></h2>
  <div class="matrix">
    <div class="tier t1">
      <div class="head">⟩ TIER 1 — TRIAGE</div>
      <div class="body">
        <div class="meta"><b>MISSION</b> first line of defense<br/><b>SLA</b> triage + escalate in 15–30 min</div>
        <div class="lbl">CORE OPS</div>
        <ul><li>Monitor SIEM dashboards 24/7</li><li>True / false positive classification</li><li>IOC enrichment + context</li><li>Ticketing & documentation</li><li>Run SOPs / playbooks</li><li>Escalate with full timeline</li><li>Tune alerts, cut false positives</li></ul>
        <div class="lbl">TOOLS</div>
        <div class="toolrow"><span class="tool">Splunk</span><span class="tool">Sentinel</span><span class="tool">QRadar</span><span class="tool">Wazuh</span><span class="tool">ServiceNow</span><span class="tool">Jira</span></div>
        <div class="cert">CERT → Security+ · CySA+ · CCOA</div>
      </div>
    </div>
    <div class="tier t2">
      <div class="head">⟩ TIER 2 — INCIDENT RESPONSE</div>
      <div class="body">
        <div class="meta"><b>MISSION</b> heavy-lifter / deep dive<br/><b>AUTH</b> containment actions approved</div>
        <div class="lbl">CORE OPS</div>
        <ul><li>Investigate escalated incidents</li><li>Cross-source event correlation</li><li>Contain → eradicate → recover</li><li>Isolate hosts, block IPs, disable accts</li><li>Basic malware (behavioral) analysis</li><li>Write & tune SIEM detection rules</li><li>Threat-intel & IOC integration</li></ul>
        <div class="lbl">TOOLS</div>
        <div class="toolrow"><span class="tool">EDR</span><span class="tool">XDR</span><span class="tool">NDR</span><span class="tool">SOAR</span><span class="tool">MISP</span><span class="tool">VirusTotal</span><span class="tool">CrowdStrike</span></div>
        <div class="cert">CERT → GCIH · ECIH · BTL1</div>
      </div>
    </div>
    <div class="tier t3">
      <div class="head">⟩ TIER 3 — HUNT / ENGINEER</div>
      <div class="body">
        <div class="meta"><b>MISSION</b> apex experts<br/><b>SCOPE</b> major incidents + strategy</div>
        <div class="lbl">CORE OPS</div>
        <ul><li>Proactive threat hunting</li><li>Digital forensics (memory + disk)</li><li>Malware reverse engineering</li><li>Detection engineering @ scale</li><li>Lead major IR / post-mortems</li><li>Security architecture input</li><li>Adversary emulation (ATT&CK)</li></ul>
        <div class="lbl">TOOLS</div>
        <div class="toolrow"><span class="tool">Volatility</span><span class="tool">Ghidra</span><span class="tool">IDA</span><span class="tool">Velociraptor</span><span class="tool">Sigma</span><span class="tool">Sliver</span></div>
        <div class="cert">CERT → GCFA · GREM · OSCP · GXPN</div>
      </div>
    </div>
  </div>
  <blockquote>◉ <b>2026 reality:</b> agentic AI now auto-handles much of Tier 1 triage — so the edge is <b>AI oversight</b>: validating AI verdicts, tuning detections, and hunting. <b>64% of 2026 SOC listings require AI / ML / automation skills.</b> That's the lane I'm built for.</blockquote>

  <h2><span class="br">┌──[</span> ./ai_augmented_soc <span class="br">]</span> <span class="sub">// the 2026 differentiator</span></h2>
  <div class="aiflow">
    <span class="h">[ HUMAN ]</span> judgment · context · attacker-mindset · final call<br/>
    &nbsp;&nbsp;&nbsp;║<br/>
    &nbsp;&nbsp;&nbsp;╠═══&gt; <span class="a">[ AGENTIC AI LAYER ]</span> auto-triage · enrichment · correlation · response<br/>
    &nbsp;&nbsp;&nbsp;║<br/>
    <span class="o">[ OUTCOME ]</span> alert fatigue eliminated · analysts freed for Tier 2/3 work
  </div>

  <h2><span class="br">┌──[</span> ./tech_arsenal <span class="br">]</span></h2>
  <div class="arsenal">
    <h3>◢ SIEM & DETECTION</h3>
    <img src="https://img.shields.io/badge/Splunk-0a1929?style=flat-square&logo=splunk&logoColor=64ffda"/>
    <img src="https://img.shields.io/badge/Microsoft_Sentinel-0a1929?style=flat-square&logo=microsoftazure&logoColor=00b4d8"/>
    <img src="https://img.shields.io/badge/Wazuh-0a1929?style=flat-square&logo=wazuh&logoColor=64ffda"/>
    <img src="https://img.shields.io/badge/IBM_QRadar-0a1929?style=flat-square&logo=ibm&logoColor=00b4d8"/>
    <img src="https://img.shields.io/badge/Elastic_ELK-0a1929?style=flat-square&logo=elasticstack&logoColor=64ffda"/>
    <img src="https://img.shields.io/badge/Sigma_Rules-0a1929?style=flat-square&logo=elastic&logoColor=00b4d8"/>
    <img src="https://img.shields.io/badge/MITRE_ATT%26CK-0a1929?style=flat-square&logo=mitre&logoColor=ff4d6d"/>
    <h3>◢ EDR / XDR / NDR & RESPONSE</h3>
    <img src="https://img.shields.io/badge/CrowdStrike-0a1929?style=flat-square&logo=crowdstrike&logoColor=ff4d6d"/>
    <img src="https://img.shields.io/badge/MS_Defender-0a1929?style=flat-square&logo=microsoft&logoColor=00b4d8"/>
    <img src="https://img.shields.io/badge/Velociraptor-0a1929?style=flat-square&logoColor=64ffda"/>
    <h3>◢ SOAR / AUTOMATION / AI</h3>
    <img src="https://img.shields.io/badge/Shuffle_SOAR-0a1929?style=flat-square&logo=zapier&logoColor=64ffda"/>
    <img src="https://img.shields.io/badge/FastAPI-0a1929?style=flat-square&logo=fastapi&logoColor=00b4d8"/>
    <img src="https://img.shields.io/badge/LangChain-0a1929?style=flat-square&logo=langchain&logoColor=64ffda"/>
    <img src="https://img.shields.io/badge/ChromaDB-0a1929?style=flat-square&logo=databricks&logoColor=00b4d8"/>
    <h3>◢ THREAT INTEL & FORENSICS / MALWARE RE</h3>
    <img src="https://img.shields.io/badge/MISP-0a1929?style=flat-square&logo=misp&logoColor=00b4d8"/>
    <img src="https://img.shields.io/badge/VirusTotal-0a1929?style=flat-square&logo=virustotal&logoColor=64ffda"/>
    <img src="https://img.shields.io/badge/Volatility-0a1929?style=flat-square&logoColor=ff4d6d"/>
    <img src="https://img.shields.io/badge/Ghidra-0a1929?style=flat-square&logoColor=64ffda"/>
    <img src="https://img.shields.io/badge/Wireshark-0a1929?style=flat-square&logo=wireshark&logoColor=00b4d8"/>
    <h3>◢ CLOUD · CASE MGMT · LANGUAGES</h3>
    <img src="https://img.shields.io/badge/Azure-0a1929?style=flat-square&logo=microsoftazure&logoColor=00b4d8"/>
    <img src="https://img.shields.io/badge/AWS-0a1929?style=flat-square&logo=amazonaws&logoColor=64ffda"/>
    <img src="https://img.shields.io/badge/ServiceNow-0a1929?style=flat-square&logo=servicenow&logoColor=00b4d8"/>
    <img src="https://img.shields.io/badge/Jira-0a1929?style=flat-square&logo=jira&logoColor=64ffda"/>
    <img src="https://img.shields.io/badge/Python-0a1929?style=flat-square&logo=python&logoColor=64ffda"/>
    <img src="https://img.shields.io/badge/PowerShell-0a1929?style=flat-square&logo=powershell&logoColor=00b4d8"/>
    <img src="https://img.shields.io/badge/Bash-0a1929?style=flat-square&logo=gnubash&logoColor=64ffda"/>
    <img src="https://img.shields.io/badge/Linux-0a1929?style=flat-square&logo=linux&logoColor=00b4d8"/>
  </div>

  <h2><span class="br">┌──[</span> ./featured_ops <span class="br">]</span> <span class="sub">// projects mapped to tier</span></h2>
  <div class="ops">
    <div class="op"><span class="tag">T1</span><h4>SIEM DETECTION & TRIAGE LAB</h4><div class="bar">[████████░░] ACTIVE</div><p>Splunk + Sentinel alert pipeline — TP/FP triage, IOC enrichment, documented escalation runbooks.</p><a href="#">▸ ACCESS OP</a></div>
    <div class="op"><span class="tag">T1·T2</span><h4>WAZUH HOME SOC</h4><div class="bar">[████████░░] ACTIVE</div><p>End-to-end SIEM on a multi-VM lab — agent rollout, custom rules, dashboards, attack/defend cycles.</p><a href="#">▸ ACCESS OP</a></div>
    <div class="op"><span class="tag">T2</span><h4>AI-POWERED SOAR PLATFORM</h4><div class="bar">[████████░░] ACTIVE</div><p>Multi-agent auto-triage, enrichment & response. Python · FastAPI · LangChain · ChromaDB.</p><a href="#">▸ ACCESS OP</a></div>
    <div class="op"><span class="tag">T2·T3</span><h4>AD ATTACK & DEFEND LAB</h4><div class="bar">[██████░░░░] BUILDING</div><p>Full AD kill-chain → blue-team detection & hardening, logs piped to SIEM for rule validation.</p><a href="#">▸ ACCESS OP</a></div>
    <div class="op"><span class="tag">T3</span><h4>AI THREAT HUNTING FRAMEWORK</h4><div class="bar">[██████░░░░] BUILDING</div><p>Agentic hunting mapped to MITRE ATT&CK, IOC enrichment via VirusTotal, auto-ranked behavior.</p><a href="#">▸ ACCESS OP</a></div>
    <div class="op"><span class="tag">T1→T3</span><h4>30-DAY SOC CHALLENGE</h4><div class="bar">[█████░░░░░] BUILDING</div><p>Daily investigations + write-ups documenting the full Tier 1 → Tier 3 journey, reproducible.</p><a href="#">▸ ACCESS OP</a></div>
  </div>

  <h2><span class="br">┌──[</span> ./credentials <span class="br">]</span></h2>
  <div class="arsenal">
    <img src="https://img.shields.io/badge/SC--200-IN_PROGRESS-0a1929?style=for-the-badge&logo=microsoft&logoColor=ffb703"/>
    <img src="https://img.shields.io/badge/CySA%2B-PLANNED-0a1929?style=for-the-badge&logo=comptia&logoColor=64ffda"/>
    <img src="https://img.shields.io/badge/PG_DIPLOMA-CYBERSECURITY-0a1929?style=for-the-badge&logoColor=00b4d8"/>
    <img src="https://img.shields.io/badge/B.E.-COMPUTER_ENGINEERING-0a1929?style=for-the-badge&logoColor=64ffda"/>
  </div>
  <blockquote>◉ next target: Microsoft <b>SC-200</b> // Aug 2026 &nbsp;&nbsp; ◉ tier path: T1 Security+/CySA+ → T2 GCIH → T3 GCFA/OSCP</blockquote>

  <h2><span class="br">┌──[</span> ./telemetry <span class="br">]</span></h2>
  <div class="stats">
    <div class="stat"><div class="t">◉ HR'S GITHUB STATS</div><div class="r"><span>⭐ Total Stars</span><span>—</span></div><div class="r"><span>📝 Commits</span><span>—</span></div><div class="r"><span>🔀 PRs</span><span>—</span></div><div class="r"><span>Rank</span><span style="color:var(--ok)">A+</span></div></div>
    <div class="stat"><div class="t">🔥 STREAK</div><div class="r"><span>Current</span><span>— days</span></div><div class="r"><span>Longest</span><span>— days</span></div><div class="r"><span>Contributions</span><span>—</span></div></div>
  </div>
  <div class="ghost">↑ auto-fills with real numbers + trophies + a snake animation that eats your contribution graph, once your username is in the file.</div>

  <div class="footer">
    <div class="q">"I think like an attacker to defend like a professional."</div>
    <div class="q">// calm mind, relentless hunt</div>
  </div>
</div>
<script>
  const lines=["> initializing threat console...","> SOC Tier 1 → 2 → 3 | full-spectrum defense","> AI-augmented | agentic | 2026-ready","> calm mind | relentless hunt"];
  let li=0,ci=0;const el=document.getElementById('tw');
  function tick(){const l=lines[li];el.textContent=l.slice(0,ci);
    if(ci<l.length){ci++;setTimeout(tick,36);}else{setTimeout(()=>{ci=0;li=(li+1)%lines.length;tick();},1400);}}
  tick();
</script>
</body>
</html>
