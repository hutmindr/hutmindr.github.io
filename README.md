# hutmindr.github.io[escape-aventura.html](https://github.com/user-attachments/files/28256366/escape-aventura.html)
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Aventura Quest — Escape Game GPS</title>
<link href="https://fonts.googleapis.com/css2?family=Cinzel+Decorative:wght@700;900&family=Crimson+Pro:ital,wght@0,300;0,400;0,600;1,400&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
<style>
  :root {
    --gold: #c9a84c;
    --gold-light: #e8c97a;
    --gold-dark: #7a5c1e;
    --parchment: #f5ecd7;
    --parchment-dark: #e8d5b0;
    --ink: #1a1208;
    --forest: #2d4a1e;
    --forest-light: #4a7a30;
    --rust: #8b3a1a;
    --sky: #1a3a5c;
    --mist: rgba(245,236,215,0.08);
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'Crimson Pro', serif;
    background: var(--ink);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ===== BACKGROUND TEXTURE ===== */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background:
      radial-gradient(ellipse at 20% 20%, rgba(42,70,28,0.4) 0%, transparent 50%),
      radial-gradient(ellipse at 80% 80%, rgba(26,58,92,0.3) 0%, transparent 50%),
      radial-gradient(ellipse at 50% 50%, rgba(139,58,26,0.15) 0%, transparent 70%);
    pointer-events: none;
    z-index: 0;
  }

  /* ===== NAV ===== */
  nav {
    position: fixed;
    top: 0; left: 0; right: 0;
    z-index: 100;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 12px 24px;
    background: linear-gradient(180deg, rgba(10,8,4,0.95) 0%, rgba(10,8,4,0.7) 100%);
    border-bottom: 1px solid rgba(201,168,76,0.3);
    backdrop-filter: blur(12px);
  }

  .nav-logo {
    font-family: 'Cinzel Decorative', cursive;
    font-size: 1.1rem;
    color: var(--gold);
    letter-spacing: 2px;
    text-shadow: 0 0 20px rgba(201,168,76,0.5);
  }

  .nav-tabs {
    display: flex;
    gap: 4px;
    background: rgba(255,255,255,0.05);
    border-radius: 8px;
    padding: 4px;
  }

  .nav-tab {
    padding: 6px 16px;
    border: none;
    border-radius: 6px;
    font-family: 'Space Mono', monospace;
    font-size: 0.7rem;
    letter-spacing: 1px;
    cursor: pointer;
    transition: all 0.3s;
    text-transform: uppercase;
    background: transparent;
    color: rgba(201,168,76,0.6);
  }

  .nav-tab.active {
    background: var(--gold);
    color: var(--ink);
    font-weight: 700;
  }

  /* ===== PAGES ===== */
  .page {
    display: none;
    min-height: 100vh;
    padding-top: 70px;
    position: relative;
    z-index: 1;
  }
  .page.active { display: block; }

  /* ===== ADMIN PAGE ===== */
  .admin-container {
    max-width: 780px;
    margin: 0 auto;
    padding: 32px 20px 60px;
  }

  .admin-title {
    font-family: 'Cinzel Decorative', cursive;
    font-size: 1.6rem;
    color: var(--gold);
    text-align: center;
    margin-bottom: 6px;
    text-shadow: 0 0 30px rgba(201,168,76,0.4);
  }

  .admin-subtitle {
    text-align: center;
    color: rgba(245,236,215,0.5);
    font-size: 1rem;
    margin-bottom: 32px;
    font-style: italic;
  }

  /* General info */
  .admin-section {
    background: rgba(245,236,215,0.04);
    border: 1px solid rgba(201,168,76,0.2);
    border-radius: 12px;
    padding: 24px;
    margin-bottom: 20px;
  }

  .admin-section-title {
    font-family: 'Cinzel Decorative', cursive;
    font-size: 0.75rem;
    color: var(--gold);
    letter-spacing: 3px;
    text-transform: uppercase;
    margin-bottom: 16px;
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .admin-section-title::after {
    content: '';
    flex: 1;
    height: 1px;
    background: rgba(201,168,76,0.2);
  }

  label {
    display: block;
    font-size: 0.78rem;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    color: rgba(201,168,76,0.7);
    margin-bottom: 6px;
    font-family: 'Space Mono', monospace;
  }

  input[type="text"], input[type="number"], textarea, select {
    width: 100%;
    background: rgba(10,8,4,0.6);
    border: 1px solid rgba(201,168,76,0.25);
    border-radius: 8px;
    color: var(--parchment);
    padding: 10px 14px;
    font-family: 'Crimson Pro', serif;
    font-size: 1rem;
    outline: none;
    transition: border-color 0.2s;
    margin-bottom: 14px;
  }

  input:focus, textarea:focus, select:focus {
    border-color: var(--gold);
    box-shadow: 0 0 0 2px rgba(201,168,76,0.1);
  }

  textarea { resize: vertical; min-height: 70px; }

  .gps-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
  }

  /* Step cards */
  .step-card {
    background: rgba(245,236,215,0.03);
    border: 1px solid rgba(201,168,76,0.15);
    border-left: 3px solid var(--gold);
    border-radius: 12px;
    padding: 20px;
    margin-bottom: 16px;
    position: relative;
    transition: border-color 0.2s;
  }

  .step-card:hover { border-color: rgba(201,168,76,0.35); }

  .step-card-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 16px;
  }

  .step-badge {
    font-family: 'Cinzel Decorative', cursive;
    font-size: 0.7rem;
    color: var(--ink);
    background: var(--gold);
    padding: 4px 12px;
    border-radius: 20px;
    letter-spacing: 1px;
  }

  .step-remove {
    background: rgba(139,58,26,0.3);
    border: 1px solid rgba(139,58,26,0.4);
    color: #e07050;
    border-radius: 6px;
    padding: 4px 10px;
    font-size: 0.75rem;
    cursor: pointer;
    font-family: 'Space Mono', monospace;
    transition: all 0.2s;
  }

  .step-remove:hover { background: rgba(139,58,26,0.6); }

  /* Buttons */
  .btn {
    border: none;
    border-radius: 8px;
    cursor: pointer;
    font-family: 'Space Mono', monospace;
    font-size: 0.75rem;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    padding: 12px 24px;
    transition: all 0.25s;
  }

  .btn-add {
    background: transparent;
    border: 1px dashed rgba(201,168,76,0.4);
    color: var(--gold);
    width: 100%;
    margin-bottom: 20px;
    padding: 14px;
  }

  .btn-add:hover {
    background: rgba(201,168,76,0.08);
    border-color: var(--gold);
  }

  .btn-gold {
    background: linear-gradient(135deg, var(--gold) 0%, var(--gold-light) 50%, var(--gold) 100%);
    color: var(--ink);
    font-weight: 700;
    width: 100%;
    padding: 15px;
    font-size: 0.8rem;
    letter-spacing: 2px;
    box-shadow: 0 4px 20px rgba(201,168,76,0.3);
  }

  .btn-gold:hover {
    transform: translateY(-2px);
    box-shadow: 0 8px 30px rgba(201,168,76,0.45);
  }

  .btn-gold:active { transform: translateY(0); }

  .btn-secondary {
    background: rgba(201,168,76,0.1);
    border: 1px solid rgba(201,168,76,0.3);
    color: var(--gold);
    font-size: 0.7rem;
  }

  .btn-secondary:hover { background: rgba(201,168,76,0.2); }

  /* ===== PLAYER PAGE ===== */
  .player-hero {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 20px;
    position: relative;
    overflow: hidden;
  }

  /* Compass decorative */
  .compass-ring {
    position: absolute;
    width: 600px;
    height: 600px;
    border-radius: 50%;
    border: 1px solid rgba(201,168,76,0.08);
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    animation: compassSpin 60s linear infinite;
    pointer-events: none;
  }

  .compass-ring::before {
    content: '◆  N  ◆  E  ◆  S  ◆  O  ◆';
    position: absolute;
    top: 10px;
    left: 50%;
    transform: translateX(-50%);
    color: rgba(201,168,76,0.2);
    font-size: 0.7rem;
    letter-spacing: 8px;
    font-family: 'Space Mono', monospace;
    white-space: nowrap;
  }

  @keyframes compassSpin {
    from { transform: translate(-50%, -50%) rotate(0deg); }
    to { transform: translate(-50%, -50%) rotate(360deg); }
  }

  /* Game screen */
  .game-screen {
    width: 100%;
    max-width: 520px;
    position: relative;
    z-index: 2;
  }

  /* START SCREEN */
  .start-screen {
    text-align: center;
    animation: fadeInUp 0.8s ease;
  }

  @keyframes fadeInUp {
    from { opacity: 0; transform: translateY(30px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .game-title {
    font-family: 'Cinzel Decorative', cursive;
    font-size: clamp(1.8rem, 6vw, 3rem);
    color: var(--gold);
    line-height: 1.2;
    margin-bottom: 8px;
    text-shadow: 0 0 40px rgba(201,168,76,0.6), 0 2px 4px rgba(0,0,0,0.8);
  }

  .game-tagline {
    font-size: 1.15rem;
    font-style: italic;
    color: rgba(245,236,215,0.65);
    margin-bottom: 40px;
  }

  .player-name-input {
    background: rgba(245,236,215,0.06) !important;
    border: 1px solid rgba(201,168,76,0.3) !important;
    text-align: center !important;
    font-size: 1.2rem !important;
    letter-spacing: 2px;
    color: var(--parchment) !important;
    margin-bottom: 20px !important;
    padding: 14px !important;
  }

  /* GPS SCREEN */
  .gps-screen {
    animation: fadeInUp 0.6s ease;
  }

  .step-header {
    text-align: center;
    margin-bottom: 24px;
  }

  .step-counter {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    letter-spacing: 4px;
    color: rgba(201,168,76,0.6);
    text-transform: uppercase;
    margin-bottom: 8px;
  }

  .step-title-text {
    font-family: 'Cinzel Decorative', cursive;
    font-size: 1.1rem;
    color: var(--gold);
    text-shadow: 0 0 20px rgba(201,168,76,0.4);
  }

  /* GPS Card */
  .gps-card {
    background: linear-gradient(135deg, rgba(245,236,215,0.06) 0%, rgba(42,70,28,0.12) 100%);
    border: 1px solid rgba(201,168,76,0.25);
    border-radius: 16px;
    padding: 24px;
    margin-bottom: 16px;
    position: relative;
    overflow: hidden;
  }

  .gps-card::before {
    content: '📍';
    position: absolute;
    top: -10px;
    right: -10px;
    font-size: 80px;
    opacity: 0.05;
    transform: rotate(-15deg);
  }

  .gps-hint-title {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    letter-spacing: 3px;
    color: var(--gold);
    text-transform: uppercase;
    margin-bottom: 10px;
  }

  .gps-hint-text {
    color: var(--parchment);
    font-size: 1.1rem;
    font-style: italic;
    line-height: 1.6;
    margin-bottom: 18px;
  }

  .gps-coords {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
  }

  .coord-chip {
    background: rgba(10,8,4,0.6);
    border: 1px solid rgba(201,168,76,0.3);
    border-radius: 8px;
    padding: 8px 14px;
    font-family: 'Space Mono', monospace;
    font-size: 0.8rem;
    color: var(--gold-light);
    display: flex;
    align-items: center;
    gap: 6px;
    cursor: pointer;
    transition: all 0.2s;
  }

  .coord-chip:hover {
    background: rgba(201,168,76,0.1);
    border-color: var(--gold);
  }

  .coord-chip span { color: rgba(201,168,76,0.5); font-size: 0.65rem; }

  .open-maps-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    width: 100%;
    background: linear-gradient(135deg, rgba(45,74,30,0.6), rgba(42,70,28,0.8));
    border: 1px solid rgba(74,122,48,0.4);
    border-radius: 10px;
    padding: 12px;
    color: #8bc34a;
    font-family: 'Space Mono', monospace;
    font-size: 0.7rem;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    cursor: pointer;
    transition: all 0.2s;
    text-decoration: none;
    margin-top: 14px;
  }

  .open-maps-btn:hover { background: rgba(74,122,48,0.25); border-color: #8bc34a; }

  /* Question Card */
  .question-card {
    background: linear-gradient(135deg, rgba(26,58,92,0.2) 0%, rgba(245,236,215,0.04) 100%);
    border: 1px solid rgba(201,168,76,0.2);
    border-radius: 16px;
    padding: 24px;
    margin-bottom: 16px;
  }

  .question-label {
    font-family: 'Space Mono', monospace;
    font-size: 0.65rem;
    letter-spacing: 3px;
    color: rgba(201,168,76,0.6);
    text-transform: uppercase;
    margin-bottom: 10px;
  }

  .question-text {
    color: var(--parchment);
    font-size: 1.15rem;
    line-height: 1.6;
    margin-bottom: 18px;
  }

  .answer-input {
    background: rgba(10,8,4,0.6) !important;
    border: 1px solid rgba(201,168,76,0.25) !important;
    font-size: 1.1rem !important;
    margin-bottom: 12px !important;
    padding: 12px 16px !important;
    color: var(--parchment) !important;
  }

  .answer-feedback {
    border-radius: 8px;
    padding: 10px 16px;
    font-size: 0.9rem;
    margin-bottom: 12px;
    text-align: center;
    display: none;
    font-family: 'Space Mono', monospace;
    font-size: 0.75rem;
    letter-spacing: 1px;
  }

  .answer-feedback.error {
    background: rgba(139,58,26,0.2);
    border: 1px solid rgba(139,58,26,0.4);
    color: #e07050;
    display: block;
  }

  .answer-feedback.success {
    background: rgba(42,70,28,0.3);
    border: 1px solid rgba(74,122,48,0.4);
    color: #8bc34a;
    display: block;
  }

  /* Progress bar */
  .progress-bar-wrap {
    background: rgba(255,255,255,0.06);
    border-radius: 20px;
    height: 4px;
    margin-bottom: 20px;
    overflow: hidden;
  }

  .progress-bar-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--gold-dark), var(--gold), var(--gold-light));
    border-radius: 20px;
    transition: width 0.8s cubic-bezier(0.4, 0, 0.2, 1);
    box-shadow: 0 0 8px rgba(201,168,76,0.5);
  }

  /* END SCREEN */
  .end-screen {
    text-align: center;
    animation: fadeInUp 0.8s ease;
  }

  .trophy {
    font-size: 5rem;
    margin-bottom: 16px;
    animation: trophyBounce 1s ease 0.3s both;
    display: block;
  }

  @keyframes trophyBounce {
    0% { transform: scale(0) rotate(-10deg); opacity: 0; }
    60% { transform: scale(1.2) rotate(5deg); opacity: 1; }
    100% { transform: scale(1) rotate(0deg); opacity: 1; }
  }

  .end-title {
    font-family: 'Cinzel Decorative', cursive;
    font-size: 1.8rem;
    color: var(--gold);
    margin-bottom: 8px;
    text-shadow: 0 0 30px rgba(201,168,76,0.6);
  }

  .end-message {
    color: var(--parchment);
    font-size: 1.2rem;
    font-style: italic;
    line-height: 1.7;
    background: rgba(245,236,215,0.05);
    border: 1px solid rgba(201,168,76,0.2);
    border-radius: 12px;
    padding: 20px 24px;
    margin: 20px 0;
  }

  .confetti-star {
    position: fixed;
    pointer-events: none;
    font-size: 1.2rem;
    animation: starFall linear forwards;
    z-index: 999;
  }

  @keyframes starFall {
    from { transform: translateY(-50px) rotate(0deg); opacity: 1; }
    to { transform: translateY(110vh) rotate(720deg); opacity: 0; }
  }

  /* ===== NO GAME STATE ===== */
  .no-game {
    text-align: center;
    padding: 60px 20px;
    color: rgba(245,236,215,0.4);
  }

  .no-game-icon { font-size: 3rem; margin-bottom: 12px; }
  .no-game-text { font-size: 1.1rem; font-style: italic; }

  /* ===== TOAST ===== */
  .toast {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%) translateY(80px);
    background: rgba(42,70,28,0.95);
    border: 1px solid rgba(74,122,48,0.6);
    border-radius: 10px;
    padding: 12px 24px;
    color: #8bc34a;
    font-family: 'Space Mono', monospace;
    font-size: 0.75rem;
    letter-spacing: 1px;
    transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
    z-index: 1000;
    white-space: nowrap;
  }

  .toast.show { transform: translateX(-50%) translateY(0); }

  /* ===== RESPONSIVE ===== */
  @media (max-width: 480px) {
    .gps-row { grid-template-columns: 1fr; }
    .gps-coords { flex-direction: column; }
    nav { padding: 10px 16px; }
    .nav-logo { font-size: 0.85rem; }
  }
