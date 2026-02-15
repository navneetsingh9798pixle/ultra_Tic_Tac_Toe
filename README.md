<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Ultra Tic Tac Toe</title>
<script src="https://cdn.tailwindcss.com"></script>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">
<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  -webkit-tap-highlight-color: transparent;
  touch-action: manipulation;
}

html, body {
  width: 100%;
  height: 100%;
  overflow: hidden;
  font-family: 'Inter', sans-serif;
  background: #0f0c29;
}

/* Screens */
.screen {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow-y: auto;
  overflow-x: hidden;
  display: none;
  background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
}

.screen.active {
  display: block;
}

/* Menu Screen */
.menu-content {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
  padding-bottom: 100px;
}

.menu-title {
  font-size: clamp(2rem, 8vw, 3rem);
  font-weight: 800;
  background: linear-gradient(135deg, #667eea 0%, #f472b6 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  margin-bottom: 8px;
  text-align: center;
}

.menu-subtitle {
  color: rgba(255,255,255,0.6);
  margin-bottom: 40px;
  font-size: 0.9rem;
}

.btn {
  width: min(280px, 80vw);
  padding: 16px;
  margin: 8px 0;
  border-radius: 12px;
  border: none;
  font-size: 1rem;
  font-weight: 700;
  cursor: pointer;
  transition: transform 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.btn:active {
  transform: scale(0.95);
}

.btn-primary {
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
}

.btn-secondary {
  background: rgba(255,255,255,0.1);
  color: white;
  border: 1px solid rgba(255,255,255,0.2);
}

/* Game Screen */
.game-content {
  min-height: 100vh;
  padding: 16px;
  padding-bottom: 100px;
}

/* Stats */
.stats {
  display: flex;
  justify-content: center;
  gap: 8px;
  margin-bottom: 16px;
  flex-wrap: wrap;
}

.stat {
  background: rgba(255,255,255,0.1);
  padding: 10px 16px;
  border-radius: 12px;
  text-align: center;
  min-width: 70px;
}

.stat-num {
  font-size: 1.2rem;
  font-weight: 800;
}

.stat-label {
  font-size: 0.65rem;
  opacity: 0.7;
  text-transform: uppercase;
}

/* Mode Selector */
.modes {
  display: flex;
  gap: 8px;
  overflow-x: auto;
  margin-bottom: 16px;
  padding-bottom: 4px;
  -webkit-overflow-scrolling: touch;
}

.mode-btn {
  flex-shrink: 0;
  padding: 8px 16px;
  border-radius: 20px;
  border: 1px solid rgba(255,255,255,0.2);
  background: rgba(255,255,255,0.05);
  color: white;
  font-size: 0.85rem;
  cursor: pointer;
  white-space: nowrap;
}

.mode-btn.active {
  background: linear-gradient(135deg, #667eea, #764ba2);
  border-color: transparent;
}

/* Status */
.status-box {
  text-align: center;
  margin-bottom: 16px;
}

.status {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: rgba(255,255,255,0.1);
  padding: 8px 16px;
  border-radius: 20px;
  font-weight: 600;
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #60a5fa;
  box-shadow: 0 0 8px #60a5fa;
}

/* BOARD - FIXED */
#board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 8px;
  width: min(95vw, 350px);
  height: min(95vw, 350px);
  margin: 0 auto 20px;
  padding: 10px;
  background: rgba(255,255,255,0.05);
  border-radius: 16px;
}

.cell {
  background: rgba(255,255,255,0.1);
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 2rem;
  font-weight: 800;
  cursor: pointer;
  border: 1px solid rgba(255,255,255,0.05);
  transition: all 0.2s;
  /* Ensure visible */
  min-height: 0;
  min-width: 0;
}

.cell:active:not(.taken) {
  background: rgba(255,255,255,0.2);
  transform: scale(0.95);
}

.cell.x { color: #60a5fa; text-shadow: 0 0 10px #60a5fa; }
.cell.o { color: #f472b6; text-shadow: 0 0 10px #f472b6; }
.cell.win { animation: pulse 1s infinite; background: rgba(255,255,255,0.3); }

@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.05); }
}

/* Bottom Nav */
.bottom-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0,0,0,0.9);
  backdrop-filter: blur(10px);
  display: flex;
  justify-content: space-around;
  padding: 10px;
  padding-bottom: calc(10px + env(safe-area-inset-bottom));
  z-index: 100;
}

