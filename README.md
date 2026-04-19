<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FWGS Project Tracker</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; background: #f5f4f0; color: #1a1a1a; font-size: 13px; }

/* HEADER */
.header { background: #1c1c1a; color: #f0efe8; padding: 14px 20px; display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 8px; }
.header-title { font-size: 16px; font-weight: 600; letter-spacing: -0.01em; }
.header-sub { font-size: 11px; color: #888780; text-transform: uppercase; letter-spacing: 0.08em; margin-bottom: 2px; }

/* VIEW TABS */
.view-tabs { display: flex; gap: 2px; background: #333; border-radius: 8px; padding: 3px; }
.view-tab { font-size: 11px; padding: 4px 12px; border-radius: 6px; border: none; background: transparent; color: #888780; cursor: pointer; transition: all 0.15s; }
.view-tab.active { background: #f0efe8; color: #1c1c1a; font-weight: 600; }

/* STATS */
.stats-bar { display: grid; grid-template-columns: repeat(4, 1fr); gap: 1px; background: #d3d1c7; border-bottom: 0.5px solid #d3d1c7; }
.stat-card { background: #fff; padding: 12px 18px; }
.stat-label { font-size: 10px; color: #888780; text-transform: uppercase; letter-spacing: 0.07em; margin-bottom: 4px; }
.stat-value { font-size: 22px; font-weight: 600; color: #1a1a1a; line-height: 1; }
.stat-value.critical { color: #A32D2D; }
.stat-value.cost { font-size: 15px; color: #27500a; }
.stat-value.done-val { color: #3B6D11; }

/* FILTERS */
.filters { display: flex; gap: 6px; flex-wrap: wrap; padding: 10px 20px; border-bottom: 0.5px solid #d3d1c7; background: #f5f4f0; }
.filter-btn { font-size: 11px; padding: 3px 11px; border-radius: 20px; border: 0.5px solid #b4b2a9; background: #fff; color: #5f5e5a; cursor: pointer; transition: all 0.15s; }
.filter-btn:hover { border-color: #888780; }
.filter-btn.active { background: #1c1c1a; color: #f0efe8; border-color: #1c1c1a; }

/* BADGES */
.badge { font-size: 11px; padding: 2px 9px; border-radius: 20px; white-space: nowrap; font-weight: 500; }
.badge-critical { background: #FCEBEB; color: #A32D2D; }
.badge-regular { background: #f1efe8; color: #5f5e5a; }
.badge-pending { background: #FAEEDA; color: #854F0B; }
.badge-inprogress { background: #E6F1FB; color: #185FA5; }
.badge-done { background: #EAF3DE; color: #3B6D11; }
.badge-blocked { background: #FCEBEB; color: #A32D2D; }
.badge-person { background: #EEEDFE; color: #3C3489; }
.dot { width: 7px; height: 7px; border-radius: 50%; flex-shrink: 0; display: inline-block; }
.dot-pending { background: #EF9F27; }
.dot-inprogress { background: #378ADD; }
.dot-done { background: #639922; }
.dot-blocked { background: #E24B4A; }
.deadline { font-size: 11px; color: #888780; white-space: nowrap; }
.deadline.urgent { color: #A32D2D; font-weight: 600; }

/* ── LIST VIEW ── */
.task-list { padding: 12px 20px; display: flex; flex-direction: column; gap: 6px; }
.task-card-list { border: 0.5px solid #d3d1c7; border-radius: 10px; background: #fff; overflow: hidden; cursor: pointer; transition: border-color 0.15s; }
.task-card-list:hover { border-color: #888780; }
.task-row { display: flex; align-items: center; gap: 8px; padding: 11px 14px; flex-wrap: wrap; }
.task-title { flex: 1; font-size: 13px; font-weight: 500; color: #1a1a1a; min-width: 120px; }
.expand { display: none; padding: 10px 14px 14px; border-top: 0.5px solid #f1efe8; background: #faf9f7; }
.expand.open { display: block; }
.expand-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px 20px; margin-bottom: 10px; }
.expand-label { font-size: 10px; color: #888780; text-transform: uppercase; letter-spacing: 0.07em; margin-bottom: 2px; }
.expand-val { font-size: 12px; color: #1a1a1a; }
.expand-val.empty { color: #b4b2a9; font-style: italic; }
.notes-box { background: #fff; border: 0.5px solid #d3d1c7; border-radius: 6px; padding: 8px 10px; font-size: 12px; color: #5f5e5a; margin-top: 8px; }
.cost-box { background: #eaf3de; border: 0.5px solid #c0dd97; border-radius: 6px; padding: 8px 10px; font-size: 12px; color: #27500a; margin-top: 8px; }
.status-select { font-size: 12px; padding: 4px 8px; border-radius: 6px; border: 0.5px solid #b4b2a9; background: #fff; color: #1a1a1a; cursor: pointer; margin-top: 10px; }

/* ── CARD VIEW ── */
.card-grid { padding: 16px 20px; display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 12px; }
.task-card-tile { border: 0.5px solid #d3d1c7; border-radius: 12px; background: #fff; padding: 14px; display: flex; flex-direction: column; gap: 8px; cursor: pointer; transition: box-shadow 0.15s, border-color 0.15s; }
.task-card-tile:hover { border-color: #888780; box-shadow: 0 2px 8px rgba(0,0,0,0.06); }
.tile-title { font-size: 13px; font-weight: 600; color: #1a1a1a; line-height: 1.35; }
.tile-meta { display: flex; flex-wrap: wrap; gap: 5px; }
.tile-deadline { font-size: 11px; color: #888780; margin-top: auto; padding-top: 6px; border-top: 0.5px solid #f1efe8; }
.tile-deadline.urgent { color: #A32D2D; font-weight: 600; }
.tile-people { font-size: 11px; color: #3C3489; background: #EEEDFE; border-radius: 6px; padding: 4px 8px; }
.tile-cost { font-size: 11px; color: #27500a; background: #eaf3de; border-radius: 6px; padding: 4px 8px; }

/* ── TIMELINE VIEW ── */
.timeline-wrap { padding: 16px 20px; }
.timeline-month { margin-bottom: 20px; }
.timeline-month-label { font-size: 11px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.1em; color: #888780; margin-bottom: 8px; padding-bottom: 6px; border-bottom: 0.5px solid #d3d1c7; }
.timeline-tasks { display: flex; flex-direction: column; gap: 5px; }
.timeline-task { display: flex; align-items: center; gap: 10px; background: #fff; border: 0.5px solid #d3d1c7; border-radius: 8px; padding: 9px 12px; }
.timeline-date { font-size: 11px; font-weight: 600; color: #1a1a1a; min-width: 72px; }
.timeline-date.urgent { color: #A32D2D; }
.timeline-name { flex: 1; font-size: 13px; font-weight: 500; color: #1a1a1a; }
.timeline-right { display: flex; gap: 6px; align-items: center; flex-wrap: wrap; justify-content: flex-end; }

/* BOTTOM BAR */
.bottom-bar { display: flex; gap: 14px; padding: 9px 20px; border-top: 0.5px solid #d3d1c7; background: #fff; flex-wrap: wrap; position: sticky; bottom: 0; }
.bottom-item { display: flex; align-items: center; gap: 5px; font-size: 11px; color: #5f5e5a; }
</style>
</head>
<body>

<div class="header">
  <div>
    <div class="header-sub">FWGS Leadership</div>
    <div class="header-title">Project Tracker</div>
  </div>
  <div class="view-tabs">
    <button class="view-tab active" onclick="setView('list',this)">List</button>
    <button class="view-tab" onclick="setView('card',this)">Cards</button>
    <button class="view-tab" onclick="setView('timeline',this)">Timeline</button>
  </div>
</div>

<div class="stats-bar" id="stats-bar"></div>

<div class="filters">
  <button class="filter-btn active" onclick="setFilter('all',this)">All</button>
  <button class="filter-btn" onclick="setFilter('critical',this)">Critical</button>
  <button class="filter-btn" onclick="setFilter('kiran',this)">Kiran</button>
  <button class="filter-btn" onclick="setFilter('karan',this)">Karan</button>
  <button class="filter-btn" onclick="setFilter('inprogress',this)">In progress</button>
  <button class="filter-btn" onclick="setFilter('done',this)">Done</button>
</div>

<div id="main-content"></div>
<div class="bottom-bar" id="bottom-bar"></div>

<script>
const TASKS = [
  { id:"task-training", title:"Task Management Training",                            deadline:"21 Apr 2026", completionDate:"", status:"pending",  priority:"regular",  responsible:"Karan Sharma",        fwgsTeam:"Vardan Kabra",              notes:"One from Surat, one from CSN",                  cost:"" },
  { id:"fee-notes",     title:"Fee Data & Note Sending for Parents",                deadline:"24 Apr 2026", completionDate:"", status:"pending",  priority:"critical", responsible:"Jatin Chadha",        fwgsTeam:"Mangesh, Sunanda",          notes:"",                                              cost:"" },
  { id:"academic-cal",  title:"Academic Calendar & Working Days",                    deadline:"25 Apr 2026", completionDate:"", status:"pending",  priority:"critical", responsible:"Suparna Khadepaw",    fwgsTeam:"Pradeep Sharma",            notes:"Right away",                                    cost:"" },
  { id:"ac-buses",      title:"AC Buses — Trials, Marketing & Costing",             deadline:"30 Apr 2026", completionDate:"", status:"pending",  priority:"critical", responsible:"Kiran Kante",         fwgsTeam:"Saket Kandi",               notes:"Three workstreams: Trials, Marketing, Costing", cost:"" },
  { id:"nucleus",       title:"Nucleus Implementation",                              deadline:"30 Apr 2026", completionDate:"", status:"pending",  priority:"critical", responsible:"Karan Sharma",        fwgsTeam:"Muzaffar, Mangesh",         notes:"Compare with ManageBac for academic module",    cost:"" },
  { id:"it-infra",      title:"IT Infrastructure (TV Size & Procurement)",          deadline:"15 May 2026", completionDate:"", status:"pending",  priority:"critical", responsible:"Kiran Kante",         fwgsTeam:"Muzaffar",                  notes:"24–25 TVs required",                            cost:"₹9.6–10 Lakhs (24–25 TVs × ₹40,000 avg)" },
  { id:"hr-policies",   title:"HR Policies",                                         deadline:"15 May 2026", completionDate:"", status:"pending",  priority:"critical", responsible:"Suparna Khadepaw",    fwgsTeam:"Sandeep Wagh, Ritu Chopra, Pradeep Sharma", notes:"Current no. Include admin; working days first", cost:"" },
  { id:"ambulance",     title:"Ambulance Purchase",                                  deadline:"15 May 2026", completionDate:"", status:"pending",  priority:"critical", responsible:"Kiran Kante",         fwgsTeam:"Saket Kandi",               notes:"BH registration required",                      cost:"₹7–8 Lakhs" },
  { id:"increments",    title:"Increments Work",                                     deadline:"15 May 2026", completionDate:"", status:"pending",  priority:"regular",  responsible:"Kiran Kante",         fwgsTeam:"Sandeep Wagh, Ritu Chopra", notes:"",                                              cost:"" },
  { id:"section-names", title:"Section Names",                                       deadline:"15 May 2026", completionDate:"", status:"pending",  priority:"regular",  responsible:"Karan Sharma",        fwgsTeam:"Hetal Ahivasi",             notes:"",                                              cost:"" },
  { id:"curriculum",    title:"Inset — Curriculum Alignment",                       deadline:"15 May 2026", completionDate:"", status:"pending",  priority:"regular",  responsible:"Suparna Khadepaw",    fwgsTeam:"Pradeep Sharma",            notes:"MYP/DP to move to FSK curriculum",               cost:"" },
];

let tasks = JSON.parse(JSON.stringify(TASKS));
let activeFilter = 'all';
let activeView = 'list';
let openId = null;
const TODAY = new Date('2026-04-19');

function parseDeadline(d) {
  if (!d) return null;
  const months = {jan:0,feb:1,mar:2,apr:3,may:4,jun:5,jul:6,aug:7,sep:8,oct:9,nov:10,dec:11};
  const p = d.split(' ');
  if (p.length === 3) {
    const day = parseInt(p[0]), mon = months[p[1].toLowerCase().slice(0,3)], yr = parseInt(p[2]);
    if (!isNaN(day) && mon !== undefined && !isNaN(yr)) return new Date(yr, mon, day);
  }
  return null;
}
function isUrgent(d) {
  const dt = parseDeadline(d);
  if (!dt) return false;
  return (dt - TODAY) / (1000*60*60*24) <= 5;
}
function dotClass(s){ return {pending:'dot-pending','in-progress':'dot-inprogress',done:'dot-done',blocked:'dot-blocked'}[s]||'dot-pending'; }
function badgeClass(s){ return {pending:'badge-pending','in-progress':'badge-inprogress',done:'badge-done',blocked:'badge-blocked'}[s]||'badge-pending'; }
function badgeLabel(s){ return {pending:'Pending','in-progress':'In progress',done:'Done',blocked:'Blocked'}[s]||'Pending'; }

function setView(v, btn) {
  activeView = v;
  document.querySelectorAll('.view-tab').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  render();
}
function setFilter(f, btn) {
  activeFilter = f;
  document.querySelectorAll('.filter-btn').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  render();
}
function toggleTask(id) { openId = openId===id ? null : id; render(); }
function changeStatus(id, val, e) { e.stopPropagation(); tasks.find(t=>t.id===id).status = val; render(); }

function filtered() {
  return tasks.filter(t => {
    if(activeFilter==='all') return true;
    if(activeFilter==='critical') return t.priority==='critical';
    if(activeFilter==='kiran') return t.responsible.toLowerCase().includes('kiran');
    if(activeFilter==='karan') return t.responsible.toLowerCase().includes('karan');
    if(activeFilter==='inprogress') return t.status==='in-progress';
    if(activeFilter==='done') return t.status==='done';
    return true;
  });
}

function expandHTML(task) {
  return `<div class="expand ${openId===task.id?'open':''}">
    <div class="expand-grid">
      <div><div class="expand-label">Responsible (Central)</div><div class="expand-val ${!task.responsible?'empty':''}">${task.responsible||'Not assigned'}</div></div>
      <div><div class="expand-label">Responsible (FWGS)</div><div class="expand-val ${!task.fwgsTeam?'empty':''}">${task.fwgsTeam||'Not assigned'}</div></div>
      <div><div class="expand-label">Deadline</div><div class="expand-val">${task.deadline}</div></div>
      <div><div class="expand-label">Completion date</div><div class="expand-val ${!task.completionDate?'empty':''}">${task.completionDate||'Not set'}</div></div>
      <div><div class="expand-label">Priority</div><div class="expand-val">${task.priority==='critical'?'Critical':'Regular'}</div></div>
      <div><div class="expand-label">Status</div><div class="expand-val">${badgeLabel(task.status)}</div></div>
    </div>
    ${task.notes?`<div class="notes-box">${task.notes}</div>`:''}
    ${task.cost?`<div class="cost-box"><strong>Estimated cost:</strong> ${task.cost}</div>`:''}
    <div style="margin-top:10px;display:flex;align-items:center;gap:8px;">
      <span style="font-size:11px;color:#888780;">Update status:</span>
      <select class="status-select" onchange="changeStatus('${task.id}',this.value,event)">
        <option value="pending" ${task.status==='pending'?'selected':''}>Pending</option>
        <option value="in-progress" ${task.status==='in-progress'?'selected':''}>In progress</option>
        <option value="done" ${task.status==='done'?'selected':''}>Done</option>
        <option value="blocked" ${task.status==='blocked'?'selected':''}>Blocked</option>
      </select>
    </div>
  </div>`;
}

function renderList(ft) {
  if (!ft.length) return '<div style="text-align:center;padding:32px;color:#888780">No tasks match this filter</div>';
  return `<div class="task-list">${ft.map(task=>`
    <div class="task-card-list">
      <div class="task-row" onclick="toggleTask('${task.id}')">
        <span class="dot ${dotClass(task.status)}"></span>
        <span class="task-title">${task.title}</span>
        <span class="badge ${task.priority==='critical'?'badge-critical':'badge-regular'}">${task.priority==='critical'?'Critical':'Regular'}</span>
        ${task.responsible?`<span class="badge badge-person">${task.responsible}</span>`:''}
        <span class="deadline ${isUrgent(task.deadline)?'urgent':''}">${task.deadline}</span>
        ${task.completionDate?`<span style="font-size:11px;color:#3B6D11;font-weight:500;">✓ ${task.completionDate}</span>`:''}
        <span class="badge ${badgeClass(task.status)}">${badgeLabel(task.status)}</span>
      </div>
      ${expandHTML(task)}
    </div>`).join('')}</div>`;
}

function renderCards(ft) {
  if (!ft.length) return '<div style="text-align:center;padding:32px;color:#888780">No tasks match this filter</div>';
  return `<div class="card-grid">${ft.map(task=>`
    <div class="task-card-tile" onclick="toggleTask('${task.id}')">
      <div class="tile-meta">
        <span class="badge ${task.priority==='critical'?'badge-critical':'badge-regular'}">${task.priority==='critical'?'Critical':'Regular'}</span>
        <span class="badge ${badgeClass(task.status)}">${badgeLabel(task.status)}</span>
      </div>
      <div class="tile-title">${task.title}</div>
      ${task.responsible||task.fwgsTeam?`<div class="tile-people">${[task.responsible,task.fwgsTeam].filter(Boolean).join(' · ')}</div>`:''}
      ${task.cost?`<div class="tile-cost">₹ ${task.cost}</div>`:''}
      <div class="tile-deadline ${isUrgent(task.deadline)?'urgent':''}">Due: ${task.deadline}</div>
      ${expandHTML(task)}
    </div>`).join('')}</div>`;
}

function renderTimeline(ft) {
  if (!ft.length) return '<div style="text-align:center;padding:32px;color:#888780">No tasks match this filter</div>';
  const groups = {};
  ft.forEach(t => {
    const dt = parseDeadline(t.deadline);
    const key = dt ? dt.toLocaleString('en-IN',{month:'long',year:'numeric'}) : 'TBD';
    if (!groups[key]) groups[key] = [];
    groups[key].push(t);
  });
  return `<div class="timeline-wrap">${Object.entries(groups).map(([month, gtasks])=>`
    <div class="timeline-month">
      <div class="timeline-month-label">${month}</div>
      <div class="timeline-tasks">${gtasks.map(task=>`
        <div class="timeline-task">
          <span class="timeline-date ${isUrgent(task.deadline)?'urgent':''}">${task.deadline.split(' ').slice(0,2).join(' ')}</span>
          <span class="timeline-name">${task.title}</span>
          <div class="timeline-right">
            <span class="badge ${task.priority==='critical'?'badge-critical':'badge-regular'}">${task.priority==='critical'?'Critical':'Regular'}</span>
            ${task.responsible?`<span class="badge badge-person">${task.responsible}</span>`:''}
            <span class="badge ${badgeClass(task.status)}">${badgeLabel(task.status)}</span>
          </div>
        </div>`).join('')}
      </div>
    </div>`).join('')}</div>`;
}

function renderStats() {
  const total = tasks.length;
  const critical = tasks.filter(t=>t.priority==='critical').length;
  const done = tasks.filter(t=>t.status==='done').length;
  document.getElementById('stats-bar').innerHTML = `
    <div class="stat-card"><div class="stat-label">Total tasks</div><div class="stat-value">${total}</div></div>
    <div class="stat-card"><div class="stat-label">Critical</div><div class="stat-value critical">${critical}</div></div>
    <div class="stat-card"><div class="stat-label">Est. cost</div><div class="stat-value cost">₹16.6–18 L</div></div>
    <div class="stat-card"><div class="stat-label">Completed</div><div class="stat-value done-val">${done} / ${total}</div></div>`;
}

function renderBottom() {
  const counts = {pending:0,'in-progress':0,done:0,blocked:0};
  tasks.forEach(t=>{ if(counts[t.status]!==undefined) counts[t.status]++; });
  document.getElementById('bottom-bar').innerHTML = `
    <div class="bottom-item"><span class="dot dot-pending"></span>${counts.pending} Pending</div>
    <div class="bottom-item"><span class="dot dot-inprogress"></span>${counts['in-progress']} In progress</div>
    <div class="bottom-item"><span class="dot dot-done"></span>${counts.done} Done</div>
    <div class="bottom-item"><span class="dot dot-blocked"></span>${counts.blocked} Blocked</div>
    <div class="bottom-item" style="margin-left:auto;color:#A32D2D;font-weight:500;">${tasks.filter(t=>t.priority==='critical').length} Critical</div>`;
}

function render() {
  renderStats();
  const ft = filtered();
  const main = document.getElementById('main-content');
  if (activeView==='list') main.innerHTML = renderList(ft);
  else if (activeView==='card') main.innerHTML = renderCards(ft);
  else main.innerHTML = renderTimeline(ft);
  renderBottom();
}

render();
</script>
</body>
</html>