</style>
</head>
<body>

<!-- NAV -->
<nav>
  <div class="nav-logo">⚔ Aventura</div>
  <div class="nav-tabs">
    <button class="nav-tab active" onclick="showPage('admin')">⚙ Admin</button>
    <button class="nav-tab" onclick="showPage('player')">🧭 Joueur</button>
  </div>
</nav>

<!-- ===== PAGE ADMIN ===== -->
<div class="page active" id="page-admin">
  <div class="admin-container">
    <h1 class="admin-title">⚙ Configuration</h1>
    <p class="admin-subtitle">Préparez votre escape game en quelques étapes</p>

    <!-- General settings -->
    <div class="admin-section">
      <div class="admin-section-title">🗺 Informations générales</div>

      <label>Titre du jeu</label>
      <input type="text" id="game-title" placeholder="Ex: La Quête des Templiers" value="La Quête des Anciens">

      <label>Message de bienvenue</label>
      <textarea id="game-intro" placeholder="Introduction affichée au joueur au démarrage…">Aventurier, une quête ancestrale vous attend. Suivez les indices, résolvez les énigmes et découvrez ce que les anciens ont caché.</textarea>

      <label>Message de fin (succès)</label>
      <textarea id="game-end" placeholder="Message affiché à la fin du jeu…">Félicitations ! Vous avez résolu toutes les énigmes et découvert le secret. La légende dit que seuls les plus courageux y parviennent. Vous êtes de ceux-là.</textarea>
    </div>

    <!-- Steps -->
    <div class="admin-section">
      <div class="admin-section-title">📍 Étapes GPS</div>
      <div id="steps-container"></div>
      <button class="btn btn-add" onclick="addStep()">+ Ajouter une étape</button>
    </div>

    <button class="btn btn-gold" onclick="saveGame()">💾 Sauvegarder & Lancer le jeu</button>

    <div style="margin-top:12px; display:flex; gap:10px;">
      <button class="btn btn-secondary" style="flex:1" onclick="exportGame()">📤 Exporter JSON</button>
      <button class="btn btn-secondary" style="flex:1" onclick="document.getElementById('import-file').click()">📥 Importer JSON</button>
      <input type="file" id="import-file" accept=".json" style="display:none" onchange="importGame(event)">
    </div>
  </div>
