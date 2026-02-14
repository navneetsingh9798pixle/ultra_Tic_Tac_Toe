<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<meta name="theme-color" content="#0f0c29">
<title>Ultra Tic Tac Toe</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">

<!-- Google AdMob/AdSense Script -->
<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-6851464163673391" crossorigin="anonymous"></script>

<style>
* {
  -webkit-tap-highlight-color: transparent;
  -webkit-touch-callout: none;
  user-select: none;
}

body {
  font-family: 'Inter', sans-serif;
  min-height: 100vh;
  min-height: 100dvh;
  background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
  overflow-x: hidden;
  padding-bottom: 80px;
}

@supports (padding-top: env(safe-area-inset-top)) {
  .safe-top { padding-top: env(safe-area-inset-top); }
  .safe-bottom { padding-bottom: env(safe-area-inset-bottom); }
}

.glass {
  background: rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.3);
}

#board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: clamp(8px, 2vw, 12px);
  width: min(95vw, 380px);
  aspect-ratio: 1;
  margin: 0 auto;
  padding: clamp(8px, 2vw, 12px);
  background: rgba(255, 255, 255, 0.05);
  border-radius: 20px;
  box-shadow: inset 0 2px 10px rgba(0, 0, 0, 0.3);
}

.cell {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: clamp(2rem, 8vw, 3rem);
  font-weight: 800;
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  position: relative;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.05);
}

.cell:active:not(.taken) {
  transform: scale(0.95);
  background: rgba(255, 255, 255, 0.2);
}

.cell.x {
  color: #60a5fa;
  text-shadow: 0 0 20px rgba(96, 165, 250, 0.6);
  animation: popIn 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.cell.o {
  color: #f472b6;
  text-shadow: 0 0 20px rgba(244, 114, 182, 0.6);
  animation: popIn 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.cell.taken {
  cursor: default;
}

.cell.win {
  background: rgba(255, 255, 255, 0.25);
  animation: winPulse 1s ease infinite;
  z-index: 1;
}

@keyframes popIn {
  0% { transform: scale(0) rotate(-180deg); opacity: 0; }
  100% { transform: scale(1) rotate(0deg); opacity: 1; }
}

@keyframes winPulse {
  0%, 100% { transform: scale(1); box-shadow: 0 0 20px currentColor; }
  50% { transform: scale(1.05); box-shadow: 0 0 40px currentColor; }
}

.bottom-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(15, 12, 41, 0.95);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  padding: 8px 20px calc(8px + env(safe-area-inset-bottom));
  display: flex;
  justify-content: space-around;
  align-items: center;
  z-index: 100;
}

.nav-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 4px;
  padding: 8px 16px;
  border-radius: 12px;
  transition: all 0.2s;
  color: rgba(255, 255, 255, 0.6);
  font-size: 11px;
  font-weight: 600;
  min-width: 64px;
}

.nav-btn:active {
  transform: scale(0.95);
  background: rgba(255, 255, 255, 0.1);
}

.nav-btn.active {
  color: #60a5fa;
  background: rgba(96, 165, 250, 0.15);
}

.nav-btn svg {
  width: 24px;
  height: 24px;
  stroke-width: 2;
}

.mode-scroll {
  display: flex;
  gap: 8px;
  overflow-x: auto;
  padding: 4px;
  margin: 0 -4px;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none;
}

.mode-scroll::-webkit-scrollbar {
  display: none;
}

.mode-btn {
  flex-shrink: 0;
  padding: 10px 20px;
  border-radius: 20px;
  font-size: 13px;
  font-weight: 600;
  border: 1px solid rgba(255, 255, 255, 0.2);
  background: rgba(255, 255, 255, 0.05);
  color: rgba(255, 255, 255, 0.8);
  transition: all 0.2s;
  white-space: nowrap;
}

.mode-btn.active {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-color: transparent;
  color: white;
  box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
}

.stat-card {
  text-align: center;
  padding: 12px;
  border-radius: 16px;
  background: rgba(255, 255, 255, 0.05);
  min-width: 70px;
}

.stat-value {
  font-size: 20px;
  font-weight: 800;
  line-height: 1;
}

.stat-label {
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  opacity: 0.7;
  margin-top: 4px;
}

.status-bar {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.1);
  font-weight: 600;
  font-size: 14px;
}

.turn-indicator {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #60a5fa;
  box-shadow: 0 0 10px #60a5fa;
  animation: blink 2s infinite;
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}

