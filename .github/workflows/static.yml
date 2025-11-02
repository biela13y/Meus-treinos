<!doctype html>
<html lang="pt-BR">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Meus Treinos — Estável (corrigido)</title>

<!-- Chart.js -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
  :root{
    --bg:#0f1113; --card:#131516; --accent:#00ff99; --muted:#9aa3a1; --text:#e9f0ee;
  }
  body{margin:0;font-family:Inter,system-ui,Arial,sans-serif;background:var(--bg);color:var(--text);padding:18px;transition:background .25s,color .25s}
  body.light{--bg:#f6f7f8;--card:#fff;--accent:#0aa66b;--muted:#556566;--text:#071017}
  .container{max-width:980px;margin:0 auto}
  header{display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:12px;margin-bottom:14px}
  h1{margin:0;font-size:20px;color:var(--accent)}
  .controls{display:flex;gap:8px;align-items:center;flex-wrap:wrap}
  .btn{background:var(--accent);color:#000;border:none;padding:8px 12px;border-radius:8px;cursor:pointer;font-weight:700}
  .ghost{background:transparent;border:1px solid rgba(255,255,255,.06);color:var(--text);padding:8px 10px;border-radius:8px;cursor:pointer}
  .progress-wrap{margin:0 auto 14px}
  .progress-top{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px;font-size:13px;color:var(--muted)}
  .progress-bg{height:14px;background:rgba(255,255,255,.05);border-radius:999px;overflow:hidden}
  .progress-bar{height:100%;background:linear-gradient(90deg,var(--accent),#00cc77);width:0%;transition:width .45s}
  .days{display:flex;gap:6px;justify-content:center;flex-wrap:wrap;margin-bottom:14px}
  .day-btn{background:transparent;border:1px solid rgba(255,255,255,.06);padding:8px 12px;border-radius:10px;color:var(--text);cursor:pointer}
  .day-btn.active{box-shadow:0 6px 24px rgba(0,0,0,.35);outline:3px solid rgba(0,255,153,.12)}
  .cards{display:block}
  .card{display:none;background:linear-gradient(rgba(0,0,0,.25), rgba(0,0,0,.25)), var(--card);border-radius:12px;padding:14px;margin-bottom:16px;box-shadow:0 8px 26px rgba(0,0,0,.45);backdrop-filter: blur(4px)}
  .card.active{display:block}
  .card.done{opacity:.85}
  .card-header{display:flex;justify-content:space-between;align-items:flex-start;gap:12px;flex-wrap:wrap}
  .title{font-size:17px;font-weight:800}
  .meta{font-size:13px;color:var(--muted)}
  .card-body{display:flex;gap:14px;flex-wrap:wrap;margin-top:12px}
  .left{flex:1;min-width:220px}
  .right{width:340px;min-width:220px}
  .exercise-img{width:100%;height:150px;object-fit:cover;border-radius:10px;border:1px solid rgba(0,0,0,.12);background:#222}
  ul.ex-list{padding-left:6px;margin:10px 0;max-height:260px;overflow:auto}
  ul.ex-list li{display:flex;align-items:center;gap:8px;padding:6px;border-radius:8px}
  ul.ex-list li.done{opacity:.6;text-decoration:line-through;color:var(--muted)}
  .ex-checkbox{width:18px;height:18px;cursor:pointer}
  .ex-text{flex:1;outline:none}
  .actions{display:flex;gap:8px;flex-wrap:wrap;margin-top:8px}
  .mark{padding:8px 12px;border-radius:8px;border:none;cursor:pointer;font-weight:800;background:#00ff99;color:#000}
  .mark.done{background:linear-gradient(90deg,#ffcc66,#ff9966);color:#000}
  .save{background:#ffcc00;color:#000;border-radius:8px;padding:8px 12px;border:none;font-weight:700}
  .chart-modal{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,.9);display:none;align-items:center;justify-content:center;flex-direction:column;z-index:999;}
  .chart-modal.active{display:flex;}
  .chart-modal canvas{width:90%;max-width:600px;height:300px !important;}
  .chart-modal .back-btn{margin-top:12px;padding:8px 12px;border-radius:8px;border:none;background:var(--accent);color:#000;font-weight:700;cursor:pointer}
  footer{max-width:980px;margin:0 auto;text-align:center;color:var(--muted);font-size:13px;padding:18px 0}
  @media (max-width:900px){.right{width:100%}}
</style>
</head>
<body>
<header class="container">
  <h1>💪 Meus Treinos — Estável</h1>
  <div class="controls">
    <div id="counter" style="font-weight:700;color:var(--muted)">0 / 0 exercícios</div>
    <button class="btn" id="save-all">💾 Salvar progresso</button>
    <button class="btn" id="show-chart">📊 Ver gráfico</button>
    <button class="ghost" id="export-json">📥 Exportar</button>
    <button class="ghost" id="import-json">📤 Importar</button>
    <button class="ghost" id="reset-week">Reset</button>
    <button class="ghost" id="toggle-mode">🌞/🌙</button>
  </div>
</header>

<main class="container">
  <div class="progress-wrap" style="display:none">
    <div class="progress-top"><div>Progresso geral</div><div id="percent">0%</div></div>
    <div class="progress-bg"><div class="progress-bar" id="progress-bar"></div></div>
  </div>

  <div class="days" id="days">
    <button class="day-btn active" data-day="segunda">Seg</button>
    <button class="day-btn" data-day="terca">Ter</button>
    <button class="day-btn" data-day="quarta">Qua</button>
    <button class="day-btn" data-day="quinta">Qui</button>
    <button class="day-btn" data-day="sexta">Sex</button>
    <button class="day-btn" data-day="sabado">Sáb</button>
    <button class="day-btn" data-day="domingo">Dom</button>
  </div>

  <div id="cards" class="cards"></div>
</main>

<!-- Chart modal -->
<div class="chart-modal" id="chart-modal" aria-hidden="true">
  <canvas id="progress-chart" aria-label="Gráfico de progresso" role="img"></canvas>
  <button class="back-btn" id="close-chart">🔙 Voltar</button>
</div>

<footer>Marque exercícios, salve e acompanhe. Backup via Export/Import.</footer>

<script>
/* ---------- Config e helpers ---------- */
const DAYS = ['segunda','terca','quarta','quinta','sexta','sabado','domingo'];
const LS_PREFIX = 'meustreinos_fixed_';
const THEME_KEY = LS_PREFIX + 'theme';

function $(sel){ return document.querySelector(sel); }
function $all(sel){ return Array.from(document.querySelectorAll(sel)); }

function safeGet(key){
  try{ return JSON.parse(localStorage.getItem(key)); }
  catch(e){ return null; }
}
function safeSet(key,val){
  try{ localStorage.setItem(key, JSON.stringify(val)); }
  catch(e){}
}
function dayKey(d){ return LS_PREFIX + d; }
function capitalize(s){ if(!s) return ''; return s.charAt(0).toUpperCase()+s.slice(1); }
function escapeHtml(s){ if(!s) return ''; return String(s).replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;').replaceAll('"','&quot;'); }

/* ---------- Defaults (seus treinos) ---------- */
const DEFAULTS = {
  segunda:{title:'Segunda — Treino A', time:'15–20 min', img:'https://i.imgur.com/3Xq6L2P.jpg', exercises:[
    {text:'Flexões tradicionais — 4x10', done:false},
    {text:'Flexões inclinadas (pés apoiados em algo) — 3x8', done:false},
    {text:'Flexões diamante (mãos juntas) — 3x10', done:false},
    {text:'Prancha abdominal — 3x30s', done:false},
    {text:'Abdominais curtos — 3x20', done:false}
  ]},
  terca:{title:'Terça — Treino B', time:'15–20 min', img:'https://i.imgur.com/O9x5VmX.jpg', exercises:[
    {text:'Agachamento — 4x15', done:false},
    {text:'Agachamento com salto — 3x10', done:false},
    {text:'Afundo (um pé à frente, outro atrás) — 3x12 cada perna', done:false},
    {text:'Ponte de glúteos — 3x15', done:false},
    {text:'Prancha com joelhos alternando no peito — 3x20', done:false}
  ]},
  quarta:{title:'Quarta — Descanso leve', time:'10-15 min', img:'https://i.imgur.com/nQr0x9P.jpg', exercises:[
    {text:'Caminhada leve / alongamento', done:false}
  ]},
  quinta:{title:'Quinta — Treino C', time:'15 min', img:'https://i.imgur.com/Jc9v1I2.jpg', exercises:[
    {text:'Burpees — 3x10', done:false},
    {text:'Polichinelos — 3x30s', done:false},
    {text:'Circuito: Flexões + Abdominais + Agachamentos — 3 voltas sem pausa', done:false},
    {text:'Prancha — 3x30s', done:false}
  ]},
  sexta:{title:'Sexta — Treino A', time:'15–20 min', img:'https://i.imgur.com/3Xq6L2P.jpg', exercises:[
    {text:'Flexões tradicionais — 4x10', done:false},
    {text:'Flexões inclinadas (pés apoiados em algo) — 3x8', done:false},
    {text:'Flexões diamante (mãos juntas) — 3x10', done:false},
    {text:'Prancha abdominal — 3x30s', done:false},
    {text:'Abdominais curtos — 3x20', done:false}
  ]},
  sabado:{title:'Sábado — Descanso / Caminhada', time:'20-30 min', img:'https://i.imgur.com/3LxI9w5.jpg', exercises:[
    {text:'Caminhada leve', done:false},
    {text:'Alongamento', done:false}
  ]},
  domingo:{title:'Domingo — Treino B', time:'15–20 min', img:'https://i.imgur.com/O9x5VmX.jpg', exercises:[
    {text:'Agachamento — 4x15', done:false},
    {text:'Agachamento com salto — 3x10', done:false},
    {text:'Afundo (um pé à frente, outro atrás) — 3x12 cada perna', done:false},
    {text:'Ponte de glúteos — 3x15', done:false},
    {text:'Prancha com joelhos alternando no peito — 3x20', done:false}
  ]}
};

/* ---------- Manipulação de dados por dia ---------- */
function loadDayData(day){
  const raw = safeGet(dayKey(day));
  if(raw && raw.exercises && Array.isArray(raw.exercises)) return raw;
  // devolve cópia profunda do default
  return JSON.parse(JSON.stringify(DEFAULTS[day]));
}

function saveDayData(day, payload){
  safeSet(dayKey(day), payload);
}

/* ---------- Render dos cards ---------- */
const cardsContainer = $('#cards');

function createCardElement(day){
  const data = loadDayData(day);
  const doneCount = data.exercises.filter(e=>e.done).length;
  const total = data.exercises.length;

  const article = document.createElement('article');
  article.className = 'card';
  article.id = 'card-' + day;
  article.dataset.day = day;

  // header
  const header = document.createElement('div');
  header.className = 'card-header';
  const leftH = document.createElement('div');
  const title = document.createElement('div'); title.className='title'; title.contentEditable = 'true'; title.innerText = data.title;
  const meta = document.createElement('div'); meta.className='meta'; meta.contentEditable = 'true'; meta.innerText = data.time;
  leftH.appendChild(title); leftH.appendChild(meta);
  const rightH = document.createElement('div'); rightH.className='meta'; rightH.style.textAlign='right'; rightH.innerText = capitalize(day);
  header.appendChild(leftH); header.appendChild(rightH);

  // body
  const body = document.createElement('div'); body.className='card-body';
  const left = document.createElement('div'); left.className='left';
  const img = document.createElement('img'); img.className='exercise-img'; img.src = data.img || '';
  left.appendChild(img);
  const imgBtns = document.createElement('div'); imgBtns.style.display='flex'; imgBtns.style.gap='8px'; imgBtns.style.marginTop='8px';
  const btnImg = document.createElement('button'); btnImg.className='img-edit ghost'; btnImg.innerText = 'Trocar imagem';
  const btnImgReset = document.createElement('button'); btnImgReset.className='img-reset ghost'; btnImgReset.innerText = 'Restaurar';
  imgBtns.appendChild(btnImg); imgBtns.appendChild(btnImgReset);
  left.appendChild(imgBtns);
  const note = document.createElement('div'); note.className='note'; note.style.marginTop='10px'; note.innerText = 'Clique no texto do exercício para editar. Marque com o checkbox.';
  left.appendChild(note);

  const right = document.createElement('div'); right.className='right';
  const ul = document.createElement('ul'); ul.className='ex-list'; ul.setAttribute('aria-label','lista de exercícios');
  data.exercises.forEach((ex, idx) => {
    const li = document.createElement('li');
    li.dataset.idx = idx;
    if(ex.done) li.classList.add('done');

    const cb = document.createElement('input'); cb.type='checkbox'; cb.className='ex-checkbox'; cb.checked = !!ex.done;
    const text = document.createElement('div'); text.className='ex-text'; text.contentEditable='true'; text.innerText = ex.text;
    const editBtn = document.createElement('button'); editBtn.className='ex-edit-btn ghost'; editBtn.title='Editar texto'; editBtn.innerText = '✏️';
    li.appendChild(cb); li.appendChild(text); li.appendChild(editBtn);
    ul.appendChild(li);
  });

  right.appendChild(ul);

  const actions = document.createElement('div'); actions.className='actions';
  const markBtn = document.createElement('button'); markBtn.className='mark btn'; markBtn.innerText = doneCount === total ? '🔁 Desmarcar' : '✅ Marcar como feito';
  const saveBtn = document.createElement('button'); saveBtn.className='save'; saveBtn.innerText = '💾 Salvar dia';
  const counts = document.createElement('div'); counts.style.marginTop='6px'; counts.style.color='var(--muted)'; counts.innerHTML = `Exercícios: <span class="day-done-count">${doneCount}</span>/<span class="day-total-count">${total}</span>`;
  actions.appendChild(markBtn); actions.appendChild(saveBtn); right.appendChild(actions); right.appendChild(counts);

  body.appendChild(left); body.appendChild(right);

  article.appendChild(header); article.appendChild(body);

  if(doneCount === total) article.classList.add('done');

  /* ---------- eventos do card ---------- */
  // trocar imagem
  btnImg.onclick = () => {
    const url = prompt('Cole a URL da imagem (https://...)', img.src);
    if(url === null) return;
    if(url.trim() && (url.startsWith('http://') || url.startsWith('https://'))){
      img.src = url.trim();
      persistCard(article);
      updateAll();
    } else alert('URL inválida.');
  };
  btnImgReset.onclick = () => {
    img.src = DEFAULTS[day].img || '';
    persistCard(article);
    updateAll();
  };

  // checkbox e edição de texto
  ul.querySelectorAll('li').forEach(li => {
    const cb = li.querySelector('.ex-checkbox');
    const txt = li.querySelector('.ex-text');
    cb.onchange = () => {
      if(cb.checked) li.classList.add('done'); else li.classList.remove('done');
      persistCard(article);
      updateAll();
    };
    // salvar texto ao tirar foco
    txt.addEventListener('blur', () => { persistCard(article); updateAll(); });
  });

  // marcar/desmarcar tudo
  markBtn.onclick = () => {
    const lis = ul.querySelectorAll('li');
    const allDone = Array.from(lis).every(l => l.classList.contains('done'));
    lis.forEach(l => {
      const cb = l.querySelector('.ex-checkbox');
      if(allDone){ l.classList.remove('done'); cb.checked = false; } else { l.classList.add('done'); cb.checked = true; }
    });
    persistCard(article);
    updateAll();
    markBtn.innerText = allDone ? '✅ Marcar como feito' : '🔁 Desmarcar';
  };

  // salvar dia
  saveBtn.onclick = () => {
    persistCard(article);
    saveBtn.innerText = '💾 Salvo';
    setTimeout(()=> saveBtn.innerText = '💾 Salvar dia', 900);
    updateAll();
  };

  return article;
}

/* salva o estado do card no localStorage */
function persistCard(card){
  const day = card.dataset.day;
  const title = card.querySelector('.title').innerText.trim();
  const time = card.querySelector('.meta').innerText.trim();
  const img = card.querySelector('.exercise-img').src;
  const exercises = Array.from(card.querySelectorAll('ul.ex-list li')).map(li => ({
    text: li.querySelector('.ex-text').innerText.trim(),
    done: li.classList.contains('done')
  }));
  const payload = { title, time, img, exercises, savedAt: new Date().toISOString() };
  saveDayData(day, payload);
}

/* renderiza todos os cards */
function renderAllCards(){
  cardsContainer.innerHTML = '';
  DAYS.forEach(day => {
    const c = createCardElement(day);
    cardsContainer.appendChild(c);
  });
  // ativa o primeiro card por padrão
  const first = cardsContainer.querySelector('.card');
  if(first) first.classList.add('active');
  attachGlobalEvents();
  updateAll();
}

/* ---------- eventos globais ---------- */
function attachGlobalEvents(){
  // dia nav
  $all('#days .day-btn').forEach(btn => {
    btn.onclick = () => {
      $all('#days .day-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      const day = btn.dataset.day;
      $all('.card').forEach(c => c.classList.remove('active'));
      const target = document.getElementById('card-' + day);
      if(target) target.classList.add('active');
    };
  });

  // save all
  $('#save-all').onclick = () => {
    $all('.card').forEach(c => persistCard(c));
    $('#save-all').innerText = '✅ Salvo';
    setTimeout(()=> $('#save-all').innerText = '💾 Salvar progresso', 1200);
    updateAll();
  };

  // export
  $('#export-json').onclick = () => {
    const out = {};
    DAYS.forEach(d => {
      const v = safeGet(dayKey(d));
      out[d] = v ? v : DEFAULTS[d];
    });
    const blob = new Blob([JSON.stringify(out, null, 2)], {type:'application/json'});
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a'); a.href = url; a.download = 'meus_treinos_backup.json'; a.click(); URL.revokeObjectURL(url);
  };

  // import
  $('#import-json').onclick = () => {
    const input = document.createElement('input'); input.type='file'; input.accept='.json,application/json';
    input.onchange = (e) => {
      const file = e.target.files && e.target.files[0]; if(!file) return;
      const reader = new FileReader();
      reader.onload = () => {
        try{
          const parsed = JSON.parse(reader.result);
          if(typeof parsed !== 'object') throw new Error('JSON inválido');
          DAYS.forEach(d => { if(parsed[d]) safeSet(dayKey(d), parsed[d]); });
          alert('Importação concluída — recarregando a página.');
          location.reload();
        }catch(err){ alert('Erro ao importar: ' + (err.message || err)); }
      };
      reader.readAsText(file);
    };
    input.click();
  };

  // reset
  $('#reset-week').onclick = () => {
    if(!confirm('Remover todos os dados salvos localmente e restaurar padrões?')) return;
    DAYS.forEach(d => localStorage.removeItem(dayKey(d)));
    localStorage.removeItem(THEME_KEY);
    location.reload();
  };

  // theme
  $('#toggle-mode').onclick = () => {
    const isLight = document.body.classList.toggle('light');
    safeSet(THEME_KEY, isLight ? '1' : '0');
  };
  if(safeGet(THEME_KEY) === '1') document.body.classList.add('light');

  // chart modal open/close
  $('#show-chart').onclick = () => { $('#chart-modal').classList.add('active'); initChart(); updateChart(); $('#chart-modal').setAttribute('aria-hidden','false'); };
  $('#close-chart').onclick = () => { $('#chart-modal').classList.remove('active'); $('#chart-modal').setAttribute('aria-hidden','true'); };

  // delegate checkbox quick update (keeps listeners simple)
  document.addEventListener('change', (e) => {
    if(e.target.matches('.ex-checkbox')){
      const li = e.target.closest('li');
      if(!li) return;
      if(e.target.checked) li.classList.add('done'); else li.classList.remove('done');
      const card = e.target.closest('.card');
      if(card) persistCard(card);
      updateAll();
    }
  });
}

/* ---------- contadores e gráfico ---------- */
function computeTotals(){
  const perDay = DAYS.map(d => {
    const raw = safeGet(dayKey(d));
    const arr = raw && raw.exercises ? raw.exercises : DEFAULTS[d].exercises;
    const done = arr.filter(x => x.done).length;
    const total = arr.length;
    return { day:d, done, total };
  });
  const totalDone = perDay.reduce((s,p)=>s+p.done,0);
  const totalExercises = perDay.reduce((s,p)=>s+p.total,0);
  return { perDay, totalDone, totalExercises };
}

function updateAll(){
  const totals = computeTotals();
  $('#counter').innerText = `${totals.totalDone} / ${totals.totalExercises} exercícios`;
  // progress bar (hidden by default in this layout)
  const pct = totals.totalExercises ? Math.round((totals.totalDone / totals.totalExercises) * 100) : 0;
  $('#percent') && ($('#percent').innerText = pct + '%');
  $('#progress-bar') && ($('#progress-bar').style.width = pct + '%');
  // update per-card counters
  $all('.card').forEach(card => {
    const lis = Array.from(card.querySelectorAll('ul.ex-list li'));
    const done = lis.filter(l=>l.classList.contains('done')).length;
    const total = lis.length;
    const doneSpan = card.querySelector('.day-done-count');
    const totalSpan = card.querySelector('.day-total-count');
    if(doneSpan) doneSpan.innerText = done;
    if(totalSpan) totalSpan.innerText = total;
    const markBtn = card.querySelector('.mark');
    if(markBtn){
      if(done === total){ card.classList.add('done'); markBtn.classList.add('done'); markBtn.innerText = '🔁 Desmarcar'; } 
      else { card.classList.remove('done'); markBtn.classList.remove('done'); markBtn.innerText = '✅ Marcar como feito'; }
    }
  });
}

/* Chart.js */
let chartInstance = null;
function initChart(){
  if(chartInstance) return;
  const ctx = document.getElementById('progress-chart').getContext('2d');
  chartInstance = new Chart(ctx, {
    type: 'bar',
    data: { labels: DAYS.map(d => d.substr(0,3)), datasets: [{ label: 'Exercícios concluídos', data: [], backgroundColor: 'rgba(0,255,153,0.75)' }] },
    options: { responsive: true, plugins:{ legend:{ display:false } }, scales: { y: { beginAtZero: true, precision:0 } } }
  });
}

function updateChart(){
  if(!chartInstance) return;
  const data = DAYS.map(d => {
    const raw = safeGet(dayKey(d));
    const arr = raw && raw.exercises ? raw.exercises : DEFAULTS[d].exercises;
    return arr.filter(x => x.done).length;
  });
  chartInstance.data.datasets[0].data = data;
  chartInstance.update();
}

/* ---------- inicialização ---------- */
document.addEventListener('DOMContentLoaded', () => {
  renderAllCards();
  // garante que tooltip/labels corretos
  initChart(); // inicializa mas modal só mostra quando abrir
});
</script>
</body>
</html>