</div>

<!-- ===== PAGE JOUEUR ===== -->
<div class="page" id="page-player">
  <div class="player-hero">
    <div class="compass-ring"></div>
    <div class="game-screen" id="game-screen">
      <!-- Injected by JS -->
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
// ==============================
// DATA
// ==============================
let gameData = JSON.parse(localStorage.getItem('aventura_game') || 'null');
let stepCount = 0;

// Game state
let playerName = '';
let currentStep = 0;
let gameSteps = [];
let gameConfig = {};

// ==============================
// ADMIN LOGIC
// ==============================
function addStep(data = {}) {
  stepCount++;
  const id = stepCount;
  const container = document.getElementById('steps-container');
  const div = document.createElement('div');
  div.className = 'step-card';
  div.id = `step-${id}`;
  div.innerHTML = `
    <div class="step-card-header">
      <span class="step-badge">Étape ${container.children.length + 1}</span>
      <button class="step-remove" onclick="removeStep('step-${id}')">✕ Supprimer</button>
    </div>
    <label>Indice / Texte de navigation</label>
    <textarea class="step-hint" placeholder="Ex: Rendez-vous devant l'église du village, face au vieux chêne centenaire…" rows="2">${data.hint || ''}</textarea>
    <div class="gps-row">
      <div>
        <label>Latitude</label>
        <input type="text" class="step-lat" placeholder="Ex: 46.123456" value="${data.lat || ''}">
      </div>
      <div>
        <label>Longitude</label>
        <input type="text" class="step-lng" placeholder="Ex: 1.234567" value="${data.lng || ''}">
      </div>
    </div>
    <label>Question</label>
    <input type="text" class="step-question" placeholder="Ex: Quelle année figure sur la plaque commémorative ?" value="${data.question || ''}">
    <label>Réponse(s) acceptée(s) <span style="font-size:0.7em;opacity:.6">(séparez par | pour plusieurs variantes)</span></label>
    <input type="text" class="step-answer" placeholder="Ex: 1789 | mil sept cent quatre-vingt-neuf" value="${data.answer || ''}">
  `;
  container.appendChild(div);
  renumberSteps();
}

