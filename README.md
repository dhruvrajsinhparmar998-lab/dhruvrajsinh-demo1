# dhruvrajsinh-demo1
THIS IS MY FIRST GIT REPOSITORY.
<br>
Author-dhruvrajsinh parmar
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Cyber Security Quiz — Attack Simulation (Safe Demo)</title>
  <style>
    :root{--bg:#0f1724;--card:#0b1220;--accent:#06b6d4;--muted:#94a3b8;--glass: rgba(255,255,255,0.03)}
    html,body{height:100%;margin:0;font-family:Inter,Segoe UI,Roboto,Arial;background:linear-gradient(180deg,#071026 0%, #031426 100%);color:#e6eef6}
    .wrap{max-width:920px;margin:28px auto;padding:20px}
    header{display:flex;align-items:center;gap:16px}
    h1{font-size:20px;margin:0}
    p.lead{margin:6px 0 18px;color:var(--muted)}
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border-radius:12px;padding:18px;box-shadow:0 6px 24px rgba(2,6,23,0.6);}
    .question{margin:14px 0;padding:14px;border-radius:10px;background:var(--glass);}
    .email{font-family:monospace;background:#021022;padding:10px;border-radius:8px;margin-bottom:10px}
    .choices{display:flex;flex-direction:column;gap:8px}
    button.choice{padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:inherit;cursor:pointer;text-align:left}
    button.choice:hover{transform:translateY(-2px)}
    .controls{display:flex;gap:10px;align-items:center;margin-top:12px}
    .score{margin-left:auto;font-weight:600}
    .explain{margin-top:10px;padding:12px;border-radius:8px;background:rgba(0,0,0,0.25)}
    .correct{outline:3px solid rgba(6,182,212,0.14)}
    .wrong{outline:3px solid rgba(255,95,95,0.12)}
    footer{margin-top:18px;color:var(--muted);font-size:13px}
    .small{font-size:13px;color:var(--muted)}
    .badge{background:rgba(255,255,255,0.03);padding:6px 10px;border-radius:999px;border:1px solid rgba(255,255,255,0.03)}
    .controls button, .controls .badge{cursor:pointer}
    .question-count{font-weight:600}
    .explain .title{font-weight:700;margin-bottom:6px}
    .result{font-size:16px;font-weight:700;margin-top:12px}
    .try-again{margin-top:12px}
    .meta{display:flex;gap:8px;flex-wrap:wrap}
    @media(max-width:640px){.wrap{padding:12px}}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div style="width:56px;height:56px;border-radius:12px;background:linear-gradient(135deg,var(--accent),#3b82f6);display:flex;align-items:center;justify-content:center;font-weight:700">CS</div>
      <div>
        <h1>Cyber Security Quiz — Attack Simulation (Safe Demo)</h1>
        <p class="lead">Train to spot phishing and other social-engineering attacks using safe simulated examples. Choose <strong>Safe</strong> or <strong>Unsafe</strong>, get instant feedback and explanations.</p>
      </div>
    </header>

    <div class="card" id="app">
      <div style="display:flex;align-items:center;gap:12px">
        <div class="question-count">Question <span id="qIndex">1</span>/<span id="qTotal">1</span></div>
        <div class="meta">
          <div class="badge" id="difficulty">Beginner</div>
          <div class="small">Time per question: <span id="timer">—</span></div>
        </div>
        <div class="score" id="score">Score: 0</div>
      </div>

      <div id="questionArea">
        <!-- question goes here -->
      </div>

      <div class="controls">
        <button id="nextBtn" class="badge">Next ➜</button>
        <button id="revealBtn" class="badge">Reveal Explanation</button>
        <div style="flex:1"></div>
        <button id="restartBtn" class="badge">Restart Quiz</button>
      </div>

      <div id="summary" style="display:none"></div>
    </div>

    <footer>
      <div class="small">How to use: Open this file in a browser. The quiz uses safe simulated messages — no real malicious payloads. Perfect for classroom demos and viva practice.</div>
    </footer>
  </div>

<script>
// ===== SAMPLE QUESTIONS =====
const QUESTIONS = [
  {
    id: 1,
    difficulty: 'Beginner',
    subject: 'Action Required: Verify your bank account',
    from: 'service@secure-bank-alerts.com',
    body: `Dear user,\n\nWe noticed suspicious activity on your account. Click the link below to verify your details immediately or your account will be frozen.\n\nhttps://secure-bank-verify.example/login\n\nThank you,\nSecure Bank Team`,
    correctAnswer: 'Unsafe',
    explanation: 'This is a classic phishing email: urgent language, a link to a suspicious domain (note the domain is not the official bank domain). Legitimate banks rarely ask to verify by clicking links in emails. Always open bank website manually.'
  },
  {
    id: 2,
    difficulty: 'Beginner',
    subject: 'Invoice attached for your order #4521',
    from: 'orders@onlineshop.example',
    body: `Hi,\n\nThanks for your purchase. The invoice is attached. If you did not place this order, reply to this email immediately.\n\nRegards,\nOnlineShop Team`,
    correctAnswer: 'Unsafe',
    explanation: 'Unexpected attachments can contain malware. Also the sender domain looks generic. Safe practice: do not open attachments from unknown/unexpected sources and validate the sender.'
  },
  {
    id: 3,
    difficulty: 'Beginner',
    subject: 'Your account statement is ready',
    from: 'noreply@yourbank.example',
    body: `Dear Customer,\n\nYour monthly statement is available. Log in to your official account to view it.\n\nNo action required if you did not request anything.\n\nRegards, Bank`,
    correctAnswer: 'Safe',
    explanation: 'This message is generic and does not request urgent action or links. If it came from your real bank domain and you are expecting statements, it is likely safe. Still verify sender domain and avoid clicking unexpected links.'
  },
  {
    id: 4,
    difficulty: 'Intermediate',
    subject: 'Security Update — Install this patch',
    from: 'it-support@company-it.example',
    body: `Hello,\n\nWe require you to install the attached "patch.exe" to protect your workstation. Run it immediately.\n\nIT Support`,
    correctAnswer: 'Unsafe',
    explanation: 'Legitimate IT teams never send executable attachments to users to run directly. This is a malware delivery technique. Always confirm with IT through a known channel.'
  },
  {
    id: 5,
    difficulty: 'Intermediate',
    subject: 'Can you review this doc?',
    from: 'colleague@gmail.com',
    body: `Hey — I shared a Google Doc with you: https://docs.google.com/document/d/FAKE_ID\n\nPlease review the doc before EOD.`,
    correctAnswer: 'Unsafe',
    explanation: 'The link points to a docs.google.com URL but that alone is not enough. Phishers use genuine hosting (Google, Dropbox) to host malicious pages. Verify the sender identity and the exact link. When in doubt, open Drive/docs via your own Google account not via email link.'
  }
];

// ===== APP STATE =====
let state = { index: 0, score: 0, answers: [] };

// ===== DOM =====
const qIndex = document.getElementById('qIndex');
const qTotal = document.getElementById('qTotal');
const questionArea = document.getElementById('questionArea');
const scoreEl = document.getElementById('score');
const difficultyEl = document.getElementById('difficulty');
const timerEl = document.getElementById('timer');
const nextBtn = document.getElementById('nextBtn');
const restartBtn = document.getElementById('restartBtn');
const revealBtn = document.getElementById('revealBtn');
const summary = document.getElementById('summary');

qTotal.textContent = QUESTIONS.length;

function renderQuestion(){
  const q = QUESTIONS[state.index];
  qIndex.textContent = state.index + 1;
  difficultyEl.textContent = q.difficulty;
  scoreEl.textContent = `Score: ${state.score}`;
  questionArea.innerHTML = `
    <div class='question'>
      <div style='display:flex;justify-content:space-between;align-items:center'>
        <div><strong>Subject:</strong> ${escapeHtml(q.subject)}</div>
        <div class='small'>From: <em>${escapeHtml(q.from)}</em></div>
      </div>
      <div class='email'>${escapeHtml(q.body).replace(/\n/g,'<br>')}</div>

      <div class='choices'>
        <button class='choice' data-choice='Safe'>🔒 Mark as SAFE (No suspicious action)</button>
        <button class='choice' data-choice='Unsafe'>⚠️ Mark as UNSAFE (Phishing/malicious)</button>
      </div>

      <div id='explain-${q.id}' class='explain' style='display:none'></div>
    </div>
  `;

  // attach listeners
  document.querySelectorAll('.choice').forEach(btn => {
    btn.addEventListener('click', () => handleChoice(btn.dataset.choice));
  });

  // reset timer
  startTimer(120); // 2 minutes per question (editable)
}

function handleChoice(choice){
  const q = QUESTIONS[state.index];
  const correct = q.correctAnswer;
  const explainEl = document.getElementById(`explain-${q.id}`);
  const isCorrect = choice === correct;
  state.answers.push({ qid: q.id, choice, correct: isCorrect });
  if(isCorrect) state.score += 10;
  // mark
  document.querySelectorAll('.choice').forEach(b => b.disabled = true);
  // visual
  Array.from(document.querySelectorAll('.choice')).forEach(b => {
    if(b.dataset.choice === correct) b.classList.add('correct');
    if(b.dataset.choice !== correct && b.dataset.choice === choice) b.classList.add('wrong');
  });
  explainEl.style.display = 'block';
  explainEl.innerHTML = `<div class='title'>Explanation:</div><div>${escapeHtml(q.explanation)}</div>`;
  scoreEl.textContent = `Score: ${state.score}`;
}

nextBtn.addEventListener('click', ()=>{
  // if last question, show summary
  if(state.index < QUESTIONS.length -1){
    state.index +=1; renderQuestion();
  } else {
    showSummary();
  }
});

revealBtn.addEventListener('click', ()=>{
  // reveal explanation without answering
  const q = QUESTIONS[state.index];
  const explainEl = document.getElementById(`explain-${q.id}`);
  explainEl.style.display = 'block';
  explainEl.innerHTML = `<div class='title'>Explanation (Revealed):</div><div>${escapeHtml(q.explanation)}</div>`;
  document.querySelectorAll('.choice').forEach(b => b.disabled = true);
  // indicate correct
  Array.from(document.querySelectorAll('.choice')).forEach(b => {
    if(b.dataset.choice === q.correctAnswer) b.classList.add('correct');
  });
});

restartBtn.addEventListener('click', ()=>{
  state = { index:0, score:0, answers:[] };
  summary.style.display = 'none';
  renderQuestion();
});

function showSummary(){
  questionArea.innerHTML = '';
  summary.style.display = 'block';
  const total = QUESTIONS.length;
  const earned = state.score;
  const max = total * 10;
  const percent = Math.round((earned/max)*100);
  summary.innerHTML = `
    <div class='card'>
      <div class='result'>Your score: ${earned} / ${max} (${percent}%)</div>
      <div style='margin-top:12px'>
        <strong>Review:</strong>
        <ol>
          ${QUESTIONS.map(q => {
            const ans = state.answers.find(a=>a.qid===q.id);
            const chosen = ans ? ans.choice : 'No answer';
            const ok = ans && ans.correct ? '✅' : '❌';
            return `<li><strong>${escapeHtml(q.subject)}</strong> — You: ${chosen} ${ok}<div class='small'>${escapeHtml(q.explanation)}</div></li>`;
          }).join('')}
        </ol>
      </div>
      <button class='try-again badge' onclick='window.location.reload()'>Try Again</button>
    </div>
  `;
}

// ===== TIMER (simple) =====
let timerInterval = null;
function startTimer(seconds){
  clearInterval(timerInterval);
  let t = seconds;
  timerEl.textContent = formatTime(t);
  timerInterval = setInterval(()=>{
    t -=1;
    timerEl.textContent = formatTime(t);
    if(t<=0){
      clearInterval(timerInterval);
      timerEl.textContent = 'Time up';
      // auto reveal
      revealBtn.click();
    }
  },1000);
}
function formatTime(s){
  const mm = String(Math.floor(s/60)).padStart(2,'0');
  const ss = String(s%60).padStart(2,'0');
  return `${mm}:${ss}`;
}

function escapeHtml(unsafe) {
  return unsafe
       .replace(/&/g, "&amp;")
       .replace(/</g, "&lt;")
       .replace(/>/g, "&gt;")
       .replace(/\"/g, "&quot;")
       .replace(/'/g, "&#039;");
}

// Initial render
renderQuestion();
</script>
</body>
</html>
