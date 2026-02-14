<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ultra Tic Tac Toe - Celebration Edition</title>
<script src="https://cdn.tailwindcss.com"></script>

<style>
body{
  min-height:100vh;
  transition:0.5s;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  overflow-x:hidden;
}
.theme-default{
  background:linear-gradient(135deg,#0f0c29,#302b63,#24243e);
}
.theme-fire{
  background:linear-gradient(135deg,#1a1a2e,#e94560,#0f3460);
}
.theme-ocean{
  background:linear-gradient(135deg,#16213e,#0f3460,#e94560);
}
.theme-forest{
  background:linear-gradient(135deg,#1b4332,#2d6a4f,#40916c);
}
.glass{
  background:rgba(255,255,255,0.08);
  backdrop-filter:blur(12px);
  border:1px solid rgba(255,255,255,0.18);
  box-shadow:0 8px 32px 0 rgba(0,0,0,0.37);
}
.cell{
  transition:all 0.3s ease;
  position:relative;
  overflow:hidden;
}
.cell:hover:not(.taken){
  background:rgba(255,255,255,0.15);
  transform:scale(1.05);
}
.cell.taken{
  cursor:not-allowed;
}
.cell.x{
  color:#60a5fa;
  text-shadow:0 0 20px #60a5fa;
}
.cell.o{
  color:#f472b6;
  text-shadow:0 0 20px #f472b6;
}
.win-anim{
  animation:winner-pulse 0.8s ease infinite;
  background:rgba(255,255,255,0.3) !important;
  box-shadow:0 0 30px rgba(255,255,255,0.5);
}
@keyframes winner-pulse{
  0%,100%{transform:scale(1); box-shadow:0 0 20px rgba(255,255,255,0.3);}
  50%{transform:scale(1.05); box-shadow:0 0 40px rgba(255,255,255,0.6);}
}
.mode-btn{
  transition:all 0.3s;
}
.mode-btn.active{
  background:rgba(255,255,255,0.3);
  border-color:#fff;
  transform:scale(1.05);
  box-shadow:0 0 20px rgba(96,165,250,0.5);
}
.thinking{
  opacity:0.6;
  pointer-events:none;
}
/* Confetti Canvas */
#confetti-canvas{
  position:fixed;
  top:0;
  left:0;
  width:100%;
  height:100%;
  pointer-events:none;
  z-index:100;
}
/* Celebration Text */
.celebration-text{
  animation:celebration-pop 0.6s cubic-bezier(0.175,0.885,0.32,1.275);
}
@keyframes celebration-pop{
  0%{transform:scale(0) rotate(-10deg); opacity:0;}
  50%{transform:scale(1.2) rotate(5deg);}
  100%{transform:scale(1) rotate(0deg); opacity:1;}
}
/* Auto-reset progress bar */
.reset-bar{
  height:4px;
  background:linear-gradient(90deg,#60a5fa,#f472b6);
  width:0%;
  transition:width 3s linear;
  border-radius:2px;
  margin-top:10px;
}
.reset-bar.active{
  width:100%;
}
</style>
</head>

<body class="theme-default text-white">

<canvas id="confetti-canvas"></canvas>

<div class="container mx-auto px-4 py-6 pb-24 max-w-2xl relative z-10">

<h1 class="text-4xl md:text-6xl font-black text-center mb-2 tracking-tight bg-clip-text text-transparent bg-gradient-to-r from-blue-400 to-pink-400">
ULTRA TIC TAC TOE
</h1>
<p class="text-center text-gray-400 mb-6 text-sm">2 Players or vs Unbeatable AI</p>

<!-- PROFILE -->
<div class="glass rounded-2xl p-5 mb-4">
  <div class="flex flex-col md:flex-row gap-4 items-center justify-between">
    <div class="flex gap-2 w-full md:w-auto">
      <input id="nameInput" placeholder="Enter Name" value=""
      class="bg-white/10 px-4 py-2 rounded-lg text-center outline-none border border-white/20 focus:border-blue-400 transition w-full md:w-40">
      <button onclick="saveProfile()" class="bg-gradient-to-r from-blue-600 to-purple-600 px-4 py-2 rounded-lg font-semibold hover:scale-105 transition">
        Save
      </button>
    </div>
    <div class="text-center md:text-right text-sm space-y-1">
      <div class="text-lg font-bold text-blue-300" id="playerName">Guest</div>
      <div class="flex gap-4 justify-center md:justify-end text-xs">
        <span>Level <span id="level" class="text-yellow-400 font-bold">1</span></span>
        <span>XP <span id="xp" class="text-green-400">0</span></span>
      </div>
    </div>
  </div>
  <div class="mt-3 pt-3 border-t border-white/10 flex justify-around text-sm">
    <div class="text-center"><div class="text-green-400 font-bold text-xl" id="wins">0</div><div class="text-xs opacity-70">Wins</div></div>
    <div class="text-center"><div class="text-red-400 font-bold text-xl" id="loss">0</div><div class="text-xs opacity-70">Losses</div></div>
    <div class="text-center"><div class="text-yellow-400 font-bold text-xl" id="draw">0</div><div class="text-xs opacity-70">Draws</div></div>
  </div>
</div>

<!-- GAME MODE SELECTOR -->
<div class="glass rounded-2xl p-4 mb-4">
  <div class="text-xs text-gray-400 mb-3 text-center font-bold tracking-wider">SELECT GAME MODE</div>
  <div class="flex gap-3">
    <button onclick="setGameMode('2p')" id="mode-2p" class="mode-btn flex-1 py-3 rounded-xl border border-white/20 font-bold text-sm bg-white/5">
      👥 2 Players
    </button>
    <button onclick="setGameMode('ai-easy')" id="mode-ai-easy" class="mode-btn flex-1 py-3 rounded-xl border border-white/20 font-bold text-sm bg-white/5">
      🤖 Easy
    </button>
    <button onclick="setGameMode('ai-hard')" id="mode-ai-hard" class="mode-btn active flex-1 py-3 rounded-xl border border-white/20 font-bold text-sm bg-gradient-to-r from-purple-600 to-pink-600">
      🔥 Hard AI
    </button>
  </div>
</div>

<!-- CONTROLS -->
<div class="flex flex-wrap gap-3 justify-center mb-6">
  <button onclick="newGame()" class="bg-gradient-to-r from-green-600 to-emerald-600 px-6 py-3 rounded-xl font-bold hover:scale-105 transition shadow-lg">New Game</button>
  <button onclick="changeTheme()" class="bg-gradient-to-r from-pink-600 to-rose-600 px-6 py-3 rounded-xl font-bold hover:scale-105 transition shadow-lg">Theme</button>
</div>

<!-- STATUS -->
<div id="status" class="text-center text-2xl font-bold mb-2 h-8 text-blue-300 drop-shadow-lg">
  Player X Turn
</div>

<!-- Auto reset indicator -->
<div class="text-center mb-4">
  <div id="resetText" class="text-xs text-gray-400 opacity-0 transition-opacity">Auto-reset in 3 seconds...</div>
  <div class="w-48 mx-auto reset-bar" id="resetBar"></div>
</div>

<!-- BOARD -->
<div id="board" class="grid gap-3 justify-center mx-auto mb-6"></div>

<!-- GAME INFO -->
<div class="text-center text-sm text-gray-400 mt-4">
  <span id="modeDisplay">Playing vs Hard AI</span>
</div>

</div>

<!-- MOBILE NAV -->
<div class="fixed bottom-0 left-0 right-0 glass lg:hidden flex justify-around py-4 z-50">
  <button onclick="newGame()" class="text-2xl">🎮</button>
  <button onclick="setGameMode(gameMode === '2p' ? 'ai-hard' : '2p')" class="text-2xl">🔄</button>
  <button onclick="changeTheme()" class="text-2xl">🎨</button>
</div>

<audio id="clickSound" src="https://assets.mixkit.co/sfx/preview/mixkit-game-click-1114.mp3"></audio>
<audio id="winSound" src="https://assets.mixkit.co/sfx/preview/mixkit-winning-chimes-2015.mp3"></audio>
<audio id="loseSound" src="https://assets.mixkit.co/sfx/preview/mixkit-arcade-retro-game-over-213.mp3"></audio>

<script>
// Game State
let board = Array(9).fill(null);
let currentPlayer = 'X';
let gameActive = true;
let gameMode = 'ai-hard';
let thinking = false;
let resetTimeout = null;

// Stats
let xp = parseInt(localStorage.getItem('xp')) || 0;
let wins = parseInt(localStorage.getItem('wins')) || 0;
let loss = parseInt(localStorage.getItem('loss')) || 0;
let draw = parseInt(localStorage.getItem('draw')) || 0;
let level = Math.floor(xp / 100) + 1;

// Themes
let themes = ['theme-default', 'theme-fire', 'theme-ocean', 'theme-forest'];
let themeIndex = 0;

// Confetti System
const canvas = document.getElementById('confetti-canvas');
const ctx = canvas.getContext('2d');
let confettiParticles = [];
let confettiAnimation = null;

function resizeCanvas() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}
window.addEventListener('resize', resizeCanvas);
resizeCanvas();

class Confetti {
  constructor() {
    this.x = Math.random() * canvas.width;
    this.y = -10;
    this.size = Math.random() * 10 + 5;
    this.speedY = Math.random() * 3 + 2;
    this.speedX = Math.random() * 2 - 1;
    this.color = ['#60a5fa', '#f472b6', '#fbbf24', '#34d399', '#a78bfa'][Math.floor(Math.random() * 5)];
    this.rotation = Math.random() * 360;
    this.rotationSpeed = Math.random() * 10 - 5;
  }
  update() {
    this.y += this.speedY;
    this.x += this.speedX;
    this.rotation += this.rotationSpeed;
    if (this.y > canvas.height) {
      this.y = -10;
      this.x = Math.random() * canvas.width;
    }
  }
  draw() {
    ctx.save();
    ctx.translate(this.x, this.y);
    ctx.rotate(this.rotation * Math.PI / 180);
    ctx.fillStyle = this.color;
    ctx.fillRect(-this.size/2, -this.size/2, this.size, this.size);
    ctx.restore();
  }
}

function startConfetti() {
  confettiParticles = [];
  for (let i = 0; i < 100; i++) {
    confettiParticles.push(new Confetti());
  }
  if (confettiAnimation) cancelAnimationFrame(confettiAnimation);
  animateConfetti();
}

function animateConfetti() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  confettiParticles.forEach(p => {
    p.update();
    p.draw();
  });
  confettiAnimation = requestAnimationFrame(animateConfetti);
}

function stopConfetti() {
  if (confettiAnimation) {
    cancelAnimationFrame(confettiAnimation);
    confettiAnimation = null;
  }
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  confettiParticles = [];
}

// Initialize
document.getElementById('xp').textContent = xp;
document.getElementById('wins').textContent = wins;
document.getElementById('loss').textContent = loss;
document.getElementById('draw').textContent = draw;
document.getElementById('level').textContent = level;

let savedName = localStorage.getItem('name');
if (savedName) {
  document.getElementById('playerName').textContent = savedName;
  document.getElementById('nameInput').value = savedName;
}

function saveProfile() {
  let name = document.getElementById('nameInput').value.trim() || 'Guest';
  localStorage.setItem('name', name);
  document.getElementById('playerName').textContent = name;
}

function setGameMode(mode) {
  gameMode = mode;
  
  document.querySelectorAll('.mode-btn').forEach(btn => {
    btn.classList.remove('active', 'bg-gradient-to-r', 'from-purple-600', 'to-pink-600');
    btn.classList.add('bg-white/5');
  });
  
  const activeBtn = document.getElementById('mode-' + mode);
  activeBtn.classList.add('active', 'bg-gradient-to-r', 'from-purple-600', 'to-pink-600');
  activeBtn.classList.remove('bg-white/5');
  
  const modeText = {
    '2p': '2 Players (Local)',
    'ai-easy': 'vs Easy AI',
    'ai-hard': 'vs Hard AI (Unbeatable)'
  };
  document.getElementById('modeDisplay').textContent = 'Playing: ' + modeText[mode];
  
  newGame();
}

function renderBoard() {
  const boardEl = document.getElementById('board');
  boardEl.innerHTML = '';
  boardEl.style.gridTemplateColumns = "repeat(3, 100px)";
  boardEl.style.gap = "12px";
  
  for (let i = 0; i < 9; i++) {
    let cell = document.createElement('div');
    cell.className = "cell w-24 h-24 glass rounded-xl flex items-center justify-center text-4xl font-black cursor-pointer";
    cell.dataset.index = i;
    
    if (board[i]) {
      cell.textContent = board[i];
      cell.classList.add('taken', board[i].toLowerCase());
    }
    
    if (gameActive && !board[i] && !thinking && canPlayerMove()) {
      cell.onclick = () => handleMove(i);
    }
    
    if (!gameActive && isWinningCell(i)) {
      cell.classList.add('win-anim');
    }
    
    boardEl.appendChild(cell);
  }
}

function canPlayerMove() {
  if (gameMode === '2p') return true;
  return currentPlayer === 'X';
}

function isWinningCell(index) {
  const winsArr = [[0,1,2], [3,4,5], [6,7,8], [0,3,6], [1,4,7], [2,5,8], [0,4,8], [2,4,6]];
  for (let combo of winsArr) {
    if (combo.includes(index)) {
      let [a, b, c] = combo;
      if (board[a] && board[a] === board[b] && board[a] === board[c]) {
        return true;
      }
    }
  }
  return false;
}

function handleMove(index) {
  if (!gameActive || board[index] || thinking) return;
  
  board[index] = currentPlayer;
  document.getElementById('clickSound').play();
  
  if (checkWin(currentPlayer)) {
    celebrateWin(currentPlayer);
    return;
  }
  
  if (isBoardFull()) {
    endGame(null);
    return;
  }
  
  currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
  updateStatus();
  renderBoard();
  
  if (gameMode !== '2p' && currentPlayer === 'O' && gameActive) {
    triggerAIMove();
  }
}

function celebrateWin(winner) {
  gameActive = false;
  
  // Start confetti
  startConfetti();
  
  // Update stats
  if (winner === 'X') {
    document.getElementById('status').innerHTML = '<span class="celebration-text text-green-400">X Wins! 🎉</span>';
    if (gameMode !== '2p') {
      wins++;
      xp += gameMode === 'ai-hard' ? 50 : 30;
    } else {
      xp += 10;
    }
    document.getElementById('winSound').play();
  } else {
    document.getElementById('status').innerHTML = '<span class="celebration-text text-red-400">O Wins! ' + (gameMode !== '2p' ? '🤖' : '🎉') + '</span>';
    if (gameMode !== '2p') loss++;
    document.getElementById('loseSound').play();
  }
  
  saveStats();
  renderBoard();
  
  // Auto reset after 3 seconds
  document.getElementById('resetText').style.opacity = '1';
  document.getElementById('resetBar').classList.add('active');
  
  resetTimeout = setTimeout(() => {
    newGame();
  }, 3000);
}

function triggerAIMove() {
  thinking = true;
  document.getElementById('board').classList.add('thinking');
  updateStatus();
  
  setTimeout(() => {
    const move = calculateAIMove();
    if (move !== -1) {
      executeAIMove(move);
    }
    thinking = false;
    document.getElementById('board').classList.remove('thinking');
    renderBoard();
  }, 600);
}

function executeAIMove(index) {
  board[index] = 'O';
  document.getElementById('clickSound').play();
  
  if (checkWin('O')) {
    celebrateWin('O');
    return;
  }
  
  if (isBoardFull()) {
    endGame(null);
    return;
  }
  
  currentPlayer = 'X';
  updateStatus();
}

function calculateAIMove() {
  if (gameMode === 'ai-easy') {
    return getRandomMove();
  } else {
    return getBestMove();
  }
}

function getRandomMove() {
  let empty = [];
  for (let i = 0; i < 9; i++) {
    if (!board[i]) empty.push(i);
  }
  if (empty.length === 0) return -1;
  return empty[Math.floor(Math.random() * empty.length)];
}

function getBestMove() {
  let bestScore = -Infinity;
  let move = -1;
  
  for (let i = 0; i < 9; i++) {
    if (!board[i]) {
      board[i] = 'O';
      let score = minimax(board, 0, false);
      board[i] = null;
      if (score > bestScore) {
        bestScore = score;
        move = i;
      }
    }
  }
  return move;
}

function minimax(board, depth, isMaximizing) {
  if (checkWin('O')) return 10 - depth;
  if (checkWin('X')) return depth - 10;
  if (isBoardFull()) return 0;
  
  if (isMaximizing) {
    let bestScore = -Infinity;
    for (let i = 0; i < 9; i++) {
      if (!board[i]) {
        board[i] = 'O';
        let score = minimax(board, depth + 1, false);
        board[i] = null;
        bestScore = Math.max(score, bestScore);
      }
    }
    return bestScore;
  } else {
    let bestScore = Infinity;
    for (let i = 0; i < 9; i++) {
      if (!board[i]) {
        board[i] = 'X';
        let score = minimax(board, depth + 1, true);
        board[i] = null;
        bestScore = Math.min(score, bestScore);
      }
    }
    return bestScore;
  }
}

function checkWin(player) {
  const winsArr = [[0,1,2], [3,4,5], [6,7,8], [0,3,6], [1,4,7], [2,5,8], [0,4,8], [2,4,6]];
  return winsArr.some(combo => {
    return combo.every(i => board[i] === player);
  });
}

function isBoardFull() {
  return board.every(cell => cell !== null);
}

function endGame(winner) {
  gameActive = false;
  
  if (winner === null) {
    document.getElementById('status').innerHTML = '<span class="celebration-text text-yellow-400">Draw! 🤝</span>';
    draw++;
    xp += gameMode === 'ai-hard' ? 20 : 10;
    document.getElementById('winSound').play();
    saveStats();
    renderBoard();
    
    // Auto reset for draw too
    document.getElementById('resetText').style.opacity = '1';
    document.getElementById('resetBar').classList.add('active');
    resetTimeout = setTimeout(() => {
      newGame();
    }, 3000);
  }
}

function updateStatus() {
  const statusEl = document.getElementById('status');
  
  if (thinking) {
    statusEl.innerHTML = '<span class="text-purple-400 animate-pulse">AI is thinking...</span>';
  } else if (gameMode === '2p') {
    statusEl.innerHTML = `Player <span class="${currentPlayer === 'X' ? 'text-blue-400' : 'text-pink-400'}">${currentPlayer}</span> Turn`;
  } else {
    if (currentPlayer === 'X') {
      statusEl.innerHTML = '<span class="text-blue-400">Your Turn (X)</span>';
    } else {
      statusEl.innerHTML = '<span class="text-pink-400">AI Turn (O)</span>';
    }
  }
}

function saveStats() {
  localStorage.setItem('xp', xp);
  localStorage.setItem('wins', wins);
  localStorage.setItem('loss', loss);
  localStorage.setItem('draw', draw);
  level = Math.floor(xp / 100) + 1;
  document.getElementById('xp').textContent = xp;
  document.getElementById('wins').textContent = wins;
  document.getElementById('loss').textContent = loss;
  document.getElementById('draw').textContent = draw;
  document.getElementById('level').textContent = level;
}

function newGame() {
  // Clear any pending reset
  if (resetTimeout) {
    clearTimeout(resetTimeout);
    resetTimeout = null;
  }
  
  // Stop confetti
  stopConfetti();
  
  // Reset game state completely
  board = Array(9).fill(null);
  currentPlayer = 'X';
  gameActive = true;
  thinking = false;
  
  // Reset UI
  document.getElementById('board').classList.remove('thinking');
  document.getElementById('resetText').style.opacity = '0';
  document.getElementById('resetBar').classList.remove('active');
  
  updateStatus();
  renderBoard();
}

function changeTheme() {
  document.body.classList.remove(themes[themeIndex]);
  themeIndex = (themeIndex + 1) % themes.length;
  document.body.classList.add(themes[themeIndex]);
}

// Start
renderBoard();
</script>

</body>
</html>