function removeStep(id) {
  document.getElementById(id)?.remove();
  renumberSteps();
}

function renumberSteps() {
  document.querySelectorAll('#steps-container .step-card').forEach((card, i) => {
    card.querySelector('.step-badge').textContent = `Étape ${i + 1}`;
  });
}

function collectSteps() {
  return Array.from(document.querySelectorAll('#steps-container .step-card')).map(card => ({
    hint: card.querySelector('.step-hint').value.trim(),
    lat: card.querySelector('.step-lat').value.trim(),
    lng: card.querySelector('.step-lng').value.trim(),
    question: card.querySelector('.step-question').value.trim(),
    answer: card.querySelector('.step-answer').value.trim(),
  }));
}

function saveGame() {
  const title = document.getElementById('game-title').value.trim();
  const intro = document.getElementById('game-intro').value.trim();
  const endMsg = document.getElementById('game-end').value.trim();
  const steps = collectSteps();

  if (!title) { showToast('❌ Ajoutez un titre au jeu'); return; }
  if (steps.length < 1) { showToast('❌ Ajoutez au moins une étape'); return; }

  for (let i = 0; i < steps.length; i++) {
    const s = steps[i];
    if (!s.question || !s.answer) {
      showToast(`❌ Étape ${i + 1} : question et réponse obligatoires`); return;
    }
  }

  const data = { title, intro, endMsg, steps };
  localStorage.setItem('aventura_game', JSON.stringify(data));
  gameData = data;
  showToast('✅ Jeu sauvegardé !');
  setTimeout(() => showPage('player'), 800);
}

