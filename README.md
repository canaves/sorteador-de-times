<html lang="pt-br">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Sorteador de Times (Vôlei)</title>
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  
  <!-- 🏐 FAVICON BOLA DE VÔLEI -->
  <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><circle fill='%23f4a226' cx='50' cy='50' r='45' stroke='%23262626' stroke-width='3' stroke-linecap='round' stroke-linejoin='round'/><circle fill='%23ffffff' cx='50' cy='50' r='28' opacity='0.4'/><circle fill='%23262626' cx='28' cy='28' r='8'/><circle fill='%23262626' cx='72' cy='28' r='8'/><circle fill='%23262626' cx='28' cy='72' r='8'/><circle fill='%23262626' cx='72' cy='72' r='8'/><path fill='none' stroke='%23262626' stroke-width='3' stroke-linecap='round' d='M 20 50 Q 50 30 80 50 Q 50 70 20 50'/></svg>">
  <link rel="apple-touch-icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><circle fill='%23f4a226' cx='50' cy='50' r='45' stroke='%23262626' stroke-width='3' stroke-linecap='round' stroke-linejoin='round'/><circle fill='%23ffffff' cx='50' cy='50' r='28' opacity='0.4'/><circle fill='%23262626' cx='28' cy='28' r='8'/><circle fill='%23262626' cx='72' cy='28' r='8'/><circle fill='%23262626' cx='28' cy='72' r='8'/><circle fill='%23262626' cx='72' cy='72' r='8'/><path fill='none' stroke='%23262626' stroke-width='3' stroke-linecap='round' d='M 20 50 Q 50 30 80 50 Q 50 70 20 50'/></svg>">

  <style>
    :root{
      --bg:#0b1020; --card:#121a33; --card2:#0f1730; --text:#e8ecff; --muted:#aab3df;
      --accent:#7c5cff; --good:#29d39a; --warn:#ffcc66; --bad:#ff5c7a; --border:#22305e;
    }
    
    [data-theme="light"]{
      --bg:#f5f7fa; --card:#ffffff; --card2:#f8f9fb; --text:#1a1f36; --muted:#6b7385;
      --accent:#6344d6; --good:#1eb386; --warn:#e6a700; --bad:#e63757; --border:#dfe3eb;
    }
    
    *{box-sizing:border-box}
    body{
      margin:0; font-family:system-ui,-apple-system,Segoe UI,Roboto,Ubuntu,Arial;
      background: var(--bg);
      color:var(--text);
      transition: background 0.3s, color 0.3s;
    }
    
    body[data-theme="dark"]{
      background: radial-gradient(1200px 700px at 10% 10%, #16204a 0%, var(--bg) 55%);
    }
    
    body[data-theme="light"]{
      background: linear-gradient(135deg, #e8f0fe 0%, #f5f7fa 100%);
    }
    
    header{
      padding:24px 16px; max-width:1200px; margin:0 auto;
      display:flex; gap:12px; align-items:flex-start; justify-content:space-between; flex-wrap:wrap;
    }
    h1{margin:0; font-size:20px; display:flex; align-items:center; gap:8px;}
    .sub{color:var(--muted); font-size:13px; margin-top:6px}
    
    .theme-toggle{
      background: var(--card2); border:1px solid var(--border);
      padding:8px 12px; border-radius:10px; cursor:pointer;
      display:flex; align-items:center; gap:6px;
      font-size:20px; transition: all 0.3s;
    }
    .theme-toggle:hover{transform: scale(1.05)}
    
    main{max-width:1200px; margin:0 auto; padding:0 16px 32px}
    .grid{
      display:grid; grid-template-columns: 1.2fr 1fr; gap:16px;
    }
    @media (max-width: 980px){ .grid{grid-template-columns:1fr} }
    .card{
      background:var(--card);
      border:1px solid var(--border); border-radius:14px; padding:16px;
      box-shadow: 0 10px 30px rgba(0,0,0,.15);
      transition: all 0.3s;
    }
    [data-theme="light"] .card{
      box-shadow: 0 4px 20px rgba(0,0,0,.08);
    }
    
    .card h2{margin:0 0 10px 0; font-size:16px}
    .row{display:flex; gap:10px; flex-wrap:wrap; align-items:center}
    .row-config{display:flex; gap:12px; flex-wrap:wrap; align-items:center; margin-bottom:12px;}
    select{
      padding:10px 12px; border-radius:10px; border:1px solid var(--border); 
      background: var(--card2); color:var(--text);
      transition: all 0.3s;
    }
    button{
      border:1px solid var(--border);
      background: rgba(124,92,255,.18);
      color:var(--text);
      padding:10px 12px; border-radius:10px;
      cursor:pointer; font-weight:600;
      transition: all 0.3s;
    }
    button:hover{border-color: rgba(124,92,255,.8); transform: translateY(-1px)}
    button.primary{
      background: linear-gradient(135deg, rgba(124,92,255,.9), rgba(41,211,154,.75));
      border-color: transparent;
    }
    button.ghost{background:transparent}
    button.danger{background: rgba(255,92,122,.16)}
    button.success{
      background: linear-gradient(135deg, rgba(41,211,154,.9), rgba(41,211,154,.6));
      border-color: transparent;
    }
    button:disabled{opacity:0.5; cursor:not-allowed; transform:none !important}
    
    input[type="text"]{
      flex:1; padding:10px 12px; border-radius:10px;
      border:1px solid var(--border); background: var(--card2); color:var(--text);
      outline:none; transition: all 0.3s;
    }
    input[type="text"]:focus{border-color: var(--accent)}
    
    .hint{font-size:12px; color:var(--muted)}
    .pill{
      display:inline-flex; align-items:center; gap:8px;
      border:1px solid var(--border); background: var(--card2);
      padding:6px 10px; border-radius:999px; font-size:12px; color:var(--muted);
    }
    .count.ok{color:var(--good)}
    .count.warn{color:var(--warn)}
    .count.bad{color:var(--bad)}
    .list{
      display:grid; grid-template-columns:repeat(2, minmax(0, 1fr)); gap:8px;
      margin-top:12px;
    }
    @media (max-width: 520px){ .list{grid-template-columns:1fr} }
    .player{
      display:flex; align-items:center; justify-content:space-between;
      gap:10px; padding:10px; border-radius:12px;
      border:1px solid var(--border);
      background: var(--card2);
      transition: all 0.3s;
    }
    .player:hover{transform: translateX(3px)}
    
    .left{display:flex; gap:10px; align-items:center}
    .badge{
      width:28px; height:28px; border-radius:9px;
      display:grid; place-items:center;
      background: rgba(124,92,255,.20); border:1px solid rgba(124,92,255,.35);
      font-weight:800; font-size:12px;
    }
    .name{font-weight:650}
    .small{font-size:12px; color:var(--muted)}
    
    /* ✨ ANIMAÇÃO DE SORTEIO */
    .loading-overlay{
      position:fixed; top:0; left:0; right:0; bottom:0;
      background: rgba(0,0,0,.85); backdrop-filter: blur(8px);
      display:none; place-items:center; z-index:9999;
      animation: fadeIn 0.3s;
    }
    .loading-overlay.active{display:grid}
    
    @keyframes fadeIn{
      from{opacity:0}
      to{opacity:1}
    }
    
    .loading-content{
      text-align:center; padding:40px;
      background: var(--card); border-radius:20px;
      border:1px solid var(--border);
      box-shadow: 0 20px 60px rgba(0,0,0,.5);
      animation: scaleIn 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
    }
    
    @keyframes scaleIn{
      from{transform:scale(0.8); opacity:0}
      to{transform:scale(1); opacity:1}
    }
    
    .volleyball-spinner{
      width:80px; height:80px; margin:0 auto 20px;
      animation: spin 1.5s linear infinite;
    }
    
    @keyframes spin{
      from{transform:rotate(0deg)}
      to{transform:rotate(360deg)}
    }
    
    .loading-text{
      font-size:20px; font-weight:700; color:var(--text);
      margin-bottom:8px;
    }
    .loading-sub{
      font-size:14px; color:var(--muted);
    }
    
    .teams{
      display:grid; grid-template-columns:repeat(auto-fit, minmax(280px, 1fr)); gap:12px;
    }
    .team{
      border:1px solid var(--border);
      border-radius:14px; padding:14px;
      background: var(--card2);
      min-height:180px;
      animation: slideUp 0.5s ease-out backwards;
    }
    
    @keyframes slideUp{
      from{opacity:0; transform:translateY(20px)}
      to{opacity:1; transform:translateY(0)}
    }
    
    .team:nth-child(1){animation-delay:0.1s}
    .team:nth-child(2){animation-delay:0.2s}
    .team:nth-child(3){animation-delay:0.3s}
    
    .team h3{margin:0 0 12px 0; font-size:16px}
    .ul{margin:0; padding-left:16px}
    .ul li{margin:6px 0; padding:4px 0;}
    
    /* 📊 INDICADOR DE EQUILÍBRIO */
    .balance-indicator{
      margin-top:16px; padding:16px; border-radius:12px;
      border:1px solid var(--border); background: var(--card2);
    }
    .balance-title{
      font-size:14px; font-weight:600; margin-bottom:12px;
      display:flex; align-items:center; gap:8px;
    }
    .balance-bars{
      display:flex; flex-direction:column; gap:10px;
    }
    .balance-bar-item{
      display:flex; align-items:center; gap:10px;
    }
    .bar-label{
      font-size:13px; font-weight:600; min-width:140px;
    }
    .bar-wrapper{
      flex:1; height:24px; background: rgba(0,0,0,.1);
      border-radius:6px; overflow:hidden; position:relative;
    }
    .bar-fill{
      height:100%; border-radius:6px;
      transition: width 0.8s cubic-bezier(0.34, 1.56, 0.64, 1);
      display:flex; align-items:center; justify-content:flex-end;
      padding-right:8px; font-size:11px; font-weight:700;
      color:white;
    }
    .bar-value{
      font-size:13px; font-weight:700; min-width:40px; text-align:right;
    }
    
    .balance-score{
      margin-top:12px; padding:10px; border-radius:8px;
      text-align:center; font-size:13px;
      border:1px dashed var(--border);
    }
    .balance-score.excellent{background: rgba(41,211,154,.1); color:var(--good)}
    .balance-score.good{background: rgba(255,204,102,.1); color:var(--warn)}
    .balance-score.fair{background: rgba(255,92,122,.1); color:var(--bad)}
    
    .status{
      margin-top:12px; padding:10px 12px; border-radius:12px;
      border:1px dashed var(--border);
      color:var(--muted); font-size:13px;
    }
    .status strong{color:var(--text)}
    .reserves{margin-top:10px}
    .reserves span{
      display:inline-block; margin:4px 6px 0 0;
      padding:6px 10px; border-radius:999px;
      border:1px solid var(--border);
      background: var(--card2);
      font-size:12px; color:var(--muted);
    }
    .copy-btn-container{
      margin-top:16px; text-align:center;
    }
    .toast{
      position:fixed; top:20px; right:20px;
      background: linear-gradient(135deg, rgba(41,211,154,.95), rgba(41,211,154,.85));
      color: white;
      padding:12px 20px; border-radius:12px;
      box-shadow: 0 4px 12px rgba(0,0,0,.3);
      font-weight:600; font-size:14px;
      opacity:0; pointer-events:none;
      transition: opacity 0.3s;
      z-index:9999;
    }
    .toast.show{opacity:1}
    footer{max-width:1200px; margin:0 auto; padding:16px; color:var(--muted); font-size:12px}
    a{color:#b7b3ff}
  </style>
</head>
<body data-theme="dark">

<!-- ✨ LOADING OVERLAY -->
<div class="loading-overlay" id="loadingOverlay">
  <div class="loading-content">
    <div class="volleyball-spinner">🏐</div>
    <div class="loading-text">Sorteando times...</div>
    <div class="loading-sub">Balanceando as equipes</div>
  </div>
</div>

<header>
  <div>
    <h1>
      🏐 Sorteador de Times (Vôlei)
      <button class="theme-toggle" id="themeToggle" title="Alternar tema">
        <span id="themeIcon">🌙</span>
      </button>
    </h1>
    <div class="sub">
      Selecione quem vai jogar e clique em <b>Sortear</b>.
    </div>
  </div>
  <div class="row">
    <span id="selPill" class="pill"><span>Selecionados:</span> <b id="selCount">0</b></span>
    <span id="totalNeeded" class="pill" style="font-size:11px;">Total necessário: <b>9</b></span>
    <button class="ghost" id="btnSelectAll">Selecionar todos</button>
    <button class="ghost" id="btnClear">Limpar seleção</button>
    <button class="ghost" id="btnEditPlayers" title="Adicionar novos jogadores">👥 Editar</button>
    <button class="danger" id="btnResetDB" title="Restaura a lista padrão">Resetar base</button>
    <button class="primary" id="btnDraw">🎲 Sortear</button>
  </div>
</header>

<main class="grid">
  <!-- Seleção -->
  <section class="card">
    <h2>Configuração</h2>
    <div class="row-config">
      <label>Nº de Times: 
        <select id="numTeams">
          <option value="2">2</option>
          <option value="3" selected>3</option>
        </select>
      </label>
      <label>Jogadores por Time: 
        <select id="playersPerTeam">
          <option value="3">3</option>
          <option value="4">4</option>
          <option value="5">5</option>
          <option value="6">6</option>
        </select>
      </label>
    </div>

    <h2>Jogadores</h2>
    <div class="row">
      <input id="filter" type="text" placeholder="Filtrar por nome…" />
      <span class="hint">Se selecionar mais que o necessário, o sorteio escolhe aleatoriamente e mostra os reservas.</span>
    </div>

    <div id="playersList" class="list"></div>

    <div class="status" id="statusBox">
      <strong>Como funciona:</strong> o sorteio faz milhares de tentativas aleatórias considerando as pontuações (1-10) e escolhe a divisão mais equilibrada.
    </div>
  </section>

  <!-- Times -->
  <section class="card">
    <h2>Times sorteados</h2>
    <div class="teams" id="teamsContainer">
      <!-- Times serão inseridos aqui dinamicamente -->
    </div>

    <!-- 📊 INDICADOR DE EQUILÍBRIO -->
    <div class="balance-indicator" id="balanceIndicator" style="display:none">
      <div class="balance-title">
        📊 Equilíbrio dos Times
      </div>
      <div class="balance-bars" id="balanceBars"></div>
      <div class="balance-score" id="balanceScore"></div>
    </div>

    <div class="status">
      <div><strong>Reservas (se houver):</strong></div>
      <div class="reserves" id="reserves"></div>
    </div>

    <div class="copy-btn-container">
      <button class="success" id="btnCopyWhatsApp" style="display:none;">
        📋 Copiar para WhatsApp
      </button>
    </div>
  </section>
</main>

<div id="toast" class="toast">✅ Copiado para a área de transferência!</div>

<footer>
  <div>
    "Banco de dados" local: usa <b>LocalStorage</b>. Hospede este HTML em qualquer lugar pra compartilhar!
  </div>
</footer>

<script>
/** =========================
 *  TEMA CLARO/ESCURO
 *  ========================= */
const LS_THEME_KEY = "sorteador_theme";

function loadTheme(){
  const saved = localStorage.getItem(LS_THEME_KEY);
  return saved || "dark";
}

function saveTheme(theme){
  localStorage.setItem(LS_THEME_KEY, theme);
}

function applyTheme(theme){
  document.body.setAttribute("data-theme", theme);
  document.getElementById("themeIcon").textContent = theme === "dark" ? "🌙" : "☀️";
}

let currentTheme = loadTheme();
applyTheme(currentTheme);

document.getElementById("themeToggle").addEventListener("click", () => {
  currentTheme = currentTheme === "dark" ? "light" : "dark";
  applyTheme(currentTheme);
  saveTheme(currentTheme);
});

/** =========================
 *  "BANCO DE DADOS" (LocalStorage)
 *  ========================= */
const LS_DB_KEY = "sorteador_players_db_v1";
const LS_SEL_KEY = "sorteador_players_selected_v1";

const DEFAULT_PLAYERS = [
  { name: "Canaves", points: 7 },
  { name: "Luis", points: 7 },
  { name: "Jeffin", points: 7 },
  { name: "Gabriel", points: 7 },
  { name: "Wellington", points: 7 },
  { name: "Bryan", points: 8 },
  { name: "Kayo", points: 8 },
  { name: "Mateus", points: 4 },
  { name: "Luan", points: 4 },
  { name: "Leticia", points: 6 },
  { name: "Carol", points: 8 },
  { name: "Thais", points: 2 },
  { name: "Gislaine", points: 3 },
  { name: "Isabel", points: 3 }
];

function loadPlayersDB(){
  const raw = localStorage.getItem(LS_DB_KEY);
  if(!raw){
    localStorage.setItem(LS_DB_KEY, JSON.stringify(DEFAULT_PLAYERS));
    return structuredClone(DEFAULT_PLAYERS);
  }
  
  try{
    const parsed = JSON.parse(raw);
    if(Array.isArray(parsed) && parsed.length){
      const defaultsMap = new Map(DEFAULT_PLAYERS.map(p => [p.name, p]));
      const merged = parsed.map(p => {
        const def = defaultsMap.get(p.name);
        return def ? { ...def } : p;
      });
      localStorage.setItem(LS_DB_KEY, JSON.stringify(merged));
      return merged;
    }
  }catch(e){}
  
  localStorage.setItem(LS_DB_KEY, JSON.stringify(DEFAULT_PLAYERS));
  return structuredClone(DEFAULT_PLAYERS);
}

function savePlayersDB(players){
  localStorage.setItem(LS_DB_KEY, JSON.stringify(players));
}

function loadSelectedSet(){
  const raw = localStorage.getItem(LS_SEL_KEY);
  if(!raw) return new Set();
  try{
    const arr = JSON.parse(raw);
    if(Array.isArray(arr)) return new Set(arr);
  }catch(e){}
  return new Set();
}

function saveSelectedSet(set){
  localStorage.setItem(LS_SEL_KEY, JSON.stringify([...set]));
}

/** =========================
 *  RNG forte (crypto) + shuffle
 *  ========================= */
function randInt(maxExclusive){
  const buf = new Uint32Array(1);
  crypto.getRandomValues(buf);
  return buf[0] % maxExclusive;
}

function shuffleInPlace(arr){
  for(let i = arr.length - 1; i > 0; i--){
    const j = randInt(i + 1);
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
  return arr;
}

/** =========================
 *  Balanceamento: Monte Carlo (1-10 pontos)
 *  ========================= */
function teamScore(teams){
  const sums = teams.map(t => t.reduce((acc,p)=>acc+p.points,0));
  const max = Math.max(...sums);
  const min = Math.min(...sums);
  const mean = sums.reduce((a,b)=>a+b,0)/sums.length;
  const variance = sums.reduce((acc,s)=>acc + (s-mean)*(s-mean), 0)/sums.length;
  return (max - min) * 2 + Math.sqrt(variance);
}

function bestBalancedTeams(playing, numTeams, playersPerTeam, iterations=15000){
  let bestScore = Infinity;
  let bestTeams = null;
  
  for(let i = 0; i < iterations; i++){
    const shuffledPool = structuredClone(playing);
    shuffleInPlace(shuffledPool);
    
    const teams = [];
    for(let t = 0; t < numTeams; t++){
      const start = t * playersPerTeam;
      const end = start + playersPerTeam;
      teams.push(shuffledPool.slice(start, end));
    }
    
    const score = teamScore(teams);
    if(score < bestScore){
      bestScore = score;
      bestTeams = structuredClone(teams);
    }
  }
  
  return bestTeams;
}

/** =========================
 *  UI
 *  ========================= */
let players = loadPlayersDB();
let selected = loadSelectedSet();
let lastDrawResult = null;

const els = {
  list: document.getElementById("playersList"),
  filter: document.getElementById("filter"),
  selCount: document.getElementById("selCount"),
  selPill: document.getElementById("selPill"),
  totalNeeded: document.getElementById("totalNeeded"),
  btnSelectAll: document.getElementById("btnSelectAll"),
  btnClear: document.getElementById("btnClear"),
  btnDraw: document.getElementById("btnDraw"),
  btnResetDB: document.getElementById("btnResetDB"),
  btnEditPlayers: document.getElementById("btnEditPlayers"),
  teamsContainer: document.getElementById("teamsContainer"),
  reserves: document.getElementById("reserves"),
  statusBox: document.getElementById("statusBox"),
  numTeams: document.getElementById("numTeams"),
  playersPerTeam: document.getElementById("playersPerTeam"),
  btnCopyWhatsApp: document.getElementById("btnCopyWhatsApp"),
  toast: document.getElementById("toast"),
  loadingOverlay: document.getElementById("loadingOverlay"),
  balanceIndicator: document.getElementById("balanceIndicator"),
  balanceBars: document.getElementById("balanceBars"),
  balanceScore: document.getElementById("balanceScore")
};

function updateTotalNeeded(){
  const numTeams = parseInt(els.numTeams.value);
  const playersPerTeam = parseInt(els.playersPerTeam.value);
  const total = numTeams * playersPerTeam;
  els.totalNeeded.innerHTML = `Total necessário: <b>${total}</b>`;
}

function updateSelectedCounter(){
  const n = selected.size;
  els.selCount.textContent = n;
  els.selPill.classList.remove("ok","warn","bad");
  const needed = parseInt(els.totalNeeded.textContent.match(/\d+/)?.[0] || 0);
  if(n >= needed) els.selPill.classList.add("ok");
  else els.selPill.classList.add("bad");
}

function renderPlayers(){
  const q = els.filter.value.trim().toLowerCase();
  els.list.innerHTML = "";

  const filtered = players
    .slice()
    .sort((a,b)=>a.name.localeCompare(b.name, "pt-BR"))
    .filter(p => !q || p.name.toLowerCase().includes(q));

  for(const p of filtered){
    const id = "p_" + p.name.replace(/\s+/g,"_");

    const wrapper = document.createElement("label");
    wrapper.className = "player";
    wrapper.htmlFor = id;

    const left = document.createElement("div");
    left.className = "left";

    const cb = document.createElement("input");
    cb.type = "checkbox";
    cb.id = id;
    cb.checked = selected.has(p.name);
    cb.addEventListener("change", () => {
      if(cb.checked) selected.add(p.name);
      else selected.delete(p.name);
      saveSelectedSet(selected);
      updateSelectedCounter();
    });

    const badge = document.createElement("div");
    badge.className = "badge";
    badge.textContent = p.points;

    const info = document.createElement("div");
    const name = document.createElement("div");
    name.className = "name";
    name.textContent = p.name;

    const small = document.createElement("div");
    small.className = "small";
    small.textContent = "Pontos (1-10)";

    info.appendChild(name);
    info.appendChild(small);

    left.appendChild(cb);
    left.appendChild(badge);
    left.appendChild(info);

    wrapper.appendChild(left);

    els.list.appendChild(wrapper);
  }

  updateSelectedCounter();
}

function clearTeams(){
  els.teamsContainer.innerHTML = "";
  els.reserves.innerHTML = "";
  els.btnCopyWhatsApp.style.display = "none";
  els.balanceIndicator.style.display = "none";
  lastDrawResult = null;
}

function createTeamElement(team, teamIndex){
  const teamDiv = document.createElement("div");
  teamDiv.className = "team";
  
  const teamNames = ["🔴 Time Jabur", "🔵 Time Mascarenhas", "🟢 Time Hernandes"];
  teamDiv.innerHTML = `
    <h3>${teamNames[teamIndex] || `⚪ Time ${teamIndex + 1}`}</h3>
    <ul class="ul"></ul>
  `;
  
  const ul = teamDiv.querySelector("ul");
  const view = team.slice().sort((a,b) => a.name.localeCompare(b.name,"pt-BR"));
  for(const p of view){
    const li = document.createElement("li");
    li.textContent = p.name;
    ul.appendChild(li);
  }
  
  return teamDiv;
}

function renderTeams(teams, reserves){
  els.teamsContainer.innerHTML = "";
  teams.forEach((team, idx) => {
    els.teamsContainer.appendChild(createTeamElement(team, idx));
  });

  els.reserves.innerHTML = "";
  if(reserves.length){
    const sortedReserves = reserves.slice().sort((a,b) => a.name.localeCompare(b.name,"pt-BR"));
    sortedReserves.forEach(p=>{
      const sp = document.createElement("span");
      sp.textContent = p.name;
      els.reserves.appendChild(sp);
    });
  }else{
    const sp = document.createElement("span");
    sp.textContent = "Nenhum";
    els.reserves.appendChild(sp);
  }
}

/** =========================
 *  📊 INDICADOR DE EQUILÍBRIO
 *  ========================= */
function renderBalanceIndicator(teams){
  const teamNames = ["🔴 Time Jabur", "🔵 Time Mascarenhas", "🟢 Time Hernandes"];
  const colors = ["#ff5c7a", "#7c5cff", "#29d39a"];
  
  const sums = teams.map(t => t.reduce((acc,p)=>acc+p.points,0));
  const max = Math.max(...sums);
  const min = Math.min(...sums);
  const diff = max - min;
  
  els.balanceBars.innerHTML = "";
  
  teams.forEach((team, idx) => {
    const sum = sums[idx];
    const percentage = (sum / max) * 100;
    
    const item = document.createElement("div");
    item.className = "balance-bar-item";
    
    const label = document.createElement("div");
    label.className = "bar-label";
    label.textContent = teamNames[idx] || `Time ${idx + 1}`;
    
    const wrapper = document.createElement("div");
    wrapper.className = "bar-wrapper";
    
    const fill = document.createElement("div");
    fill.className = "bar-fill";
    fill.style.width = "0%";
    fill.style.background = colors[idx] || "#7c5cff";
    
    const value = document.createElement("div");
    value.className = "bar-value";
    value.textContent = `${sum} pts`;
    
    wrapper.appendChild(fill);
    item.appendChild(label);
    item.appendChild(wrapper);
    item.appendChild(value);
    els.balanceBars.appendChild(item);
    
    // Animação após um delay
    setTimeout(() => {
      fill.style.width = `${percentage}%`;
    }, 100);
  });
  
  // Score de equilíbrio
  let scoreClass = "excellent";
  let scoreText = "🎯 Excelente equilíbrio!";
  
  if(diff <= 1){
    scoreClass = "excellent";
    scoreText = "🎯 Perfeito! Diferença de apenas " + diff + " ponto" + (diff !== 1 ? "s" : "");
  } else if(diff <= 3){
    scoreClass = "good";
    scoreText = "✅ Ótimo equilíbrio! Diferença de " + diff + " pontos";
  } else if(diff <= 5){
    scoreClass = "good";
    scoreText = "👍 Bom equilíbrio! Diferença de " + diff + " pontos";
  } else {
    scoreClass = "fair";
    scoreText = "⚠️ Equilíbrio razoável. Diferença de " + diff + " pontos";
  }
  
  els.balanceScore.className = "balance-score " + scoreClass;
  els.balanceScore.textContent = scoreText;
  
  els.balanceIndicator.style.display = "block";
}

/** =========================
 *  WHATSAPP
 *  ========================= */
function generateWhatsAppText(teams, reserves){
  const teamNames = ["🔴 *Time Jabur*", "🔵 *Time Mascarenhas*", "🟢 *Time Hernandes*"];
  
  let text = "🏐 *TIMES SORTEADOS* 🏐\n\n";
  
  teams.forEach((team, idx) => {
    const teamName = teamNames[idx] || `⚪ Time ${idx + 1}`;
    const sortedTeam = team.slice().sort((a,b) => a.name.localeCompare(b.name,"pt-BR"));
    
    text += `${teamName}\n`;
    sortedTeam.forEach(p => {
      text += `• ${p.name}\n`;
    });
    text += "\n";
  });
  
  if(reserves.length){
    text += "🪑 *Reservas:*\n";
    const sortedReserves = reserves.slice().sort((a,b) => a.name.localeCompare(b.name,"pt-BR"));
    sortedReserves.forEach(p => {
      text += `• ${p.name}\n`;
    });
  }
  
  text += "\n_Vamos para o Game! 💪_";
  
  return text;
}

function copyToClipboard(text){
  if(navigator.clipboard && navigator.clipboard.writeText){
    navigator.clipboard.writeText(text).then(() => {
      showToast();
    }).catch(err => {
      fallbackCopy(text);
    });
  } else {
    fallbackCopy(text);
  }
}

function fallbackCopy(text){
  const textarea = document.createElement("textarea");
  textarea.value = text;
  textarea.style.position = "fixed";
  textarea.style.opacity = "0";
  document.body.appendChild(textarea);
  textarea.select();
  try{
    document.execCommand("copy");
    showToast();
  }catch(err){
    alert("Não foi possível copiar. Tente manualmente.");
  }
  document.body.removeChild(textarea);
}

function showToast(){
  els.toast.classList.add("show");
  setTimeout(() => {
    els.toast.classList.remove("show");
  }, 2500);
}

/** =========================
 *  ✨ SORTEIO COM ANIMAÇÃO
 *  ========================= */
function draw(){
  clearTeams();

  const numTeams = parseInt(els.numTeams.value);
  const playersPerTeam = parseInt(els.playersPerTeam.value);
  const totalNeeded = numTeams * playersPerTeam;

  const selectedPlayers = players.filter(p => selected.has(p.name));
  
  if(selectedPlayers.length < totalNeeded){
    els.statusBox.innerHTML = `❌ <strong>Selecione pelo menos ${totalNeeded} jogadores.</strong> Atualmente: ${selectedPlayers.length}.`;
    return;
  }

  // Mostra loading
  els.loadingOverlay.classList.add("active");
  els.btnDraw.disabled = true;

  // Simula processamento (1.5 segundos)
  setTimeout(() => {
    const pool = structuredClone(selectedPlayers);
    shuffleInPlace(pool);
    
    const playing = pool.slice(0, totalNeeded);
    const reserves = pool.slice(totalNeeded);

    const hasDuplicates = playing.some(p1 => reserves.some(p2 => p1.name === p2.name));
    if(hasDuplicates){
      els.statusBox.innerHTML = "❌ ERRO INTERNO: Duplicatas detectadas!";
      els.loadingOverlay.classList.remove("active");
      els.btnDraw.disabled = false;
      return;
    }

    const teams = bestBalancedTeams(structuredClone(playing), numTeams, playersPerTeam);

    const sums = teams.map(t => t.reduce((a,p)=>a+p.points,0));
    const max = Math.max(...sums), min = Math.min(...sums);
    
    els.statusBox.innerHTML =
      `✅ <strong>Sorteio concluído!</strong><br>
       Configuração: <strong>${numTeams}×${playersPerTeam}=${totalNeeded}</strong><br>
       Balanceamento: <strong>${min}-${max} pts</strong> (diferença: ${max-min})<br>
       ${reserves.length ? `Reservas: <strong>${reserves.length}</strong>` : "Todos jogando!"}`;

    renderTeams(teams, reserves);
    renderBalanceIndicator(teams);
    
    lastDrawResult = { teams, reserves };
    els.btnCopyWhatsApp.style.display = "inline-block";
    
    // Remove loading
    els.loadingOverlay.classList.remove("active");
    els.btnDraw.disabled = false;
  }, 1500);
}

/** =========================
 *  Eventos
 *  ========================= */
els.filter.addEventListener("input", renderPlayers);
els.numTeams.addEventListener("change", updateTotalNeeded);
els.playersPerTeam.addEventListener("change", updateTotalNeeded);

els.btnSelectAll.addEventListener("click", () => {
  players.forEach(p => selected.add(p.name));
  saveSelectedSet(selected);
  renderPlayers();
});

els.btnClear.addEventListener("click", () => {
  selected = new Set();
  saveSelectedSet(selected);
  renderPlayers();
  clearTeams();
  els.statusBox.innerHTML = `<strong>Como funciona:</strong> sorteio faz milhares de tentativas considerando pontuações (1-10).`;
});

els.btnDraw.addEventListener("click", draw);

els.btnCopyWhatsApp.addEventListener("click", () => {
  if(!lastDrawResult) return;
  
  const text = generateWhatsAppText(lastDrawResult.teams, lastDrawResult.reserves);
  copyToClipboard(text);
});

els.btnResetDB.addEventListener("click", () => {
  if(confirm("Resetar base? Perde alterações salvas.")){
    localStorage.removeItem(LS_DB_KEY);
    localStorage.removeItem(LS_SEL_KEY);
    players = loadPlayersDB();
    selected = loadSelectedSet();
    renderPlayers();
    clearTeams();
    els.statusBox.innerHTML = `🔄 <strong>Base restaurada!</strong> Pontos 1-10 atualizados.`;
  }
});

els.btnEditPlayers.addEventListener("click", () => {
  const novoNome = prompt("Nome do novo jogador:");
  if(!novoNome?.trim()) return;
  
  const novaNota = prompt("Pontuação (1-10):", "5");
  const pontos = Math.max(1, Math.min(10, parseInt(novaNota) || 5));
  
  if(players.some(p => p.name.toLowerCase() === novoNome.trim().toLowerCase())){
    alert("❌ Jogador já existe!");
    return;
  }
  
  players.push({name: novoNome.trim(), points: pontos});
  savePlayersDB(players);
  renderPlayers();
  alert(`✅ ${novoNome.trim()} (${pontos}) adicionado!`);
});

// Inicializa
updateTotalNeeded();
renderPlayers();
clearTeams();
</script>
</body>
</html>