.nav-btn {
  background: none;
  border: none;
  color: rgba(255,255,255,0.7);
  font-size: 0.7rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  cursor: pointer;
  padding: 5px 15px;
}

.nav-btn svg {
  width: 20px;
  height: 20px;
}

/* Settings Modal */
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.9);
  display: none;
  align-items: center;
  justify-content: center;
  z-index: 200;
  padding: 20px;
}

.modal-overlay.show {
  display: flex;
}

.modal {
  background: linear-gradient(135deg, #1a1a2e, #16213e);
  border-radius: 20px;
  padding: 20px;
  width: 100%;
  max-width: 340px;
  max-height: 80vh;
  overflow-y: auto;
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.close-x {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  background: rgba(255,255,255,0.1);
  border: none;
  color: white;
  font-size: 18px;
  cursor: pointer;
}

.setting-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 14px;
  background: rgba(255,255,255,0.05);
  border-radius: 12px;
  margin-bottom: 10px;
}

.toggle {
  width: 44px;
  height: 24px;
  background: rgba(255,255,255,0.2);
  border-radius: 12px;
  position: relative;
  cursor: pointer;
  transition: 0.3s;
}

.toggle.on {
  background: linear-gradient(135deg, #667eea, #764ba2);
}

.toggle::after {
  content: '';
  position: absolute;
  top: 2px;
  left: 2px;
  width: 20px;
  height: 20px;
  background: white;
  border-radius: 50%;
  transition: 0.3s;
}

.toggle.on::after {
  transform: translateX(20px);
}

.themes {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  margin-top: 10px;
}

.theme-box {
  aspect-ratio: 1;
  border-radius: 10px;
  cursor: pointer;
  border: 2px solid transparent;
}

.theme-box.active {
  border-color: white;
}

.back-btn {
  position: fixed;
  top: 10px;
  left: 10px;
  width: 40px;
  height: 40px;
  border-radius: 10px;
  background: rgba(255,255,255,0.1);
  border: none;
  color: white;
  cursor: pointer;
  z-index: 50;
  display: none;
}

.back-btn.show {
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Ad */
.ad-box {
  margin: 20px auto;
  text-align: center;
  min-height: 50px;
  background: rgba(0,0,0,0.2);
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* Confetti */
#confetti {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 300;
}

.text-white { color: white; }
.text-green { color: #4ade80; }
.text-red { color: #f87171; }
.text-yellow { color: #fbbf24; }
.text-blue { color: #60a5fa; }
</style>
</head>
<body>

<canvas id="confetti"></canvas>

<!-- MENU SCREEN -->
<div class="screen active" id="menuScreen">
  <div class="menu-content">
    <h1 class="menu-title">TIC TAC TOE</h1>
    <p class="menu-subtitle">Ultra Edition</p>
    
    <button class="btn btn-primary" onclick="startGame()">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <polygon points="5 3 19 12 5 21 5 3"></polygon>
      </svg>
      PLAY NOW
    </button>
    
    <button class="btn btn-secondary" onclick="openSettings()">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <circle cx="12" cy="12" r="3"></circle>
        <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"></path>
      </svg>
      SETTINGS
    </button>
    
    <div class="ad-box">
      <ins class="adsbygoogle" style="display:inline-block;width:320px;height:50px" data-ad-client="ca-pub-6851464163673391" data-ad-slot="9423081212"></ins>
    </div>
  </div>
</div>

<!-- GAME SCREEN -->
<div class="screen" id="gameScreen">
  <button class="back-btn" id="backBtn" onclick="backToMenu()">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <path d="M19 12H5M12 19l-7-7 7-7"></path>
    </svg>
  </button>
  
  <div class="game-content">
    <div class="stats">
      <div class="stat">
        <div class="stat-num text-green" id="wins">0</div>
        <div class="stat-label">Wins</div>
      </div>
      <div class="stat">
        <div class="stat-num text-red" id="losses">0</div>
        <div class="stat-label">Losses</div>
      </div>
      <div class="stat">
        <div class="stat-num text-yellow" id="draws">0</div>
        <div class="stat-label">Draws</div>
      </div>
      <div class="stat">
        <div class="stat-num text-blue" id="level">1</div>
        <div class="stat-label">Level</div>
      </div>
    </div>
    
    <div class="modes">
      <button class="mode-btn active" onclick="setMode('ai-hard', this)">🔥 Hard AI</button>
      <button class="mode-btn" onclick="setMode('ai-easy', this)">🤖 Easy</button>
      <button class="mode-btn" onclick="setMode('2p', this)">👥 2 Players</button>
    </div>
    
    <div class="status-box">
      <div class="status">
        <div class="dot" id="turnDot"></div>
        <span id="statusText">Your Turn</span>
      </div>
    </div>
    
    <!-- BOARD -->
    <div id="board"></div>
    
    <div class="ad-box">
      <ins class="adsbygoogle" style="display:inline-block;width:300px;height:250px" data-ad-client="ca-pub-6851464163673391" data-ad-slot="9423081212"></ins>
    </div>
  </div>
</div>

<!-- SETTINGS MODAL -->
<div class="modal-overlay" id="settingsModal" onclick="closeSettings(event)">
  <div class="modal" onclick="event.stopPropagation()">
    <div class="modal-header">
      <h2 class="text-white font-bold text-lg">Settings</h2>
      <button class="close-x" onclick="closeSettings()">✕</button>
    </div>
    
    <div class="setting-row">
      <span class="text-white text-sm">Sound Effects</span>
      <div class="toggle on" id="soundToggle" onclick="toggleSetting('sound')"></div>
    </div>
    
    <div class="setting-row">
      <span class="text-white text-sm">Vibration</span>
      <div class="toggle on" id="vibToggle" onclick="toggleSetting('vibration')"></div>
    </div>
    
    <div class="text-white text-sm mb-2">Theme</div>
    <div class="themes">
      <div class="theme-box active" style="background: linear-gradient(135deg, #0f0c29, #302b63)" onclick="setTheme('default', this)"></div>
      <div class="theme-box" style="background: linear-gradient(135deg, #1a1a2e, #e94560)" onclick="setTheme('fire', this)"></div>
      <div class="theme-box" style="background: linear-gradient(135deg, #16213e, #0f3460)" onclick="setTheme('ocean', this)"></div>
      <div class="theme-box" style="background: linear-gradient(135deg, #1b4332, #2d6a4f)" onclick="setTheme('forest', this)"></div>
      <div class="theme-box" style="background: linear-gradient(135deg, #ff6b6b, #feca57)" onclick="setTheme('sunset', this)"></div>
      <div class="theme-box" style="background: linear-gradient(135deg, #2c3e50, #34495e)" onclick="setTheme('midnight', this)"></div>
    </div>
    
    <button class="btn btn-secondary mt-4" onclick="resetData()" style="width:100%; margin-top:16px; color:#f87171; border-color:#f87171;">
      Reset All Progress
    </button>
  </div>
</div>

<!-- BOTTOM NAV -->
<div class="bottom-nav" id="bottomNav" style="display:none;">
  <button class="nav-btn" onclick="newGame()">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <polygon points="5 3 19 12 5 21 5 3"></polygon>
    </svg>
    <span>New</span>
  </button>
  <button class="nav-btn" onclick="openSettings()">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <circle cx="12" cy="12" r="3"></circle>
      <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"></path>
    </svg>
    <span>Settings</span>
  </button>
  <button class="nav-btn" onclick="backToMenu()">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
    </svg>
    <span>Menu</span>
  </button>
</div>

<script>
// Game State
let board = Array(9).fill('');
let currentPlayer = 'X';
let gameActive = true;
let gameMode = 'ai-hard';
let data = { wins: 0, losses: 0, draws: 0, xp: 0, level: 1 };
let settings = { sound: true, vibration: true, theme: 'default' };

const wins = [[0,1,2],[3,4,5],[6,7,8],[0,3,6],[1,4,7],[2,5,8],[0,4,8],[2,4,6]];

// Initialize
window.onload = function() {
  loadData();
  loadSettings();
  initAds();
};

function initAds() {
  try { (adsbygoogle = window.adsbygoogle || []).push({}); } catch(e) {}
}

// Navigation
function startGame() {
  document.getElementById('menuScreen').classList.remove('active');
  document.getElementById('gameScreen').classList.add('active');
  document.getElementById('backBtn').classList.add('show');
  document.getElementById('bottomNav').style.display = 'flex';
  createBoard();
  updateStats();
}

function backToMenu() {
  document.getElementById('gameScreen').classList.remove('active');
  document.getElementById('menuScreen').classList.add('active');
  document.getElementById('backBtn').classList.remove('show');
  document.getElementById('bottomNav').style.display = 'none';
}

// Settings
function openSettings() {
  document.getElementById('settingsModal').classList.add('show');
}

function closeSettings(e) {
  if (!e || e.target === document.getElementById('settingsModal')) {
    document.getElementById('settingsModal').classList.remove('show');
  }
}

function toggleSetting(type) {
  if (type === 'sound') {
    settings.sound = !settings.sound;
    document.getElementById('soundToggle').classList.toggle('on');
  } else {
    settings.vibration = !settings.vibration;
    document.getElementById('vibToggle').classList.toggle('on');
  }
  saveSettings();
}

function setTheme(name, el) {
  settings.theme = name;
  document.querySelectorAll('.theme-box').forEach(b => b.classList.remove('active'));
  el.classList.add('active');
  
  const themes = {
    'default': 'linear-gradient(135deg, #0f0c29, #302b63, #24243e)',
    'fire': 'linear-gradient(135deg, #1a1a2e, #e94560, #0f3460)',
    'ocean': 'linear-gradient(135deg, #16213e, #0f3460, #e94560)',
    'forest': 'linear-gradient(135deg, #1b4332, #2d6a4f, #40916c)',
    'sunset': 'linear-gradient(135deg, #ff6b6b, #feca57, #48dbfb)',
    'midnight': 'linear-gradient(135deg, #2c3e50, #34495e, #2c3e50)'
  };
  
  document.querySelectorAll('.screen').forEach(s => s.style.background = themes[name]);
  saveSettings();
}

// Game Logic
function createBoard() {
  const b = document.getElementById('board');
  b.innerHTML = '';
  for (let i = 0; i < 9; i++) {
    const cell = document.createElement('div');
    cell.className = 'cell';
    cell.onclick = () => clickCell(i);
    b.appendChild(cell);
  }
  board = Array(9).fill('');
  currentPlayer = 'X';
  gameActive = true;
  updateStatus();
}

function clickCell(i) {
  if (!gameActive || board[i]) return;
  
  // Sound & Vibration
  if (settings.sound) playBeep();
  if (settings.vibration && navigator.vibrate) navigator.vibrate(30);
  
  // Make move
  board[i] = currentPlayer;
  const cells = document.querySelectorAll('.cell');
  cells[i].textContent = currentPlayer;
  cells[i].classList.add('taken', currentPlayer.toLowerCase());
  
  // Check win
  if (checkWin()) return;
  
  // Switch turn
  currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
  updateStatus();
  
  // AI Move
  if (gameMode !== '2p' && currentPlayer === 'O') {
    document.getElementById('board').style.pointerEvents = 'none';
    setTimeout(() => {
      aiMove();
      document.getElementById('board').style.pointerEvents = 'auto';
    }, 400);
  }
}

function aiMove() {
  if (!gameActive) return;
  
  let move;
  if (gameMode === 'ai-easy') {
    const empty = board.map((v,i) => v ? null : i).filter(v => v !== null);
    move = empty[Math.floor(Math.random() * empty.length)];
  } else {
    move = bestMove();
  }
  clickCell(move);
}

function bestMove() {
  let best = -1000, move = 0;
  for (let i = 0; i < 9; i++) {
    if (!board[i]) {
      board[i] = 'O';
      let score = minimax(0, false);
      board[i] = '';
      if (score > best) { best = score; move = i; }
    }
  }
  return move;
}

function minimax(depth, isMax) {
  let res = checkWinner();
  if (res === 'O') return 10 - depth;
  if (res === 'X') return depth - 10;
  if (res === 'draw') return 0;
  
  if (isMax) {
    let best = -1000;
    for (let i = 0; i < 9; i++) {
      if (!board[i]) {
        board[i] = 'O';
        best = Math.max(best, minimax(depth+1, false));
        board[i] = '';
      }
    }
    return best;
  } else {
    let best = 1000;
    for (let i = 0; i < 9; i++) {
      if (!board[i]) {
        board[i] = 'X';
        best = Math.min(best, minimax(depth+1, true));
        board[i] = '';
      }
    }
    return best;
  }
}

function checkWinner() {
  for (let [a,b,c] of wins) {
    if (board[a] && board[a] === board[b] && board[a] === board[c]) return board[a];
  }
  if (!board.includes('')) return 'draw';
  return null;
}

function checkWin() {
  const w = checkWinner();
  if (!w) return false;
  
  gameActive = false;
  
  if (w === 'draw') {
    document.getElementById('statusText').textContent = "Draw! 🤝";
    data.draws++;
    data.xp += 5;
  } else {
    // Highlight win
    for (let [a,b,c] of wins) {
      if (board[a] === w && board[b] === w && board[c] === w) {
        const cells = document.querySelectorAll('.cell');
        cells[a].classList.add('win');
        cells[b].classList.add('win');
        cells[c].classList.add('win');
      }
    }
    
    if (w === 'X') {
      document.getElementById('statusText').textContent = "You Win! 🎉";
      data.wins++;
      data.xp += 25;
      if (settings.sound) playWin();
      if (settings.vibration && navigator.vibrate) navigator.vibrate([50,100,50,100,200]);
      confetti();
    } else {
      document.getElementById('statusText').textContent = "AI Wins! 🤖";
      data.losses++;
      data.xp += 5;
    }
  }
  
  // Level up
  const newLvl = Math.floor(data.xp / 100) + 1;
  if (newLvl > data.level) {
    data.level = newLvl;
    setTimeout(() => document.getElementById('statusText').textContent = `Level Up! ${newLvl} 🆙`, 1000);
  }
  
  saveData();
  updateStats();
  
  // Auto reset
  setTimeout(() => newGame(), 3000);
  return true;
}

function updateStatus() {
  const isX = currentPlayer === 'X';
  document.getElementById('statusText').textContent = gameMode === '2p' ? `Player ${currentPlayer}` : isX ? 'Your Turn' : 'AI Thinking...';
  document.getElementById('turnDot').style.background = isX ? '#60a5fa' : '#f472b6';
  document.getElementById('turnDot').style.boxShadow = `0 0 8px ${isX ? '#60a5fa' : '#f472b6'}`;
}

function setMode(mode, btn) {
  gameMode = mode;
  document.querySelectorAll('.mode-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  newGame();
}

function newGame() {
  createBoard();
  try { (adsbygoogle = window.adsbygoogle || []).push({}); } catch(e) {}
}

function updateStats() {
  document.getElementById('wins').textContent = data.wins;
  document.getElementById('losses').textContent = data.losses;
  document.getElementById('draws').textContent = data.draws;
  document.getElementById('level').textContent = data.level;
}

// Audio
const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

function playBeep() {
  if (audioCtx.state === 'suspended') audioCtx.resume();
  const osc = audioCtx.createOscillator();
  const gain = audioCtx.createGain();
  osc.connect(gain);
  gain.connect(audioCtx.destination);
  osc.frequency.setValueAtTime(800, audioCtx.currentTime);
  osc.frequency.exponentialRampToValueAtTime(400, audioCtx.currentTime + 0.1);
  gain.gain.setValueAtTime(0.2, audioCtx.currentTime);
  gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.1);
  osc.start();
  osc.stop(audioCtx.currentTime + 0.1);
}

function playWin() {
  if (audioCtx.state === 'suspended') audioCtx.resume();
  [523, 659, 784].forEach((f, i) => {
    const osc = audioCtx.createOscillator();
    const gain = audioCtx.createGain();
    osc.connect(gain);
    gain.connect(audioCtx.destination);
    osc.frequency.value = f;
    gain.gain.setValueAtTime(0.15, audioCtx.currentTime + i*0.1);
    gain.gain.linearRampToValueAtTime(0, audioCtx.currentTime + i*0.1 + 0.3);
    osc.start(audioCtx.currentTime + i*0.1);
    osc.stop(audioCtx.currentTime + i*0.1 + 0.3);
  });
}

// Confetti
function confetti() {
  const c = document.getElementById('confetti');
  const ctx = c.getContext('2d');
  c.width = window.innerWidth;
  c.height = window.innerHeight;
  
  const parts = [];
  const colors = ['#60a5fa', '#f472b6', '#fbbf24', '#34d399'];
  
  for (let i = 0; i < 50; i++) {
    parts.push({
      x: c.width/2, y: c.height/2,
      vx: (Math.random()-0.5)*10, vy: (Math.random()-0.5)*10-5,
      life: 1, color: colors[Math.floor(Math.random()*colors.length)],
      size: Math.random()*4+2
    });
  }
  
  function anim() {
    if (!parts.length) return;
    ctx.clearRect(0,0,c.width,c.height);
    parts.forEach((p,i) => {
      p.x += p.vx; p.y += p.vy; p.vy += 0.3;
      p.life -= 0.02; p.size *= 0.98;
      ctx.fillStyle = p.color;
      ctx.globalAlpha = p.life;
      ctx.beginPath();
      ctx.arc(p.x,p.y,p.size,0,Math.PI*2);
      ctx.fill();
      if (p.life <= 0) parts.splice(i,1);
    });
    requestAnimationFrame(anim);
  }
  anim();
}

// Storage
function saveData() { localStorage.setItem('ttt_data', JSON.stringify(data)); }
function loadData() { 
  const d = localStorage.getItem('ttt_data'); 
  if (d) data = JSON.parse(d); 
}
function saveSettings() { localStorage.setItem('ttt_settings', JSON.stringify(settings)); }
function loadSettings() {
  const s = localStorage.getItem('ttt_settings');
  if (s) {
    settings = JSON.parse(s);
    document.getElementById('soundToggle').classList.toggle('on', settings.sound);
    document.getElementById('vibToggle').classList.toggle('on', settings.vibration);
    if (settings.theme) {
      const t = document.querySelector(`[onclick="setTheme('${settings.theme}', this)"]`);
      if (t) setTheme(settings.theme, t);
    }
  }
}
function resetData() {
  if (confirm('Reset all?')) {
    localStorage.clear();
    location.reload();
  }
}

// Prevent zoom
let lastTouch = 0;
document.addEventListener('touchend', e => {
  const now = Date.now();
  if (now - lastTouch <= 300) e.preventDefault();
  lastTouch = now;
}, {passive: false});
</script>

</body>
</html>