function exportGame() {
  const data = {
    title: document.getElementById('game-title').value,
    intro: document.getElementById('game-intro').value,
    endMsg: document.getElementById('game-end').value,
    steps: collectSteps()
  };
  const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'aventura-game.json';
  a.click();
}

function importGame(e) {
  const file = e.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = (ev) => {
    try {
      const data = JSON.parse(ev.target.result);
      loadAdminForm(data);
      showToast('✅ Jeu importé !');
    } catch { showToast('❌ Fichier invalide'); }
  };
  reader.readAsText(file);
  e.target.value = '';
}

function loadAdminForm(data) {
  document.getElementById('game-title').value = data.title || '';
  document.getElementById('game-intro').value = data.intro || '';
  document.getElementById('game-end').value = data.endMsg || '';
  document.getElementById('steps-container').innerHTML = '';
  stepCount = 0;
  (data.steps || []).forEach(s => addStep(s));
}

// ==============================
// PLAYER LOGIC
// ==============================
function renderStart() {
  if (!gameData) {
    document.getElementById('game-screen').innerHTML = `
      <div class="no-game">
        <div class="no-game-icon">🗺</div>
        <div class="no-game-text">Aucun jeu configuré.<br>Allez dans l'onglet <strong style="color:var(--gold)">Admin</strong> pour créer votre aventure.</div>
      </div>`;
    return;
  }
  gameSteps = gameData.steps;
  gameConfig = gameData;

  document.getElementById('game-screen').innerHTML = `
    <div class="start-screen">
      <div class="game-title">${escHtml(gameConfig.title)}</div>
      <div class="game-tagline">Escape Game GPS</div>
      <p style="color:var(--parchment);font-size:1.05rem;line-height:1.7;font-style:italic;margin-bottom:28px;opacity:.8;">${escHtml(gameConfig.intro)}</p>
      <label for="pname" style="color:rgba(201,168,76,.7);font-family:'Space Mono',monospace;font-size:.7rem;letter-spacing:2px;text-transform:uppercase;text-align:center;display:block;margin-bottom:8px;">Votre nom d'aventurier</label>
      <input class="player-name-input" id="pname" type="text" placeholder="Ex: Perceval">
      <button class="btn btn-gold" onclick="startGame()" style="margin-top:4px;">⚔ Commencer la quête</button>
    </div>`;
}

