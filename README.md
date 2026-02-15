<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
<meta name="theme-color" content="#0f0c29">
<title>Ultra Tic Tac Toe</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap" rel="stylesheet">

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-6851464163673391" crossorigin="anonymous"></script>

<style>
* {
  -webkit-tap-highlight-color: transparent;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  user-select: none;
  box-sizing: border-box;
}

html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  font-family: 'Inter', sans-serif;
  background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
}

/* Safe area support */
.safe-area {
  padding-top: env(safe-area-inset-top);
  padding-bottom: env(safe-area-inset-bottom);
  padding-left: env(safe-area-inset-left);
  padding-right: env(safe-area-inset-right);
}

/* Menu Screen */
.menu-screen {
  position: fixed;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
  z-index: 50;
  background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
  transition: opacity 0.4s ease, visibility 0.4s ease;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
}

.menu-screen.hidden {
  opacity: 0;
  visibility: hidden;
  pointer-events: none;
}

.menu-title {
  font-size: clamp(2rem, 8vw, 3.5rem);
  font-weight: 800;
  text-align: center;
  margin-bottom: 0.5rem;
  background: linear-gradient(135deg, #667eea 0%, #f472b6 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  animation: titlePulse 3s ease infinite;
}

@keyframes titlePulse {
  0%, 100% { filter: brightness(1); }
  50% { filter: brightness(1.2); }
}

.menu-subtitle {
  color: rgba(255, 255, 255, 0.6);
  font-size: 0.9rem;
  margin-bottom: 2rem;
  text-align: center;
}

.menu-btn {
  width: min(260px, 70vw);
  padding: 16px 24px;
  margin: 8px 0;
  border-radius: 14px;
  font-size: 1rem;
  font-weight: 700;
  border: none;
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  position: relative;
  overflow: hidden;
  touch-action: manipulation;
  -webkit-appearance: none;
}

.menu-btn:active {
  transform: scale(0.96);
}

.menu-btn-primary {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  box-shadow: 0 4px 20px rgba(102, 126, 234, 0.4);
}

.menu-btn-secondary {
  background: rgba(255, 255, 255, 0.1);
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

/* Game Screen */
.game-screen {
  display: none;
  width: 100%;
  height: 100%;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  padding-bottom: 100px;
}

.game-screen.active {
  display: block;
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

/* Settings Modal */
.settings-modal {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.95);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  display: none;
  align-items: center;
  justify-content: center;
  z-index: 100;
  padding: 20px;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
}

.settings-modal.active {
  display: flex;
}

.settings-content {
  width: min(340px, 90vw);
  background: linear-gradient(135deg, #1a1a2e, #16213e);
  border-radius: 20px;
  padding: 20px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  max-height: 85vh;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
}

.settings-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 20px;
}

.settings-title {
  font-size: 1.3rem;
  font-weight: 700;
}

.close-btn {
  width: 32px;
  height: 32px;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  touch-action: manipulation;
}

.close-btn:active {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(0.95);
}

.setting-item {
  margin-bottom: 16px;
  padding: 14px;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 14px;
  border: 1px solid rgba(255, 255, 255, 0.05);
}

.setting-label {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 8px;
  font-weight: 600;
  font-size: 0.9rem;
}

.setting-label svg {
  width: 18px;
  height: 18px;
  opacity: 0.8;
  flex-shrink: 0;
}

/* Toggle Switch */
.toggle-switch {
  position: relative;
  width: 48px;
  height: 26px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 13px;
  cursor: pointer;
  transition: background 0.3s;
  margin-left: auto;
  flex-shrink: 0;
}

.toggle-switch.active {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.toggle-switch::after {
  content: '';
  position: absolute;
  top: 2px;
  left: 2px;
  width: 22px;
  height: 22px;
  background: white;
  border-radius: 50%;
  transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 2px 6px rgba(0,0,0,0.3);
}

.toggle-switch.active::after {
  transform: translateX(22px);
}

/* Theme Grid */
.theme-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  margin-top: 10px;
}

.theme-option {
  aspect-ratio: 1;
  border-radius: 10px;
  cursor: pointer;
  border: 2px solid transparent;
  transition: all 0.2s;
  position: relative;
  overflow: hidden;
}

.theme-option.active {
  border-color: white;
  box-shadow: 0 0 15px rgba(255,255,255,0.3);
  transform: scale(1.05);
}

.theme-option::after {
  content: '✓';
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  font-weight: bold;
  opacity: 0;
  transition: opacity 0.2s;
  text-shadow: 0 2px 4px rgba(0,0,0,0.5);
}

.theme-option.active::after {
  opacity: 1;
}

.theme-default { background: linear-gradient(135deg, #0f0c29, #302b63, #24243e); }
.theme-fire { background: linear-gradient(135deg, #1a1a2e, #e94560, #0f3460); }
.theme-ocean { background: linear-gradient(135deg, #16213e, #0f3460, #e94560); }
.theme-forest { background: linear-gradient(135deg, #1b4332, #2d6a4f, #40916c); }
.theme-sunset { background: linear-gradient(135deg, #ff6b6b, #feca57, #48dbfb); }
.theme-midnight { background: linear-gradient(135deg, #2c3e50, #34495e, #2c3e50); }

/* Game Board - FIXED CENTERING */
.game-container {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 16px;
}

#board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  width: min(92vw, 360px);
  height: min(92vw, 360px);
  margin: 0 auto;
  padding: 10px;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 16px;
  box-shadow: inset 0 2px 10px rgba(0, 0, 0, 0.3);
}

.cell {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: clamp(1.8rem, 7vw, 2.5rem);
  font-weight: 800;
  cursor: pointer;
  transition: all 0.15s ease;
  position: relative;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.05);
  touch-action: manipulation;
  -webkit-appearance: none;
}

.cell:active:not(.taken) {
  transform: scale(0.92);
  background: rgba(255, 255, 255, 0.2);
}

.cell.x {
  color: #60a5fa;
  text-shadow: 0 0 15px rgba(96, 165, 250, 0.6);
  animation: popIn 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.cell.o {
  color: #f472b6;
  text-shadow: 0 0 15px rgba(244, 114, 182, 0.6);
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
  0%, 100% { transform: scale(1); box-shadow: 0 0 15px currentColor; }
  50% { transform: scale(1.03); box-shadow: 0 0 30px currentColor; }
}

/* Header Stats */
.game-header {
  width: 100%;
  padding: 12px 16px;
  padding-top: calc(12px + env(safe-area-inset-top));
}

.stats-row {
  display: flex;
  gap: 8px;
  justify-content: center;
  margin-bottom: 12px;
}

.stat-card {
  text-align: center;
  padding: 10px 14px;
  border-radius: 12px;
  background: rgba(255, 255, 255, 0.05);
  min-width: 60px;
}

.stat-value {
  font-size: 1.1rem;
  font-weight: 800;
  line-height: 1;
}

.stat-label {
  font-size: 0.65rem;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  opacity: 0.7;
  margin-top: 3px;
}

/* Mode Selector */
.mode-scroll {
  display: flex;
  gap: 8px;
  overflow-x: auto;
  padding: 4px;
  margin: 0 -4px 12px;
  -webkit-overflow-scrolling: touch;
  scrollbar-width: none;
  width: 100%;
  max-width: 400px;
}

.mode-scroll::-webkit-scrollbar {
  display: none;
}

.mode-btn {
  flex-shrink: 0;
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 0.8rem;
  font-weight: 600;
  border: 1px solid rgba(255, 255, 255, 0.2);
  background: rgba(255, 255, 255, 0.05);
  color: rgba(255, 255, 255, 0.8);
  transition: all 0.2s;
  white-space: nowrap;
  cursor: pointer;
  touch-action: manipulation;
  -webkit-appearance: none;
}

.mode-btn.active {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-color: transparent;
  color: white;
}

/* Status Bar */
.status-container {
  text-align: center;
  margin-bottom: 16px;
}

.status-bar {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.1);
  font-weight: 600;
  font-size: 0.85rem;
}

.turn-indicator {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #60a5fa;
  box-shadow: 0 0 8px #60a5fa;
  animation: blink 2s infinite;
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.4; }
}

.timer-bar-container {
  width: 120px;
  height: 3px;
  background: rgba(255,255,255,0.1);
  border-radius: 2px;
  margin: 8px auto 0;
  overflow: hidden;
}

.timer-bar {
  height: 100%;
  background: linear-gradient(90deg, #60a5fa, #f472b6);
  width: 0%;
  transition: width 0.3s linear;
}

/* Bottom Navigation */
.bottom-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(15, 12, 41, 0.98);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  padding: 8px 20px calc(8px + env(safe-area-inset-bottom));
  display: none;
  justify-content: space-around;
  align-items: center;
  z-index: 40;
}

.bottom-nav.active {
  display: flex;
}

.nav-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3px;
  padding: 6px 12px;
  border-radius: 10px;
  transition: all 0.2s;
  color: rgba(255, 255, 255, 0.6);
  font-size: 10px;
  font-weight: 600;
  background: none;
  border: none;
  cursor: pointer;
  touch-action: manipulation;
  min-width: 56px;
}

.nav-btn:active {
  background: rgba(255, 255, 255, 0.1);
}

.nav-btn svg {
  width: 22px;
  height: 22px;
  stroke-width: 2;
}

/* Back Button */
.back-btn {
  position: fixed;
  top: calc(12px + env(safe-area-inset-top));
  left: 12px;
  z-index: 45;
  width: 40px;
  height: 40px;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
  color: white;
  cursor: pointer;
  display: none;
  align-items: center;
  justify-content: center;
  touch-action: manipulation;
}

.back-btn.active {
  display: flex;
}

.back-btn:active {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(0.95);
}

/* Ads */
.ad-container {
  min-height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 12px auto;
  border-radius: 12px;
  overflow: hidden;
  background: rgba(0, 0, 0, 0.2);
  width: min(100%, 400px);
}

/* Confetti */
#confetti-canvas {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 30;
}

/* Utility */
.glass {
  background: rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.text-gradient {
  background: linear-gradient(135deg, #667eea 0%, #f472b6 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

/* Fix for iframe embedding */
@media (max-width: 400px) {
  #board {
    width: 95vw;
    height: 95vw;
  }
  
  .menu-btn {
    width: 85vw;
  }
}

/* Landscape mode fix */
@media (max-height: 500px) and (orientation: landscape) {
  .menu-screen {
    justify-content: flex-start;
    padding-top: 20px;
  }
  
  .menu-title {
    font-size: 1.8rem;
    margin-bottom: 0.3rem;
  }
  
  .menu-subtitle {
    margin-bottom: 1rem;
  }
  
  #board {
    width: 70vh;
    height: 70vh;
  }
}
</style>
</head>
<body class="text-white safe-area">

<canvas id="confetti-canvas"></canvas>

<!-- Main Menu -->
<div class="menu-screen" id="menuScreen">
  <h1 class="menu-title">TIC TAC TOE</h1>
  <p class="menu-subtitle">Ultra Edition</p>
  
  <button class="menu-btn menu-btn-primary" id="playBtn">
    <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
      <polygon points="5 3 19 12 5 21 5 3"></polygon>
    </svg>
    PLAY NOW
  </button>
  
  <button class="menu-btn menu-btn-secondary" id="settingsBtn">
    <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <circle cx="12" cy="12" r="3"></circle>
      <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"></path>
    </svg>
    SETTINGS
  </button>
  
  <div class="ad-container glass mt-6">
    <ins class="adsbygoogle"
         style="display:block;width:320px;height:50px"
         data-ad-client="ca-pub-6851464163673391"
         data-ad-slot="9423081212"
         data-ad-format="auto"
         data-full-width-responsive="true"></ins>
  </div>
</div>

<!-- Settings Modal -->
<div class="settings-modal" id="settingsModal">
  <div class="settings-content">
    <div class="settings-header">
      <h2 class="settings-title">Settings</h2>
      <button class="close-btn" id="closeSettings">✕</button>
    </div>
    
    <div class="setting-item">
      <div class="setting-label">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <polygon points="11 5 6 9 2 9 2 15 6 15 11 19 11 5"></polygon>
          <path d="M19.07 4.93a10 10 0 0 1 0 14.14M15.54 8.46a5 5 0 0 1 0 7.07"></path>
        </svg>
        Sound Effects
        <div class="toggle-switch active" id="soundToggle"></div>
      </div>
      <p class="text-xs text-gray-400">Play sounds during gameplay</p>
    </div>
    
    <div class="setting-item">
      <div class="setting-label">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M9 18V5l12-2v13"></path>
          <circle cx="6" cy="18" r="3"></circle>
          <circle cx="18" cy="16" r="3"></circle>
        </svg>
        Background Music
        <div class="toggle-switch" id="musicToggle"></div>
      </div>
      <p class="text-xs text-gray-400">Ambient background music</p>
    </div>
    
    <div class="setting-item">
      <div class="setting-label">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M13 2L3 14h9l-1 8 10-12h-9l1-8z"></path>
        </svg>
        Haptic Feedback
        <div class="toggle-switch active" id="hapticToggle"></div>
      </div>
      <p class="text-xs text-gray-400">Vibrate on moves and wins</p>
    </div>
    
    <div class="setting-item">
      <div class="setting-label" style="margin-bottom: 4px;">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="12" cy="12" r="5"></circle>
          <path d="M12 1v2M12 21v2M4.22 4.22l1.42 1.42M18.36 18.36l1.42 1.42M1 12h2M21 12h2M4.22 19.78l1.42-1.42M18.36 5.64l1.42-1.42"></path>
        </svg>
        Theme Color
      </div>
      <div class="theme-grid">
        <div class="theme-option theme-default active" data-theme="default" title="Default"></div>
        <div class="theme-option theme-fire" data-theme="fire" title="Fire"></div>
        <div class="theme-option theme-ocean" data-theme="ocean" title="Ocean"></div>
        <div class="theme-option theme-forest" data-theme="forest" title="Forest"></div>
        <div class="theme-option theme-sunset" data-theme="sunset" title="Sunset"></div>
        <div class="theme-option theme-midnight" data-theme="midnight" title="Midnight"></div>
      </div>
    </div>
    
    <button id="resetBtn" class="w-full py-3 mt-2 rounded-xl bg-red-500/20 text-red-400 font-semibold text-sm border border-red-500/30 active:scale-95 transition">
      Reset All Progress
    </button>
  </div>
</div>

<!-- Back Button -->
<button class="back-btn" id="backBtn">
  <svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
    <path d="M19 12H5M12 19l-7-7 7-7"></path>
  </svg>
</button>

<!-- Game Screen -->
<div class="game-screen" id="gameScreen">
  <div class="game-header">
    <div class="stats-row">
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
  </div>

  <div class="game-container">
    <div class="mode-scroll">
      <button class="mode-btn active" data-mode="ai-hard">🔥 Hard AI</button>
      <button class="mode-btn" data-mode="ai-easy">🤖 Easy</button>
      <button class="mode-btn" data-mode="2p">👥 2 Players</button>
    </div>

    <div class="status-container">
      <div class="status-bar">
        <div class="turn-indicator" id="turnIndicator"></div>
        <span id="statusText">Your Turn</span>
      </div>
      <div class="timer-bar-container">
        <div class="timer-bar" id="timerBar"></div>
      </div>
    </div>

    <div id="board"></div>

    <div class="ad-container glass">
      <ins class="adsbygoogle"
           style="display:block;width:300px;height:250px"
           data-ad-client="ca-pub-6851464163673391"
           data-ad-slot="9423081212"
           data-ad-format="auto"
           data-full-width-responsive="true"></ins>
    </div>
  </div>
</div>

<!-- Bottom Navigation -->
<nav class="bottom-nav" id="bottomNav">
  <button class="nav-btn" id="newGameBtn">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
      <polygon points="5 3 19 12 5 21 5 3"></polygon>
    </svg>
    <span>New Game</span>
  </button>
  
  <button class="nav-btn" id="gameSettingsBtn">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
      <circle cx="12" cy="12" r="3"></circle>
      <path d="M19.4 15a1.65 1.65 0 0 0 .33 1.82l.06.06a2 2 0 0 1 0 2.83 2 2 0 0 1-2.83 0l-.06-.06a1.65 1.65 0 0 0-1.82-.33 1.65 1.65 0 0 0-1 1.51V21a2 2 0 0 1-2 2 2 2 0 0 1-2-2v-.09A1.65 1.65 0 0 0 9 19.4a1.65 1.65 0 0 0-1.82.33l-.06.06a2 2 0 0 1-2.83 0 2 2 0 0 1 0-2.83l.06-.06a1.65 1.65 0 0 0 .33-1.82 1.65 1.65 0 0 0-1.51-1H3a2 2 0 0 1-2-2 2 2 0 0 1 2-2h.09A1.65 1.65 0 0 0 4.6 9a1.65 1.65 0 0 0-.33-1.82l-.06-.06a2 2 0 0 1 0-2.83 2 2 0 0 1 2.83 0l.06.06a1.65 1.65 0 0 0 1.82.33H9a1.65 1.65 0 0 0 1-1.51V3a2 2 0 0 1 2-2 2 2 0 0 1 2 2v.09a1.65 1.65 0 0 0 1 1.51 1.65 1.65 0 0 0 1.82-.33l.06-.06a2 2 0 0 1 2.83 0 2 2 0 0 1 0 2.83l-.06.06a1.65 1.65 0 0 0-.33 1.82V9a1.65 1.65 0 0 0 1.51 1H21a2 2 0 0 1 2 2 2 2 0 0 1-2 2h-.09a1.65 1.65 0 0 0-1.51 1z"></path>
    </svg>
    <span>Settings</span>
  </button>
  
  <button class="nav-btn" id="menuBtn">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor">
      <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"></path>
      <polyline points="9 22 9 12 15 12 15 22"></polyline>
    </svg>
    <span>Menu</span>
  </button>
</nav>

<script>
// Game State
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

let settings = {
  sound: true,
  music: false,
  haptic: true,
  theme: 'default'
};

const winConditions = [
  [0,1,2], [3,4,5], [6,7,8],
  [0,3,6], [1,4,7], [2,5,8],
  [0,4,8], [2,4,6]
];

// Audio
const audioContext = new (window.AudioContext || window.webkitAudioContext)();

function playSound(type) {
  if (!settings.sound) return;
  if (audioContext.state === 'suspended') audioContext.resume();
  
  const osc = audioContext.createOscillator();
  const gain = audioContext.createGain();
  osc.connect(gain);
  gain.connect(audioContext.destination);
  
  if (type === 'move') {
    osc.frequency.setValueAtTime(600, audioContext.currentTime);
    osc.frequency.exponentialRampToValueAtTime(300, audioContext.currentTime + 0.1);
    gain.gain.setValueAtTime(0.2, audioContext.currentTime);
    gain.gain.exponentialRampToValueAtTime(0.01, audioContext.currentTime + 0.1);
    osc.start(audioContext.currentTime);
    osc.stop(audioContext.currentTime + 0.1);
  } else if (type === 'win') {
    [523.25, 659.25, 783.99].forEach((freq, i) => {
      const o = audioContext.createOscillator();
      const g = audioContext.createGain();
      o.connect(g);
      g.connect(audioContext.destination);
      o.frequency.value = freq;
      g.gain.setValueAtTime(0.15, audioContext.currentTime + i * 0.1);
      g.gain.linearRampToValueAtTime(0, audioContext.currentTime + i * 0.1 + 0.3);
      o.start(audioContext.currentTime + i * 0.1);
      o.stop(audioContext.currentTime + i * 0.1 + 0.3);
    });
  }
}

function vibrate(pattern) {
  if (settings.haptic && navigator.vibrate) navigator.vibrate(pattern);
}

// Initialize
document.addEventListener('DOMContentLoaded', () => {
  loadData();
  loadSettings();
  initAds();
  bindEvents();
});

function bindEvents() {
  // Menu buttons
  document.getElementById('playBtn').addEventListener('click', startGame);
  document.getElementById('settingsBtn').addEventListener('click', openSettings);
  document.getElementById('closeSettings').addEventListener('click', closeSettings);
  document.getElementById('backBtn').addEventListener('click', backToMenu);
  document.getElementById('newGameBtn').addEventListener('click', newGame);
  document.getElementById('gameSettingsBtn').addEventListener('click', openSettings);
  document.getElementById('menuBtn').addEventListener('click', backToMenu);
  document.getElementById('resetBtn').addEventListener('click', resetAllData);
  
  // Settings toggles
  document.getElementById('soundToggle').addEventListener('click', toggleSound);
  document.getElementById('musicToggle').addEventListener('click', toggleMusic);
  document.getElementById('hapticToggle').addEventListener('click', toggleHaptic);
  
  // Theme options
  document.querySelectorAll('.theme-option').forEach(el => {
    el.addEventListener('click', () => setTheme(el.dataset.theme, el));
  });
  
  // Mode buttons
  document.querySelectorAll('.mode-btn').forEach(btn => {
    btn.addEventListener('click', () => setMode(btn.dataset.mode, btn));
  });
  
  // Touch fix for iOS
  document.addEventListener('touchstart', function(){}, {passive: true});
}

// Navigation
function startGame() {
  document.getElementById('menuScreen').classList.add('hidden');
  document.getElementById('gameScreen').classList.add('active');
  document.getElementById('backBtn').classList.add('active');
  document.getElementById('bottomNav').classList.add('active');
  
  if (audioContext.state === 'suspended') audioContext.resume();
  createBoard();
  updateUI();
  updateStatus();
}

function backToMenu() {
  document.getElementById('gameScreen').classList.remove('active');
  document.getElementById('backBtn').classList.remove('active');
  document.getElementById('bottomNav').classList.remove('active');
  document.getElementById('menuScreen').classList.remove('hidden');
}

// Settings
function openSettings() {
  document.getElementById('settingsModal').classList.add('active');
}

function closeSettings() {
  document.getElementById('settingsModal').classList.remove('active');
}

function toggleSound() {
  settings.sound = !settings.sound;
  document.getElementById('soundToggle').classList.toggle('active', settings.sound);
  saveSettings();
  if (settings.sound) playSound('move');
}

function toggleMusic() {
  settings.music = !settings.music;
  document.getElementById('musicToggle').classList.toggle('active', settings.music);
  saveSettings();
}

function toggleHaptic() {
  settings.haptic = !settings.haptic;
  document.getElementById('hapticToggle').classList.toggle('active', settings.haptic);
  saveSettings();
  vibrate(50);
}

function setTheme(themeName, element) {
  settings.theme = themeName;
  document.querySelectorAll('.theme-option').forEach(el => el.classList.remove('active'));
  element.classList.add('active');
  
  const gradients = {
    'default': 'linear-gradient(135deg, #0f0c29, #302b63, #24243e)',
    'fire': 'linear-gradient(135deg, #1a1a2e, #e94560, #0f3460)',
    'ocean': 'linear-gradient(135deg, #16213e, #0f3460, #e94560)',
    'forest': 'linear-gradient(135deg, #1b4332, #2d6a4f, #40916c)',
    'sunset': 'linear-gradient(135deg, #ff6b6b, #feca57, #48dbfb)',
    'midnight': 'linear-gradient(135deg, #2c3e50, #34495e, #2c3e50)'
  };
  
  const bg = gradients[themeName];
  document.body.style.background = bg;
  document.getElementById('menuScreen').style.background = bg;
  saveSettings();
}

// Game Logic
function createBoard() {
  const boardEl = document.getElementById('board');
  boardEl.innerHTML = '';
  for (let i = 0; i < 9; i++) {
    const cell = document.createElement('div');
    cell.className = 'cell';
    cell.dataset.index = i;
    
    // Fix touch events
    cell.addEventListener('touchstart', (e) => {
      e.preventDefault();
      handleMove(i);
    }, {passive: false});
    
    cell.addEventListener('click', () => handleMove(i));
    
    boardEl.appendChild(cell);
  }
}

function handleMove(index) {
  if (!gameActive || board[index] !== '') return;
  
  playSound('move');
  vibrate(20);
  
  makeMove(index, currentPlayer);
  
  if (gameActive && gameMode !== '2p') {
    document.getElementById('board').style.pointerEvents = 'none';
    setTimeout(() => {
      aiMove();
      document.getElementById('board').style.pointerEvents = 'auto';
    }, 500);
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
    vibrate([100, 50, 100]);
  } else {
    const cells = getWinningCells(winner);
    cells.forEach(i => document.querySelector(`[data-index="${i}"]`).classList.add('win'));
    
    if (winner === 'X') {
      showStatus('You Win! 🎉', 'green');
      playerData.wins++;
      playerData.xp += 25;
      playSound('win');
      vibrate([50, 100, 50, 100, 200]);
      fireConfetti();
    } else {
      showStatus('AI Wins! 🤖', 'red');
      playerData.losses++;
      playerData.xp += 5;
      vibrate([200, 100, 200]);
    }
  }
  
  const newLevel = Math.floor(playerData.xp / 100) + 1;
  if (newLevel > playerData.level) {
    playerData.level = newLevel;
    setTimeout(() => showStatus(`Level Up! ${newLevel} 🆙`, 'blue'), 1000);
  }
  
  saveData();
  updateUI();
  
  // Auto reset timer
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
  const color = isPlayer ? '#60a5fa' : '#f472b6';
  document.getElementById('turnIndicator').style.background = color;
  document.getElementById('turnIndicator').style.boxShadow = `0 0 8px ${color}`;
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
  refreshAds();
}

function setMode(mode, btn) {
  gameMode = mode;
  document.querySelectorAll('.mode-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  newGame();
}

function updateUI() {
  document.getElementById('wins').textContent = playerData.wins;
  document.getElementById('losses').textContent = playerData.losses;
  document.getElementById('draws').textContent = playerData.draws;
  document.getElementById('level').textContent = playerData.level;
}

// Data
function saveData() {
  localStorage.setItem('ttt-data', JSON.stringify(playerData));
}

function loadData() {
  const saved = localStorage.getItem('ttt-data');
  if (saved) playerData = { ...playerData, ...JSON.parse(saved) };
}

function saveSettings() {
  localStorage.setItem('ttt-settings', JSON.stringify(settings));
}

function loadSettings() {
  const saved = localStorage.getItem('ttt-settings');
  if (saved) {
    settings = JSON.parse(saved);
    document.getElementById('soundToggle').classList.toggle('active', settings.sound);
    document.getElementById('musicToggle').classList.toggle('active', settings.music);
    document.getElementById('hapticToggle').classList.toggle('active', settings.haptic);
    
    if (settings.theme) {
      const themeEl = document.querySelector(`[data-theme="${settings.theme}"]`);
      if (themeEl) setTheme(settings.theme, themeEl);
    }
  }
}

function resetAllData() {
  if (confirm('Reset ALL progress and settings?')) {
    playerData = { name: 'Player', wins: 0, losses: 0, draws: 0, xp: 0, level: 1 };
    settings = { sound: true, music: false, haptic: true, theme: 'default' };
    localStorage.removeItem('ttt-data');
    localStorage.removeItem('ttt-settings');
    location.reload();
  }
}

// Confetti
const canvas = document.getElementById('confetti-canvas');
const ctx = canvas.getContext('2d');

function resizeCanvas() {
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
}
resizeCanvas();
window.addEventListener('resize', resizeCanvas);

function fireConfetti() {
  const particles = [];
  const colors = ['#60a5fa', '#f472b6', '#fbbf24', '#34d399', '#a78bfa'];
  
  for (let i = 0; i < 60; i++) {
    particles.push({
      x: canvas.width / 2,
      y: canvas.height / 2,
      vx: (Math.random() - 0.5) * 12,
      vy: (Math.random() - 0.5) * 12 - 4,
      life: 1,
      color: colors[Math.floor(Math.random() * colors.length)],
      size: Math.random() * 5 + 2
    });
  }
  
  function animate() {
    if (particles.length === 0) return;
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    
    particles.forEach((p, i) => {
      p.x += p.vx;
      p.y += p.vy;
      p.vy += 0.25;
      p.life -= 0.015;
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

// Ads
function initAds() {
  try {
    (adsbygoogle = window.adsbygoogle || []).push({});
  } catch (e) {
    console.log('Ads blocked');
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

// Prevent double-tap zoom
let lastTouchEnd = 0;
document.addEventListener('touchend', (e) => {
  const now = Date.now();
  if (now - lastTouchEnd <= 300) e.preventDefault();
  lastTouchEnd = now;
}, false);
</script>

</body>
</html>
