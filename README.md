# schach
<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8" />
  <title>Schach App</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <style>
    /* === Layout und Design === */
    html, body {
      height: 100%;
      margin: 0;
      padding: 0;
      background: #f6f8fa;
    }
    body {
      min-height: 100vh;
      display: flex;
      flex-direction: row;
      align-items: flex-start;
      justify-content: center;
      font-family: 'Segoe UI', 'Roboto', Arial, sans-serif;
      background: linear-gradient(135deg, #e0e7ef 0%, #f6f8fa 100%);
    }
    .sidebar {
      background: #fff;
      border-radius: 18px;
      box-shadow: 0 4px 24px 0 #0001;
      margin: 32px 0 32px 0;
      padding: 22px 18px 18px 18px;
      min-width: 170px;
      max-width: 220px;
      display: flex;
      flex-direction: column;
      gap: 28px;
      align-items: stretch;
      height: fit-content;
    }
    .sidebar-group {
      margin-bottom: 0;
      display: flex;
      flex-direction: column;
      gap: 8px;
    }
    .sidebar-group label {
      font-size: 1.05em;
      font-weight: bold;
      color: #2d3a4a;
      margin-bottom: 4px;
    }
    .sidebar-group button {
      border: none;
      background: #e0e7ef;
      color: #2d3a4a;
      border-radius: 8px;
      padding: 7px 0;
      font-size: 1rem;
      cursor: pointer;
      transition: background 0.15s;
      margin-bottom: 0;
      width: 100%;
    }
    .sidebar-group button.active, .sidebar-group button:hover {
      background: #0078d7;
      color: #fff;
    }
    .sidebar .controls {
      display: flex;
      flex-direction: column;
      gap: 8px;
      margin-top: 8px;
    }
    .sidebar .controls button {
      background: #0078d7;
      color: #fff;
      border-radius: 8px;
      padding: 8px 0;
      font-size: 1rem;
      font-weight: 500;
      cursor: pointer;
      transition: background 0.15s;
      border: none;
    }
    .sidebar .controls button:hover {
      background: #005fa3;
    }
    .sidebar .help-btn {
      margin-top: 18px;
      background: #e0e7ef;
      color: #2d3a4a;
      border-radius: 8px;
      padding: 8px 0;
      font-size: 1rem;
      border: none;
      cursor: pointer;
      transition: background 0.15s;
    }
    .sidebar .help-btn:hover {
      background: #0078d7;
      color: #fff;
    }
    .app-container {
      background: #fff;
      border-radius: 22px;
      box-shadow: 0 4px 24px 0 #0001;
      margin: 32px 0 32px 0;
      padding: 20px 18px 18px 18px;
      max-width: 600px;
      width: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    h1 {
      font-size: 1.5rem;
      margin: 0 0 12px 0;
      letter-spacing: 0.02em;
      color: #2d3a4a;
    }
    .timers {
      display: flex;
      gap: 18px;
      margin-bottom: 8px;
      width: 100%;
      justify-content: center;
    }
    .timer {
      font-size: 1.1rem;
      font-weight: 500;
      background: #e0e7ef;
      border-radius: 8px;
      padding: 4px 12px;
      min-width: 90px;
      text-align: center;
    }
    .timer.out { color: #d70022; }
    #startBtn, #stopBtn {
      background: #0078d7;
      color: #fff;
      border: none;
      border-radius: 8px;
      padding: 4px 14px;
      font-size: 1em;
      cursor: pointer;
      margin: 0 0 2px 0;
      transition: background 0.15s;
    }
    #startBtn:hover, #stopBtn:hover {
      background: #005fa3;
    }
    #currentPlayer {
      font-weight: bold;
      margin-bottom: 6px;
      color: #0078d7;
      text-align: center;
      width: 100%;
    }
    #checkStatus {
      color: #d70022;
      margin-bottom: 6px;
      text-align: center;
      width: 100%;
    }
    #gameStatus {
      color: #d70022;
      font-weight: bold;
      margin-bottom: 8px;
      text-align: center;
      width: 100%;
    }
    #evalStatus {
      color:#0078d7;
      font-weight:bold;
      margin-bottom:8px;
      text-align:center;
      width:100%
    }
    /* === Bewertungsgrafik === */
    #evalChart {
      width:100%;
      height:40px;
      margin-bottom:8px;
    }
    #evalCanvas {
      width:100%;
      height:40px;
      display:block;
    }
    #board {
      display: grid;
      grid-template-columns: repeat(8, 1fr);
      grid-template-rows: repeat(8, 1fr);
      width: 480px;
      height: 480px;
      border-radius: 16px;
      overflow: hidden;
      box-shadow: 0 2px 12px #0002;
      margin-bottom: 10px;
      background: #b58863;
      touch-action: manipulation;
      -webkit-user-select: none;
      user-select: none;
    }
    .square {
      width: 60px;
      height: 60px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 38px;
      user-select: none;
      transition: background 0.1s;
    }
    .theme-classic .white { background: #f0d9b5; }
    .theme-classic .black { background: #b58863; }
    .theme-blue .white { background: #e0e8ff; }
    .theme-blue .black { background: #4a6fa5; }
    .theme-green .white { background: #eaffea; }
    .theme-green .black { background: #4e944f; }
    .highlight { box-shadow: 0 0 0 4px #0078d7 inset !important; }
    .highlight-capture { box-shadow: 0 0 0 4px #d70022 inset !important; }
    .captured-bar {
      width: 100%;
      display: flex;
      justify-content: space-between;
      margin-bottom: 8px;
      margin-top: 2px;
    }
    .captured-list {
      display: flex;
      gap: 2px;
      min-height: 24px;
    }
    .captured-title {
      font-size: 0.95em;
      color: #888;
      margin-right: 4px;
      margin-left: 4px;
    }
    .captured-list img {
      width: 28px;
      height: 28px;
      vertical-align: middle;
      border-radius: 3px;
      background: #f0f0f0;
      box-shadow: 0 1px 2px #0001;
    }
    .history {
      width: 100%;
      margin-top: 8px;
      background: #f6f8fa;
      border-radius: 10px;
      padding: 8px 10px;
      box-sizing: border-box;
      box-shadow: 0 1px 4px #0001;
      max-height: 110px;
      overflow-y: auto;
    }
    .history h3 {
      font-size: 1.05em;
      margin: 0 0 4px 0;
      color: #2d3a4a;
    }
    #moveHistory {
      margin: 0;
      padding: 0;
      list-style: none;
      font-size: 1em;
      color: #333;
      display: flex;
      flex-wrap: wrap;
      gap: 8px 12px;
    }
    #moveHistory li {
      margin: 0;
      padding: 0;
      background: #e0e7ef;
      border-radius: 5px;
      padding: 2px 7px;
    }
    #promotionModal {
      display: none;
      position: fixed;
      left: 0; top: 0; right: 0; bottom: 0;
      background: rgba(0,0,0,0.5);
      align-items: center; justify-content: center;
      z-index: 1000;
    }
    #promotionBox {
      background: #fff;
      padding: 20px 30px;
      border-radius: 10px;
      box-shadow: 0 0 10px #333;
      display: flex;
      gap: 20px;
      align-items: center;
      justify-content: center;
    }
    #promotionBox img {
      width: 44px; height: 44px;
      cursor: pointer;
      border: 2px solid transparent;
      border-radius: 6px;
      transition: border 0.2s;
    }
    #promotionBox img:hover {
      border: 2px solid #0078d7;
      background: #e6f0ff;
    }
    #helpModal {
      display:none;
      position:fixed;
      left:0;top:0;right:0;bottom:0;
      background:rgba(0,0,0,0.5);
      z-index:2000;
      align-items:center;
      justify-content:center;
    }
    #helpModal .help-content {
      background:#fff;
      border-radius:14px;
      max-width:420px;
      padding:28px 22px 18px 22px;
      box-shadow:0 4px 24px #0003;
      margin:auto;
    }
    #helpModal h2 {
      margin-top:0;
    }
    #helpModal ul {
      padding-left:18px;
      font-size:1.05em;
    }
    #helpModal button {
      margin-top:12px;
      padding:7px 18px;
      border:none;
      border-radius:8px;
      background:#0078d7;
      color:#fff;
      font-size:1em;
      cursor:pointer;
    }
    #helpModal button:hover {
      background:#005fa3;
    }
    @media (max-width: 900px) {
      #board { width: 320px; height: 320px; }
      .square { width: 40px; height: 40px; font-size: 28px; }
      .captured-list img { width: 22px; height: 22px; }
      .app-container { max-width: 99vw; }
    }
    @media (max-width: 700px) {
      body { flex-direction: column; align-items: center; }
      .sidebar { flex-direction: row; min-width: 0; max-width: 99vw; width: 99vw; margin: 12px 0 0 0; padding: 10px 2vw; gap: 18px;}
      .sidebar-group { flex:1; }
      .app-container { margin: 12px 0 12px 0; }
    }
    @media (max-width: 500px) {
      #board { width: 98vw; height: 98vw; min-width: 240px; min-height: 240px;}
      .square { width: 12vw; height: 12vw; min-width: 30px; min-height: 30px; }
    }
  </style>
