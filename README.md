# F1
кликер ф1 пилотов
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>F1 Clicker — Чиби Гонщики</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: 'Comic Sans MS', 'Trebuchet MS', sans-serif;
    background: linear-gradient(135deg, #0b1e3f 0%, #1a3a6e 50%, #2d5aa0 100%);
    color: #e0f2ff;
    min-height: 100vh;
    overflow-x: hidden;
    padding: 20px;
  }
  h1 {
    text-align: center;
    color: #a8d8ff;
    text-shadow: 0 0 15px #4a90e2, 0 2px 4px #000;
    font-size: 2.2em;
    margin-bottom: 10px;
    letter-spacing: 2px;
  }
  .counter {
    text-align: center;
    font-size: 1.8em;
    color: #ffd86b;
    text-shadow: 0 0 10px #ffb300, 0 2px 3px #000;
    margin-bottom: 20px;
  }
  .counter .coin-icon {
    display: inline-block;
    width: 30px; height: 30px;
    background: radial-gradient(circle at 30% 30%, #fff3a0, #ffd86b 50%, #c99100);
    border-radius: 50%;
    border: 2px solid #8a6500;
    vertical-align: middle;
    margin-right: 8px;
    box-shadow: 0 0 12px #ffd86b;
  }
  .layout {
    display: grid;
    grid-template-columns: 1fr 340px;
    gap: 20px;
    max-width: 1200px;
    margin: 0 auto;
  }
  @media (max-width: 900px) {
    .layout { grid-template-columns: 1fr; }
  }
  .click-area {
    background: linear-gradient(180deg, #1e3c72 0%, #2a5298 100%);
    border: 3px solid #5aa9ff;
    border-radius: 20px;
    padding: 20px;
    box-shadow: 0 0 30px rgba(90, 169, 255, 0.4), inset 0 0 40px rgba(0,0,0,0.3);
    text-align: center;
    position: relative;
    overflow: hidden;
    min-height: 520px;
  }
  .current-name {
    font-size: 1.6em;
    color: #a8d8ff;
    margin-bottom: 6px;
    text-shadow: 0 0 8px #4a90e2;
  }
  .per-click {
    font-size: 1em;
    color: #ffd86b;
    margin-bottom: 12px;
  }
  .scene {
    width: 100%;
    max-width: 420px;
    aspect-ratio: 1 / 1;
    margin: 0 auto;
    cursor: pointer;
    transition: transform 0.1s;
    user-select: none;
    border-radius: 16px;
    overflow: hidden;
    box-shadow: 0 10px 30px rgba(0,0,0,0.5);
  }
  .scene:active { transform: scale(0.97); }
  .scene.bounce { animation: bounce 0.3s; }
  @keyframes bounce {
    0% { transform: scale(1); }
    50% { transform: scale(1.05) translateY(-8px); }
    100% { transform: scale(1); }
  }
  .float-coin {
    position: absolute;
    pointer-events: none;
    font-size: 1.4em;
    font-weight: bold;
    color: #ffd86b;
    text-shadow: 0 0 8px #ffb300, 0 2px 2px #000;
    animation: floatUp 1s ease-out forwards;
  }
  @keyframes floatUp {
    0% { opacity: 1; transform: translateY(0) scale(1); }
    100% { opacity: 0; transform: translateY(-120px) scale(1.5); }
  }
  .shop {
    background: linear-gradient(180deg, #152a52 0%, #1e3c72 100%);
    border: 3px solid #5aa9ff;
    border-radius: 20px;
    padding: 20px;
    box-shadow: 0 0 25px rgba(90, 169, 255, 0.3);
  }
  .shop h2 {
    text-align: center;
    color: #a8d8ff;
    margin-bottom: 15px;
    text-shadow: 0 0 8px #4a90e2;
  }
  .driver-card {
    background: linear-gradient(135deg, #2a4a80, #1a3460);
    border: 2px solid #4a90e2;
    border-radius: 12px;
    padding: 10px;
    margin-bottom: 10px;
    display: grid;
    grid-template-columns: 70px 1fr auto;
    gap: 10px;
    align-items: center;
    transition: all 0.2s;
  }
  .driver-card.active {
    border-color: #ffd86b;
    box-shadow: 0 0 15px rgba(255, 216, 107, 0.6);
    background: linear-gradient(135deg, #3a5a90, #2a4470);
  }
  .driver-card.owned { border-color: #66d9a0; }
  .mini-scene {
    width: 70px; height: 70px;
    border-radius: 50%;
    overflow: hidden;
    border: 2px solid #5aa9ff;
    background: #0b1e3f;
  }
  .mini-scene svg { width: 100%; height: 100%; display: block; }
  .driver-info { font-size: 0.9em; }
  .driver-info .name { color: #e0f2ff; font-weight: bold; }
  .driver-info .stats { color: #a8d8ff; font-size: 0.85em; margin-top: 3px; }
  .driver-info .price { color: #ffd86b; font-size: 0.85em; }
  .buy-btn {
    background: linear-gradient(180deg, #4a90e2, #2a5aa0);
    color: white;
    border: 2px solid #a8d8ff;
    padding: 8px 12px;
    border-radius: 8px;
    cursor: pointer;
    font-family: inherit;
    font-weight: bold;
    font-size: 0.85em;
    transition: all 0.2s;
    white-space: nowrap;
  }
  .buy-btn:hover:not(:disabled) {
    background: linear-gradient(180deg, #5aa9ff, #3a6ab0);
    transform: translateY(-2px);
    box-shadow: 0 4px 10px rgba(90, 169, 255, 0.5);
  }
  .buy-btn:disabled {
    background: #333;
    border-color: #555;
    color: #888;
    cursor: not-allowed;
  }
  .buy-btn.select {
    background: linear-gradient(180deg, #66d9a0, #3a9a70);
    border-color: #a8ffcc;
  }
  .buy-btn.selected {
    background: linear-gradient(180deg, #ffd86b, #c99100);
    border-color: #fff3a0;
    color: #3a2a00;
  }
  .reset-btn {
    display: block;
    margin: 15px auto 0;
    background: transparent;
    border: 1px solid #5aa9ff;
    color: #a8d8ff;
    padding: 6px 14px;
    border-radius: 6px;
    cursor: pointer;
    font-family: inherit;
    font-size: 0.8em;
  }
  .reset-btn:hover { background: rgba(90, 169, 255, 0.15); }
</style>
</head>
<body>

<h1>🏁 F1 ЧИБИ КЛИКЕР 🏁</h1>
<div class="counter">
  <span class="coin-icon"></span>
  <span id="coinCount">0</span> монеток
</div>

<div class="layout">
  <div class="click-area" id="clickArea">
    <div class="current-name" id="currentName">Макс Ферстаппен</div>
    <div class="per-click" id="perClick">+1 монетка за клик</div>
    <div id="sceneContainer"></div>
  </div>

  <div class="shop">
    <h2>🛒 Магазин гонщиков</h2>
    <div id="shopList"></div>
    <button class="reset-btn" onclick="resetGame()">Сбросить прогресс</button>
  </div>
</div>

<script>
/* =========================================================
   ФОНОВАЯ СЦЕНА — у каждого гонщика свой уникальный фон
   ========================================================= */
function backgroundScene(id) {
  switch(id) {
    // Макс — ночная трасса с огнями (как в Бахрейне/Джидде)
    case 'verstappen': return `
      <!-- Небо ночное -->
      <rect x="0" y="0" width="400" height="260" fill="url(#skyVerst)"/>
      <defs>
        <linearGradient id="skyVerst" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#0a0a2a"/>
          <stop offset="60%" stop-color="#1a1a4a"/>
          <stop offset="100%" stop-color="#ff6b1a" stop-opacity="0.4"/>
        </linearGradient>
      </defs>
      <!-- Звёзды -->
      <circle cx="40" cy="40" r="1.5" fill="#fff"/>
      <circle cx="120" cy="60" r="1" fill="#fff"/>
      <circle cx="200" cy="30" r="1.5" fill="#fff"/>
      <circle cx="280" cy="50" r="1" fill="#fff"/>
      <circle cx="350" cy="70" r="1.5" fill="#fff"/>
      <circle cx="80" cy="90" r="1" fill="#fff"/>
      <circle cx="320" cy="110" r="1" fill="#fff"/>
      <!-- Трибуны на заднем плане -->
      <rect x="0" y="200" width="400" height="60" fill="#1a1a3a"/>
      <rect x="0" y="210" width="400" height="4" fill="#ff6b1a" opacity="0.6"/>
      <rect x="0" y="225" width="400" height="4" fill="#ff6b1a" opacity="0.4"/>
      <!-- Огни трибун -->
      ${[20,60,100,140,180,220,260,300,340,380].map(x => `<circle cx="${x}" cy="215" r="2" fill="#ffd86b" opacity="0.8"/>`).join('')}
      <!-- Асфальт -->
      <rect x="0" y="260" width="400" height="140" fill="#1a1a2a"/>
      <!-- Разметка трассы -->
      <rect x="0" y="340" width="400" height="4" fill="#fff" opacity="0.5"/>
      ${[30,110,190,270,350].map(x => `<rect x="${x}" y="370" width="40" height="6" fill="#fff" opacity="0.7"/>`).join('')}
      <!-- Красные отбойники -->
      <rect x="0" y="255" width="400" height="8" fill="#c02020"/>
      <rect x="0" y="255" width="400" height="2" fill="#ff4040"/>
    `;

    // Ландо — тропический пляж с пальмами (папайя/McLaren)
    case 'norris': return `
      <defs>
        <linearGradient id="skyNorris" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#ffb070"/>
          <stop offset="50%" stop-color="#ff8c42"/>
          <stop offset="100%" stop-color="#ffd86b"/>
        </linearGradient>
        <linearGradient id="seaNorris" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#4ab8d8"/>
          <stop offset="100%" stop-color="#2a7aa0"/>
        </linearGradient>
      </defs>
      <!-- Небо закат -->
      <rect x="0" y="0" width="400" height="200" fill="url(#skyNorris)"/>
      <!-- Солнце -->
      <circle cx="300" cy="130" r="40" fill="#fff3a0" opacity="0.9"/>
      <circle cx="300" cy="130" r="55" fill="#ffd86b" opacity="0.3"/>
      <!-- Облака -->
      <ellipse cx="80" cy="60" rx="35" ry="10" fill="#fff" opacity="0.7"/>
      <ellipse cx="220" cy="40" rx="45" ry="12" fill="#fff" opacity="0.6"/>
      <!-- Море -->
      <rect x="0" y="200" width="400" height="60" fill="url(#seaNorris)"/>
      <path d="M 0 220 Q 50 215 100 220 T 200 220 T 300 220 T 400 220" stroke="#fff" stroke-width="1.5" fill="none" opacity="0.5"/>
      <path d="M 0 240 Q 50 235 100 240 T 200 240 T 300 240 T 400 240" stroke="#fff" stroke-width="1.5" fill="none" opacity="0.4"/>
      <!-- Песок -->
      <rect x="0" y="260" width="400" height="140" fill="#f4d896"/>
      <rect x="0" y="260" width="400" height="4" fill="#e0c080"/>
      <!-- Пальма слева -->
      <rect x="30" y="180" width="8" height="120" fill="#6a4020"/>
      <path d="M 34 180 Q 10 160 0 170 Q 15 170 34 185 Z" fill="#2a8a3a"/>
      <path d="M 34 180 Q 60 155 75 165 Q 55 170 34 185 Z" fill="#3a9a4a"/>
      <path d="M 34 180 Q 20 145 15 155 Q 25 160 34 185 Z" fill="#2a7a2a"/>
      <path d="M 34 180 Q 50 140 60 150 Q 45 160 34 185 Z" fill="#3a8a3a"/>
      <!-- Пальма справа -->
      <rect x="360" y="190" width="8" height="110" fill="#6a4020"/>
      <path d="M 364 190 Q 340 170 330 180 Q 345 180 364 195 Z" fill="#2a8a3a"/>
      <path d="M 364 190 Q 390 165 400 175 Q 380 180 364 195 Z" fill="#3a9a4a"/>
      <path d="M 364 190 Q 350 155 345 165 Q 355 170 364 195 Z" fill="#2a7a2a"/>
    `;

    // Алонсо — испанские горы/Астурия
    case 'alonso': return `
      <defs>
        <linearGradient id="skyAlonso" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#5a9ad8"/>
          <stop offset="100%" stop-color="#a8d8ff"/>
        </linearGradient>
      </defs>
      <!-- Небо -->
      <rect x="0" y="0" width="400" height="220" fill="url(#skyAlonso)"/>
      <!-- Солнце -->
      <circle cx="80" cy="70" r="30" fill="#fff3a0"/>
      <circle cx="80" cy="70" r="45" fill="#ffd86b" opacity="0.3"/>
      <!-- Дальние горы -->
      <path d="M 0 200 L 60 120 L 120 170 L 180 100 L 250 160 L 320 110 L 400 180 L 400 220 L 0 220 Z" fill="#6a7a9a"/>
      <!-- Средние горы -->
      <path d="M 0 220 L 80 150 L 150 200 L 230 140 L 300 190 L 400 160 L 400 240 L 0 240 Z" fill="#4a5a7a"/>
      <!-- Снежные шапки -->
      <path d="M 60 120 L 70 135 L 50 135 Z" fill="#fff"/>
      <path d="M 180 100 L 195 120 L 165 120 Z" fill="#fff"/>
      <path d="M 320 110 L 335 130 L 305 130 Z" fill="#fff"/>
      <!-- Ближние холмы зелёные -->
      <path d="M 0 240 Q 100 210 200 235 Q 300 215 400 240 L 400 280 L 0 280 Z" fill="#4a8a3a"/>
      <!-- Земля -->
      <rect x="0" y="260" width="400" height="140" fill="#6a5030"/>
      <!-- Травка -->
      <rect x="0" y="260" width="400" height="10" fill="#5a9a3a"/>
      <!-- Испанский флаг маленький -->
      <rect x="340" y="200" width="30" height="20" fill="#c02020"/>
      <rect x="340" y="206" width="30" height="8" fill="#ffd86b"/>
      <rect x="338" y="195" width="2" height="30" fill="#3a2a1a"/>
    `;

    // Хэмилтон — неоновый ночной город (Лас-Вегас/Майами стиль)
    case 'hamilton': return `
      <defs>
        <linearGradient id="skyHam" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#1a0a3a"/>
          <stop offset="50%" stop-color="#3a1a6a"/>
          <stop offset="100%" stop-color="#6a2a9a"/>
        </linearGradient>
      </defs>
      <!-- Небо -->
      <rect x="0" y="0" width="400" height="260" fill="url(#skyHam)"/>
      <!-- Звёзды -->
      <circle cx="50" cy="30" r="1" fill="#fff"/>
      <circle cx="150" cy="50" r="1.5" fill="#fff"/>
      <circle cx="250" cy="25" r="1" fill="#fff"/>
      <circle cx="350" cy="45" r="1.5" fill="#fff"/>
      <!-- Неон-луна -->
      <circle cx="330" cy="80" r="25" fill="#c94aff" opacity="0.8"/>
      <circle cx="330" cy="80" r="35" fill="#c94aff" opacity="0.2"/>
      <!-- Силуэт города -->
      <rect x="0" y="180" width="40" height="80" fill="#1a0a2a"/>
      <rect x="40" y="150" width="50" height="110" fill="#2a1a4a"/>
      <rect x="90" y="170" width="35" height="90" fill="#1a0a2a"/>
      <rect x="125" y="130" width="55" height="130" fill="#3a1a5a"/>
      <rect x="180" y="160" width="40" height="100" fill="#2a1a4a"/>
      <rect x="220" y="140" width="50" height="120" fill="#1a0a2a"/>
      <rect x="270" y="170" width="35" height="90" fill="#3a1a5a"/>
      <rect x="305" y="150" width="45" height="110" fill="#2a1a4a"/>
      <rect x="350" y="180" width="50" height="80" fill="#1a0a2a"/>
      <!-- Неоновые окна -->
      ${[55,70,85].map(x => `<rect x="${x}" y="170" width="4" height="6" fill="#c94aff"/>`).join('')}
      ${[135,150,165].map(x => `<rect x="${x}" y="150" width="4" height="6" fill="#ff4a9a"/>`).join('')}
      ${[230,245,260].map(x => `<rect x="${x}" y="160" width="4" height="6" fill="#4affff"/>`).join('')}
      ${[315,330,345].map(x => `<rect x="${x}" y="170" width="4" height="6" fill="#c94aff"/>`).join('')}
      <!-- Неоновые вывески -->
      <rect x="130" y="135" width="45" height="10" fill="none" stroke="#ff4a9a" stroke-width="1.5"/>
      <text x="152" y="143" text-anchor="middle" font-size="8" fill="#ff4a9a" font-weight="bold">F1</text>
      <rect x="225" y="145" width="40" height="10" fill="none" stroke="#4affff" stroke-width="1.5"/>
      <!-- Асфальт -->
      <rect x="0" y="260" width="400" height="140" fill="#1a0a2a"/>
      <!-- Неоновая разметка -->
      <rect x="0" y="340" width="400" height="3" fill="#c94aff" opacity="0.8"/>
      ${[30,110,190,270,350].map(x => `<rect x="${x}" y="370" width="40" height="5" fill="#c94aff" opacity="0.9"/>`).join('')}
    `;

    // Сенна — Бразилия, Интерлагос, пальмы и жёлто-зелёный флаг
    case 'senna': return `
      <defs>
        <linearGradient id="skySenna" x1="0" y1="0" x2="0" y2="1">
          <stop offset="0%" stop-color="#4ab8e8"/>
          <stop offset="70%" stop-color="#8ad8f0"/>
          <stop offset="100%" stop-color="#c8e8a0"/>
        </linearGradient>
      </defs>
      <!-- Небо -->
      <rect x="0" y="0" width="400" height="240" fill="url(#skySenna)"/>
      <!-- Солнце яркое -->
      <circle cx="320" cy="80" r="35" fill="#fff3a0"/>
      <circle cx="320" cy="80" r="50" fill="#ffd86b" opacity="0.3"/>
      <!-- Облака -->
      <ellipse cx="70" cy="60" rx="40" ry="12" fill="#fff" opacity="0.8"/>
      <ellipse cx="180" cy="40" rx="50" ry="14" fill="#fff" opacity="0.7"/>
      <!-- Дальние холмы -->
      <path d="M 0 220 Q 100 180 200 210 Q 300 185 400 215 L 400 260 L 0 260 Z" fill="#6aaa4a"/>
      <!-- Земля/трасса -->
      <rect x="0" y="240" width="400" height="160" fill="#3a3a3a"/>
      <!-- Жёлто-зелёная полоса (отсылка к флагу Бразилии) -->
      <rect x="0" y="240" width="400" height="6" fill="#1a8a3a"/>
      <rect x="0" y="246" width="400" height="6" fill="#ffd81a"/>
      <!-- Разметка -->
      ${[30,110,190,270,350].map(x => `<rect x="${x}" y="350" width="40" height="6" fill="#fff" opacity="0.8"/>`).join('')}
      <!-- Бразильский флаг слева -->
      <rect x="20" y="170" width="3" height="80" fill="#5a3a1a"/>
      <rect x="23" y="175" width="40" height="28" fill="#1a8a3a"/>
      <path d="M 25 189 L 43 178 L 61 189 L 43 200 Z" fill="#ffd81a"/>
      <circle cx="43" cy="189" r="6" fill="#1a3a8a"/>
      <!-- Пальма справа -->
      <rect x="360" y="180" width="8" height="90" fill="#6a4020"/>
      <path d="M 364 180 Q 340 160 330 170 Q 345 170 364 185 Z" fill="#2a8a3a"/>
      <path d="M 364 180 Q 390 155 400 165 Q 380 170 364 185 Z" fill="#3a9a4a"/>
      <path d="M 364 180 Q 350 145 345 155 Q 355 160 364 185 Z" fill="#2a7a2a"/>
    `;
  }
  return '';
}

/* =========================================================
   ДЕТАЛИЗИРОВАННЫЙ ЧИБИ-ГОНЩИК
   ========================================================= */
function chibiSVG(d) {
  const c = d.colors;
  return `
<svg viewBox="0 0 400 400" xmlns="http://www.w3.org/2000/svg">
  <!-- ФОН -->
  ${backgroundScene(d.id)}

  <!-- ТЕНЬ ПОД ПЕРСОНАЖЕМ -->
  <ellipse cx="200" cy="375" rx="70" ry="10" fill="#000" opacity="0.35"/>

  <!-- ===== НОГИ ===== -->
  <rect x="168" y="310" width="26" height="55" rx="8" fill="${c.suitMain}" stroke="#000" stroke-width="2"/>
  <rect x="206" y="310" width="26" height="55" rx="8" fill="${c.suitMain}" stroke="#000" stroke-width="2"/>
  <!-- Полоски на штанах -->
  <rect x="168" y="335" width="26" height="4" fill="${c.suitAccent}"/>
  <rect x="206" y="335" width="26" height="4" fill="${c.suitAccent}"/>
  <!-- Ботинки -->
  <ellipse cx="181" cy="370" rx="18" ry="9" fill="#1a1a1a" stroke="#000" stroke-width="2"/>
  <ellipse cx="219" cy="370" rx="18" ry="9" fill="#1a1a1a" stroke="#000" stroke-width="2"/>
  <ellipse cx="181" cy="367" rx="14" ry="4" fill="${c.suitAccent}"/>
  <ellipse cx="219" cy="367" rx="14" ry="4" fill="${c.suitAccent}"/>

  <!-- ===== ТЕЛО (комбинезон) ===== -->
  <path d="M 150 220 Q 145 200 160 195 L 240 195 Q 255 200 250 220 L 255 315 Q 255 325 245 325 L 155 325 Q 145 325 145 315 Z"
        fill="${c.suitMain}" stroke="#000" stroke-width="2"/>
  <!-- Молния -->
  <line x1="200" y1="200" x2="200" y2="310" stroke="#000" stroke-width="1.5" opacity="0.5"/>
  <!-- Полосы на груди -->
  <rect x="150" y="235" width="100" height="6" fill="${c.suitAccent}"/>
  <rect x="150" y="250" width="100" height="3" fill="${c.suitAccent}"/>
  <!-- Логотип-кружок на груди -->
  <circle cx="200" cy="275" r="12" fill="#fff" stroke="#000" stroke-width="1.5"/>
  <text x="200" y="280" text-anchor="middle" font-size="14" font-weight="bold" fill="${c.suitMain}" font-family="Arial">${d.number}</text>
  <!-- Спонсорские пятна -->
  <rect x="160" y="295" width="30" height="8" rx="2" fill="${c.suitAccent}" opacity="0.8"/>
  <rect x="210" y="295" width="30" height="8" rx="2" fill="#fff" opacity="0.7"/>

  <!-- ===== РУКИ ===== -->
  <!-- Левая рука -->
  <path d="M 150 210 Q 125 215 120 250 Q 118 280 130 295 L 148 290 Q 140 275 142 250 Q 145 225 155 220 Z"
        fill="${c.suitMain}" stroke="#000" stroke-width="2"/>
  <!-- Правая рука (чуть согнута, будто приветствует) -->
  <path d="M 250 210 Q 275 205 285 230 Q 290 255 278 275 L 260 270 Q 268 255 265 235 Q 260 220 245 220 Z"
        fill="${c.suitMain}" stroke="#000" stroke-width="2"/>
  <!-- Полоски на рукавах -->
  <rect x="125" y="245" width="22" height="4" fill="${c.suitAccent}" transform="rotate(-5 136 247)"/>
  <rect x="262" y="240" width="22" height="4" fill="${c.suitAccent}" transform="rotate(10 273 242)"/>
  <!-- Перчатки -->
  <circle cx="133" cy="300" r="13" fill="${c.suitAccent}" stroke="#000" stroke-width="2"/>
  <circle cx="275" cy="280" r="13" fill="${c.suitAccent}" stroke="#000" stroke-width="2"/>
  <!-- Блики на перчатках -->
  <circle cx="129" cy="296" r="3" fill="#fff" opacity="0.5"/>
  <circle cx="271" cy="276" r="3" fill="#fff" opacity="0.5"/>

  <!-- ===== ШЕЯ ===== -->
  <rect x="185" y="180" width="30" height="22" fill="${c.skin}" stroke="#000" stroke-width="2"/>
  <!-- Тень под подбородком -->
  <ellipse cx="200" cy="195" rx="15" ry="4" fill="#000" opacity="0.2"/>

  <!-- ===== ВОЛОСЫ (сзади, выбиваются из-под шлема) ===== -->
  ${d.hairStyle}

  <!-- ===== ГОЛОВА (большая чиби) ===== -->
  <circle cx="200" cy="130" r="72" fill="${c.skin}" stroke="#000" stroke-width="2"/>

  <!-- ===== УШИ ===== -->
  <ellipse cx="130" cy="135" rx="8" ry="12" fill="${c.skin}" stroke="#000" stroke-width="2"/>
  <ellipse cx="130" cy="135" rx="3" ry="6" fill="#000" opacity="0.2"/>
  <ellipse cx="270" cy="135" rx="8" ry="12" fill="${c.skin}" stroke="#000" stroke-width="2"/>
  <ellipse cx="270" cy="135" rx="3" ry="6" fill="#000" opacity="0.2"/>

  <!-- ===== ВОЛОСЫ СПЕРЕДИ (чёлка из-под шлема) ===== -->
  ${d.hairFront}

  <!-- ===== ШЛЕМ ===== -->
  <!-- Основа шлема -->
  <path d="M 128 125 Q 128 55 200 52 Q 272 55 272 125 L 272 135 Q 272 142 265 142 L 135 142 Q 128 142 128 135 Z"
        fill="${c.helmetMain}" stroke="#000" stroke-width="2.5"/>
  <!-- Узор на шлеме (диагональные полосы) -->
  <path d="M 140 80 Q 200 60 260 80 L 260 95 Q 200 75 140 95 Z" fill="${c.helmetAccent}" opacity="0.9"/>
  <path d="M 145 100 Q 200 85 255 100 L 255 108 Q 200 93 145 108 Z" fill="${c.helmetAccent}" opacity="0.6"/>
  <!-- Блик на шлеме -->
  <ellipse cx="160" cy="75" rx="18" ry="8" fill="#fff" opacity="0.3"/>
  <!-- Козырёк -->
  <path d="M 132 135 L 268 135 L 262 148 L 138 148 Z" fill="#000" opacity="0.5"/>
  <!-- Визор -->
  <path d="M 142 105 Q 200 92 258 105 L 258 132 Q 200 122 142 132 Z"
        fill="${c.visor}" stroke="#000" stroke-width="2" opacity="0.88"/>
  <!-- Блик на визоре -->
  <path d="M 155 110 Q 180 106 205 110 L 202 118 Q 180 115 158 118 Z" fill="#fff" opacity="0.5"/>
  <!-- Номер на шлеме сверху -->
  <circle cx="200" cy="72" r="12" fill="#fff" stroke="#000" stroke-width="1.5"/>
  <text x="200" y="77" text-anchor="middle" font-size="15" font-weight="bold" fill="#000" font-family="Arial">${d.number}</text>

  <!-- ===== ЛИЦО (видно под визором — нос, щёки, рот) ===== -->
  <!-- Нос -->
  <path d="M 198 125 Q 200 132 202 125" stroke="#000" stroke-width="1.5" fill="none" opacity="0.6"/>
  <!-- Рот (улыбка) -->
  <path d="M 190 140 Q 200 146 210 140" stroke="#000" stroke-width="2" fill="none" stroke-linecap="round"/>
  <!-- Румяные щёчки -->
  <circle cx="165" cy="138" r="7" fill="#ff8080" opacity="0.55"/>
  <circle cx="235" cy="138" r="7" fill="#ff8080" opacity="0.55"/>
  <!-- Блики на щёчках -->
  <circle cx="163" cy="136" r="2" fill="#fff" opacity="0.6"/>
  <circle cx="233" cy="136" r="2" fill="#fff" opacity="0.6"/>
</svg>`;
}

/* =========================================================
   ДАННЫЕ ГОНЩИКОВ
   ========================================================= */
const drivers = [
  {
    id: 'verstappen',
    name: 'Макс Ферстаппен',
    price: 0,
    perClick: 1,
    number: '1',
    colors: {
      helmetMain: '#1a2a6c', helmetAccent: '#ff6b1a', visor: '#2a4a8a',
      suitMain: '#1a2a6c', suitAccent: '#ff6b1a',
      skin: '#f4c9a0',      // светлая европейская
      hairColor: '#5a3a1a', // каштановые
    },
    // Волосы сзади — короткие каштановые пряди
    hairStyle: `
      <path d="M 135 140 Q 125 170 135 195 L 150 190 Q 142 170 148 145 Z" fill="#5a3a1a" stroke="#000" stroke-width="1.5"/>
      <path d="M 265 140 Q 275 170 265 195 L 250 190 Q 258 170 252 145 Z" fill="#5a3a1a" stroke="#000" stroke-width="1.5"/>
      <path d="M 145 150 Q 138 175 145 195 L 155 190 Q 150 175 152 155 Z" fill="#4a2a10" stroke="#000" stroke-width="1"/>
      <path d="M 255 150 Q 262 175 255 195 L 245 190 Q 250 175 248 155 Z" fill="#4a2a10" stroke="#000" stroke-width="1"/>
    `,
    // Чёлка — короткая, аккуратно торчит
    hairFront: `
      <path d="M 140 90 Q 150 100 160 95 L 165 105 Q 155 108 145 100 Z" fill="#5a3a1a" stroke="#000" stroke-width="1.5"/>
      <path d="M 240 90 Q 250 100 260 95 L 255 105 Q 245 108 235 100 Z" fill="#5a3a1a" stroke="#000" stroke-width="1.5"/>
    `
  },
  {
    id: 'norris',
    name: 'Ландо Норрис',
    price: 50,
    perClick: 1.5,
    number: '4',
    colors: {
      helmetMain: '#ff8c1a', helmetAccent: '#1a1a1a', visor: '#3a5a8a',
      suitMain: '#ff8c1a', suitAccent: '#1a1a1a',
      skin: '#f0c090',      // светлая
      hairColor: '#d4a050', // блонд
    },
    // Кудрявые светлые волосы, торчат из-под шлема
    hairStyle: `
      <circle cx="135" cy="160" r="10" fill="#d4a050" stroke="#000" stroke-width="1.5"/>
      <circle cx="145" cy="175" r="11" fill="#d4a050" stroke="#000" stroke-width="1.5"/>
      <circle cx="138" cy="190" r="10" fill="#c09040" stroke="#000" stroke-width="1.5"/>
      <circle cx="265" cy="160" r="10" fill="#d4a050" stroke="#000" stroke-width="1.5"/>
      <circle cx="255" cy="175" r="11" fill="#d4a050" stroke="#000" stroke-width="1.5"/>
      <circle cx="262" cy="190" r="10" fill="#c09040" stroke="#000" stroke-width="1.5"/>
      <circle cx="150" cy="185" r="8" fill="#e0b060" stroke="#000" stroke-width="1"/>
      <circle cx="250" cy="185" r="8" fill="#e0b060" stroke="#000" stroke-width="1"/>
    `,
    // Кудрявая чёлка блонда
    hairFront: `
      <circle cx="150" cy="95" r="8" fill="#d4a050" stroke="#000" stroke-width="1.5"/>
      <circle cx="165" cy="92" r="9" fill="#e0b060" stroke="#000" stroke-width="1.5"/>
      <circle cx="235" cy="92" r="9" fill="#e0b060" stroke="#000" stroke-width="1.5"/>
      <circle cx="250" cy="95" r="8" fill="#d4a050" stroke="#000" stroke-width="1.5"/>
      <circle cx="155" cy="105" r="6" fill="#c09040" stroke="#000" stroke-width="1"/>
      <circle cx="245" cy="105" r="6" fill="#c09040" stroke="#000" stroke-width="1"/>
    `
  },
  {
    id: 'alonso',
    name: 'Фернандо Алонсо',
    price: 100,
    perClick: 2,
    number: '14',
    colors: {
      helmetMain: '#0a5a4a', helmetAccent: '#ff3a8a', visor: '#2a4a6a',
      suitMain: '#0a5a4a', suitAccent: '#ff3a8a',
      skin: '#d8a878',      // смуглая испанская
      hairColor: '#3a2a1a', // тёмные с сединой
    },
    // Короткие тёмные волосы с проседью
    hairStyle: `
      <path d="M 135 140 Q 128 165 138 185 L 150 180 Q 142 165 148 145 Z" fill="#3a2a1a" stroke="#000" stroke-width="1.5"/>
      <path d="M 265 140 Q 272 165 262 185 L 250 180 Q 258 165 252 145 Z" fill="#3a2a1a" stroke="#000" stroke-width="1.5"/>
      <!-- Седые пряди -->
      <path d="M 140 155 Q 135 170 142 180" stroke="#c0c0c0" stroke-width="2" fill="none"/>
      <path d="M 260 155 Q 265 170 258 180" stroke="#c0c0c0" stroke-width="2" fill="none"/>
    `,
    // Аккуратная короткая чёлка
    hairFront: `
      <path d="M 145 92 Q 155 100 165 95 L 162 105 Q 152 108 145 100 Z" fill="#3a2a1a" stroke="#000" stroke-width="1.5"/>
      <path d="M 235 95 Q 245 100 255 92 L 255 100 Q 248 108 238 105 Z" fill="#3a2a1a" stroke="#000" stroke-width="1.5"/>
      <!-- Седая прядь -->
      <path d="M 150 95 Q 158 100 162 95" stroke="#c0c0c0" stroke-width="1.5" fill="none"/>
    `
  },
  {
    id: 'hamilton',
    name: 'Льюис Хэмилтон',
    price: 115,
    perClick: 10,
    number: '44',
    colors: {
      helmetMain: '#1a1a2a', helmetAccent: '#c94aff', visor: '#3a3a5a',
      suitMain: '#1a1a2a', suitAccent: '#c94aff',
      skin: '#5a3a2a',      // тёмная кожа (как и есть)
      hairColor: '#1a0a0a',
    },
    // Длинные косички/дреды, спадающие сзади
    hairStyle: `
      <!-- Дреды сзади -->
      <path d="M 140 140 Q 130 180 135 220 Q 138 240 145 245" stroke="#1a0a0a" stroke-width="8" fill="none" stroke-linecap="round"/>
      <path d="M 155 145 Q 148 185 152 225 Q 155 245 162 250" stroke="#1a0a0a" stroke-width="8" fill="none" stroke-linecap="round"/>
      <path d="M 260 140 Q 270 180 265 220 Q 262 240 255 245" stroke="#1a0a0a" stroke-width="8" fill="none" stroke-linecap="round"/>
      <path d="M 245 145 Q 252 185 248 225 Q 245 245 238 250" stroke="#1a0a0a" stroke-width="8" fill="none" stroke-linecap="round"/>
      <!-- Блики на дредах -->
      <path d="M 138 170 Q 136 190 138 210" stroke="#3a2a1a" stroke-width="2" fill="none"/>
      <path d="M 262 170 Q 264 190 262 210" stroke="#3a2a1a" stroke-width="2" fill="none"/>
    `,
    // Косички спереди, обрамляющие лицо
    hairFront: `
      <path d="M 145 95 Q 140 110 142 130" stroke="#1a0a0a" stroke-width="6" fill="none" stroke-linecap="round"/>
      <path d="M 155 92 Q 150 108 152 128" stroke="#1a0a0a" stroke-width="6" fill="none" stroke-linecap="round"/>
      <path d="M 255 95 Q 260 110 258 130" stroke="#1a0a0a" stroke-width="6" fill="none" stroke-linecap="round"/>
      <path d="M 245 92 Q 250 108 248 128" stroke="#1a0a0a" stroke-width="6" fill="none" stroke-linecap="round"/>
    `
  },
  {
    id: 'senna',
    name: 'Айртон Сенна',
    price: 1000,
    perClick: 50,
    number: '12',
    colors: {
      helmetMain: '#ffd81a', helmetAccent: '#1a8a3a', visor: '#2a4a6a',
      suitMain: '#ffd81a', suitAccent: '#1a8a3a',
      skin: '#c89068',      // загорелая бразильская
      hairColor: '#1a0a0a',
    },
    // Длинные чёрные волосы (фирменный стиль Сенны)
    hairStyle: `
      <!-- Длинные волны волос -->
      <path d="M 130 135 Q 115 180 125 230 Q 130 250 140 255 L 155 250 Q 145 230 142 190 Q 140 160 148 140 Z"
            fill="#1a0a0a" stroke="#000" stroke-width="1.5"/>
      <path d="M 270 135 Q 285 180 275 230 Q 270 250 260 255 L 245 250 Q 255 230 258 190 Q 260 160 252 140 Z"
            fill="#1a0a0a" stroke="#000" stroke-width="1.5"/>
      <!-- Блик на волосах -->
      <path d="M 125 170 Q 122 200 128 230" stroke="#3a2a1a" stroke-width="2" fill="none"/>
      <path d="M 275 170 Q 278 200 272 230" stroke="#3a2a1a" stroke-width="2" fill="none"/>
      <!-- Дополнительные пряди -->
      <path d="M 145 145 Q 138 190 148 235" stroke="#1a0a0a" stroke-width="5" fill="none" stroke-linecap="round"/>
      <path d="M 255 145 Q 262 190 252 235" stroke="#1a0a0a" stroke-width="5" fill="none" stroke-linecap="round"/>
    `,
    // Чёлка длинная, закрывает часть лба
    hairFront: `
      <path d="M 140 90 Q 160 105 180 95 Q 170 115 150 115 Q 140 110 138 100 Z" fill="#1a0a0a" stroke="#000" stroke-width="1.5"/>
      <path d="M 260 90 Q 240 105 220 95 Q 230 115 250 115 Q 260 110 262 100 Z" fill="#1a0a0a" stroke="#000" stroke-width="1.5"/>
      <!-- Блик -->
      <path d="M 150 100 Q 165 105 175 100" stroke="#3a2a1a" stroke-width="1.5" fill="none"/>
    `
  }
];

/* =========================================================
   СОСТОЯНИЕ И ЛОГИКА
   ========================================================= */
let state = {
  coins: 0,
  owned: ['verstappen'],
  current: 'verstappen'
};

try {
  const saved = localStorage.getItem('f1Clicker2');
  if (saved) state = { ...state, ...JSON.parse(saved) };
} catch(e) {}

function save() {
  try { localStorage.setItem('f1Clicker2', JSON.stringify(state)); } catch(e) {}
}

function getCurrentDriver() {
  return drivers.find(d => d.id === state.current);
}

function formatCoins(n) {
  return Number.isInteger(n) ? n : n.toFixed(1);
}

// Мини-аватар для магазина (круглый, только голова + фон)
function miniAvatar(d) {
  const c = d.colors;
  return `
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
  <!-- Мини фон (упрощённый цвет) -->
  <rect x="0" y="0" width="100" height="100" fill="${c.helmetMain}" opacity="0.3"/>
  <circle cx="50" cy="50" r="50" fill="${c.helmetMain}" opacity="0.2"/>
  <!-- Шея -->
  <rect x="42" y="62" width="16" height="12" fill="${c.skin}" stroke="#000" stroke-width="1"/>
  <!-- Голова -->
  <circle cx="50" cy="50" r="28" fill="${c.skin}" stroke="#000" stroke-width="1.5"/>
  <!-- Волосы мини (сзади) -->
  ${d.id === 'hamilton' ? `
    <path d="M 28 50 Q 22 70 28 85" stroke="#1a0a0a" stroke-width="4" fill="none" stroke-linecap="round"/>
    <path d="M 72 50 Q 78 70 72 85" stroke="#1a0a0a" stroke-width="4" fill="none" stroke-linecap="round"/>
  ` : d.id === 'senna' ? `
    <path d="M 25 48 Q 18 70 25 88 L 35 85 Q 30 70 32 52 Z" fill="#1a0a0a"/>
    <path d="M 75 48 Q 82 70 75 88 L 65 85 Q 70 70 68 52 Z" fill="#1a0a0a"/>
  ` : d.id === 'norris' ? `
    <circle cx="28" cy="55" r="5" fill="#d4a050" stroke="#000" stroke-width="0.8"/>
    <circle cx="32" cy="65" r="5" fill="#d4a050" stroke="#000" stroke-width="0.8"/>
    <circle cx="72" cy="55" r="5" fill="#d4a050" stroke="#000" stroke-width="0.8"/>
    <circle cx="68" cy="65" r="5" fill="#d4a050" stroke="#000" stroke-width="0.8"/>
  ` : `
    <path d="M 25 50 Q 22 65 28 75 L 35 72 Q 30 62 32 52 Z" fill="${c.hairColor}" stroke="#000" stroke-width="0.8"/>
    <path d="M 75 50 Q 78 65 72 75 L 65 72 Q 70 62 68 52 Z" fill="${c.hairColor}" stroke="#000" stroke-width="0.8"/>
  `}
  <!-- Шлем мини -->
  <path d="M 24 48 Q 24 22 50 20 Q 76 22 76 48 L 76 52 Q 76 55 73 55 L 27 55 Q 24 55 24 52 Z"
        fill="${c.helmetMain}" stroke="#000" stroke-width="1.5"/>
  <path d="M 30 35 Q 50 28 70 35 L 70 40 Q 50 33 30 40 Z" fill="${c.helmetAccent}"/>
  <!-- Визор -->
  <path d="M 30 42 Q 50 37 70 42 L 70 52 Q 50 48 30 52 Z"
        fill="${c.visor}" stroke="#000" stroke-width="1" opacity="0.85"/>
  <path d="M 35 44 Q 45 42 55 44 L 53 47 Q 45 45 37 47 Z" fill="#fff" opacity="0.5"/>
  <!-- Номер -->
  <circle cx="50" cy="28" r="6" fill="#fff" stroke="#000" stroke-width="0.8"/>
  <text x="50" y="31" text-anchor="middle" font-size="7" font-weight="bold" fill="#000" font-family="Arial">${d.number}</text>
  <!-- Румяна и улыбка -->
  <circle cx="38" cy="62" r="3" fill="#ff8080" opacity="0.5"/>
  <circle cx="62" cy="62" r="3" fill="#ff8080" opacity="0.5"/>
  <path d="M 45 66 Q 50 69 55 66" stroke="#000" stroke-width="1.2" fill="none" stroke-linecap="round"/>
</svg>`;
}

function render() {
  document.getElementById('coinCount').textContent = formatCoins(state.coins);
  const cur = getCurrentDriver();
  document.getElementById('currentName').textContent = cur.name;
  const word = cur.perClick === 1 ? 'монетка' : 'монеток';
  document.getElementById('perClick').textContent = `+${formatCoins(cur.perClick)} ${word} за клик`;

  document.getElementById('sceneContainer').innerHTML =
    `<div class="scene" onclick="clickDriver(event)">${chibiSVG(cur)}</div>`;

  const shop = document.getElementById('shopList');
  shop.innerHTML = '';
  drivers.forEach(d => {
    const owned = state.owned.includes(d.id);
    const isCurrent = state.current === d.id;
    const canAfford = state.coins >= d.price;

    const card = document.createElement('div');
    card.className = 'driver-card' + (isCurrent ? ' active' : '') + (owned ? ' owned' : '');

    let btnHtml;
    if (d.price === 0) {
      btnHtml = `<button class="buy-btn selected" disabled>Старт</button>`;
    } else if (!owned) {
      btnHtml = `<button class="buy-btn" ${canAfford ? '' : 'disabled'} onclick="buyDriver('${d.id}')">🪙 ${d.price}</button>`;
    } else if (isCurrent) {
      btnHtml = `<button class="buy-btn selected" disabled>Активен</button>`;
    } else {
      btnHtml = `<button class="buy-btn select" onclick="selectDriver('${d.id}')">Выбрать</button>`;
    }

    card.innerHTML = `
      <div class="mini-scene">${miniAvatar(d)}</div>
      <div class="driver-info">
        <div class="name">${d.name}</div>
        <div class="stats">+${formatCoins(d.perClick)} / клик</div>
        ${d.price > 0 ? `<div class="price">Цена: ${d.price} 🪙</div>` : ''}
      </div>
      ${btnHtml}
    `;
    shop.appendChild(card);
  });
}

function clickDriver(event) {
  const cur = getCurrentDriver();
  state.coins += cur.perClick;
  state.coins = Math.round(state.coins * 10) / 10;

  const scene = document.querySelector('.scene');
  scene.classList.remove('bounce');
  void scene.offsetWidth;
  scene.classList.add('bounce');

  const area = document.getElementById('clickArea');
  const rect = area.getBoundingClientRect();
  const coin = document.createElement('div');
  coin.className = 'float-coin';
  coin.textContent = `+${formatCoins(cur.perClick)} 🪙`;
  coin.style.left = (event.clientX - rect.left - 20) + 'px';
  coin.style.top = (event.clientY - rect.top - 20) + 'px';
  area.appendChild(coin);
  setTimeout(() => coin.remove(), 1000);

  save();
  render();
}

function buyDriver(id) {
  const d = drivers.find(x => x.id === id);
  if (!d || state.owned.includes(id) || state.coins < d.price) return;
  state.coins -= d.price;
  state.coins = Math.round(state.coins * 10) / 10;
  state.owned.push(id);
  state.current = id;
  save();
  render();
}

function selectDriver(id) {
  if (!state.owned.includes(id)) return;
  state.current = id;
  save();
  render();
}

function resetGame() {
  if (!confirm('Точно сбросить весь прогресс?')) return;
  state = { coins: 0, owned: ['verstappen'], current: 'verstappen' };
  save();
  render();
}

render();
</script>

</body>
</html>
