<!DOCTYPE html>
<html lang="am">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>Ethio Bingo (ትምቢላ)</title>
  <!-- Telegram Web App SDK -->
  <script src="https://telegram.org/js/telegram-web-app.js"></script>
  <!-- Canvas Confetti -->
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
  <style>
    :root {
      --bg-color: var(--tg-theme-bg-color, #121824);
      --text-color: var(--tg-theme-text-color, #ffffff);
      --card-bg: var(--tg-theme-secondary-bg-color, #1a2232);
      --accent-color: #f39c12;
      --cell-bg: #252e3e;
      --cell-daubed: #27ae60;
      --cell-border: #34495e;
      --font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      user-select: none;
    }

    body {
      background-color: var(--bg-color);
      color: var(--text-color);
      font-family: var(--font-family);
      display: flex;
      flex-direction: column;
      align-items: center;
      min-height: 100vh;
      padding: 12px;
    }

    header {
      width: 100%;
      max-width: 420px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;
    }

    .title-area h1 {
      font-size: 20px;
      color: var(--accent-color);
    }

    .title-area p {
      font-size: 12px;
      opacity: 0.8;
    }

    .lang-btn {
      background: var(--cell-bg);
      color: var(--text-color);
      border: 1px solid var(--cell-border);
      padding: 6px 12px;
      border-radius: 8px;
      font-size: 12px;
      cursor: pointer;
    }

    /* Ball Caller Display */
    .caller-section {
      width: 100%;
      max-width: 420px;
      background: var(--card-bg);
      border-radius: 12px;
      padding: 12px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.3);
    }

    .current-ball-container {
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .ball {
      width: 54px;
      height: 54px;
      border-radius: 50%;
      background: radial-gradient(circle at 35% 35%, #ffe100, #f39c12);
      color: #000;
      font-weight: 800;
      font-size: 22px;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 0 4px 8px rgba(0,0,0,0.4);
      border: 2px solid #fff;
    }

    .ball-info {
      display: flex;
      flex-direction: column;
    }

    .ball-letter {
      font-size: 12px;
      text-transform: uppercase;
      opacity: 0.7;
    }

    .ball-number {
      font-size: 20px;
      font-weight: bold;
      color: var(--accent-color);
    }

    .recent-balls {
      display: flex;
      gap: 6px;
    }

    .recent-ball {
      width: 28px;
      height: 28px;
      border-radius: 50%;
      background: var(--cell-bg);
      border: 1px solid var(--cell-border);
      font-size: 11px;
      font-weight: bold;
      display: flex;
      align-items: center;
      justify-content: center;
      opacity: 0.7;
    }

    /* Bingo Card Grid */
    .bingo-card {
      width: 100%;
      max-width: 420px;
      background: var(--card-bg);
      border-radius: 12px;
      padding: 12px;
      box-shadow: 0 4px 16px rgba(0,0,0,0.4);
      margin-bottom: 16px;
    }

    .grid-header {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 6px;
      margin-bottom: 6px;
      text-align: center;
      font-weight: 900;
      font-size: 18px;
      color: var(--accent-color);
    }

    .grid-body {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 6px;
    }

    .cell {
      aspect-ratio: 1;
      background: var(--cell-bg);
      border: 1px solid var(--cell-border);
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 18px;
      font-weight: bold;
      cursor: pointer;
      transition: all 0.2s ease;
    }

    .cell.daubed {
      background: var(--cell-daubed);
      border-color: #2ecc71;
      color: #fff;
      transform: scale(0.96);
      box-shadow: inset 0 2px 4px rgba(0,0,0,0.3);
    }

    .cell.free-space {
      background: #8e44ad;
      color: #fff;
      font-size: 12px;
    }

    /* Controls & Actions */
    .controls {
      width: 100%;
      max-width: 420px;
      display: flex;
      gap: 12px;
    }

    .action-btn {
      flex: 1;
      padding: 14px;
      border: none;
      border-radius: 10px;
      font-size: 16px;
      font-weight: bold;
      cursor: pointer;
      transition: background 0.2s ease;
    }

    .bingo-btn {
      background: linear-gradient(135deg, #e74c3c, #c0392b);
      color: #fff;
      box-shadow: 0 4px 12px rgba(231, 76, 60, 0.4);
    }

    .bingo-btn:active {
      transform: scale(0.98);
    }

    .secondary-btn {
      background: var(--cell-bg);
      color: var(--text-color);
      border: 1px solid var(--cell-border);
    }

    /* Modal */
    .modal {
      display: none;
      position: fixed;
      top: 0; left: 0; right: 0; bottom: 0;
      background: rgba(0,0,0,0.85);
      align-items: center;
      justify-content: center;
      padding: 20px;
      z-index: 100;
    }

    .modal-content {
      background: var(--card-bg);
      border-radius: 16px;
      padding: 24px;
      text-align: center;
      max-width: 340px;
      width: 100%;
      border: 1px solid var(--cell-border);
    }

    .modal h2 {
      color: var(--accent-color);
      font-size: 24px;
      margin-bottom: 8px;
    }

    .modal p {
      margin-bottom: 20px;
      font-size: 14px;
      opacity: 0.9;
    }

    canvas#confetti-canvas {
      position: fixed;
      top: 0; left: 0; width: 100%; height: 100%;
      pointer-events: none;
      z-index: 99;
    }
  </style>
</head>
<body>

  <canvas id="confetti-canvas"></canvas>

  <header>
    <div class="title-area">
      <h1 id="txt-title">Ethio Bingo</h1>
      <p id="txt-subtitle">ትምቢላ 75-Ball Live</p>
    </div>
    <button class="lang-btn" id="lang-toggle" onclick="toggleLanguage()">አማርኛ</button>
  </header>

  <!-- Caller Section -->
  <div class="caller-section">
    <div class="current-ball-container">
      <div class="ball" id="current-ball">--</div>
      <div class="ball-info">
        <span class="ball-letter" id="ball-letter">Status</span>
        <span class="ball-number" id="ball-status">Drawing...</span>
      </div>
    </div>
    <div class="recent-balls" id="recent-balls">
      <div class="recent-ball">-</div>
      <div class="recent-ball">-</div>
      <div class="recent-ball">-</div>
    </div>
  </div>

  <!-- Bingo Card Grid -->
  <div class="bingo-card">
    <div class="grid-header">
      <div>B</div><div>I</div><div>N</div><div>G</div><div>O</div>
    </div>
    <div class="grid-body" id="grid-body"></div>
  </div>

  <!-- Actions -->
  <div class="controls">
    <button class="action-btn secondary-btn" onclick="toggleAutoDaub()" id="btn-autodaub">Auto-Daub: OFF</button>
    <button class="action-btn bingo-btn" onclick="claimBingo()" id="btn-bingo">BINGO!</button>
  </div>

  <!-- Win Modal -->
  <div class="modal" id="win-modal">
    <div class="modal-content">
      <h2>🎉 BINGO! 🎉</h2>
      <p id="txt-win-msg">Winning pattern verified! Sending claim to bot...</p>
      <button class="action-btn bingo-btn" style="width:100%" onclick="closeModal()">Continue</button>
    </div>
  </div>

  <script>
    // Initialize Telegram WebApp SDK
    const tg = window.Telegram?.WebApp;
    if (tg) tg.expand();

    // Localization Strings
    let currentLang = 'en';
    const i18n = {
      en: {
        title: 'Ethio Bingo',
        subtitle: '75-Ball Live',
        statusDrawing: 'Drawing...',
        statusWaiting: 'Ready',
        btnAutoOff: 'Auto-Daub: OFF',
        btnAutoOn: 'Auto-Daub: ON',
        winTitle: '🎉 BINGO! 🎉',
        winMsg: 'Winning pattern verified! Sending claim to bot...',
        langBtn: 'አማርኛ'
      },
      am: {
        title: 'ኢትዮ ቢንጎ',
        subtitle: 'ትምቢላ 75-ኳስ',
        statusDrawing: 'እየወጣ ነው...',
        statusWaiting: 'ዝግጁ',
        btnAutoOff: 'ኦቶ-ማርክ: አጥፋ',
        btnAutoOn: 'ኦቶ-ማርክ: አብራ',
        winTitle: '🎉 ቢንጎ! 🎉',
        winMsg: 'አሸናፊነት ተረጋግጧል! መረጃው ወደ ቦቱ እየተላከ ነው...',
        langBtn: 'English'
      }
    };

    // State Variables
    let card = [];
    let drawnNumbers = new Set();
    let daubedCells = new Set(['2,2']); // Center Free Space is daubed by default
    let autoDaub = false;
    let recentBalls = [];
    let gameInterval = null;

    // Audio Synthesizer (Web Audio API)
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    function playBeep(freq = 440, type = 'sine', duration = 0.15) {
      if (audioCtx.state === 'suspended') audioCtx.resume();
      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      osc.type = type;
      osc.frequency.value = freq;
      gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + duration);
      osc.connect(gain);
      gain.connect(audioCtx.destination);
      osc.start();
      osc.stop(audioCtx.currentTime + duration);
    }

    // Card Generator (75-Ball Standard)
    function generateCard() {
      const getRandomNums = (min, max, count) => {
        const nums = [];
        while (nums.length < count) {
          const r = Math.floor(Math.random() * (max - min + 1)) + min;
          if (!nums.includes(r)) nums.push(r);
        }
        return nums;
      };

      const cols = {
        B: getRandomNums(1, 15, 5),
        I: getRandomNums(16, 30, 5),
        N: getRandomNums(31, 45, 5),
        G: getRandomNums(46, 60, 5),
        O: getRandomNums(61, 75, 5)
      };

      card = [];
      for (let r = 0; r < 5; r++) {
        const row = [cols.B[r], cols.I[r], r === 2 ? 0 : cols.N[r], cols.G[r], cols.O[r]];
        card.push(row);
      }
      renderCard();
    }

    // Render Grid
    function renderCard() {
      const grid = document.getElementById('grid-body');
      grid.innerHTML = '';

      for (let r = 0; r < 5; r++) {
        for (let c = 0; c < 5; c++) {
          const val = card[r][c];
          const key = `${r},${c}`;
          const cell = document.createElement('div');
          cell.className = 'cell';
          
          if (r === 2 && c === 2) {
            cell.classList.add('free-space', 'daubed');
            cell.innerText = 'FREE';
          } else {
            cell.innerText = val;
            if (daubedCells.has(key)) cell.classList.add('daubed');
            cell.onclick = () => toggleDaub(r, c, val);
          }
          grid.appendChild(cell);
        }
      }
    }

    function toggleDaub(r, c, val) {
      const key = `${r},${c}`;
      if (daubedCells.has(key)) {
        daubedCells.delete(key);
      } else {
        daubedCells.add(key);
        playBeep(520, 'sine', 0.1);
      }
      renderCard();
    }

    function toggleAutoDaub() {
      autoDaub = !autoDaub;
      const btn = document.getElementById('btn-autodaub');
      btn.innerText = autoDaub ? i18n[currentLang].btnAutoOn : i18n[currentLang].btnAutoOff;
      btn.style.borderColor = autoDaub ? '#2ecc71' : 'var(--cell-border)';
      if (autoDaub) checkAutoDaub();
    }

    function checkAutoDaub() {
      if (!autoDaub) return;
      for (let r = 0; r < 5; r++) {
        for (let c = 0; c < 5; c++) {
          const val = card[r][c];
          if (drawnNumbers.has(val)) daubedCells.add(`${r},${c}`);
        }
      }
      renderCard();
    }

    // Ball Calling Engine
    function startBallCaller() {
      const availableBalls = Array.from({ length: 75 }, (_, i) => i + 1);
      
      gameInterval = setInterval(() => {
        if (availableBalls.length === 0) return clearInterval(gameInterval);

        const idx = Math.floor(Math.random() * availableBalls.length);
        const ball = availableBalls.splice(idx, 1)[0];
        drawnNumbers.add(ball);

        let letter = 'B';
        if (ball >= 16 && ball <= 30) letter = 'I';
        else if (ball >= 31 && ball <= 45) letter = 'N';
        else if (ball >= 46 && ball <= 60) letter = 'G';
        else if (ball >= 61) letter = 'O';

        document.getElementById('current-ball').innerText = ball;
        document.getElementById('ball-letter').innerText = letter;
        document.getElementById('ball-status').innerText = `${letter}-${ball}`;
        playBeep(660, 'triangle', 0.2);

        recentBalls.unshift(`${letter}${ball}`);
        if (recentBalls.length > 3) recentBalls.pop();
        
        const recentElems = document.querySelectorAll('.recent-ball');
        recentBalls.forEach((b, i) => { if (recentElems[i]) recentElems[i].innerText = b; });

        if (autoDaub) checkAutoDaub();
      }, 4000);
    }

    // Pattern Validation (Rows, Cols, Diagonals, 4 Corners)
    function claimBingo() {
      let isWin = false;

      // Check Rows & Cols
      for (let i = 0; i < 5; i++) {
        if ([0,1,2,3,4].every(c => daubedCells.has(`${i},${c}`))) isWin = true;
        if ([0,1,2,3,4].every(r => daubedCells.has(`${r},${i}`))) isWin = true;
      }

      // Check Diagonals
      if ([0,1,2,3,4].every(i => daubedCells.has(`${i},${i}`))) isWin = true;
      if ([0,1,2,3,4].every(i => daubedCells.has(`${i},${4-i}`))) isWin = true;

      // Check 4 Corners
      if (['0,0', '0,4', '4,0', '4,4'].every(k => daubedCells.has(k))) isWin = true;

      if (isWin) {
        playBeep(880, 'sine', 0.4);
        confetti({ particleCount: 120, spread: 70, origin: { y: 0.6 } });
        document.getElementById('win-modal').style.display = 'flex';

        // Send payload back to Telegram Bot via SDK
        if (tg) {
          tg.sendData(JSON.stringify({
            event: 'BINGO_CLAIM',
            timestamp: Date.now(),
            daubedCount: daubedCells.size
          }));
        }
      } else {
        playBeep(220, 'sawtooth', 0.3);
        alert(currentLang === 'am' ? 'እስካሁን ቢንጎ አልሰሩም!' : 'No valid Bingo pattern marked yet!');
      }
    }

    function closeModal() {
      document.getElementById('win-modal').style.display = 'none';
    }

    function toggleLanguage() {
      currentLang = currentLang === 'en' ? 'am' : 'en';
      const t = i18n[currentLang];
      document.getElementById('txt-title').innerText = t.title;
      document.getElementById('txt-subtitle').innerText = t.subtitle;
      document.getElementById('lang-toggle').innerText = t.langBtn;
      document.getElementById('btn-autodaub').innerText = autoDaub ? t.btnAutoOn : t.btnAutoOff;
      document.getElementById('txt-win-msg').innerText = t.winMsg;
    }

    // Boot
    generateCard();
    startBallCaller();
  </script>
</body>
</html>
