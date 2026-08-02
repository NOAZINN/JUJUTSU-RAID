# JUJUTSU-RAID
Uma boss raid de jjk
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Jujutsu Raid - CE Recharge & Domain Clash Edition</title>
  
  <style>
    * {
      box-sizing: border-box;
    }
    body { 
      background: #000; 
      color: #fff; 
      font-family: 'Courier New', monospace; 
      display: flex; 
      justify-content: center; 
      align-items: center; 
      min-height: 100vh; 
      margin: 0; 
      padding: 10px;
      overflow: hidden; 
      position: relative;
    }

    /* CANVAS DE FUNDO ANIMADO */
    #bg-canvas {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      z-index: 1;
      background: #050508;
    }

    .bg-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      background: radial-gradient(circle, rgba(0,0,0,0.2) 0%, rgba(0,0,0,0.85) 100%);
      pointer-events: none;
      z-index: 2;
    }

    /* CONTAINER DO JOGO */
    #game-container { 
      position: relative;
      z-index: 10;
      width: 100%;
      max-width: 480px; 
      background: rgba(10, 10, 12, 0.88); 
      backdrop-filter: blur(10px);
      border: 2px solid rgba(255, 0, 68, 0.6); 
      padding: 25px; 
      text-align: center; 
      box-shadow: 0 0 35px rgba(255, 0, 70, 0.4); 
      border-radius: 8px;
    }

    .menu-title {
      font-size: 38px;
      font-weight: 900;
      color: #ff2a55;
      text-transform: uppercase;
      letter-spacing: 3px;
      text-shadow: 0 0 15px rgba(255, 42, 85, 0.8), 3px 3px 0px #000;
      margin-bottom: 30px;
      font-family: 'Impact', 'Arial Black', sans-serif;
    }

    .video-style-menu {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      padding-left: 20px;
      gap: 12px;
    }

    .menu-option {
      font-family: 'Impact', 'Arial Black', sans-serif;
      font-size: 24px;
      letter-spacing: 2px;
      color: #ff9900;
      text-shadow: 2px 2px 0px #000;
      cursor: pointer;
      background: none;
      border: none;
      text-align: left;
      transition: all 0.2s ease;
      padding: 5px 10px;
    }

    .menu-option:hover {
      color: #ffffff;
      transform: translateX(10px) scale(1.05);
      text-shadow: 0 0 10px #ff9900, 2px 2px 0px #000;
    }

    .menu-option.xtreme { color: #ff0044; }

    .bar-bg { 
      width: 100%; 
      height: 20px; 
      background: #1a1a1a; 
      border: 1px solid #444; 
      margin: 6px 0; 
      position: relative;
      border-radius: 3px;
      overflow: hidden;
    }
    #enemy-hp-fill { height: 100%; background: linear-gradient(to right, #800, #f04); width: 100%; transition: width 0.3s ease-out; }
    #hp-fill { height: 100%; background: #0f4; width: 100%; transition: width 0.3s; }
    #ce-fill { height: 100%; background: #08f; width: 100%; transition: width 0.3s; }
    #awake-fill { height: 100%; background: #0ff; width: 0%; transition: width 0.3s; box-shadow: 0 0 10px #0ff; }
    
    /* INTERFACE DO DOMAIN CLASH */
    #clash-bar-fill {
      height: 100%;
      background: linear-gradient(90deg, #ff0044, #ff9900, #00ffff);
      width: 50%;
      transition: width 0.1s ease-out;
    }

    .clash-btn {
      background: #ff0044;
      color: #fff;
      font-size: 18px;
      font-weight: 900;
      border: 3px solid #fff;
      padding: 15px 30px;
      cursor: pointer;
      width: 100%;
      margin-top: 15px;
      border-radius: 6px;
      box-shadow: 0 0 20px #ff0044;
      font-family: 'Impact', sans-serif;
      letter-spacing: 2px;
      animation: pulse 0.6s infinite alternate;
    }

    @keyframes pulse {
      0% { transform: scale(0.98); box-shadow: 0 0 15px #ff0044; }
      100% { transform: scale(1.03); box-shadow: 0 0 30px #ff0044, 0 0 10px #fff; }
    }

    .ut-btn { 
      background: rgba(0,0,0,0.7); 
      color: #f90; 
      border: 2px solid #f90; 
      padding: 10px 10px; 
      cursor: pointer; 
      margin: 3px; 
      font-weight: bold; 
      min-width: 80px; 
      font-family: inherit;
      font-size: 12px;
      border-radius: 4px;
      transition: all 0.2s;
    }
    .ut-btn:hover { background: #f90; color: #000; box-shadow: 0 0 10px #f90; }
    
    #msg-box { 
      border: 2px solid #fff; 
      min-height: 100px; 
      margin: 15px 0; 
      padding: 12px; 
      font-size: 13px; 
      text-align: left; 
      background: rgba(0, 0, 0, 0.9); 
      overflow-y: auto;
      max-height: 140px;
      line-height: 1.4;
      border-radius: 4px;
    }
    .skill-btn { 
      background: none; 
      border: none; 
      color: #fff; 
      cursor: pointer; 
      display: block; 
      text-align: left; 
      width: 100%; 
      padding: 6px 4px; 
      font-family: inherit; 
      font-size: 13px;
    }
    .skill-btn:hover { color: #0ff; background: #141414; }

    .stat-text {
      position: absolute;
      width: 100%;
      left: 0;
      top: 0;
      text-align: center;
      font-size: 12px;
      line-height: 20px;
      color: #fff;
      font-weight: bold;
      text-shadow: 1px 1px 2px #000;
    }
    .glitch { animation: shake 0.2s infinite; color: #555 !important; border-color: #333 !important; }
    @keyframes shake { 0% { transform: translate(1px, 1px); } 100% { transform: translate(-1px, -1px); } }
  </style>
</head>
<body>

<canvas id="bg-canvas"></canvas>
<div class="bg-overlay"></div>

<div id="game-container">
  <!-- TELA 1: MENU PRINCIPAL -->
  <div id="menu-diff">
    <div class="menu-title">JUJUTSU<br>RAID</div>
    <div class="video-style-menu">
      <button class="menu-option" onclick="setDiff('NORMAL')">NORMAL</button>
      <button class="menu-option xtreme" onclick="setDiff('EXTREMO')">XTREME</button>
      <button class="menu-option" style="color: #aaa;" onclick="alert('Recarrega a tua Energia Amaldiçoada (CE) com o botão RECARREGAR CE durante o combate!')">TUTORIALS</button>
    </div>
  </div>

  <!-- TELA 2: SELEÇÃO DE PERSONAGEM -->
  <div id="menu-char" style="display:none">
    <h2 style="font-size: 18px; margin-bottom: 15px; font-family: 'Impact', sans-serif; color: #ff9900;">SELECIONE O FEITICEIRO</h2>
    <div id="extremo-warning" style="color:#f04; font-size:12px; display:none; font-weight:bold; margin-bottom:10px;">MODO EXTREMO: GOJO BLOQUEADO</div>
    <div style="display:flex; flex-direction:row; justify-content:center; flex-wrap:wrap; gap:5px;">
      <button class="ut-btn" onclick="start('ITADORI')">ITADORI</button>
      <button id="gojo-btn" class="ut-btn" onclick="gojoClick()">GOJO</button>
      <button class="ut-btn" onclick="start('HAKARI')" style="color:#f0f; border-color:#f0f">HAKARI</button>
      <button class="ut-btn" onclick="start('MEGUMI')" style="color:#0f9; border-color:#0f9">MEGUMI</button>
    </div>
  </div>

  <!-- TELA DE DOMAIN CLASH -->
  <div id="domain-clash-screen" style="display:none;">
    <h2 style="color:#ff0044; font-family:'Impact', sans-serif; font-size:26px; margin:0 0 10px 0; text-shadow:0 0 10px #ff0044;">💥 DOMAIN CLASH 💥</h2>
    <div style="font-size:12px; color:#aaa; margin-bottom:15px;">PRESSIONA O BOTÃO RAPIDAMENTE PARA SOBREPOR O DOMÍNIO!</div>
    
    <div class="bar-bg" style="height:30px; border:2px solid #ff0044;">
      <div id="clash-bar-fill"></div>
      <div id="clash-timer-text" class="stat-text" style="line-height:30px; font-size:14px;">5.0s</div>
    </div>

    <button class="clash-btn" onclick="clashTap()">PRESSIONA / CLICA AQUI!</button>
  </div>

  <!-- TELA 3: BATALHA -->
  <div id="battle" style="display:none">
    <div id="boss-name" style="color:#f04; font-weight:bold; font-size: 16px;">BOSS</div>
    <div class="bar-bg">
      <div id="enemy-hp-fill"></div>
      <div id="enemy-hp-text" class="stat-text">300/300</div>
    </div>
    
    <div id="msg-box">
      <div id="text-display">PREPARE-SE PARA A BATALHA!</div>
      <div id="skill-menu" style="display:none; grid-template-columns: 1fr; gap: 4px;"></div>
    </div>

    <div style="text-align:left; font-size:13px">
      <span id="p-name" style="color:#0ff; font-weight:bold">PLAYER</span> <br>
      HP 
      <div class="bar-bg">
        <div id="hp-fill"></div>
        <div id="hp-text" class="stat-text">100/100</div>
      </div>
      CE 
      <div class="bar-bg">
        <div id="ce-fill"></div>
        <div id="ce-text" class="stat-text">50/50</div>
      </div>
      AWK 
      <div class="bar-bg">
        <div id="awake-fill"></div>
        <div id="awk-text" class="stat-text">0%</div>
      </div>
    </div>

    <!-- BOTÕES DE AÇÃO COM RECARREGAR CE INCLUÍDO -->
    <div style="margin-top:15px; display:flex; justify-content:center; gap:4px; flex-wrap:wrap;">
      <button class="ut-btn" onclick="openSkills('fisico')">LUTAR</button>
      <button class="ut-btn" onclick="openSkills('tecnica')">TÉCNICAS</button>
      <button class="ut-btn" onclick="rechargeCE()" style="color:#08f; border-color:#08f">CARREGAR CE</button>
      <button class="ut-btn" onclick="fullReset()" style="color:#888; border-color:#888">RESET</button>
    </div>
  </div>
</div>

<script>
  /* ==========================================
     CANVAS DE FUNDO
     ========================================== */
  const canvas = document.getElementById('bg-canvas');
  const ctx = canvas.getContext('2d');

  function resizeCanvas() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
  }
  window.addEventListener('resize', resizeCanvas);
  resizeCanvas();

  const particles = [];
  for (let i = 0; i < 60; i++) {
    particles.push({
      x: Math.random() * canvas.width,
      y: Math.random() * canvas.height,
      size: Math.random() * 3 + 1,
      speedY: -Math.random() * 1.8 - 0.4,
      speedX: (Math.random() - 0.5) * 0.8,
      color: Math.random() > 0.4 ? 'rgba(255, 0, 68, ' : 'rgba(0, 255, 255, ',
      opacity: Math.random() * 0.7 + 0.3
    });
  }

  function animateBG() {
    ctx.fillStyle = 'rgba(5, 5, 8, 0.25)';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    particles.forEach(p => {
      p.y += p.speedY;
      p.x += p.speedX;

      if (p.y < 0) {
        p.y = canvas.height;
        p.x = Math.random() * canvas.width;
      }

      ctx.beginPath();
      ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
      ctx.fillStyle = p.color + p.opacity + ')';
      ctx.shadowBlur = 12;
      ctx.shadowColor = p.color + '1)';
      ctx.fill();
      ctx.shadowBlur = 0;
    });

    requestAnimationFrame(animateBG);
  }

  animateBG();

  /* ==========================================
     LÓGICA DO JOGO
     ========================================== */
  let hp, ce, awk, bHp, bMax, modo, pName, jackpot, turno, phase, bossStunned;
  let clashPower = 50;
  let clashInterval = null;
  let clashTimeLeft = 5.0;
  let activeDomainSkill = "";

  function setDiff(d) {
    modo = d;
    document.getElementById('menu-diff').style.display = 'none';
    document.getElementById('menu-char').style.display = 'block';
    
    const gBtn = document.getElementById('gojo-btn');
    const warning = document.getElementById('extremo-warning');
    
    if(modo === 'EXTREMO') { 
      gBtn.classList.add('glitch'); 
      warning.style.display = 'block';
    } else { 
      gBtn.classList.remove('glitch'); 
      warning.style.display = 'none';
    }
  }

  function gojoClick() {
    if(modo === 'EXTREMO') { alert("GOJO ESTÁ BLOQUEADO NO MODO EXTREMO!"); } 
    else { start('GOJO'); }
  }

  function start(char) {
    pName = char; jackpot = false; turno = 0; hp = 100; ce = 50; awk = 0; phase = 1; bossStunned = false;
    bMax = (modo === 'EXTREMO') ? 800 : 300; bHp = bMax;

    document.getElementById('menu-char').style.display = 'none';
    document.getElementById('battle').style.display = 'block';
    document.getElementById('boss-name').innerText = (modo === 'EXTREMO') ? "🧟 GOJO ZUMBI" : "🧠 KENJAKU";
    
    updatePlayerStyle();
    update();
  }

  function updatePlayerStyle() {
    const pElement = document.getElementById('p-name');
    pElement.innerText = pName;
    if (pName === 'MEGUNA') pElement.style.color = "#ff0044";
    else if (pName === 'MEGUMI') pElement.style.color = "#0f9";
    else if (pName === 'HAKARI') pElement.style.color = "#f0f";
    else pElement.style.color = "#0ff";
  }

  function fullReset() {
    if(clashInterval) clearInterval(clashInterval);
    jackpot = false; bossStunned = false;
    document.getElementById('domain-clash-screen').style.display = 'none';
    document.getElementById('battle').style.display = 'none';
    document.getElementById('menu-char').style.display = 'none';
    document.getElementById('menu-diff').style.display = 'block';
    document.getElementById('text-display').innerText = "PREPARE-SE PARA A BATALHA!";
  }

  /* NOVA AÇÃO: RECARREGAR CE */
  function rechargeCE() {
    if (jackpot) {
      document.getElementById('text-display').innerText = "✨ CE já está infinito devido ao Jackpot!";
      return;
    }
    if (ce >= 50) {
      document.getElementById('text-display').innerText = "🔵 A tua Energia Amaldiçoada (CE) já está no máximo!";
      return;
    }

    document.getElementById('skill-menu').style.display = 'none';
    document.getElementById('text-display').style.display = 'block';

    let recoveredCE = 25;
    ce = Math.min(50, ce + recoveredCE);
    document.getElementById('text-display').innerText = "🌀 Focaste a tua mente e recarregaste +25 de Energia Amaldiçoada (CE)!";
    
    // Passar turno para o inimigo atacar após recarregar
    update();
    setTimeout(enemy, 600);
  }

  function openSkills(tipo) {
    const m = document.getElementById('skill-menu'); 
    const t = document.getElementById('text-display');
    t.style.display = 'none'; m.style.display = 'grid'; m.innerHTML = '';
    
    let s = [];

    if(pName === 'MEGUNA') {
      if(tipo === 'fisico') s = ["Corte Desmantelar", "Corte Cleave"];
      else s = ["Flesh Reversal (20CE)"];
    } else if(pName === 'MEGUMI') {
      if(tipo === 'fisico') s = ["Soco de Sombra", "Ataque do Cão Divino"];
      else s = ["Energia Reversa (20CE)"];
    } else {
      if(tipo === 'fisico') s = ["Soco", "Chute"];
      else s = ["Energia Reversa (20CE)"];
    }
    
    if(awk >= 100) {
      if(pName === 'HAKARI') s.push("EXPANSÃO: IDLE DEATH GAMBLE");
      if(pName === 'GOJO') s.push("EXPANSÃO: MUGEN");
      if(pName === 'ITADORI') s.push("DESPERTAR: BLACK FLASH");
      if(pName === 'MEGUMI') {
        if(modo === 'EXTREMO') {
          s.push("TRANSFORMAÇÃO: REINO DO MEGUNA");
        } else {
          s.push("EXPANSÃO: JARDIM DAS SOMBRAS");
        }
      }
      if(pName === 'MEGUNA') {
        s.push("EXPANSÃO: SANTUÁRIO MALEVOLENTE");
      }
    }
    if(jackpot) s.push("KOKU-SEN (JACKPOT)");
    
    s.forEach(skill => {
      let b = document.createElement('button'); 
      b.className = 'skill-btn'; 
      b.innerText = '* ' + skill;
      b.onclick = () => usar(skill); 
      m.appendChild(b);
    });
  }

  /* SISTEMA DOMAIN CLASH */
  function startDomainClash(skillName) {
    activeDomainSkill = skillName;
    document.getElementById('battle').style.display = 'none';
    document.getElementById('domain-clash-screen').style.display = 'block';
    
    clashPower = 50;
    clashTimeLeft = 5.0;
    document.getElementById('clash-bar-fill').style.width = '50%';
    document.getElementById('clash-timer-text').innerText = '5.0s';

    clashInterval = setInterval(() => {
      clashTimeLeft -= 0.1;
      clashPower -= (modo === 'EXTREMO') ? 2.5 : 1.8;
      if (clashPower < 0) clashPower = 0;
      if (clashPower > 100) clashPower = 100;

      document.getElementById('clash-bar-fill').style.width = clashPower + '%';
      document.getElementById('clash-timer-text').innerText = Math.max(0, clashTimeLeft).toFixed(1) + 's';

      if (clashTimeLeft <= 0 || clashPower <= 0 || clashPower >= 100) {
        endDomainClash();
      }
    }, 100);
  }

  function clashTap() {
    clashPower += 6;
    if (clashPower > 100) clashPower = 100;
    document.getElementById('clash-bar-fill').style.width = clashPower + '%';
  }

  function endDomainClash() {
    clearInterval(clashInterval);
    document.getElementById('domain-clash-screen').style.display = 'none';
    document.getElementById('battle').style.display = 'block';
    const t = document.getElementById('text-display');

    if (clashPower >= 50) {
      awk = 0;
      let d = 0;
      if (activeDomainSkill === "EXPANSÃO: SANTUÁRIO MALEVOLENTE") d = 260;
      else if (activeDomainSkill === "EXPANSÃO: MUGEN") { d = 180; bossStunned = true; }
      else if (activeDomainSkill === "EXPANSÃO: JARDIM DAS SOMBRAS") { d = 160; bossStunned = true; }
      
      bHp -= d;
      t.innerHTML = "🔥 <strong>VENCESTE O DOMAIN CLASH!</strong><br>" + activeDomainSkill + " dominou o inimigo causando " + d + " de dano!";
    } else {
      awk = 0;
      let dmg = (modo === 'EXTREMO') ? 45 : 30;
      hp -= dmg;
      t.innerHTML = "💀 <strong>PERDESTE O DOMAIN CLASH!</strong><br>O Domínio do Chefe destruiu a tua barreira e sofreste " + dmg + " de dano!";
    }

    checkPostBattleState();
  }

  function usar(s) {
    const t = document.getElementById('text-display'); 
    document.getElementById('skill-menu').style.display = 'none'; 
    t.style.display = 'block';
    
    if(s === "EXPANSÃO: SANTUÁRIO MALEVOLENTE" || s === "EXPANSÃO: MUGEN" || s === "EXPANSÃO: JARDIM DAS SOMBRAS") {
      startDomainClash(s);
      return;
    }

    if(s === "TRANSFORMAÇÃO: REINO DO MEGUNA") {
      awk = 0;
      pName = 'MEGUNA';
      hp = 100;
      ce = 50;
      updatePlayerStyle();
      t.innerHTML = "😈 <strong>SUKUNA ASSUMIU O CORPO DE MEGUMI PERMANENTEMENTE!</strong><br>Vida/Energia restauradas e novas habilidades de destruição ativadas!";
    }
    else if(s === "EXPANSÃO: IDLE DEATH GAMBLE") {
      if(Math.random() > 0.3) { 
        jackpot = true; turno = 5; awk = 0; 
        t.innerText = "🎰 JACKPOT!!! HP/CE INFINITOS POR 5 TURNOS!"; 
        document.getElementById('p-name').style.color = "#f0f";
      } else { t.innerText = "A Expansão do Hakari falhou! AWK resetado."; awk = 0; }
    } 
    else if(s === "DESPERTAR: BLACK FLASH") {
      awk = 0; bHp -= 140; hp = Math.min(100, hp + 30);
      t.innerText = "⚡ BLACK FLASH SEQUENCIAL! Causou 140 de dano e recuperou 30 HP!";
    }
    else if(s === "Energia Reversa (20CE)" || s === "Flesh Reversal (20CE)") {
      if(ce >= 20 || jackpot) { 
        hp = Math.min(100, hp + 50); if(!jackpot) ce -= 20; 
        t.innerText = "HP Recuperado (+50)! (-20 CE)"; 
      } else { t.innerText = "Sem Energia Amaldiçoada (CE) suficiente!"; update(); return; }
    } 
    else {
      let d = 35;
      if(s === "Corte Cleave") d = 60;
      else if(s === "Corte Desmantelar") d = 45;
      else if(s === "Ataque do Cão Divino") d = 40;
      else if(s === "KOKU-SEN (JACKPOT)") d = 180;
      else if(jackpot) d = 75;

      bHp -= d; 
      t.innerText = "Usou " + s + "! Causou " + d + " de dano!";
      if(!jackpot) awk = Math.min(100, awk + 25);
    }

    checkPostBattleState();
  }

  function checkPostBattleState() {
    const t = document.getElementById('text-display');
    if (bHp <= 0 && phase === 1 && modo !== 'EXTREMO') {
      phase = 2; bMax = 600; bHp = bMax;
      document.getElementById('boss-name').innerText = "👺 SUKUNA";
      t.innerHTML += "<br><strong style='color:red;'>KENJAKU DERROTADO! SUKUNA SURGIU COM 600 HP!</strong>";
      update(); return;
    } else if (bHp <= 0) {
      bHp = 0; t.innerHTML = "✨ <strong>VITÓRIA FINAL!!!</strong> ✨"; update(); return;
    }

    if(jackpot) { 
      hp = 100; ce = 50; turno--; 
      if(turno <= 0){ 
        jackpot = false; 
        updatePlayerStyle();
        t.innerHTML += "<br><small style='color:#f90;'>O efeito do Jackpot terminou!</small>";
      } 
    }
    
    update(); 
    if(bHp > 0) setTimeout(enemy, 600);
  }

  function enemy() { 
    if(bHp <= 0) return; 
    const t = document.getElementById('text-display');

    if(bossStunned) {
      bossStunned = false;
      t.innerHTML += "<br><span style='color:#0ff;'>O Chefe está paralisado!</span>";
      return;
    }

    if(jackpot) {
      t.innerHTML += "<br><span style='color:#f0f;'>O Jackpot do Hakari anulou o ataque!</span>";
      return;
    }

    let damage = (modo === 'EXTREMO') ? 25 : 15;
    hp -= damage; update(); 
    t.innerHTML += "<br><span style='color:red;'>O Chefe causou " + damage + " de dano!</span>";
    
    if(hp <= 0){ hp = 0; update(); alert("VOCÊ FOI DERROTADO!"); fullReset(); } 
  }

  function update() {
    document.getElementById('enemy-hp-fill').style.width = Math.max(0, (bHp/bMax*100)) + '%';
    document.getElementById('enemy-hp-text').innerText = Math.max(0, bHp) + '/' + bMax;
    document.getElementById('hp-fill').style.width = hp + '%';
    document.getElementById('hp-text').innerText = hp + '/100';
    document.getElementById('ce-fill').style.width = (ce/50*100) + '%';
    document.getElementById('ce-text').innerText = ce + '/50';
    document.getElementById('awake-fill').style.width = awk + '%';
    document.getElementById('awk-text').innerText = awk + '%';
  }
</script>

</body>
</html>