#confetti-canvas {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 50;
}

/* Ad Styling */
.ad-container {
  min-height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 8px 0;
  border-radius: 12px;
  overflow: hidden;
  background: rgba(0, 0, 0, 0.2);
}

.ad-banner {
  width: 100%;
  max-width: 468px;
  height: 60px;
}

.ad-large {
  width: 100%;
  max-width: 336px;
  height: 280px;
}

.modal {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.8);
  backdrop-filter: blur(8px);
  display: none;
  align-items: center;
  justify-content: center;
  z-index: 200;
  padding: 20px;
}

.modal.active {
  display: flex;
}

.modal-content {
  background: linear-gradient(135deg, #1a1a2e, #16213e);
  border-radius: 24px;
  padding: 24px;
  width: 100%;
  max-width: 320px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  animation: slideUp 0.3s ease;
}

@keyframes slideUp {
  from { transform: translateY(50px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

.text-gradient {
  background: linear-gradient(135deg, #667eea 0%, #f472b6 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.no-scrollbar::-webkit-scrollbar {
  display: none;
}
.no-scrollbar {
  -ms-overflow-style: none;
  scrollbar-width: none;
}
</style>
</head>
<body class="text-white safe-top safe-bottom">

<canvas id="confetti-canvas"></canvas>

<!-- Top Banner Ad with Your Ad Unit ID -->
<div class="ad-container glass mx-4 mt-2">
  <ins class="adsbygoogle ad-banner"
       style="display:block"
       data-ad-client="ca-pub-6851464163673391"
       data-ad-slot="9423081212"
       data-ad-format="auto"
       data-full-width-responsive="true"></ins>
  <script>
       (adsbygoogle = window.adsbygoogle || []).push({});
  </script>
</div>

<header class="px-4 pt-4 pb-2">
  <div class="flex items-center justify-between mb-4">
    <div>
      <h1 class="text-2xl font-black tracking-tight text-gradient">TIC TAC TOE</h1>
      <p class="text-xs text-gray-400 mt-0.5">Ultra Edition</p>
    </div>
    <button onclick="openProfile()" class="w-10 h-10 rounded-full glass flex items-center justify-center active:scale-95 transition">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M20 21v-2a4 4 0 0 0-4-4H8a4 4 0 0 0-4 4v2"></path>
        <circle cx="12" cy="7" r="4"></circle>
      </svg>
    </button>
  </div>

  <div class="flex gap-3 justify-center mb-4">
    <div class="stat-card">
      <div class="stat-value text-green-400" id="wins">0</div>
      <div class="stat-label">Wins</div>
    </div>
    <div class="stat-card">
      <div class="stat-value text-red-400" id="losses">0</div>
      <div class="stat-label">Losses</div>
    </div>
    <div class="stat-card">
      <div class="stat-value text-yellow-400" id="draws">0</div>
      <div class="stat-label">Draws</div>
    </div>
    <div class="stat-card">
      <div class="stat-value text-blue-400" id="level">1</div>
      <div class="stat-label">Level</div>
    </div>
  </div>
</header>

<main class="px-4 py-2">
  <div class="mb-4">
    <div class="mode-scroll" id="modeSelector">
      <button class="mode-btn active" onclick="setMode('ai-hard')">🔥 Hard AI</button>
      <button class="mode-btn" onclick="setMode('ai-easy')">🤖 Easy</button>
      <button class="mode-btn" onclick="setMode('2p')">👥 2 Players</button>
    </div>
  </div>

  <div class="text-center mb-4">
    <div class="status-bar">
      <div class="turn-indicator" id="turnIndicator"></div>
      <span id="statusText">Your Turn</span>
    </div>
    <div class="h-1 w-32 mx-auto mt-3 bg-gray-700 rounded-full overflow-hidden">
      <div class="h-full bg-gradient-to-r from-blue-400 to-pink-400 w-0 transition-all duration-300" id="timerBar"></div>
    </div>
  </div>

  <div id="board"></div>

  <!-- Middle Large Ad with Same Ad Unit ID (Different Slot) -->
  <div class="ad-container glass mt-4">
    <ins class="adsbygoogle ad-large"
         style="display:block"
         data-ad-client="ca-pub-6851464163673391"
         data-ad-slot="9423081212"
         data-ad-format="auto"
         data-full-width-responsive="true"></ins>
    <script>
         (adsbygoogle = window.adsbygoogle || []).push({});
    </script>
  </div>
</main>

<nav class="bottom-nav safe-bottom">
  <button class="nav-btn active" onclick="newGame()">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
      <polygon points="5 3 19 12 5 21 5 3"></polygon>
    </svg>
    <span>New Game</span>
  </button>
  
  <button class="nav-btn" onclick="toggleTheme()">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
      <circle cx="12" cy="12" r="5"></circle>
      <path d="M12 1v2M12 21v2M4.22 4.22l1.42 1.42M18.36 18.36l1.42 1.42M1 12h2M21 12h2M4.22 19.78l1.42-1.42M18.36 5.64l1.42-1.42"></path>
    </svg>
    <span>Theme</span>
  </button>
  
  <button class="nav-btn" onclick="openSettings()">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
      <circle cx="12" cy="12" r="3"></circle>
      <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"></path>
    </svg>
    <span>Settings</span>
  </button>
</nav>

<div class="modal" id="profileModal" onclick="closeModal(event)">
  <div class="modal-content" onclick="event.stopPropagation()">
    <div class="flex items-center justify-between mb-4">
      <h2 class="text-xl font-bold">Profile</h2>
      <button onclick="closeModal()" class="w-8 h-8 rounded-full bg-white/10 flex items-center justify-center">✕</button>
    </div>
    
    <div class="space-y-4">
      <div>
        <label class="text-xs text-gray-400 uppercase tracking-wider">Player Name</label>
        <input type="text" id="nameInput" placeholder="Enter name" 
          class="w-full mt-1 px-4 py-3 rounded-xl bg-white/10 border border-white/20 focus:border-blue-400 outline-none text-white placeholder-gray-500">
      </div>
      
      <div class="grid grid-cols-2 gap-3">
        <div class="p-3 rounded-xl bg-white/5">
          <div class="text-xs text-gray-400">Total XP</div>
          <div class="text-xl font-bold text-green-400" id="modalXp">0</div>
        </div>
        <div class="p-3 rounded-xl bg-white/5">
          <div class="text-xs text-gray-400">Win Rate</div>
          <div class="text-xl font-bold text-blue-400" id="winRate">0%</div>
        </div>
      </div>
      
      <button onclick="saveProfile()" class="w-full py-3 rounded-xl bg-gradient-to-r from-blue-600 to-purple-600 font-bold active:scale-95 transition">
        Save Changes
      </button>
    </div>
  </div>
</div>

<div class="modal" id="settingsModal" onclick="closeModal(event)">
  <div class="modal-content" onclick="event.stopPropagation()">
    <div class="flex items-center justify-between mb-4">
      <h2 class="text-xl font-bold">Settings</h2>
      <button onclick="closeModal()" class="w-8 h-8 rounded-full bg-white/10 flex items-center justify-center">✕</button>
    </div>
    
    <div class="space-y-3">
      <button onclick="resetStats()" class="w-full py-3 rounded-xl bg-red-500/20 text-red-400 font-semibold active:scale-95 transition">
        Reset All Stats
      </button>
      <button onclick="closeModal()" class="w-full py-3 rounded-xl bg-white/10 font-semibold active:scale-95 transition">
        Close
      </button>
    </div>
  </div>
</div>

<script>
let board = Array(9).fill('');
let currentPlayer = 'X';
let gameActive = true;
let gameMode = 'ai-hard';
let playerData = {
  name: 'Player',
  wins: 0,
  losses: 0,
  draws: 0,
  xp: 0,
  level: 1
};

const winConditions = [
  [0,1,2], [3,4,5], [6,7,8],
  [0,3,6], [1,4,7], [2,5,8],
  [0,4,8], [2,4,6]
];

document.addEventListener('DOMContentLoaded', () => {
  loadData();
  createBoard();
  updateUI();
  initAds();
});

function createBoard() {
  const boardEl = document.getElementById('board');
  boardEl.innerHTML = '';
  for (let i = 0; i < 9; i++) {
    const cell = document.createElement('div');
    cell.className = 'cell';
    cell.dataset.index = i;
    cell.addEventListener('click', () => handleMove(i));
    boardEl.appendChild(cell);
  }
}

function handleMove(index) {
  if (!gameActive || board[index] !== '') return;
  
  makeMove(index, currentPlayer);
  
  if (gameActive && gameMode !== '2p') {
    document.getElementById('board').style.pointerEvents = 'none';
    setTimeout(() => {
      aiMove();
      document.getElementById('board').style.pointerEvents = 'auto';
    }, 600);
  }
}

function makeMove(index, player) {
  board[index] = player;
  const cell = document.querySelector(`[data-index="${index}"]`);
  cell.textContent = player;
  cell.classList.add('taken', player.toLowerCase());
  
  checkWin();
  
  if (gameActive) {
    currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
    updateStatus();
  }
}

function aiMove() {
  if (!gameActive) return;
  
  let index;
  if (gameMode === 'ai-easy') {
    const available = board.map((v, i) => v === '' ? i : null).filter(v => v !== null);
    index = available[Math.floor(Math.random() * available.length)];
  } else {
    index = getBestMove();
  }
  
  makeMove(index, 'O');
}

function getBestMove() {
  let bestScore = -Infinity;
  let move = 0;
  
  for (let i = 0; i < 9; i++) {
    if (board[i] === '') {
      board[i] = 'O';
      let score = minimax(board, 0, false);
      board[i] = '';
      if (score > bestScore) {
        bestScore = score;
        move = i;
      }
    }
  }
  return move;
}

function minimax(board, depth, isMaximizing) {
  let result = checkWinner();
  if (result !== null) {
    return result === 'O' ? 10 - depth : result === 'X' ? depth - 10 : 0;
  }
  
  if (isMaximizing) {
    let bestScore = -Infinity;
    for (let i = 0; i < 9; i++) {
      if (board[i] === '') {
        board[i] = 'O';
        let score = minimax(board, depth + 1, false);
        board[i] = '';
        bestScore = Math.max(score, bestScore);
      }
    }
    return bestScore;
  } else {
    let bestScore = Infinity;
    for (let i = 0; i < 9; i++) {
      if (board[i] === '') {
        board[i] = 'X';
        let score = minimax(board, depth + 1, true);
        board[i] = '';
        bestScore = Math.min(score, bestScore);
      }
    }
    return bestScore;
  }
}

function checkWinner() {
  for (let [a, b, c] of winConditions) {
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a];
    }
  }
  if (!board.includes('')) return 'draw';
  return null;
}

function checkWin() {
  const winner = checkWinner();
  if (!winner) return;
  
  gameActive = false;
  
  if (winner === 'draw') {
    showStatus('Draw! 🤝', 'yellow');
    playerData.draws++;
    playerData.xp += 5;
  } else {
    const cells = getWinningCells(winner);
    cells.forEach(i => document.querySelector(`[data-index="${i}"]`).classList.add('win'));
    
    if (winner === 'X') {
      showStatus('You Win! 🎉', 'green');
      playerData.wins++;
      playerData.xp += 25;
      fireConfetti();
    } else {
      showStatus('AI Wins! 🤖', 'red');
      playerData.losses++;
      playerData.xp += 5;
    }
  }
  
  const newLevel = Math.floor(playerData.xp / 100) + 1;
  if (newLevel > playerData.level) {
    playerData.level = newLevel;
    setTimeout(() => showStatus(`Level Up! ${newLevel} 🆙`, 'blue'), 1000);
  }
  
  saveData();
  updateUI();
  
  let timeLeft = 3;
  const timer = setInterval(() => {
    document.getElementById('timerBar').style.width = ((3 - timeLeft) / 3 * 100) + '%';
    timeLeft--;
    if (timeLeft < 0) {
      clearInterval(timer);
      newGame();
    }
  }, 1000);
}

function getWinningCells(player) {
  for (let [a, b, c] of winConditions) {
    if (board[a] === player && board[b] === player && board[c] === player) {
      return [a, b, c];
    }
  }
  return [];
}

function updateStatus() {
  const isPlayer = currentPlayer === 'X';
  const text = gameMode === '2p' ? `Player ${currentPlayer}` : isPlayer ? 'Your' : 'AI';
  document.getElementById('statusText').textContent = `${text} Turn`;
  document.getElementById('turnIndicator').style.background = isPlayer ? '#60a5fa' : '#f472b6';
  document.getElementById('turnIndicator').style.boxShadow = `0 0 10px ${isPlayer ? '#60a5fa' : '#f472b6'}`;
}

function showStatus(text, color) {
  const status = document.getElementById('statusText');
  status.textContent = text;
  status.className = `font-bold text-${color}-400`;
}

function newGame() {
  board = Array(9).fill('');
  currentPlayer = 'X';
  gameActive = true;
  document.getElementById('timerBar').style.width = '0';
  createBoard();
  updateStatus();
  
  // Refresh ads on new game
  refreshAds();
}

function setMode(mode) {
  gameMode = mode;
  document.querySelectorAll('.mode-btn').forEach(btn => btn.classList.remove('active'));
  event.target.classList.add('active');
  newGame();
}

function updateUI() {
  document.getElementById('wins').textContent = playerData.wins;
  document.getElementById('losses').textContent = playerData.losses;
  document.getElementById('draws').textContent = playerData.draws;
  document.getElementById('level').textContent = playerData.level;
  
  const total = playerData.wins + playerData.losses + playerData.draws;
  const rate = total > 0 ? Math.round((playerData.wins / total) * 100) : 0;
  document.getElementById('winRate').textContent = rate + '%';
  document.getElementById('modalXp').textContent = playerData.xp;
}

function saveData() {
  localStorage.setItem('ttt-data', JSON.stringify(playerData));
}

function loadData() {
  const saved = localStorage.getItem('ttt-data');
  if (saved) {
    playerData = { ...playerData, ...JSON.parse(saved) };
    document.getElementById('nameInput').value = playerData.name;
  }
}

function saveProfile() {
  const name = document.getElementById('nameInput').value.trim();
  if (name) playerData.name = name;
  saveData();
  closeModal();
}

function resetStats() {
  if (confirm('Reset all progress?')) {
    playerData = { name: playerData.name, wins: 0, losses: 0, draws: 0, xp: 0, level: 1 };
    saveData();
    updateUI();
    closeModal();
  }
}

function openProfile() {
  document.getElementById('profileModal').classList.add('active');
}

function openSettings() {
  document.getElementById('settingsModal').classList.add('active');
}

function closeModal(e) {
  if (!e || e.target === e.currentTarget) {
    document.querySelectorAll('.modal').forEach(m => m.classList.remove('active'));
  }
}

const themes = [
  'linear-gradient(135deg, #0f0c29, #302b63, #24243e)',
  'linear-gradient(135deg, #1a1a2e, #e94560, #0f3460)',
  'linear-gradient(135deg, #16213e, #0f3460, #e94560)',
  'linear-gradient(135deg, #1b4332, #2d6a4f, #40916c)'
];
let themeIndex = 0;

function toggleTheme() {
  themeIndex = (themeIndex + 1) % themes.length;
  document.body.style.background = themes[themeIndex];
}

const canvas = document.getElementById('confetti-canvas');
const ctx = canvas.getContext('2d');
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

function fireConfetti() {
  const particles = [];
  const colors = ['#60a5fa', '#f472b6', '#fbbf24', '#34d399', '#a78bfa'];
  
  for (let i = 0; i < 80; i++) {
    particles.push({
      x: canvas.width / 2,
      y: canvas.height / 2,
      vx: (Math.random() - 0.5) * 15,
      vy: (Math.random() - 0.5) * 15 - 5,
      life: 1,
      color: colors[Math.floor(Math.random() * colors.length)],
      size: Math.random() * 6 + 2
    });
  }
  
  function animate() {
    if (particles.length === 0) return;
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    
    particles.forEach((p, i) => {
      p.x += p.vx;
      p.y += p.vy;
      p.vy += 0.3;
      p.life -= 0.02;
      p.size *= 0.98;
      
      ctx.fillStyle = p.color;
      ctx.globalAlpha = p.life;
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
      ctx.fill();
      
      if (p.life <= 0) particles.splice(i, 1);
    });
    
    requestAnimationFrame(animate);
  }
  animate();
}

// AdMob Functions
function initAds() {
  try {
    (adsbygoogle = window.adsbygoogle || []).push({});
  } catch (e) {
    console.log('AdBlock detected or AdSense not loaded');
  }
}

function refreshAds() {
  try {
    if (typeof adsbygoogle !== 'undefined') {
      (adsbygoogle = window.adsbygoogle || []).push({});
    }
  } catch (e) {
    console.error('Ad refresh failed:', e);
  }
}

window.addEventListener('resize', () => {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
});

let lastTouchEnd = 0;
document.addEventListener('touchend', (e) => {
  const now = Date.now();
  if (now - lastTouchEnd <= 300) {
    e.preventDefault();
  }
  lastTouchEnd = now;
}, false);
</script>

</body>
</html>