function startGame() {
  playerName = document.getElementById('pname').value.trim() || 'Aventurier';
  currentStep = 0;
  renderStep();
}

function renderStep() {
  const step = gameSteps[currentStep];
  const progress = Math.round((currentStep / gameSteps.length) * 100);
  const mapsUrl = (step.lat && step.lng)
    ? `https://www.google.com/maps/search/?api=1&query=${step.lat},${step.lng}`
    : null;

  document.getElementById('game-screen').innerHTML = `
    <div class="gps-screen">
      <div class="step-header">
        <div class="step-counter">Étape ${currentStep + 1} sur ${gameSteps.length}</div>
        <div class="step-title-text">${escHtml(playerName)}</div>
      </div>

      <div class="progress-bar-wrap">
        <div class="progress-bar-fill" id="prog-bar" style="width:0%"></div>
      </div>

      <div class="gps-card">
        <div class="gps-hint-title">🧭 Indice de navigation</div>
        <div class="gps-hint-text">${escHtml(step.hint) || '<em style="opacity:.5">Aucun indice fourni</em>'}</div>
        ${(step.lat && step.lng) ? `
          <div class="gps-coords">
            <div class="coord-chip" onclick="copyToClipboard('${step.lat}')" title="Copier la latitude">
              <span>LAT</span>${step.lat}
            </div>
            <div class="coord-chip" onclick="copyToClipboard('${step.lng}')" title="Copier la longitude">
              <span>LNG</span>${step.lng}
            </div>
          </div>
          <a class="open-maps-btn" href="${mapsUrl}" target="_blank">
            🗺 Ouvrir dans Google Maps
          </a>` : `<em style="color:rgba(245,236,215,.4);font-size:.9rem">Aucune coordonnée GPS pour cette étape</em>`}
      </div>

      <div class="question-card">
        <div class="question-label">🔍 Énigme</div>
        <div class="question-text">${escHtml(step.question)}</div>
        <input class="answer-input" type="text" id="answer-input" placeholder="Votre réponse…" onkeydown="if(event.key==='Enter') checkAnswer()">
        <div class="answer-feedback" id="answer-feedback"></div>
        <button class="btn btn-gold" onclick="checkAnswer()">Valider la réponse →</button>
      </div>
    </div>`;

  setTimeout(() => {
    const bar = document.getElementById('prog-bar');
    if (bar) bar.style.width = progress + '%';
  }, 100);
}