</head>
<body class="theme-classic">
  <div class="sidebar">
    <!-- === Brett-Design Auswahl === -->
    <div class="sidebar-group" id="themeSelect">
      <label>Brett-Design</label>
      <button class="active" onclick="setTheme('classic', this)">Klassisch</button>
      <button onclick="setTheme('blue', this)">Blau</button>
      <button onclick="setTheme('green', this)">Grün</button>
    </div>
    <!-- === Modus Auswahl === -->
    <div class="sidebar-group" id="modeSelect">
      <label>Modus</label>
      <button id="botBtn" class="active">Gegen Bot</button>
      <button id="friendBtn">Gegen Freund</button>
    </div>
    <!-- === Bot-Stärke Auswahl (NEU) === -->
    <div class="sidebar-group" id="botLevelSelect">
      <label>Bot-Stärke</label>
      <button class="active" onclick="setBotLevel(0, this)">Leicht</button>
      <button onclick="setBotLevel(1, this)">Mittel</button>
      <button onclick="setBotLevel(2, this)">Stark</button>
    </div>
    <!-- === Farbwahl === -->
    <div class="sidebar-group" id="colorSelect">
      <label>Farbe</label>
      <button id="playWhiteBtn">Weiß</button>
      <button id="playBlackBtn">Schwarz</button>
    </div>
    <!-- === Steuerungs-Buttons === -->
    <div class="controls">
      <button id="resetBtn">Neues Spiel</button>
      <button id="undoBtn">Rückgängig</button>
      <button id="backBtn">← Zurück</button>
      <button id="forwardBtn">→ Vor</button>
    </div>
    <button class="help-btn" onclick="document.getElementById('helpModal').style.display='flex';">Hilfe</button>
  </div>
  <div class="app-container">
    <h1>Schach App</h1>
    <!-- === Uhren und Start/Stop === -->
    <div class="timers">
      <div id="whiteTimer" class="timer white">Weiß: 15:00</div>
      <div style="display:flex;flex-direction:column;align-items:center;gap:4px;">
        <button id="startBtn" style="margin-bottom:2px;">Start</button>
        <button id="stopBtn">Stop</button>
      </div>
      <div id="blackTimer" class="timer black">Schwarz: 15:00</div>
    </div>
    <div id="currentPlayer">Weiß am Zug</div>
    <div id="checkStatus"></div>
    <div id="gameStatus"></div>
    <!-- === Bewertungstext === -->
    <div id="evalStatus"></div>
    <!-- === Bewertungsgrafik (NEU) === -->
    <div id="evalChart">
      <canvas id="evalCanvas" width="400" height="40"></canvas>
    </div>
    <div class="captured-bar">
      <span class="captured-title">Schwarz:</span>
      <div id="capturedBlack" class="captured-list"></div>
      <span style="flex:1"></span>
      <span class="captured-title">Weiß:</span>
      <div id="capturedWhite" class="captured-list"></div>
    </div>
    <div id="board" class="board"></div>
    <div class="history">
      <h3>Zughistorie</h3>
      <ul id="moveHistory"></ul>
    </div>
  </div>
  <!-- === Promotion Modal === -->
  <div id="promotionModal">
    <div id="promotionBox"></div>
  </div>
  <!-- === Hilfe-Modal === -->
  <div id="helpModal">
    <div class="help-content">
      <h2>Schachregeln (Kurzfassung)</h2>
      <ul>
        <li>Weiß beginnt, dann abwechselnd ein Zug.</li>
        <li>Jede Figur zieht nach ihren eigenen Regeln.</li>
        <li>Schach: Der König wird bedroht. Schach muss abgewehrt werden.</li>
        <li>Schachmatt: Der König kann nicht mehr entkommen – das Spiel ist vorbei.</li>
        <li>Patt: Der Spieler am Zug kann keinen legalen Zug machen, aber steht nicht im Schach – das Spiel endet unentschieden.</li>
        <li>Bauernumwandlung: Ein Bauer, der die gegnerische Grundreihe erreicht, wird zur Dame, Turm, Läufer oder Springer.</li>
        <li>Rochade: König und Turm dürfen unter bestimmten Bedingungen gemeinsam ziehen.</li>
        <li>En passant: Ein Bauer kann einen gegnerischen Bauern im Vorbeigehen schlagen.</li>
        <li>Remis: z.B. durch Patt, dreifache Stellungswiederholung oder 50-Züge-Regel.</li>
      </ul>
      <button onclick="document.getElementById('helpModal').style.display='none';">Schließen</button>
    </div>
  </div>
  <script>
    // === Globale Variablen ===
    let evalHistory = []; // Für Bewertungsgrafik
    let botLevel = 0;     // Bot-Stärke (0=leicht, 1=mittel, 2=stark)

    // === Theme-Wechsel ===
    function setTheme(theme, btn) {
      document.body.classList.remove('theme-classic', 'theme-blue', 'theme-green');
      document.body.classList.add('theme-' + theme);
      document.querySelectorAll('#themeSelect button').forEach(b => b.classList.remove('active'));
      if(btn) btn.classList.add('active');
    }

    // === Bot-Stärke Auswahl ===
    function setBotLevel(level, btn) {
      botLevel = level;
      document.querySelectorAll('#botLevelSelect button').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
    }

    // === Spielfeld und Spielstand ===
    const board = document.getElementById('board');
    const moveHistory = document.getElementById('moveHistory');
    let history = [];
    let boardStates = [];
    let capturedWhite = [];
    let capturedBlack = [];
    let historyPointer = 0;

    // === Timer-Variablen ===
    let whiteTime = 15 * 60;
    let blackTime = 15 * 60;
    let timerInterval = null;
    let timersRunning = false; // Zeit läuft erst nach Start

    const whiteTimerDiv = document.getElementById('whiteTimer');
    const blackTimerDiv = document.getElementById('blackTimer');

    // === Farbauswahl und Modus ===
    let playerColor = null; // 'white' oder 'black'
    let playMode = 'bot'; // 'bot' oder 'friend'

    // === Promotion Modal ===
    const promotionModal = document.getElementById('promotionModal');
    const promotionBox = document.getElementById('promotionBox');
    let promotionCallback = null;

    // === Highlight für mögliche Züge ===
    let highlightedSquares = [];

    // === Figurenbilder (Unicode zu Bilddatei) ===
    const pieceImages = {
      '♙': 'img/white_pawn.png',
      '♖': 'img/white_rook.png',
      '♘': 'img/white_knight.png',
      '♗': 'img/white_bishop.png',
      '♕': 'img/white_queen.png',
      '♔': 'img/white_king.png',
      '♟': 'img/black_pawn.png',
      '♜': 'img/black_rook.png',
      '♞': 'img/black_knight.png',
      '♝': 'img/black_bishop.png',
      '♛': 'img/black_queen.png',
      '♚': 'img/black_king.png'
    };

    // === Promotion-Auswahl ===
    const promotionChoices = {
      'white': ['♕', '♖', '♗', '♘'],
      'black': ['♛', '♜', '♝', '♞']
    };

    // === Startaufstellung ===
    const initialPieces = {
      'a8': '♜', 'b8': '♞', 'c8': '♝', 'd8': '♛', 'e8': '♚', 'f8': '♝', 'g8': '♞', 'h8': '♜',
      'a7': '♟', 'b7': '♟', 'c7': '♟', 'd7': '♟', 'e7': '♟', 'f7': '♟', 'g7': '♟', 'h7': '♟',
      'a2': '♙', 'b2': '♙', 'c2': '♙', 'd2': '♙', 'e2': '♙', 'f2': '♙', 'g2': '♙', 'h2': '♙',
      'a1': '♖', 'b1': '♘', 'c1': '♗', 'd1': '♕', 'e1': '♔', 'f1': '♗', 'g1': '♘', 'h1': '♖'
    };

    let pieces = {};

    const files = ['a','b','c','d','e','f','g','h'];

    let currentPlayer = 'white'; // 'white' oder 'black'

    // === Anzeige: Wer ist am Zug ===
    function updateCurrentPlayerDisplay() {
      document.getElementById('currentPlayer').textContent =
        currentPlayer === 'white' ? 'Weiß am Zug' : 'Schwarz am Zug';
    }

    // === Prüft, ob der König im Schach steht ===
    function isKingInCheck(color) {
      const kingSymbol = color === 'white' ? '♔' : '♚';
      let kingSquare = null;
      for (const sq in pieces) {
        if (pieces[sq] === kingSymbol) kingSquare = sq;
      }
      if (!kingSquare) return false;
      const enemyPieces = color === 'white'
        ? ['♟','♜','♞','♝','♛','♚']
        : ['♙','♖','♘','♗','♕','♔'];
      for (const sq in pieces) {
        if (enemyPieces.includes(pieces[sq])) {
          if (isMoveValid(pieces[sq], sq, kingSquare, true)) return true;
        }
      }
      return false;
    }

    // === Anzeige: Schach-Status ===
    function updateCheckStatus() {
      let status = '';
      if (isKingInCheck('white')) status += 'Weiß steht im Schach! ';
      if (isKingInCheck('black')) status += 'Schwarz steht im Schach!';
      document.getElementById('checkStatus').textContent = status;
    }

    // === Prüft, ob ein legaler Zug für die Farbe möglich ist ===
    function isLegalMoveForPlayer(color) {
      const ownPieces = color === 'white'
        ? ['♙','♖','♘','♗','♕','♔']
        : ['♟','♜','♞','♝','♛','♚'];
      for (const from in pieces) {
        if (!ownPieces.includes(pieces[from])) continue;
        for (let file of files) {
          for (let rank = 1; rank <= 8; rank++) {
            const to = file + rank;
            if (from === to) continue;
            const target = pieces[to];
            const isCapture = !!target;
            if (isCapture && !isEnemy(pieces[from], target)) continue;
            if (isMoveValid(pieces[from], from, to, isCapture)) {
              // Simuliere den Zug
              const backupFrom = pieces[from];
              const backupTo = pieces[to];
              pieces[to] = pieces[from];
              delete pieces[from];
              const inCheck = isKingInCheck(color);
              // Rückgängig machen
              pieces[from] = backupFrom;
              if (backupTo) pieces[to] = backupTo; else delete pieces[to];
              if (!inCheck) return true;
            }
          }
        }
      }
      return false;
    }

    // === Anzeige: Spielende (Matt/Patt) ===
    function updateGameStatus() {
      const statusDiv = document.getElementById('gameStatus');
      statusDiv.textContent = '';
      const color = currentPlayer;
      if (isKingInCheck(color)) {
        if (!isLegalMoveForPlayer(color)) {
          statusDiv.textContent = (color === 'white' ? 'Weiß' : 'Schwarz') + ' ist SCHACHMATT!';
          stopTimers();
        }
      } else {
        if (!isLegalMoveForPlayer(color)) {
          statusDiv.textContent = 'Patt! Unentschieden.';
          stopTimers();
        }
      }
    }

    // === Bewertungsfunktion: Materialzählung ===
    function evaluatePosition() {
      const values = {'♙':1,'♖':5,'♘':3,'♗':3,'♕':9,
                      '♟':-1,'♜':-5,'♞':-3,'♝':-3,'♛':-9};
      let score = 0;
      for (const sq in pieces) {
        const v = values[pieces[sq]];
        if (v) score += v;
      }
      return score;
    }

    // === Bewertungsanzeige und Grafik ===
    function showEvaluation() {
      const evalDiv = document.getElementById('evalStatus');
      const score = evaluatePosition();
      let txt = '';
      if (score > 0.2) txt = `Bewertung: +${score.toFixed(1)} (Weiß steht besser)`;
      else if (score < -0.2) txt = `Bewertung: ${score.toFixed(1)} (Schwarz steht besser)`;
      else txt = `Bewertung: 0.0 (Ausgeglichen)`;
      evalDiv.textContent = txt;
      evalHistory.push(score); // Bewertungshistorie für Grafik
      drawEvalChart();
    }

    // === Bewertungsgrafik zeichnen ===
    function drawEvalChart() {
      const canvas = document.getElementById('evalCanvas');
      if (!canvas) return;
      const ctx = canvas.getContext('2d');
      ctx.clearRect(0,0,canvas.width,canvas.height);
      if (evalHistory.length < 2) return;
      const min = Math.min(...evalHistory, -10);
      const max = Math.max(...evalHistory, 10);
      const range = max - min || 1;
      ctx.beginPath();
      ctx.moveTo(0, 20 - (evalHistory[0] - min) / range * 30);
      for (let i = 1; i < evalHistory.length; i++) {
        const x = i * (canvas.width / (evalHistory.length-1));
        const y = 20 - (evalHistory[i] - min) / range * 30;
        ctx.lineTo(x, y);
      }
      ctx.strokeStyle = "#0078d7";
      ctx.lineWidth = 2;
      ctx.stroke();
      // Null-Linie (Remis)
      ctx.beginPath();
      ctx.moveTo(0,20);
      ctx.lineTo(canvas.width,20);
      ctx.strokeStyle = "#bbb";
      ctx.lineWidth = 1;
      ctx.stroke();
    }

    // === Brett aufbauen und alles zurücksetzen ===
    function createBoard() {
      board.innerHTML = '';
      history = [];
      moveHistory.innerHTML = '';
      currentPlayer = 'white';
      pieces = Object.assign({}, initialPieces);
      boardStates = [JSON.parse(JSON.stringify(pieces))];
      historyPointer = 0;
      capturedWhite = [];
      capturedBlack = [];
      evalHistory = []; // Bewertungshistorie zurücksetzen
      updateCaptured();
      updateCurrentPlayerDisplay();
      updateCheckStatus();
      updateGameStatus();
      resetTimers();

      for (let rank = 8; rank >= 1; rank--) {
        for (let file = 0; file < 8; file++) {
          const square = document.createElement('div');
          const squareName = files[file] + rank;
          square.className = 'square ' + ((rank + file) % 2 === 0 ? 'white' : 'black');
          square.dataset.square = squareName;
          square.ondragover = allowDrop;
          square.ondrop = drop;

          if (pieces[squareName]) {
            const piece = document.createElement('img');
            piece.src = pieceImages[pieces[squareName]];
            piece.alt = pieces[squareName];
            piece.draggable = true;
            piece.ondragstart = drag;
            piece.onclick = highlightMoves;
            piece.style.width = '48px';
            piece.style.height = '48px';
            piece.dataset.symbol = pieces[squareName];
            square.appendChild(piece);
          }
          board.appendChild(square);
        }
      }
      showEvaluation();
    }

    // === Drag & Drop für Figuren ===
    function allowDrop(ev) { ev.preventDefault(); }

    let draggedPiece = null;
    let sourceSquare = null;

    function drag(ev) {
      if (!timersRunning) return;
      if (playMode === 'bot' && ((playerColor === 'white' && currentPlayer !== 'white') ||
          (playerColor === 'black' && currentPlayer !== 'black'))) return;
      draggedPiece = ev.target;
      sourceSquare = ev.target.parentElement.dataset.square;
      const symbol = draggedPiece.dataset.symbol;
      if (
        (currentPlayer === 'white' && !"♙♖♘♗♕♔".includes(symbol)) ||
        (currentPlayer === 'black' && !"♟♜♞♝♛♚".includes(symbol))
      ) {
        draggedPiece = null;
        sourceSquare = null;
        ev.preventDefault();
      }
      clearHighlights();
    }

    function drop(ev) {
      if (!timersRunning) return;
      if (playMode === 'bot' && ((playerColor === 'white' && currentPlayer !== 'white') ||
          (playerColor === 'black' && currentPlayer !== 'black'))) return;
      ev.preventDefault();
      if (!draggedPiece) return;
      const targetSquare = ev.currentTarget.dataset.square;
      const targetPieceEl = ev.currentTarget.querySelector('img');
      const targetSymbol = targetPieceEl ? targetPieceEl.dataset.symbol : null;
      const isCapture = !!targetPieceEl;

      if (
        (!isCapture || isEnemy(draggedPiece.dataset.symbol, targetSymbol)) &&
        isMoveValid(draggedPiece.dataset.symbol, sourceSquare, targetSquare, isCapture)
      ) {
        if (targetPieceEl) {
          if ("♙♖♘♗♕".includes(targetSymbol)) {
            capturedWhite.push(targetSymbol);
          } else if ("♟♜♞♝♛".includes(targetSymbol)) {
            capturedBlack.push(targetSymbol);
          }
          updateCaptured();
          targetPieceEl.remove();
        }
        ev.currentTarget.appendChild(draggedPiece);

        checkPromotion(draggedPiece, targetSquare, function() {
          logMove(draggedPiece.dataset.symbol, sourceSquare, targetSquare, targetSymbol);

          delete pieces[sourceSquare];
          pieces[targetSquare] = draggedPiece.dataset.symbol;

          nachZugSpeichern();

          currentPlayer = currentPlayer === 'white' ? 'black' : 'white';
          updateCurrentPlayerDisplay();
          updateCheckStatus();
          updateGameStatus();

          if (timersRunning) switchTimers();

          if (timersRunning && playMode === 'bot' && currentPlayer !== playerColor) {
            setTimeout(computerMove, 500);
          }
        });
      }
      draggedPiece = null;
      sourceSquare = null;
      clearHighlights();
    }

    // === Bauernumwandlung ===
    function checkPromotion(pieceEl, square, callback) {
      const symbol = pieceEl.dataset.symbol;
      const rank = parseInt(square[1]);
      if ((symbol === '♙' && rank === 8) || (symbol === '♟' && rank === 1)) {
        const color = symbol === '♙' ? 'white' : 'black';
        showPromotionDialog(color, function(newSymbol) {
          pieceEl.src = pieceImages[newSymbol];
          pieceEl.dataset.symbol = newSymbol;
          pieces[square] = newSymbol;
          if (callback) callback();
        });
      } else {
        if (callback) callback();
      }
    }

    function showPromotionDialog(color, cb) {
      promotionBox.innerHTML = '';
      promotionChoices[color].forEach(sym => {
        const img = document.createElement('img');
        img.src = pieceImages[sym];
        img.alt = sym;
        img.title = {
          '♕': 'Dame', '♖': 'Turm', '♗': 'Läufer', '♘': 'Springer',
          '♛': 'Dame', '♜': 'Turm', '♝': 'Läufer', '♞': 'Springer'
        }[sym];
        img.onclick = function() {
          promotionModal.style.display = 'none';
          cb(sym);
        };
        promotionBox.appendChild(img);
      });
      promotionModal.style.display = 'flex';
    }

    // === Zughistorie ===
    function logMove(symbol, from, to, captured) {
      const moveText = captured 
        ? `${symbol} ${from}x${to} ${captured}` 
        : `${symbol} ${from}→${to}`;
      history.push(moveText);
      const li = document.createElement('li');
      li.textContent = moveText;
      moveHistory.appendChild(li);
    }

    // === Hilfsfunktionen für Züge ===
    function isEnemy(symbol1, symbol2) {
      const white = '♙♖♘♗♕♔';
      const black = '♟♜♞♝♛♚';
      return (white.includes(symbol1) && black.includes(symbol2)) || (black.includes(symbol1) && white.includes(symbol2));
    }

    function isMoveValid(symbol, from, to, isCapture) {
      const fileFrom = from.charCodeAt(0);
      const rankFrom = parseInt(from[1]);
      const fileTo = to.charCodeAt(0);
      const rankTo = parseInt(to[1]);
      const fileDiff = fileTo - fileFrom;
      const rankDiff = rankTo - rankFrom;

      if (symbol === '♙') {
        if (isCapture) {
          if (Math.abs(fileDiff) === 1 && rankDiff === 1) return true;
        } else {
          if (fileDiff === 0 && (rankDiff === 1 || (rankFrom === 2 && rankDiff === 2))) return true;
        }
      } else if (symbol === '♟') {
        if (isCapture) {
          if (Math.abs(fileDiff) === 1 && rankDiff === -1) return true;
        } else {
          if (fileDiff === 0 && (rankDiff === -1 || (rankFrom === 7 && rankDiff === -2))) return true;
        }
      } else if (symbol === '♖' || symbol === '♜') {
        if ((fileDiff === 0 || rankDiff === 0) && isPathClear(from, to)) return true;
      } else if (symbol === '♘' || symbol === '♞') {
        if ((Math.abs(fileDiff) === 2 && Math.abs(rankDiff) === 1) ||
            (Math.abs(fileDiff) === 1 && Math.abs(rankDiff) === 2)) return true;
      } else if (symbol === '♗' || symbol === '♝') {
        if (Math.abs(fileDiff) === Math.abs(rankDiff) && isPathClear(from, to)) return true;
      } else if (symbol === '♕' || symbol === '♛') {
        if ((fileDiff === 0 || rankDiff === 0 || Math.abs(fileDiff) === Math.abs(rankDiff)) && isPathClear(from, to)) return true;
      } else if (symbol === '♔' || symbol === '♚') {
        if (Math.abs(fileDiff) <= 1 && Math.abs(rankDiff) <= 1) return true;
      }
      return false;
    }

    function isPathClear(from, to) {
      const fileFrom = from.charCodeAt(0);
      const rankFrom = parseInt(from[1]);
      const fileTo = to.charCodeAt(0);
      const rankTo = parseInt(to[1]);
      const fileStep = Math.sign(fileTo - fileFrom);
      const rankStep = Math.sign(rankTo - rankFrom);
      let file = fileFrom + fileStep;
      let rank = rankFrom + rankStep;
      while (file !== fileTo || rank !== rankTo) {
        const squareId = String.fromCharCode(file) + rank;
        if (pieces[squareId]) return false;
        file += fileStep;
        rank += rankStep;
      }
      return true;
    }

    // === Rückgängig machen ===
    function undoMove() {
      if (boardStates.length <= 1 || !timersRunning) return;
      boardStates.pop();
      pieces = JSON.parse(JSON.stringify(boardStates[boardStates.length - 1]));
      history.pop();
      moveHistory.removeChild(moveHistory.lastChild);
      recalculateCaptured();
      currentPlayer = currentPlayer === 'white' ? 'black' : 'white';
      updateCurrentPlayerDisplay();
      updateCheckStatus();
      updateGameStatus();
      board.innerHTML = '';
      for (let rank = 8; rank >= 1; rank--) {
        for (let file = 0; file < 8; file++) {
          const square = document.createElement('div');
          const squareName = files[file] + rank;
          square.className = 'square ' + ((rank + file) % 2 === 0 ? 'white' : 'black');
          square.dataset.square = squareName;
          square.ondragover = allowDrop;
          square.ondrop = drop;

          if (pieces[squareName]) {
            const piece = document.createElement('img');
            piece.src = pieceImages[pieces[squareName]];
            piece.alt = pieces[squareName];
            piece.draggable = true;
            piece.ondragstart = drag;
            piece.onclick = highlightMoves;
            piece.style.width = '48px';
            piece.style.height = '48px';
            piece.dataset.symbol = pieces[squareName];
            square.appendChild(piece);
          }
          board.appendChild(square);
        }
      }
      clearHighlights();
      updateCaptured();
      showEvaluation();
    }

    // === Rückspulfunktion (Züge vor/zurück) ===
    function showBoardAt(index) {
      if (index < 0 || index >= boardStates.length) return;
      pieces = JSON.parse(JSON.stringify(boardStates[index]));
      board.innerHTML = '';
      for (let rank = 8; rank >= 1; rank--) {
        for (let file = 0; file < 8; file++) {
          const square = document.createElement('div');
          const squareName = files[file] + rank;
          square.className = 'square ' + ((rank + file) % 2 === 0 ? 'white' : 'black');
          square.dataset.square = squareName;
          square.ondragover = allowDrop;
          square.ondrop = drop;

          if (pieces[squareName]) {
            const piece = document.createElement('img');
            piece.src = pieceImages[pieces[squareName]];
            piece.alt = pieces[squareName];
            piece.draggable = true;
            piece.ondragstart = drag;
            piece.onclick = highlightMoves;
            piece.style.width = '48px';
            piece.style.height = '48px';
            piece.dataset.symbol = pieces[squareName];
            square.appendChild(piece);
          }
          board.appendChild(square);
        }
      }
      clearHighlights();
      updateCaptured();
      currentPlayer = (index % 2 === 0) ? 'white' : 'black';
      updateCurrentPlayerDisplay();
      updateCheckStatus();
      updateGameStatus();
      showEvaluation();
    }

    // === Buttons für Rückspulen ===
    document.getElementById('backBtn').onclick = function() {
      if (historyPointer > 0) {
        historyPointer--;
        showBoardAt(historyPointer);
      }
    };
    document.getElementById('forwardBtn').onclick = function() {
      if (historyPointer < boardStates.length - 1) {
        historyPointer++;
        showBoardAt(historyPointer);
      }
    };

    // === Nach jedem Zug: Spielstand speichern ===
    function nachZugSpeichern() {
      boardStates = boardStates.slice(0, historyPointer + 1);
      boardStates.push(JSON.parse(JSON.stringify(pieces)));
      historyPointer = boardStates.length - 1;
      showEvaluation();
    }

    // === Mögliche Züge hervorheben ===
    function highlightMoves(ev) {
      clearHighlights();
      if (!timersRunning) return;
      if (playMode === 'bot' && ((playerColor === 'white' && currentPlayer !== 'white') ||
          (playerColor === 'black' && currentPlayer !== 'black'))) return;
      const pieceEl = ev.target;
      const from = pieceEl.parentElement.dataset.square;
      const symbol = pieceEl.dataset.symbol;
      if (
        (currentPlayer === 'white' && !"♙♖♘♗♕♔".includes(symbol)) ||
        (currentPlayer === 'black' && !"♟♜♞♝♛♚".includes(symbol))
      ) return;
      for (let file of files) {
        for (let rank = 1; rank <= 8; rank++) {
          const to = file + rank;
          if (from === to) continue;
          const target = pieces[to];
          const isCapture = !!target;
          if (isCapture && !isEnemy(symbol, target)) continue;
          if (isMoveValid(symbol, from, to, isCapture)) {
            const sq = document.querySelector(`[data-square='${to}']`);
            if (sq) {
              if (isCapture) {
                sq.classList.add('highlight-capture');
              } else {
                sq.classList.add('highlight');
              }
              highlightedSquares.push(sq);
            }
          }
        }
      }
    }

    function clearHighlights() {
      highlightedSquares.forEach(sq => {
        sq.classList.remove('highlight');
        sq.classList.remove('highlight-capture');
      });
      highlightedSquares = [];
    }

    // === Gefangene Figuren anzeigen ===
    function updateCaptured() {
      const cW = document.getElementById('capturedWhite');
      const cB = document.getElementById('capturedBlack');
      cW.innerHTML = '';
      cB.innerHTML = '';
      capturedWhite.forEach(sym => {
        const img = document.createElement('img');
        img.src = pieceImages[sym];
        img.alt = sym;
        img.style.width = '28px';
        img.style.height = '28px';
        cW.appendChild(img);
      });
      capturedBlack.forEach(sym => {
        const img = document.createElement('img');
        img.src = pieceImages[sym];
        img.alt = sym;
        img.style.width = '28px';
        img.style.height = '28px';
        cB.appendChild(img);
      });
    }

    // === Gefangene Figuren nach Rückgängig neu berechnen ===
    function recalculateCaptured() {
      capturedWhite = [];
      capturedBlack = [];
      const allWhite = ['♙','♖','♘','♗','♕'];
      const allBlack = ['♟','♜','♞','♝','♛'];
      const startWhite = { '♙':8, '♖':2, '♘':2, '♗':2, '♕':1 };
      const startBlack = { '♟':8, '♜':2, '♞':2, '♝':2, '♛':1 };
      const nowWhite = { '♙':0, '♖':0, '♘':0, '♗':0, '♕':0 };
      const nowBlack = { '♟':0, '♜':0, '♞':0, '♝':0, '♛':0 };
      for (const sq in pieces) {
        if (allWhite.includes(pieces[sq])) nowWhite[pieces[sq]]++;
        if (allBlack.includes(pieces[sq])) nowBlack[pieces[sq]]++;
      }
      allWhite.forEach(sym => {
        for (let i = 0; i < startWhite[sym] - nowWhite[sym]; i++) capturedWhite.push(sym);
      });
      allBlack.forEach(sym => {
        for (let i = 0; i < startBlack[sym] - nowBlack[sym]; i++) capturedBlack.push(sym);
      });
    }

    // === Zeitformatierung ===
    function formatTime(sec) {
      const m = Math.floor(sec / 60);
      const s = sec % 60;
      return (m < 10 ? "0" : "") + m + ":" + (s < 10 ? "0" : "") + s;
    }

    // === Timer-Anzeige ===
    function updateTimers() {
      whiteTimerDiv.textContent = "Weiß: " + formatTime(whiteTime);
      blackTimerDiv.textContent = "Schwarz: " + formatTime(blackTime);
      whiteTimerDiv.classList.toggle("out", whiteTime === 0);
      blackTimerDiv.classList.toggle("out", blackTime === 0);
    }

    // === Timer starten ===
    function startTimers() {
      if (timerInterval) clearInterval(timerInterval);
      timerInterval = setInterval(() => {
        if (!timersRunning) return;
        if (currentPlayer === "white") {
          if (whiteTime > 0) {
            whiteTime--;
            updateTimers();
            if (whiteTime === 0) {
              timersRunning = false;
              document.getElementById('gameStatus').textContent = "Weiß hat auf Zeit verloren!";
              stopTimers();
            }
          }
        } else {
          if (blackTime > 0) {
            blackTime--;
            updateTimers();
            if (blackTime === 0) {
              timersRunning = false;
              document.getElementById('gameStatus').textContent = "Schwarz hat auf Zeit verloren!";
              stopTimers();
            }
          }
        }
      }, 1000);
      updateTimers();
    }

    // === Timer stoppen ===
    function stopTimers() {
      if (timerInterval) clearInterval(timerInterval);
      updateTimers();
    }

    // === Timer wechseln nach Zug ===
    function switchTimers() {
      updateTimers();
      startTimers();
    }

    // === Timer zurücksetzen ===
    function resetTimers() {
      whiteTime = 15 * 60;
      blackTime = 15 * 60;
      updateTimers();
      stopTimers();
    }

    // === Bot-Zug mit Schwierigkeitsgrad ===
    function computerMove() {
      if (!timersRunning || playMode !== 'bot' || currentPlayer === playerColor) return;
      const kiColor = currentPlayer;
      const kiPieces = kiColor === 'white'
        ? ['♙','♖','♘','♗','♕','♔']
        : ['♟','♜','♞','♝','♛','♚'];
      const moves = [];
      for (const from in pieces) {
        if (!kiPieces.includes(pieces[from])) continue;
        for (let file of files) {
          for (let rank = 1; rank <= 8; rank++) {
            const to = file + rank;
            if (from === to) continue;
            const target = pieces[to];
            const isCapture = !!target;
            if (isCapture && !isEnemy(pieces[from], target)) continue;
            if (isMoveValid(pieces[from], from, to, isCapture)) {
              const backupFrom = pieces[from];
              const backupTo = pieces[to];
              pieces[to] = pieces[from];
              delete pieces[from];
              const inCheck = isKingInCheck(kiColor);
              pieces[from] = backupFrom;
              if (backupTo) pieces[to] = backupTo; else delete pieces[to];
              if (!inCheck) moves.push({from, to, symbol: backupFrom, captured: backupTo});
            }
          }
        }
      }
      if (moves.length === 0) return;

      let move;
      if (botLevel === 0) {
        // Leicht: Zufall
        move = moves[Math.floor(Math.random() * moves.length)];
      } else if (botLevel === 1) {
        // Mittel: Bevorzuge Schlagen, sonst Zufall
        const captures = moves.filter(m => m.captured);
        move = (captures.length > 0) ? captures[Math.floor(Math.random() * captures.length)] : moves[Math.floor(Math.random() * moves.length)];
      } else {
        // Stark: Wähle Zug mit bester Materialbewertung
        let bestScore = kiColor === 'white' ? -Infinity : Infinity;
        let bestMoves = [];
        for (const m of moves) {
          // Simuliere Zug
          const backupFrom = pieces[m.from];
          const backupTo = pieces[m.to];
          pieces[m.to] = pieces[m.from];
          delete pieces[m.from];
          const score = evaluatePosition();
          pieces[m.from] = backupFrom;
          if (backupTo) pieces[m.to] = backupTo; else delete pieces[m.to];
          if ((kiColor === 'white' && score > bestScore) || (kiColor === 'black' && score < bestScore)) {
            bestScore = score;
            bestMoves = [m];
          } else if (score === bestScore) {
            bestMoves.push(m);
          }
        }
        move = bestMoves[Math.floor(Math.random() * bestMoves.length)];
      }

      const fromSquare = document.querySelector(`[data-square='${move.from}']`);
      const toSquare = document.querySelector(`[data-square='${move.to}']`);
      const pieceEl = fromSquare.querySelector('img');
      const targetPieceEl = toSquare.querySelector('img');
      if (targetPieceEl) {
        if ("♙♖♘♗♕".includes(targetPieceEl.dataset.symbol)) {
          capturedWhite.push(targetPieceEl.dataset.symbol);
        } else if ("♟♜♞♝♛".includes(targetPieceEl.dataset.symbol)) {
          capturedBlack.push(targetPieceEl.dataset.symbol);
        }
        updateCaptured();
        toSquare.removeChild(targetPieceEl);
      }
      toSquare.appendChild(pieceEl);
      checkPromotion(pieceEl, move.to, function() {
        logMove(move.symbol, move.from, move.to, move.captured ? move.captured : null);
        delete pieces[move.from];
        pieces[move.to] = pieceEl.dataset.symbol;
        nachZugSpeichern();
        currentPlayer = currentPlayer === 'white' ? 'black' : 'white';
        updateCurrentPlayerDisplay();
        updateCheckStatus();
        updateGameStatus();
        if (timersRunning) switchTimers();
      });
    }

    // === Modus-Auswahl (Bot/Freund) ===
    document.getElementById('botBtn').onclick = function() {
      playMode = 'bot';
      this.classList.add('active');
      document.getElementById('friendBtn').classList.remove('active');
      document.getElementById('colorSelect').style.display = '';
      document.getElementById('botLevelSelect').style.display = '';
    };
    document.getElementById('friendBtn').onclick = function() {
      playMode = 'friend';
      this.classList.add('active');
      document.getElementById('botBtn').classList.remove('active');
      document.getElementById('colorSelect').style.display = 'none';
      document.getElementById('botLevelSelect').style.display = 'none';
      playerColor = null;
      document.getElementById('colorSelect').querySelectorAll('button').forEach(b => b.classList.remove('active'));
      createBoard();
    };

    // === Farbwahl für Bot-Spieler ===
    document.getElementById('playWhiteBtn').onclick = function() {
      playerColor = 'white';
      this.classList.add('active');
      document.getElementById('playBlackBtn').classList.remove('active');
      document.getElementById('colorSelect').style.display = 'none';
      createBoard();
      if (currentPlayer !== playerColor && playMode === 'bot') setTimeout(computerMove, 500);
    };
    document.getElementById('playBlackBtn').onclick = function() {
      playerColor = 'black';
      this.classList.add('active');
      document.getElementById('playWhiteBtn').classList.remove('active');
      document.getElementById('colorSelect').style.display = 'none';
      createBoard();
      if (currentPlayer !== playerColor && playMode === 'bot') setTimeout(computerMove, 500);
    };

    // === Steuerungs-Buttons ===
    document.getElementById('resetBtn').addEventListener('click', function() {
      if (playMode === 'bot') {
        document.getElementById('colorSelect').style.display = '';
        playerColor = null;
        document.getElementById('colorSelect').querySelectorAll('button').forEach(b => b.classList.remove('active'));
      }
      stopTimers();
      board.innerHTML = '';
      moveHistory.innerHTML = '';
      document.getElementById('gameStatus').textContent = '';
      document.getElementById('checkStatus').textContent = '';
      document.getElementById('currentPlayer').textContent = '';
      whiteTimerDiv.textContent = "Weiß: 15:00";
      blackTimerDiv.textContent = "Schwarz: 15:00";
      clearHighlights();
      capturedWhite = [];
      capturedBlack = [];
      updateCaptured();
      showEvaluation();
    });
    document.getElementById('undoBtn').addEventListener('click', undoMove);

    // === Start/Stop Button Logik ===
    document.addEventListener('DOMContentLoaded', function() {
      document.getElementById('startBtn').onclick = function() {
        timersRunning = true;
        startTimers();
      };
      document.getElementById('stopBtn').onclick = function() {
        timersRunning = false;
        stopTimers();
      };
    });

    // === Zeit läuft erst nach Start ===
    window.onload = function() {
      createBoard();
      timersRunning = false;
      stopTimers();
    };

    // === Touch-Bedienung für Mobilgeräte ===
    let touchSelectedPiece = null;
    let touchSourceSquare = null;
    board.addEventListener('touchstart', function(ev) {
      const target = ev.target;
      if (target.tagName === 'IMG') {
        touchSelectedPiece = target;
        touchSourceSquare = target.parentElement.dataset.square;
        clearHighlights();
        highlightMoves({target});
        ev.preventDefault();
      } else if (target.classList.contains('square') && touchSelectedPiece) {
        const targetSquare = target.dataset.square;
        const targetPieceEl = target.querySelector('img');
        const targetSymbol = targetPieceEl ? targetPieceEl.dataset.symbol : null;
        const isCapture = !!targetPieceEl;
        if (
          (!isCapture || isEnemy(touchSelectedPiece.dataset.symbol, targetSymbol)) &&
          isMoveValid(touchSelectedPiece.dataset.symbol, touchSourceSquare, targetSquare, isCapture)
        ) {
          if (targetPieceEl) {
            if ("♙♖♘♗♕".includes(targetSymbol)) {
              capturedWhite.push(targetSymbol);
            } else if ("♟♜♞♝♛".includes(targetSymbol)) {
              capturedBlack.push(targetSymbol);
            }
            updateCaptured();
            targetPieceEl.remove();
          }
          target.appendChild(touchSelectedPiece);
          checkPromotion(touchSelectedPiece, targetSquare, function() {
            logMove(touchSelectedPiece.dataset.symbol, touchSourceSquare, targetSquare, targetSymbol);
            delete pieces[touchSourceSquare];
            pieces[targetSquare] = touchSelectedPiece.dataset.symbol;
            nachZugSpeichern();
            currentPlayer = currentPlayer === 'white' ? 'black' : 'white';
            updateCurrentPlayerDisplay();
            updateCheckStatus();
            updateGameStatus();
            if (timersRunning) switchTimers();
            if (timersRunning && playMode === 'bot' && currentPlayer !== playerColor) {
              setTimeout(computerMove, 500);
            }
          });
          touchSelectedPiece = null;
          touchSourceSquare = null;
          clearHighlights();
          ev.preventDefault();
        }
      }
    }, {passive: false});
  </script>
</body>
</html>
