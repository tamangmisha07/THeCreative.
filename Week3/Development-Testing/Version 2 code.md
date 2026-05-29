<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
<title>CybShield - 2FA Adventure | Level Selection</title>
<link href="https://fonts.googleapis.com/css2?family=Fredoka+One&family=Nunito:wght@400;600;700;800;900&display=swap" rel="stylesheet">
<style>
  :root {
    --sky: #00c9ff;
    --sky2: #92effd;
    --yellow: #FFD700;
    --green: #2ecc40;
    --green2: #27ae60;
    --pink: #ff6eb4;
    --purple: #a855f7;
    --orange: #ff7f11;
    --white: #fff;
    --dark: #1a1a2e;
    --card: rgba(255,255,255,0.18);
    --shadow: 0 8px 32px rgba(0,0,0,0.18);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'Nunito', sans-serif;
    background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
    min-height: 100vh;
    overflow-x: hidden;
    position: relative;
  }

  .stars { position: fixed; inset: 0; z-index: 0; pointer-events: none; }
  .star {
    position: absolute;
    border-radius: 50%;
    background: #fff;
    animation: twinkle 2s infinite alternate;
  }
  @keyframes twinkle { from { opacity:.2; transform:scale(1); } to { opacity:1; transform:scale(1.4); } }

  .bubble {
    position: fixed;
    border-radius: 50%;
    opacity: 0.12;
    animation: floatUp linear infinite;
    pointer-events: none;
    z-index: 0;
  }
  @keyframes floatUp {
    0%   { transform: translateY(100vh) scale(1);   opacity: .12; }
    100% { transform: translateY(-200px) scale(1.3); opacity: 0;   }
  }

  .wrapper {
    position: relative;
    z-index: 1;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 0 16px 140px;
  }

  /* Welcome Page */
  .page {
    width: 100%;
    transition: all 0.4s ease;
  }
  .page.hidden {
    display: none;
  }

  .title-wrap {
    margin-top: 28px;
    text-align: center;
    animation: popIn .8s cubic-bezier(.36,1.6,.4,1) both;
  }
  @keyframes popIn { from { opacity:0; transform:scale(.5) translateY(-40px); } to { opacity:1; transform:none; } }

  .title-main {
    font-family: 'Fredoka One', cursive;
    font-size: clamp(3rem, 10vw, 6rem);
    background: linear-gradient(90deg, #FFD700, #ff6eb4, #00c9ff, #2ecc40, #FFD700);
    background-size: 300%;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    animation: shimmer 4s linear infinite;
    line-height: 1;
  }
  @keyframes shimmer { 0%{background-position:0%} 100%{background-position:300%} }

  .title-sub {
    font-family: 'Fredoka One', cursive;
    font-size: clamp(1rem, 3vw, 1.6rem);
    color: var(--sky2);
    letter-spacing: 2px;
    margin-top: 4px;
  }

  .badge-2fa {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    background: linear-gradient(135deg, #ff6eb4, #a855f7);
    color: #fff;
    font-family: 'Fredoka One', cursive;
    font-size: 1.1rem;
    padding: 6px 20px;
    border-radius: 50px;
    margin: 10px auto 0;
    box-shadow: 0 4px 18px #a855f788;
  }

  .score-section, .reward-section, .char-section {
    width: 100%;
    max-width: 560px;
    margin: 18px auto;
    background: rgba(255,255,255,0.07);
    border-radius: 20px;
    padding: 14px 20px;
  }

  .char-grid {
    display: flex;
    gap: 12px;
    justify-content: center;
    flex-wrap: wrap;
  }
  .char-card {
    width: 100px;
    background: rgba(255,255,255,0.08);
    border: 3px solid transparent;
    border-radius: 20px;
    padding: 12px 8px;
    text-align: center;
    cursor: pointer;
    transition: all .35s;
  }
  .char-card.selected {
    border-color: var(--yellow);
    background: rgba(255,215,0,0.12);
    transform: scale(1.05);
  }
  .char-avatar {
    width: 60px;
    height: 60px;
    border-radius: 50%;
    margin: 0 auto 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
  }
  .char-name { font-family: 'Fredoka One', cursive; font-size: .85rem; color: #fff; }

  /* LEVEL SELECTION PAGE (CANDY CRUSH STYLE) */
  .level-select-container {
    text-align: center;
    padding: 20px 0;
  }
  .level-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(90px, 100px));
    gap: 20px;
    justify-content: center;
    margin: 30px 0;
  }
  .level-card {
    background: linear-gradient(145deg, #2a2a4a, #1e1e3a);
    border-radius: 30px;
    padding: 18px 8px;
    text-align: center;
    cursor: pointer;
    transition: all 0.2s;
    border: 2px solid #ffdd77aa;
    position: relative;
  }
  .level-card.locked {
    filter: grayscale(0.4);
    opacity: 0.6;
    cursor: not-allowed;
    border-color: #666;
  }
  .level-card.unlocked {
    background: linear-gradient(145deg, #3a2e6e, #2a1e5e);
    box-shadow: 0 8px 20px #ffd70033;
  }
  .level-card.unlocked:hover {
    transform: translateY(-6px);
    border-color: #FFD700;
  }
  .level-number {
    font-family: 'Fredoka One', cursive;
    font-size: 2rem;
    color: #FFD700;
  }
  .lock-icon {
    font-size: 1.3rem;
    position: absolute;
    top: 5px;
    right: 8px;
  }
  .level-info {
    background: #00000099;
    border-radius: 20px;
    padding: 15px;
    margin: 15px 0;
    border-left: 5px solid #FFD700;
    text-align: left;
  }
  .back-btn {
    background: #ff6eb4cc;
    border: none;
    border-radius: 40px;
    padding: 12px 25px;
    font-family: 'Fredoka One', cursive;
    margin-top: 15px;
    cursor: pointer;
  }

  /* QUIZ PAGE */
  .quiz-panel {
    width: 100%;
    max-width: 650px;
    margin: 20px auto;
    background: rgba(0,0,0,0.65);
    backdrop-filter: blur(12px);
    border-radius: 40px;
    padding: 20px;
    border: 2px solid #FFD700;
  }
  .quiz-header {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
    font-weight: bold;
    color: #FFD700;
  }
  .question-text {
    font-size: 1.4rem;
    font-weight: 800;
    color: white;
    margin: 15px 0;
  }
  .options-grid {
    display: flex;
    flex-direction: column;
    gap: 12px;
    margin-bottom: 20px;
  }
  .quiz-option {
    background: rgba(255,255,240,0.12);
    border-radius: 60px;
    padding: 12px 20px;
    font-weight: 700;
    cursor: pointer;
    border: 1px solid #ccc8;
    color: #f0f0ff;
    transition: 0.2s;
  }
  .quiz-option:hover {
    background: rgba(255,215,0,0.3);
    transform: scale(1.02);
  }
  .quiz-option.disabled {
    pointer-events: none;
    opacity: 0.6;
  }
  .quiz-feedback {
    background: #00000088;
    border-radius: 28px;
    padding: 12px;
    text-align: center;
    margin: 10px 0;
  }
  .next-btn, .quit-btn {
    background: linear-gradient(135deg, #FFD700, #ff9f00);
    border: none;
    border-radius: 50px;
    padding: 12px 20px;
    font-family: 'Fredoka One', cursive;
    font-weight: bold;
    cursor: pointer;
    width: 100%;
    margin-top: 10px;
  }
  .quit-btn {
    background: #a855f7;
    color: white;
  }

  .bottom-bar {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: center;
    gap: 20px;
    padding: 16px;
    background: linear-gradient(to top, #0f0c29dd, transparent);
    z-index: 50;
  }
  .btn-start {
    background: linear-gradient(135deg, #2ecc40, #27ae60);
    color: white;
    font-family: 'Fredoka One', cursive;
    border: none;
    border-radius: 50px;
    padding: 14px 30px;
    cursor: pointer;
  }
  .overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(10,10,30,0.85);
    backdrop-filter: blur(6px);
    z-index: 200;
    align-items: center;
    justify-content: center;
  }
  .overlay.active { display: flex; }
  .modal {
    background: linear-gradient(135deg, #1a1a3e, #2d1b69);
    border: 3px solid var(--yellow);
    border-radius: 28px;
    padding: 28px;
    max-width: 400px;
    text-align: center;
  }
  .modal-close {
    background: var(--yellow);
    border: none;
    border-radius: 50px;
    padding: 10px 24px;
    font-family: 'Fredoka One', cursive;
    cursor: pointer;
    margin-top: 16px;
  }
</style>
</head>
<body>
<div class="stars" id="stars"></div>
<div id="bubbles"></div>

<!-- WELCOME PAGE -->
<div id="welcomePage" class="page">
  <div class="wrapper">
    <div class="title-wrap">
      <span class="shield-icon">🛡️</span>
      <div class="title-main">CybShield</div>
      <div class="title-sub">✨ Cyber Security Adventure! ✨</div>
      <div class="badge-2fa">🔐 2FA HERO QUEST</div>
    </div>
    <div class="score-section">
      <div class="score-label">⭐ Your Score</div>
      <div class="score-track"><div class="score-fill" id="welcomeScoreFill" style="width:0%"></div><span class="score-value" id="welcomeScoreValue">0 / 100</span></div>
    </div>
    <div class="reward-section">
      <div class="reward-title">🏆 Reward Badges</div>
      <div class="reward-badges" id="badgesContainer">
        <div class="reward-badge"><span class="badge-icon">🌟</span><span class="badge-name">Starter</span></div>
        <div class="reward-badge"><span class="badge-icon">🔓</span><span class="badge-name">Breaker</span></div>
        <div class="reward-badge"><span class="badge-icon">🛡️</span><span class="badge-name">Shield</span></div>
        <div class="reward-badge"><span class="badge-icon">🥷</span><span class="badge-name">Ninja</span></div>
        <div class="reward-badge"><span class="badge-icon">👑</span><span class="badge-name">Legend</span></div>
      </div>
    </div>
    <div class="char-section">
      <div class="char-title">🎮 Choose Your Hero!</div>
      <div class="char-grid" id="charGridWelcome">
        <div class="char-card" data-char="priksha"><div class="char-avatar" style="background:#00c9ff">🧙‍♀️</div><div class="char-name">Priksha</div></div>
        <div class="char-card" data-char="pawan"><div class="char-avatar" style="background:#ff7f11">⚔️</div><div class="char-name">Pawan</div></div>
        <div class="char-card" data-char="misha"><div class="char-avatar" style="background:#ff6eb4">🛡️</div><div class="char-name">Misha</div></div>
        <div class="char-card" data-char="yosheph"><div class="char-avatar" style="background:#a855f7">🔍</div><div class="char-name">Yosheph</div></div>
        <div class="char-card" data-char="krishna"><div class="char-avatar" style="background:#2ecc40">🌿</div><div class="char-name">Krishna</div></div>
      </div>
    </div>
    <div class="bottom-bar" style="position: relative; margin-top: 40px;">
      <button class="btn-start" id="startGameBtn">▶️ START ADVENTURE</button>
    </div>
  </div>
</div>

<!-- LEVEL SELECTION PAGE -->
<div id="levelSelectPage" class="page hidden">
  <div class="wrapper">
    <div class="title-wrap">
      <div class="title-main">🌍 Select Level</div>
      <div class="title-sub">Choose your mission</div>
    </div>
    <div class="level-select-container">
      <div class="level-grid" id="levelGrid"></div>
      <div id="levelInfoBox" class="level-info" style="display: none;"></div>
      <button class="back-btn" id="backToWelcomeBtn">🔙 Back to Heroes</button>
    </div>
  </div>
</div>

<!-- QUIZ GAME PAGE (LEVEL 1) -->
<div id="quizPage" class="page hidden">
  <div class="wrapper">
    <div class="quiz-panel">
      <div class="quiz-header">
        <span id="levelBadge">🔐 LEVEL 1: EASY</span>
        <span>💰 NPR: <span id="quizNrsDisplay">0</span></span>
      </div>
      <div class="question-text" id="quizQuestion">Loading...</div>
      <div class="options-grid" id="quizOptions"></div>
      <div class="quiz-feedback" id="quizFeedbackMsg"></div>
      <button class="next-btn" id="quizNextBtn" disabled>Next Question ➡️</button>
      <button class="quit-btn" id="quitQuizBtn">🏠 Exit Level</button>
    </div>
  </div>
</div>

<!-- Modals -->
<div class="overlay" id="infoModal"><div class="modal"><span class="modal-icon">ℹ️</span><div class="modal-title" id="infoTitle"></div><div class="modal-body" id="infoBody"></div><button class="modal-close" onclick="closeModal('infoModal')">OK</button></div></div>

<script>
  // ---------- GLOBAL STATE ----------
  let selectedHeroChar = null;
  let currentLevel = 1;        // only level 1 unlocked initially
  let unlockedLevels = [1];     // level 1 is unlocked
  let levelQuestions = [];      // will store 5 easy 2FA questions for level 1
  let currentQIndex = 0;
  let totalEarnedNPR = 0;
  let totalCyberScore = 0;      // max 100 (20 per correct)
  let quizActive = false;
  let answered = false;
  let currentQuestionObj = null;

  // Easy 2FA Questions for LEVEL 1
  const easyQuestions = [
    { text: "What does '2FA' stand for?", options: ["Two Factor Authentication", "Two File Access", "Dual Firewall Action", "Second Password Lock"], correct: 0 },
    { text: "Which of these is an example of a second factor?", options: ["SMS one-time code", "Your pet's name", "Your birth date", "Your favorite color"], correct: 0 },
    { text: "What is the main benefit of 2FA?", options: ["Extra security layer", "Faster login", "No need for passwords", "It's free"], correct: 0 },
    { text: "Which app is commonly used for 2FA codes?", options: ["Google Authenticator", "Facebook", "WhatsApp", "Snapchat"], correct: 0 },
    { text: "If someone gets your password but you have 2FA, can they access your account?", options: ["No, they need the second factor too", "Yes, password is enough", "Only on weekends", "2FA is useless"], correct: 0 }
  ];

  // Helper: generate stars, bubbles (existing)
  const starsEl = document.getElementById('stars');
  for (let i = 0; i < 80; i++) {
    const s = document.createElement('div'); s.className = 'star';
    const size = Math.random() * 3 + 1;
    s.style.cssText = width:${size}px;height:${size}px;left:${Math.random()*100}%;top:${Math.random()*100}%;animation-delay:${Math.random()*4}s;;
    starsEl.appendChild(s);
  }
  const bubblesEl = document.getElementById('bubbles');
  const colors = ['#00c9ff','#ff6eb4','#FFD700','#2ecc40','#a855f7'];
  for (let i=0;i<18;i++) {
    const b = document.createElement('div'); b.className = 'bubble';
    const sz = 25+Math.random()*80;
    b.style.cssText = width:${sz}px;height:${sz}px;left:${Math.random()*100}%;background:${colors[Math.floor(Math.random()*colors.length)]};animation-duration:${8+Math.random()*14}s;;
    bubblesEl.appendChild(b);
  }

  // Update welcome page UI
  function updateWelcomeUI() {
    let percent = (totalCyberScore / 100) * 100;
    document.getElementById('welcomeScoreFill').style.width = ${percent}%;
    document.getElementById('welcomeScoreValue').innerText = ${totalCyberScore} / 100;
    const badges = document.querySelectorAll('#badgesContainer .reward-badge');
    const thresholds = [0, 20, 40, 70, 100];
    badges.forEach((badge, idx) => {
      if (totalCyberScore >= thresholds[idx]) badge.classList.add('earned');
      else badge.classList.remove('earned');
    });
  }

  // Hero selection
  document.querySelectorAll('#charGridWelcome .char-card').forEach(card => {
    card.addEventListener('click', function() {
      document.querySelectorAll('#charGridWelcome .char-card').forEach(c => c.classList.remove('selected'));
      this.classList.add('selected');
      selectedHeroChar = this.dataset.char;
    });
  });

  // Build level grid (10 levels, only level 1 unlocked)
  function buildLevelGrid() {
    const grid = document.getElementById('levelGrid');
    grid.innerHTML = '';
    for (let i = 1; i <= 10; i++) {
      const isUnlocked = unlockedLevels.includes(i);
      const levelDiv = document.createElement('div');
      levelDiv.className = level-card ${isUnlocked ? 'unlocked' : 'locked'};
      levelDiv.innerHTML = `
        <div class="level-number">${i}</div>
        ${!isUnlocked ? '<div class="lock-icon">🔒</div>' : '<div class="lock-icon">🔓</div>'}
        <div style="font-size:0.7rem; margin-top:5px;">${isUnlocked ? 'Play' : 'Locked'}</div>
      `;
      if (isUnlocked) {
        levelDiv.addEventListener('click', () => selectLevel(i));
      } else {
        levelDiv.addEventListener('click', () => {
          showInfoModal("🔒 Level Locked", "Complete previous level to unlock this challenge!");
        });
      }
      grid.appendChild(levelDiv);
    }
  }

  function selectLevel(levelNum) {
    if (levelNum === 1) {
      // Show level information message
      document.getElementById('levelInfoBox').style.display = 'block';
      document.getElementById('levelInfoBox').innerHTML = `
        <strong>📘 LEVEL 1: EASY MODE</strong><br>
        🎯 Mission: Answer 5 basic 2FA questions.<br>
        💰 Reward: 100 Nepali Rupees per correct answer!<br>
        ⭐ Total possible: 500 NPR + Cyber Score.<br>
        ✨ "Level one is the easy level, you can do it!" ✨
        <button id="startLevel1Btn" style="margin-top:12px; background:#FFD700; border:none; border-radius:30px; padding:8px 18px; font-weight:bold; cursor:pointer;">🚀 START LEVEL 1</button>
      `;
      const startBtn = document.getElementById('startLevel1Btn');
      if (startBtn) startBtn.onclick = () => startLevelOneGame();
    } else {
      showInfoModal("⚠️ Not Available", "Only Level 1 is ready for now. Complete Level 1 to unlock more!");
    }
  }

  function startLevelOneGame() {
    // Reset quiz state
    currentQIndex = 0;
    totalEarnedNPR = 0;
    totalCyberScore = 0;
    answered = false;
    quizActive = true;
    levelQuestions = [...easyQuestions];  // 5 easy questions
    updateWelcomeUI();
    document.getElementById('quizNrsDisplay').innerText = totalEarnedNPR;
    document.getElementById('quizFeedbackMsg').innerHTML = '';
    showQuizPage();
    renderCurrentQuestion();
  }

  function renderCurrentQuestion() {
    if (!quizActive) return;
    if (currentQIndex >= levelQuestions.length) {
      finishLevelComplete();
      return;
    }
    answered = false;
    currentQuestionObj = levelQuestions[currentQIndex];
    document.getElementById('quizQuestion').innerText = currentQuestionObj.text;
    const optsDiv = document.getElementById('quizOptions');
    optsDiv.innerHTML = '';
    currentQuestionObj.options.forEach((opt, idx) => {
      const optEl = document.createElement('div');
      optEl.className = 'quiz-option';
      optEl.innerHTML = <span style="font-weight:900;">${String.fromCharCode(65+idx)}.</span> ${opt};
      optEl.addEventListener('click', () => handleAnswer(idx, optEl));
      optsDiv.appendChild(optEl);
    });
    document.getElementById('quizNextBtn').disabled = true;
    document.getElementById('quizNextBtn').innerText = '⏳ Choose an answer';
  }

  function handleAnswer(selectedIdx, element) {
    if (!quizActive || answered) return;
    const isCorrect = (selectedIdx === currentQuestionObj.correct);
    if (isCorrect) {
      // Reward: 100 NPR per question + 20 Cyber Score
      totalEarnedNPR += 100;
      totalCyberScore += 20;
      if (totalCyberScore > 100) totalCyberScore = 100;
      updateWelcomeUI();
      document.getElementById('quizNrsDisplay').innerText = totalEarnedNPR;
      document.getElementById('quizFeedbackMsg').innerHTML = '✅ CORRECT! +100 NPR & +20 Cyber Score! 🎉';
      document.getElementById('quizFeedbackMsg').style.color = '#a0ffa0';
    } else {
      document.getElementById('quizFeedbackMsg').innerHTML = ❌ OOPS the wrong ans. Correct: ${currentQuestionObj.options[currentQuestionObj.correct]} 🔐;
      document.getElementById('quizFeedbackMsg').style.color = '#ffaaaa';
    }
    answered = true;
    // disable all options
    document.querySelectorAll('#quizOptions .quiz-option').forEach(opt => opt.classList.add('disabled'));
    document.getElementById('quizNextBtn').disabled = false;
    document.getElementById('quizNextBtn').innerText = '➡️ Next Question';
  }

  function nextQuestionStep() {
    if (!quizActive) return;
    if (!answered) {
      document.getElementById('quizFeedbackMsg').innerHTML = '⚠️ Please select an answer first!';
      return;
    }
    if (currentQIndex + 1 < levelQuestions.length) {
      currentQIndex++;
      renderCurrentQuestion();
    } else if (currentQIndex + 1 === levelQuestions.length) {
      finishLevelComplete();
    }
  }

  function finishLevelComplete() {
    quizActive = false;
    document.getElementById('quizFeedbackMsg').innerHTML = 🏆 LEVEL 1 COMPLETE! 🏆<br>💰 Total NPR Earned: ${totalEarnedNPR}<br>⭐ Cyber Score: ${totalCyberScore}/100<br>🎉 Great job, hero! Level 1 conquered!;
    document.getElementById('quizNextBtn').disabled = true;
    document.getElementById('quizNextBtn').innerText = '✅ Completed';
    // Unlock next level (level 2) for future but keep only level1 now according to requirement, but for continuity we unlock level2 in level grid but not used yet
    if (!unlockedLevels.includes(2)) {
      unlockedLevels.push(2);
      showInfoModal("🎉 New Level Unlocked!", "Level 2 is now available! (Coming soon in next update)");
    }
    // disable answer clicks
    document.querySelectorAll('#quizOptions .quiz-option').forEach(opt => opt.classList.add('disabled'));
  }

  function quitQuiz() {
    quizActive = false;
    showLevelSelection();
  }

  function showWelcomePage() {
    document.getElementById('welcomePage').classList.remove('hidden');
    document.getElementById('levelSelectPage').classList.add('hidden');
    document.getElementById('quizPage').classList.add('hidden');
    updateWelcomeUI();
  }

  function showLevelSelection() {
    document.getElementById('welcomePage').classList.add('hidden');
    document.getElementById('levelSelectPage').classList.remove('hidden');
    document.getElementById('quizPage').classList.add('hidden');
    buildLevelGrid();
    document.getElementById('levelInfoBox').style.display = 'none';
  }

  function showQuizPage() {
    document.getElementById('welcomePage').classList.add('hidden');
    document.getElementById('levelSelectPage').classList.add('hidden');
    document.getElementById('quizPage').classList.remove('hidden');
  }

  function showInfoModal(title, msg) {
    document.getElementById('infoTitle').innerText = title;
    document.getElementById('infoBody').innerHTML = msg;
    document.getElementById('infoModal').classList.add('active');
  }

  function closeModal(id) {
    document.getElementById(id).classList.remove('active');
  }

  // Event listeners
  document.getElementById('startGameBtn').addEventListener('click', () => {
    if (!selectedHeroChar) {
      showInfoModal("⚠️ Choose Your Hero!", "Please select a hero from above before starting your adventure!");
      return;
    }
    showLevelSelection();
  });
  document.getElementById('backToWelcomeBtn').addEventListener('click', showWelcomePage);
  document.getElementById('quizNextBtn').addEventListener('click', nextQuestionStep);
  document.getElementById('quitQuizBtn').addEventListener('click', quitQuiz);

  window.closeModal = closeModal;
  document.querySelectorAll('.overlay').forEach(o => {
    o.addEventListener('click', e => { if (e.target === o) o.classList.remove('active'); });
  });

  // initial setup
  updateWelcomeUI();
  showWelcomePage();
</script>
</body>
</html>