function checkAnswer() {
  const step = gameSteps[currentStep];
  const input = document.getElementById('answer-input');
  const feedback = document.getElementById('answer-feedback');
  const userAnswer = (input?.value || '').trim().toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g, '');

  const accepted = step.answer.split('|').map(a =>
    a.trim().toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g, '')
  );

  if (accepted.includes(userAnswer)) {
    feedback.textContent = '✓ Bonne réponse ! La suite se déverrouille…';
    feedback.className = 'answer-feedback success';
    input.disabled = true;
    setTimeout(() => {
      currentStep++;
      if (currentStep >= gameSteps.length) renderEnd();
      else renderStep();
    }, 1400);
  } else {
    feedback.textContent = '✗ Ce n\'est pas la bonne réponse. Cherchez encore…';
    feedback.className = 'answer-feedback error';
    input.style.borderColor = 'rgba(139,58,26,0.6)';
    setTimeout(() => {
      input.style.borderColor = '';
    }, 1000);
  }
}

function renderEnd() {
  launchConfetti();
  document.getElementById('game-screen').innerHTML = `
    <div class="end-screen">
      <span class="trophy">🏆</span>
      <div class="end-title">Quête accomplie !</div>
      <div class="end-message">${escHtml(gameConfig.endMsg)}</div>
      <p style="color:rgba(245,236,215,.5);font-size:.9rem;margin-bottom:24px;font-style:italic;">— ${escHtml(playerName)}, Aventurier légendaire</p>
      <button class="btn btn-secondary" onclick="renderStart()">↩ Rejouer</button>
    </div>`;
}

