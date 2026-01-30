<!doctype html>
<html lang="pt-br">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Sorteador de Times (Equilibrado)</title>
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><circle fill='%237c5cff' cx='50' cy='50' r='50'/><text fill='white' font-size='60' font-weight='bold' text-anchor='middle' y='65'>🎲</text></svg>">
  <style>
    :root{
      --bg:#0b1020; --card:#121a33; --card2:#0f1730; --text:#e8ecff; --muted:#aab3df;
      --accent:#7c5cff; --good:#29d39a; --warn:#ffcc66; --bad:#ff5c7a; --border:#22305e;
    }
    *{box-sizing:border-box}
    body{
      margin:0; font-family:system-ui,-apple-system,Segoe UI,Roboto,Ubuntu,Arial;
      background: radial-gradient(1200px 700px at 10% 10%, #16204a 0%, var(--bg) 55%);
      color:var(--text);
    }
    header{
      padding:24px 16px; max-width:1200px; margin:0 auto;
      display:flex; gap:12px; align-items:flex-start; justify-content:space-between; flex-wrap:wrap;
    }
    h1{margin:0; font-size:20px}
    .sub{color:var(--muted); font-size:13px; margin-top:6px}
    main{max-width:1200px; margin:0 auto; padding:0 16px 32px}
    .grid{
      display:grid; grid-template-columns: 1.2fr 1fr; gap:16px;
    }
    @media (max-width: 980px){ .grid{grid-template-columns:1fr} }
    .card{
      background:linear-gradient(180deg, rgba(255,255,255,0.03), transparent 40%), var(--card);
      border:1px solid var(--border); border-radius:14px; padding:16px;
      box-shadow: 0 10px 30px rgba(0,0,0,.25);
    }
    .card h2{margin:0 0 10px 0; font-size:16px}
    .row{display:flex; gap:10px; flex-wrap:wrap; align-items:center}
    .row-config{display:flex; gap:12px; flex-wrap:wrap; align-items:center; margin-bottom:12px;}
    select{padding:10px 12px; border-radius:10px; border:1px solid var(--border); background: var(--card2); color:var(--text);}
    button{
      border:1px solid var(--border);
      background: rgba(124,92,255,.18);
      color:var(--text);
      padding:10px 12px; border-radius:10px;
      cursor:pointer; font-weight:600;
    }
    button:hover{border-color: rgba(124,92,255,.8)}
    button.primary{
      background: linear-gradient(135deg, rgba(124,92,255,.9), rgba(41,211,154,.75));
      border-color: transparent;
    }
    button.ghost{background:transparent}
    button.danger{background: rgba(255,92,122,.16)}
    input[type="text"]{
      flex:1; padding:10px 12px; border-radius:10px;
      border:1px solid var(--border); background: var(--card2); color:var(--text);
      outline:none;
    }
    .hint{font-size:12px; color:var(--muted)}
    .pill{
      display:inline-flex; align-items:center; gap:8px;
      border:1px solid var(--border); background: rgba(255,255,255,.03);
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
      border:1px solid rgba(255,255,255,.06);
      background: rgba(0,0,0,.12);
    }
    .left{display:flex; gap:10px; align-items:center}
    .badge{
      width:28px; height:28px; border-radius:9px;
      display:grid; place-items:center;
      background: rgba(124,92,255,.20); border:1px solid rgba(124,92,255,.35);
      font-weight:800; font-size:12px;
    }
    .name{font-weight:650}
    .small{font-size:12px; color:var(--muted)}
    .teams{
      display:grid; grid-template-columns:repeat(auto-fit, minmax(280px, 1fr)); gap:12px;
    }
    .team{
      border:1px solid var(--border);
      border-radius:14px; padding:14px;
      background: linear-gradient(180deg, rgba(255,255,255,.03), transparent 50%), rgba(0,0,0,.12);
      min-height:180px;
    }
    .team h3{margin:0 0 12px 0; font-size:16px}
    .ul{margin:0; padding-left:16px}
    .ul li{margin:6px 0; padding:4px 0;}
    .status{
      margin-top:12px; padding:10px 12px; border-radius:12px;
      border:1px dashed rgba(255,255,255,.15);
      color:var(--muted); font-size:13px;
    }
    .status strong{color:var(--text)}
    .reserves{margin-top:10px}
    .reserves span{
      display:inline-block; margin:4px 6px 0 0;
      padding:6px 10px; border-radius:999px;
      border:1px solid rgba(255,255,255,.10);
      background: rgba(255,255,255,.03);
      font-size:12px; color:var(--muted);
    }
    footer{max-width:1200px; margin:0 auto; padding:16px; color:var(--muted); font-size:12px}
    a{color:#b7b3ff}
  </style>
</head>
<body>
<header>
  <div>
    <h1>Sorteador de Times (Equilibrado)</h1>
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

    <div class="status">
      <div><strong>Reservas (se houver):</strong></div>
      <div class="reserves" id="reserves"></div>
    </div>
  </section>
</main>

<footer>
  <div>
    "Banco de dados" local: usa <b>LocalStorage</b>. Hospede este HTML em qualquer lugar pra compartilhar!
  </div>
</footer>

<script>
/** =========================
 *  "BANCO DE DADOS" (LocalStorage) - ATUALIZA PONTOS!
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
      // ✅ MERGE: força novos pontos dos DEFAULTs!
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
    // ✅ CÓPIA COMPLETA a cada iteração
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
      bestTeams = structuredClone(teams); // ✅ Cópia profunda final
    }
  }
  
  return bestTeams;
}

/** =========================
 *  UI
 *  ========================= */
let players = loadPlayersDB();
let selected = loadSelectedSet();

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
  playersPerTeam: document.getElementById("playersPerTeam")
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
}

function createTeamElement(team, teamIndex){
  const teamDiv = document.createElement("div");
  teamDiv.className = "team";
  
  const teamNames = ["🔴 Jabur", "🔵 Mascarenhas", "🟢 Hernandes"];
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

  // ✅ SEM DUPLICATAS: Cópia profunda + slice limpo
  const pool = structuredClone(selectedPlayers);
  shuffleInPlace(pool);
  
  const playing = pool.slice(0, totalNeeded);
  const reserves = pool.slice(totalNeeded);

  // ✅ VERIFICAÇÃO: NENHUM jogador em comum
  const hasDuplicates = playing.some(p1 => reserves.some(p2 => p1.name === p2.name));
  if(hasDuplicates){
    els.statusBox.innerHTML = "❌ ERRO INTERNO: Duplicatas detectadas!";
    return;
  }

  // ✅ Times balanceados (cópia profunda)
  const teams = bestBalancedTeams(structuredClone(playing), numTeams, playersPerTeam);

  const sums = teams.map(t => t.reduce((a,p)=>a+p.points,0));
  const max = Math.max(...sums), min = Math.min(...sums);
  
  els.statusBox.innerHTML =
    `✅ <strong>Sorteio concluído!</strong><br>
     Configuração: <strong>${numTeams}×${playersPerTeam}=${totalNeeded}</strong><br>
     Balanceamento: <strong>${min}-${max} pts</strong> (diferença: ${max-min})<br>
     ${reserves.length ? `Reservas: <strong>${reserves.length}</strong>` : "Todos jogando!"}`;

  renderTeams(teams, reserves);
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
