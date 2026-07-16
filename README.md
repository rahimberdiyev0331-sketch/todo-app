<!DOCTYPE html>
<html lang="uz">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Focus — To-Do</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Sora:wght@400;600;700;800&family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root{
    --bg: #0a0b10;
    --bg-2: #0f1117;
    --glass: rgba(255,255,255,0.045);
    --glass-border: rgba(255,255,255,0.08);
    --glass-hover: rgba(255,255,255,0.07);
    --text: #eef0f5;
    --text-dim: #8b8fa3;
    --text-faint: #565a6e;
    --accent-a: #7c5cff;
    --accent-b: #4dd8c4;
    --danger: #ff6b7a;
    --radius: 16px;
  }
  *{margin:0;padding:0;box-sizing:border-box;}
  html,body{height:100%;}
  body{
    font-family:'Inter',sans-serif;
    background:
      radial-gradient(ellipse 900px 500px at 15% -5%, rgba(124,92,255,0.16), transparent 60%),
      radial-gradient(ellipse 700px 500px at 100% 10%, rgba(77,216,196,0.10), transparent 55%),
      var(--bg);
    color:var(--text);
    min-height:100vh;
    display:flex;
    justify-content:center;
    padding:56px 20px 80px;
  }
  .app{width:100%;max-width:620px;}

  /* Header */
  header{margin-bottom:28px;}
  .eyebrow{
    font-family:'JetBrains Mono',monospace;
    font-size:12px;
    letter-spacing:0.14em;
    text-transform:uppercase;
    color:var(--accent-b);
    margin-bottom:10px;
    display:flex;
    align-items:center;
    gap:8px;
  }
  .eyebrow::before{content:'';width:6px;height:6px;border-radius:50%;background:var(--accent-b);box-shadow:0 0 10px var(--accent-b);}
  h1{
    font-family:'Sora',sans-serif;
    font-weight:800;
    font-size:38px;
    letter-spacing:-0.02em;
    background:linear-gradient(120deg,#fff 30%,#b8bacb 100%);
    -webkit-background-clip:text;
    background-clip:text;
    color:transparent;
  }

  /* Progress ring bar - signature element */
  .progress-wrap{
    margin-top:22px;
    background:var(--glass);
    border:1px solid var(--glass-border);
    border-radius:var(--radius);
    padding:16px 18px;
    backdrop-filter:blur(20px);
    -webkit-backdrop-filter:blur(20px);
  }
  .progress-top{display:flex;justify-content:space-between;align-items:baseline;margin-bottom:10px;}
  .progress-label{font-size:13px;color:var(--text-dim);font-weight:500;}
  .progress-count{font-family:'JetBrains Mono',monospace;font-size:13px;color:var(--text);}
  .progress-track{height:6px;border-radius:10px;background:rgba(255,255,255,0.06);overflow:hidden;}
  .progress-fill{
    height:100%;
    border-radius:10px;
    background:linear-gradient(90deg,var(--accent-a),var(--accent-b));
    width:0%;
    transition:width .45s cubic-bezier(.4,0,.2,1);
    box-shadow:0 0 12px rgba(124,92,255,0.5);
  }

  /* Input row */
  .input-row{
    margin-top:20px;
    display:flex;
    gap:10px;
  }
  .input-row input[type="text"]{
    flex:1;
    background:var(--glass);
    border:1px solid var(--glass-border);
    border-radius:var(--radius);
    padding:15px 18px;
    color:var(--text);
    font-size:15px;
    font-family:'Inter',sans-serif;
    backdrop-filter:blur(20px);
    -webkit-backdrop-filter:blur(20px);
    transition:border-color .2s, background .2s;
  }
  .input-row input[type="text"]::placeholder{color:var(--text-faint);}
  .input-row input[type="text"]:focus{
    outline:none;
    border-color:rgba(124,92,255,0.55);
    background:var(--glass-hover);
  }
  .add-btn{
    width:52px;height:52px;flex-shrink:0;
    border:none;border-radius:var(--radius);
    background:linear-gradient(135deg,var(--accent-a),#5f42e0);
    color:#fff;
    font-size:24px;
    cursor:pointer;
    display:flex;align-items:center;justify-content:center;
    transition:transform .15s, box-shadow .15s;
    box-shadow:0 4px 20px rgba(124,92,255,0.35);
  }
  .add-btn:hover{transform:translateY(-2px);box-shadow:0 6px 26px rgba(124,92,255,0.5);}
  .add-btn:active{transform:translateY(0);}

  /* Search + Filters */
  .controls{
    margin-top:18px;
    display:flex;
    gap:10px;
    flex-wrap:wrap;
  }
  .search-box{
    flex:1;
    min-width:160px;
    position:relative;
  }
  .search-box svg{position:absolute;left:14px;top:50%;transform:translateY(-50%);width:15px;height:15px;stroke:var(--text-faint);}
  .search-box input{
    width:100%;
    background:var(--glass);
    border:1px solid var(--glass-border);
    border-radius:12px;
    padding:11px 14px 11px 38px;
    color:var(--text);
    font-size:13.5px;
    font-family:'Inter',sans-serif;
  }
  .search-box input::placeholder{color:var(--text-faint);}
  .search-box input:focus{outline:none;border-color:rgba(124,92,255,0.4);}

  .filters{display:flex;gap:6px;background:var(--glass);border:1px solid var(--glass-border);border-radius:12px;padding:4px;}
  .filter-btn{
    border:none;background:transparent;color:var(--text-dim);
    font-size:12.5px;font-family:'Inter',sans-serif;font-weight:500;
    padding:7px 13px;border-radius:9px;cursor:pointer;
    transition:background .2s, color .2s;
    white-space:nowrap;
  }
  .filter-btn.active{background:rgba(124,92,255,0.18);color:#c9bfff;}
  .filter-btn:hover:not(.active){color:var(--text);}

  /* Task list */
  .list{margin-top:22px;display:flex;flex-direction:column;gap:10px;}
  .task{
    background:var(--glass);
    border:1px solid var(--glass-border);
    border-radius:14px;
    padding:15px 16px;
    display:flex;
    align-items:center;
    gap:13px;
    backdrop-filter:blur(20px);
    -webkit-backdrop-filter:blur(20px);
    animation:rise .35s cubic-bezier(.4,0,.2,1) both;
    transition:border-color .2s, background .2s;
  }
  .task:hover{background:var(--glass-hover);border-color:rgba(255,255,255,0.14);}
  @keyframes rise{from{opacity:0;transform:translateY(8px);}to{opacity:1;transform:translateY(0);}}

  .check{
    width:22px;height:22px;flex-shrink:0;border-radius:7px;
    border:1.5px solid var(--text-faint);
    cursor:pointer;
    display:flex;align-items:center;justify-content:center;
    transition:all .2s;
  }
  .check svg{width:13px;height:13px;opacity:0;transition:opacity .15s;stroke:#0a0b10;}
  .task.done .check{background:linear-gradient(135deg,var(--accent-a),var(--accent-b));border-color:transparent;}
  .task.done .check svg{opacity:1;}

  .task-text{
    flex:1;
    font-size:14.5px;
    line-height:1.4;
    word-break:break-word;
    transition:color .2s;
  }
  .task.done .task-text{color:var(--text-faint);text-decoration:line-through;text-decoration-color:var(--text-faint);}

  .task-edit-input{
    flex:1;
    background:rgba(255,255,255,0.06);
    border:1px solid rgba(124,92,255,0.5);
    border-radius:8px;
    padding:7px 10px;
    color:var(--text);
    font-size:14.5px;
    font-family:'Inter',sans-serif;
  }
  .task-edit-input:focus{outline:none;}

  .task-actions{display:flex;gap:4px;flex-shrink:0;}
  .icon-btn{
    width:32px;height:32px;border:none;background:transparent;
    color:var(--text-faint);cursor:pointer;border-radius:8px;
    display:flex;align-items:center;justify-content:center;
    transition:background .15s, color .15s;
  }
  .icon-btn svg{width:15px;height:15px;stroke:currentColor;fill:none;}
  .icon-btn:hover{background:rgba(255,255,255,0.08);color:var(--text);}
  .icon-btn.delete:hover{color:var(--danger);background:rgba(255,107,122,0.12);}

  /* Empty state */
  .empty{
    text-align:center;
    padding:60px 20px;
    color:var(--text-faint);
  }
  .empty svg{width:40px;height:40px;stroke:var(--text-faint);margin-bottom:14px;opacity:.6;}
  .empty p{font-size:14px;}
  .empty span{font-family:'JetBrains Mono',monospace;font-size:12px;color:var(--text-faint);opacity:.7;}

  @media (max-width:480px){
    h1{font-size:30px;}
    body{padding:40px 14px 60px;}
    .controls{flex-direction:column;}
    .filters{justify-content:space-between;}
  }

  ::selection{background:rgba(124,92,255,0.35);}
  :focus-visible{outline:2px solid var(--accent-b);outline-offset:2px;}
</style>
</head>
<body>
<div class="app">
  <header>
    <div class="eyebrow">Bugungi vazifalar</div>
    <h1>Vazifalar</h1>
    <div class="progress-wrap">
      <div class="progress-top">
        <span class="progress-label" id="progressLabel">Hali vazifa yo'q</span>
        <span class="progress-count" id="progressCount">0/0</span>
      </div>
      <div class="progress-track"><div class="progress-fill" id="progressFill"></div></div>
    </div>
  </header>

  <div class="input-row">
    <input type="text" id="taskInput" placeholder="Yangi vazifa qo'shish..." maxlength="200" autocomplete="off">
    <button class="add-btn" id="addBtn" aria-label="Qo'shish">+</button>
  </div>

  <div class="controls">
    <div class="search-box">
      <svg viewBox="0 0 24 24" fill="none" stroke-width="2" stroke-linecap="round"><circle cx="11" cy="11" r="7"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
      <input type="text" id="searchInput" placeholder="Qidirish...">
    </div>
    <div class="filters" id="filters">
      <button class="filter-btn active" data-filter="all">Barchasi</button>
      <button class="filter-btn" data-filter="active">Faol</button>
      <button class="filter-btn" data-filter="completed">Bajarilgan</button>
    </div>
  </div>

  <div class="list" id="list"></div>
</div>

<script>
(function(){
  const STORAGE_KEY = 'focus_todo_tasks_v1';
  let tasks = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
  let currentFilter = 'all';
  let searchQuery = '';
  let editingId = null;

  const listEl = document.getElementById('list');
  const inputEl = document.getElementById('taskInput');
  const addBtn = document.getElementById('addBtn');
  const searchEl = document.getElementById('searchInput');
  const filtersEl = document.getElementById('filters');
  const progressFill = document.getElementById('progressFill');
  const progressLabel = document.getElementById('progressLabel');
  const progressCount = document.getElementById('progressCount');

  function save(){
    localStorage.setItem(STORAGE_KEY, JSON.stringify(tasks));
  }

  function uid(){
    return Date.now().toString(36) + Math.random().toString(36).slice(2,7);
  }

  function addTask(text){
    text = text.trim();
    if(!text) return;
    tasks.unshift({ id: uid(), text, done: false, createdAt: Date.now() });
    save();
    render();
  }

  function deleteTask(id){
    tasks = tasks.filter(t => t.id !== id);
    save();
    render();
  }

  function toggleTask(id){
    const t = tasks.find(t => t.id === id);
    if(t){ t.done = !t.done; save(); render(); }
  }

  function startEdit(id){
    editingId = id;
    render();
  }

  function commitEdit(id, value){
    const t = tasks.find(t => t.id === id);
    const val = value.trim();
    if(t && val){ t.text = val; }
    editingId = null;
    save();
    render();
  }

  function getFiltered(){
    return tasks.filter(t => {
      if(currentFilter === 'active' && t.done) return false;
      if(currentFilter === 'completed' && !t.done) return false;
      if(searchQuery && !t.text.toLowerCase().includes(searchQuery.toLowerCase())) return false;
      return true;
    });
  }

  function updateProgress(){
    const total = tasks.length;
    const done = tasks.filter(t => t.done).length;
    const pct = total ? Math.round((done/total)*100) : 0;
    progressFill.style.width = pct + '%';
    progressCount.textContent = `${done}/${total}`;
    progressLabel.textContent = total === 0
      ? "Hali vazifa yo'q"
      : done === total
        ? 'Barchasi bajarildi 🎉'
        : `${pct}% tugallandi`;
  }

  function iconCheck(){
    return '<svg viewBox="0 0 24 24" fill="none" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"/></svg>';
  }
  function iconEdit(){
    return '<svg viewBox="0 0 24 24" fill="none" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17 3a2.83 2.83 0 1 1 4 4L7.5 20.5 2 22l1.5-5.5L17 3z"/></svg>';
  }
  function iconTrash(){
    return '<svg viewBox="0 0 24 24" fill="none" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="3 6 5 6 21 6"/><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"/></svg>';
  }

  function escapeHtml(str){
    const div = document.createElement('div');
    div.textContent = str;
    return div.innerHTML;
  }

  function render(){
    const filtered = getFiltered();
    updateProgress();

    if(filtered.length === 0){
      const msg = tasks.length === 0
        ? "Birinchi vazifangizni qo'shing"
        : "Hech narsa topilmadi";
      listEl.innerHTML = `
        <div class="empty">
          <svg viewBox="0 0 24 24" fill="none" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M9 11l3 3L22 4"/><path d="M21 12v7a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h11"/></svg>
          <p>${msg}</p>
          <span>${tasks.length === 0 ? 'Yuqoridagi maydondan foydalaning' : 'Filtr yoki qidiruvni tekshiring'}</span>
        </div>`;
      return;
    }

    listEl.innerHTML = filtered.map(t => {
      if(editingId === t.id){
        return `
        <div class="task" data-id="${t.id}">
          <div class="check" style="visibility:hidden;"></div>
          <input type="text" class="task-edit-input" value="${escapeHtml(t.text)}" data-edit-input="${t.id}" maxlength="200">
          <div class="task-actions">
            <button class="icon-btn" data-save="${t.id}" aria-label="Saqlash">${iconCheck()}</button>
          </div>
        </div>`;
      }
      return `
      <div class="task ${t.done ? 'done' : ''}" data-id="${t.id}">
        <div class="check" data-toggle="${t.id}">${iconCheck()}</div>
        <div class="task-text">${escapeHtml(t.text)}</div>
        <div class="task-actions">
          <button class="icon-btn" data-edit="${t.id}" aria-label="Tahrirlash">${iconEdit()}</button>
          <button class="icon-btn delete" data-delete="${t.id}" aria-label="O'chirish">${iconTrash()}</button>
        </div>
      </div>`;
    }).join('');

    // Focus the edit input if one is open
    if(editingId){
      const inp = listEl.querySelector(`[data-edit-input="${editingId}"]`);
      if(inp){
        inp.focus();
        inp.setSelectionRange(inp.value.length, inp.value.length);
      }
    }
  }

  // Event delegation for list actions
  listEl.addEventListener('click', (e) => {
    const toggle = e.target.closest('[data-toggle]');
    const del = e.target.closest('[data-delete]');
    const edit = e.target.closest('[data-edit]');
    const save = e.target.closest('[data-save]');

    if(toggle) toggleTask(toggle.dataset.toggle);
    else if(del) deleteTask(del.dataset.delete);
    else if(edit) startEdit(edit.dataset.edit);
    else if(save){
      const input = listEl.querySelector(`[data-edit-input="${save.dataset.save}"]`);
      commitEdit(save.dataset.save, input.value);
    }
  });

  listEl.addEventListener('keydown', (e) => {
    if(e.target.matches('[data-edit-input]')){
      const id = e.target.dataset.editInput;
      if(e.key === 'Enter') commitEdit(id, e.target.value);
      if(e.key === 'Escape'){ editingId = null; render(); }
    }
  });

  listEl.addEventListener('blur', (e) => {
    if(e.target.matches('[data-edit-input]')){
      const id = e.target.dataset.editInput;
      commitEdit(id, e.target.value);
    }
  }, true);

  addBtn.addEventListener('click', () => {
    addTask(inputEl.value);
    inputEl.value = '';
    inputEl.focus();
  });

  inputEl.addEventListener('keydown', (e) => {
    if(e.key === 'Enter'){
      addTask(inputEl.value);
      inputEl.value = '';
    }
  });

  searchEl.addEventListener('input', (e) => {
    searchQuery = e.target.value;
    render();
  });

  filtersEl.addEventListener('click', (e) => {
    const btn = e.target.closest('.filter-btn');
    if(!btn) return;
    currentFilter = btn.dataset.filter;
    filtersEl.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    render();
  });

  render();
})();
</script>
</body>
</html>