function launchConfetti() {
  const icons = ['⭐','✨','🌟','💫','🗺','🔑','⚔'];
  for (let i = 0; i < 22; i++) {
    setTimeout(() => {
      const el = document.createElement('div');
      el.className = 'confetti-star';
      el.textContent = icons[Math.floor(Math.random() * icons.length)];
      el.style.left = Math.random() * 100 + 'vw';
      el.style.animationDuration = (2 + Math.random() * 3) + 's';
      el.style.animationDelay = Math.random() * 1.5 + 's';
      el.style.fontSize = (0.8 + Math.random() * 1.2) + 'rem';
      document.body.appendChild(el);
      setTimeout(() => el.remove(), 6000);
    }, i * 80);
  }
}

// ==============================
// UTILS
// ==============================
function showPage(page) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
  document.getElementById(`page-${page}`).classList.add('active');
  const tabs = document.querySelectorAll('.nav-tab');
  if (page === 'admin') tabs[0].classList.add('active');
  else { tabs[1].classList.add('active'); renderStart(); }
}

function escHtml(str = '') {
  return str.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;').replace(/\n/g,'<br>');
}

function copyToClipboard(text) {
  navigator.clipboard.writeText(text).then(() => showToast('📋 Copié : ' + text));
}

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2500);
}

// ==============================
// INIT
// ==============================
window.addEventListener('DOMContentLoaded', () => {
  // Load saved game into admin form if exists
  if (gameData) loadAdminForm(gameData);
  else {
    // Pre-fill with 3 example steps
    addStep({
      hint: "Rendez-vous face à la mairie du village. Cherchez la façade principale.",
      lat: "46.232193",
      lng: "2.209667",
      question: "Quelle année figure sur le fronton de la mairie ?",
      answer: "1889 | mil huit cent quatre-vingt-neuf"
    });
    addStep({
      hint: "Marchez jusqu'au vieux pont de pierre enjambant la rivière.",
      lat: "46.234100",
      lng: "2.211200",
      question: "Combien d'arches possède ce pont ?",
      answer: "3 | trois"
    });
    addStep({
      hint: "Rejoignez la fontaine centrale de la place du marché.",
      lat: "46.230800",
      lng: "2.208500",
      question: "Quel animal figure sur le sommet de la fontaine ?",
      answer: "lion | un lion"
    });
  }
});
</script>
</body>
</html>
