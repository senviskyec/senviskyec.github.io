
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>WYRMTECH // Datastream</title>
<style>
  :root{
    --bg:#0a0c0d;
    --bg-2:#101415;
    --panel:#141a1b;
    --panel-2:#1a2123;
    --line:#2a3437;
    --line-bright:#3c4a4d;
    --ink:#d9e2e3;
    --ink-dim:#7c8b8d;
    --ink-faint:#566164;
    --rust:#c8531f;
    --rust-bright:#ff6a2b;
    --rust-deep:#7a2f10;
    --gold:#d6a445;
    --cyan:#36c5c5;
    --siva:#e0451f;
    --ok:#3a9d6a;
    --warn:#d6a445;
    --danger:#c43c3c;
    --mono:'JetBrains Mono','SF Mono',ui-monospace,Menlo,Consolas,monospace;
    --sans:'Rajdhani','Segoe UI',system-ui,sans-serif;
  }
  *{box-sizing:border-box;margin:0;padding:0}
  html,body{height:100%}
  body{
    background:var(--bg);
    color:var(--ink);
    font-family:var(--sans);
    font-size:16px;
    line-height:1.6;
    overflow:hidden;
    -webkit-font-smoothing:antialiased;
  }
  /* scanline + grid texture */
  body::before{
    content:"";position:fixed;inset:0;pointer-events:none;z-index:9999;
    background:
      repeating-linear-gradient(0deg, rgba(0,0,0,0) 0px, rgba(0,0,0,0) 2px, rgba(0,0,0,.18) 3px, rgba(0,0,0,0) 4px);
    opacity:.35;mix-blend-mode:multiply;
  }
  body::after{
    content:"";position:fixed;inset:0;pointer-events:none;z-index:-1;
    background-image:
      linear-gradient(rgba(60,74,77,.05) 1px,transparent 1px),
      linear-gradient(90deg,rgba(60,74,77,.05) 1px,transparent 1px);
    background-size:44px 44px;
  }
  ::-webkit-scrollbar{width:10px;height:10px}
  ::-webkit-scrollbar-track{background:var(--bg-2)}
  ::-webkit-scrollbar-thumb{background:var(--line-bright);border:2px solid var(--bg-2)}
  ::-webkit-scrollbar-thumb:hover{background:var(--rust)}

  /* ===== LAYOUT ===== */
  #app{display:grid;grid-template-columns:268px 1fr;height:100vh}
  /* ---- sidebar ---- */
  #sidebar{
    background:linear-gradient(180deg,var(--bg-2),var(--bg));
    border-right:1px solid var(--line);
    display:flex;flex-direction:column;overflow:hidden;
  }
  .brand{
    padding:18px 18px 14px;border-bottom:1px solid var(--line);
    position:relative;
  }
  .brand .glyph{
    display:flex;align-items:center;gap:10px;
  }
  .brand .mark{
    width:34px;height:34px;flex:none;position:relative;
  }
  .brand .mark svg{display:block}
  .brand h1{
    font-size:21px;font-weight:700;letter-spacing:3px;line-height:1;
    color:var(--ink);
  }
  .brand h1 b{color:var(--rust-bright);font-weight:700}
  .brand .sub{
    font-family:var(--mono);font-size:9.5px;letter-spacing:2.5px;
    color:var(--ink-faint);margin-top:5px;text-transform:uppercase;
  }
  .brand .corner{position:absolute;top:8px;right:10px;font-family:var(--mono);font-size:8px;color:var(--rust);letter-spacing:1px}

  .nav{flex:1;overflow-y:auto;padding:10px 0}
  .nav-sec{padding:6px 18px 4px;font-family:var(--mono);font-size:9px;
    letter-spacing:2.5px;color:var(--ink-faint);text-transform:uppercase;
    display:flex;justify-content:space-between;align-items:center;margin-top:8px}
  .nav-sec .add{cursor:pointer;color:var(--ink-faint);font-size:13px;line-height:1;
    width:18px;height:18px;display:grid;place-items:center;border:1px solid transparent;border-radius:2px}
  .nav-sec .add:hover{color:var(--rust-bright);border-color:var(--line-bright)}
  .nav-item{
    display:flex;align-items:center;gap:9px;padding:7px 18px;cursor:pointer;
    font-size:15px;color:var(--ink-dim);letter-spacing:.4px;
    border-left:2px solid transparent;position:relative;transition:.12s;
  }
  .nav-item:hover{background:var(--panel);color:var(--ink)}
  .nav-item.active{background:var(--panel-2);color:var(--ink);border-left-color:var(--rust-bright)}
  .nav-item.active::after{content:"";position:absolute;right:14px;width:5px;height:5px;background:var(--rust-bright);border-radius:50%;box-shadow:0 0 6px var(--rust-bright)}
  .nav-item .ic{width:16px;flex:none;color:var(--ink-faint);font-size:14px}
  .nav-item.active .ic{color:var(--rust)}
  .nav-item .lbl{flex:1;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
  .nav-item .fac-dot{width:8px;height:8px;border-radius:1px;flex:none}
  .nav-sub{padding-left:30px;font-size:13.5px}

  .sb-foot{border-top:1px solid var(--line);padding:10px 14px;display:flex;gap:6px}
  .sb-foot button{flex:1}

  /* ---- main ---- */
  #main{overflow-y:auto;position:relative}
  .topbar{
    position:sticky;top:0;z-index:50;
    background:rgba(10,12,13,.86);backdrop-filter:blur(8px);
    border-bottom:1px solid var(--line);
    padding:11px 26px;display:flex;align-items:center;gap:16px;
  }
  .topbar .crumb{font-family:var(--mono);font-size:11px;letter-spacing:1.5px;color:var(--ink-faint);text-transform:uppercase;flex:1;display:flex;align-items:center;gap:8px;min-width:0}
  .topbar .crumb b{color:var(--rust)}
  .topbar .crumb .here{color:var(--ink);overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
  .status-chip{font-family:var(--mono);font-size:9.5px;letter-spacing:1.5px;
    padding:3px 9px;border:1px solid var(--line-bright);color:var(--ink-dim);
    display:flex;align-items:center;gap:6px;text-transform:uppercase}
  .status-chip .pulse{width:6px;height:6px;border-radius:50%;background:var(--ok);box-shadow:0 0 6px var(--ok);animation:pulse 2s infinite}
  @keyframes pulse{0%,100%{opacity:1}50%{opacity:.35}}

  .view{padding:26px 34px 90px;max-width:1080px;animation:fade .25s ease}
  @keyframes fade{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:none}}

  /* ===== TYPO / SHARED ===== */
  h2.page-title{font-size:34px;font-weight:700;letter-spacing:1px;line-height:1.05;margin-bottom:4px;color:var(--ink)}
  h2.page-title .tag{font-family:var(--mono);font-size:11px;color:var(--rust);letter-spacing:2px;display:block;margin-bottom:6px;font-weight:500}
  .page-sub{color:var(--ink-dim);font-size:15px;margin-bottom:22px;max-width:680px}
  .divider{height:1px;background:linear-gradient(90deg,var(--line-bright),transparent);margin:22px 0}
  .hdr-row{display:flex;align-items:flex-end;gap:18px;margin-bottom:18px}
  .hdr-row .grow{flex:1;min-width:0}

  /* buttons / inputs */
  button.btn{
    font-family:var(--mono);font-size:11px;letter-spacing:1.5px;text-transform:uppercase;
    background:var(--panel-2);color:var(--ink-dim);border:1px solid var(--line-bright);
    padding:8px 14px;cursor:pointer;transition:.12s;display:inline-flex;align-items:center;gap:7px;
    clip-path:polygon(7px 0,100% 0,100% calc(100% - 7px),calc(100% - 7px) 100%,0 100%,0 7px);
  }
  button.btn:hover{color:var(--ink);border-color:var(--rust);background:var(--panel)}
  button.btn.primary{background:var(--rust-deep);color:#ffd9c4;border-color:var(--rust)}
  button.btn.primary:hover{background:var(--rust);color:#fff}
  button.btn.ghost{background:transparent}
  button.btn.sm{padding:5px 9px;font-size:10px}
  button.btn.danger:hover{border-color:var(--danger);color:#ff9a9a}
  .icon-btn{background:transparent;border:1px solid transparent;color:var(--ink-faint);cursor:pointer;
    width:28px;height:28px;display:grid;place-items:center;border-radius:2px;font-size:15px}
  .icon-btn:hover{color:var(--rust-bright);border-color:var(--line-bright)}

  input.f,textarea.f,select.f{
    width:100%;background:var(--bg-2);border:1px solid var(--line);color:var(--ink);
    font-family:var(--sans);font-size:15px;padding:9px 12px;outline:none;transition:.12s;
  }
  input.f:focus,textarea.f:focus,select.f:focus{border-color:var(--rust);background:var(--panel)}
  textarea.f{resize:vertical;line-height:1.6;min-height:90px;font-size:15px}
  label.fl{font-family:var(--mono);font-size:9.5px;letter-spacing:2px;color:var(--ink-faint);
    text-transform:uppercase;display:block;margin-bottom:5px}
  .field{margin-bottom:16px}
  .field-row{display:grid;grid-template-columns:1fr 1fr;gap:14px}

  /* cards */
  .card{background:var(--panel);border:1px solid var(--line);position:relative}
  .card.notch{clip-path:polygon(0 0,calc(100% - 14px) 0,100% 14px,100% 100%,0 100%)}
  .card .card-tab{position:absolute;top:0;right:0;width:14px;height:14px;background:var(--rust);clip-path:polygon(0 0,100% 0,100% 100%)}

  /* dashboard */
  .stat-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:12px;margin-bottom:26px}
  .stat{background:var(--panel);border:1px solid var(--line);padding:14px 16px;position:relative;overflow:hidden}
  .stat::before{content:"";position:absolute;left:0;top:0;bottom:0;width:3px;background:var(--rust)}
  .stat .k{font-family:var(--mono);font-size:9px;letter-spacing:2px;color:var(--ink-faint);text-transform:uppercase}
  .stat .v{font-size:32px;font-weight:700;line-height:1.1;margin-top:4px;color:var(--ink)}
  .stat .d{font-size:12px;color:var(--ink-dim)}
  .stat.alt::before{background:var(--cyan)}
  .stat.alt2::before{background:var(--gold)}

  .panel-h{font-family:var(--mono);font-size:10px;letter-spacing:2.5px;color:var(--ink-faint);
    text-transform:uppercase;margin:24px 0 12px;display:flex;align-items:center;gap:10px}
  .panel-h::after{content:"";flex:1;height:1px;background:var(--line)}

  .tile-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(230px,1fr));gap:14px}
  .tile{background:var(--panel);border:1px solid var(--line);padding:16px;cursor:pointer;
    transition:.14s;position:relative;overflow:hidden}
  .tile:hover{border-color:var(--rust);transform:translateY(-2px)}
  .tile:hover .tile-go{opacity:1;transform:translateX(0)}
  .tile .tile-go{position:absolute;top:14px;right:14px;color:var(--rust);opacity:0;transform:translateX(-4px);transition:.14s;font-family:var(--mono);font-size:16px}
  .tile .tt{font-size:18px;font-weight:600;letter-spacing:.5px;margin-bottom:3px;padding-right:20px}
  .tile .tm{font-family:var(--mono);font-size:9px;letter-spacing:1.5px;color:var(--ink-faint);text-transform:uppercase;margin-bottom:8px}
  .tile .td{font-size:13.5px;color:var(--ink-dim);line-height:1.5;display:-webkit-box;-webkit-line-clamp:3;-webkit-box-orient:vertical;overflow:hidden}
  .tile .tbar{height:4px;background:var(--bg-2);margin-top:12px;position:relative;overflow:hidden}
  .tile .tbar i{position:absolute;left:0;top:0;bottom:0;background:var(--rust)}

  /* badges */
  .badge{font-family:var(--mono);font-size:9px;letter-spacing:1.5px;text-transform:uppercase;
    padding:3px 8px;border:1px solid;display:inline-flex;align-items:center;gap:5px;line-height:1}
  .badge.b-plan{color:var(--ink-dim);border-color:var(--line-bright)}
  .badge.b-prog{color:var(--gold);border-color:var(--gold)}
  .badge.b-done{color:var(--ok);border-color:var(--ok)}
  .badge.b-hold{color:var(--danger);border-color:var(--danger)}

  /* lore editor */
  .editor-wrap{display:grid;grid-template-columns:1fr;gap:0}
  .md-body{font-size:16px;line-height:1.75;color:var(--ink)}
  .md-body h1{font-size:27px;font-weight:700;letter-spacing:.5px;margin:18px 0 10px;color:var(--ink)}
  .md-body h2{font-size:21px;font-weight:600;margin:20px 0 8px;color:var(--ink);border-bottom:1px solid var(--line);padding-bottom:5px}
  .md-body h3{font-size:17px;font-weight:600;margin:16px 0 6px;color:var(--rust-bright);font-family:var(--mono);letter-spacing:1px;text-transform:uppercase}
  .md-body p{margin:10px 0;color:var(--ink)}
  .md-body ul,.md-body ol{margin:10px 0 10px 22px}
  .md-body li{margin:4px 0}
  .md-body a{color:var(--cyan);text-decoration:none;border-bottom:1px dashed var(--cyan)}
  .md-body code{font-family:var(--mono);background:var(--bg-2);padding:1px 6px;font-size:13px;color:var(--gold);border:1px solid var(--line)}
  .md-body blockquote{border-left:3px solid var(--rust);padding:6px 16px;margin:14px 0;color:var(--ink-dim);background:var(--panel);font-style:italic}
  .md-body hr{border:none;height:1px;background:var(--line-bright);margin:18px 0}
  .md-body img{max-width:100%;border:1px solid var(--line);margin:10px 0;display:block}
  .md-body strong{color:var(--ink);font-weight:600}
  .md-body table{border-collapse:collapse;margin:14px 0;width:100%}
  .md-body th,.md-body td{border:1px solid var(--line);padding:7px 12px;text-align:left;font-size:14px}
  .md-body th{background:var(--panel-2);font-family:var(--mono);font-size:11px;letter-spacing:1px;text-transform:uppercase;color:var(--ink-dim)}

  /* district builder */
  .dist-head{display:flex;gap:20px;align-items:flex-start;margin-bottom:8px}
  .dist-head .cover{width:200px;height:130px;flex:none;border:1px solid var(--line);background:var(--bg-2) center/cover;
    display:grid;place-items:center;color:var(--ink-faint);font-family:var(--mono);font-size:10px;cursor:pointer;position:relative;overflow:hidden}
  .dist-head .cover:hover{border-color:var(--rust)}
  .dist-head .cover .ov{position:absolute;inset:0;background:rgba(10,12,13,.7);display:grid;place-items:center;opacity:0;transition:.15s;font-size:11px;letter-spacing:1px}
  .dist-head .cover:hover .ov{opacity:1}

  .bld-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:16px;margin-top:14px}
  .bld{background:var(--panel);border:1px solid var(--line);overflow:hidden;display:flex;flex-direction:column}
  .bld .bld-img{height:150px;background:var(--bg-2) center/cover;position:relative;display:grid;place-items:center;color:var(--ink-faint);cursor:pointer;border-bottom:1px solid var(--line)}
  .bld .bld-img .ph{font-family:var(--mono);font-size:10px;letter-spacing:1px;text-align:center}
  .bld .bld-img .ph i{font-size:24px;display:block;margin-bottom:4px}
  .bld .bld-img .imgcount{position:absolute;bottom:6px;right:6px;font-family:var(--mono);font-size:9px;background:rgba(10,12,13,.8);padding:2px 7px;border:1px solid var(--line-bright);color:var(--ink-dim)}
  .bld .bld-body{padding:13px 15px;flex:1;display:flex;flex-direction:column}
  .bld .bld-name{font-size:17px;font-weight:600;letter-spacing:.4px;display:flex;justify-content:space-between;align-items:start;gap:8px}
  .bld .bld-meta{font-family:var(--mono);font-size:9px;letter-spacing:1px;color:var(--ink-faint);text-transform:uppercase;margin:4px 0 8px}
  .bld .bld-notes{font-size:13.5px;color:var(--ink-dim);line-height:1.55;flex:1;white-space:pre-wrap}
  .bld .bld-foot{display:flex;gap:6px;margin-top:12px;align-items:center}
  .bld .bld-foot .sp{flex:1}

  .thumbstrip{display:flex;gap:6px;flex-wrap:wrap;margin-top:10px}
  .thumbstrip img{width:54px;height:54px;object-fit:cover;border:1px solid var(--line);cursor:pointer}
  .thumbstrip .addt{width:54px;height:54px;border:1px dashed var(--line-bright);display:grid;place-items:center;color:var(--ink-faint);cursor:pointer;font-size:18px}
  .thumbstrip .addt:hover{border-color:var(--rust);color:var(--rust)}

  /* faction */
  .fac-banner{padding:22px 26px;border:1px solid var(--line);position:relative;overflow:hidden;margin-bottom:20px}
  .fac-banner::before{content:"";position:absolute;left:0;top:0;bottom:0;width:6px}
  .fac-banner .fn{font-size:30px;font-weight:700;letter-spacing:2px}
  .fac-banner .fm{font-family:var(--mono);font-size:11px;letter-spacing:2px;text-transform:uppercase;color:var(--ink-dim);margin-top:4px}

  /* modal */
  .modal-bg{position:fixed;inset:0;background:rgba(5,7,8,.78);backdrop-filter:blur(3px);z-index:1000;display:grid;place-items:center;padding:20px;animation:fade .15s}
  .modal{background:var(--bg-2);border:1px solid var(--line-bright);max-width:560px;width:100%;max-height:88vh;overflow-y:auto;position:relative;
    clip-path:polygon(0 0,calc(100% - 18px) 0,100% 18px,100% 100%,18px 100%,0 calc(100% - 18px))}
  .modal-h{padding:16px 22px;border-bottom:1px solid var(--line);display:flex;align-items:center;justify-content:space-between;position:sticky;top:0;background:var(--bg-2);z-index:2}
  .modal-h .mt{font-family:var(--mono);font-size:13px;letter-spacing:2px;text-transform:uppercase;color:var(--ink)}
  .modal-h .mt b{color:var(--rust)}
  .modal-body{padding:20px 22px}
  .modal-foot{padding:14px 22px;border-top:1px solid var(--line);display:flex;gap:10px;justify-content:flex-end}

  /* image lightbox */
  .lightbox{position:fixed;inset:0;background:rgba(3,4,5,.92);z-index:2000;display:grid;place-items:center;padding:30px;cursor:zoom-out}
  .lightbox img{max-width:92%;max-height:90%;border:1px solid var(--line-bright)}

  .empty{text-align:center;padding:50px 20px;color:var(--ink-faint)}
  .empty i{font-size:40px;opacity:.5}
  .empty p{font-family:var(--mono);font-size:11px;letter-spacing:1px;margin-top:10px}

  .toast{position:fixed;bottom:22px;left:50%;transform:translateX(-50%) translateY(80px);
    background:var(--panel-2);border:1px solid var(--rust);color:var(--ink);
    font-family:var(--mono);font-size:11px;letter-spacing:1px;padding:10px 18px;z-index:3000;
    transition:.3s;text-transform:uppercase;display:flex;align-items:center;gap:8px}
  .toast.show{transform:translateX(-50%) translateY(0)}
  .toast .pulse{width:6px;height:6px;border-radius:50%;background:var(--rust-bright)}

  .seg{display:inline-flex;border:1px solid var(--line-bright)}
  .seg button{background:transparent;border:none;color:var(--ink-dim);font-family:var(--mono);font-size:10px;letter-spacing:1.5px;padding:6px 13px;cursor:pointer;text-transform:uppercase}
  .seg button.on{background:var(--rust-deep);color:#ffd9c4}

  .tagline{font-family:var(--mono);font-size:10px;color:var(--ink-faint);letter-spacing:1px;line-height:1.7}
  .hl{color:var(--rust-bright)}
  .meta-line{font-family:var(--mono);font-size:10px;letter-spacing:1px;color:var(--ink-faint);text-transform:uppercase}
</style>
</head>
<body>
<div id="app">
  <!-- SIDEBAR -->
  <aside id="sidebar">
    <div class="brand">
      <span class="corner">v2.7.1</span>
      <div class="glyph">
        <div class="mark">
          <svg width="34" height="34" viewBox="0 0 34 34" fill="none">
            <path d="M17 2 L31 10 L31 24 L17 32 L3 24 L3 10 Z" stroke="#ff6a2b" stroke-width="1.4" fill="none"/>
            <path d="M17 8 L25.5 13 L25.5 21 L17 26 L8.5 21 L8.5 13 Z" stroke="#3c4a4d" stroke-width="1"/>
            <circle cx="17" cy="17" r="3.4" fill="#ff6a2b"/>
            <path d="M17 2 L17 8 M31 10 L25.5 13 M3 10 L8.5 13 M17 32 L17 26 M31 24 L25.5 21 M3 24 L8.5 21" stroke="#3c4a4d" stroke-width=".8"/>
          </svg>
        </div>
        <div>
          <h1>WYRM<b>TECH</b></h1>
          <div class="sub">Datastream // Archive</div>
        </div>
      </div>
    </div>
    <nav class="nav" id="nav"></nav>
    <div class="sb-foot">
      <button class="btn sm ghost" onclick="exportData()" title="Export backup JSON"><i class="ti ti-download"></i>Export</button>
      <button class="btn sm ghost" onclick="document.getElementById('importFile').click()" title="Import backup"><i class="ti ti-upload"></i>Import</button>
      <input type="file" id="importFile" accept="application/json" style="display:none" onchange="importData(event)">
    </div>
  </aside>

  <!-- MAIN -->
  <section id="main">
    <div class="topbar">
      <div class="crumb"><b>WYRMTECH</b> <span style="color:var(--ink-faint)">/</span> <span class="here" id="crumb">DASHBOARD</span></div>
      <div class="status-chip"><span class="pulse"></span> Link Stable</div>
      <div class="status-chip" id="saveChip"><i class="ti ti-device-floppy" style="font-size:11px"></i> Saved</div>
    </div>
    <div class="view" id="view"></div>
  </section>
</div>

<div id="modalRoot"></div>
<div id="lightboxRoot"></div>
<div class="toast" id="toast"><span class="pulse"></span><span id="toastMsg"></span></div>

<!-- Tabler icons -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@3.7.0/dist/tabler-icons.min.css">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Rajdhani:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">

<script>
/* =========================================================
   WYRMTECH DATASTREAM  —  single-file lore wiki + build tracker
   Storage: localStorage (key: wyrmtech_db_v1) + JSON export/import
   ========================================================= */

const LS_KEY = 'wyrmtech_db_v1';

/* ---------- seed data ---------- */
function seedDB(){
  return {
    meta:{ project:'WyrmTech World', updated:Date.now() },
    lore:[
      { id:uid(), title:'The Premise', icon:'ti-broadcast', updated:Date.now(),
        body:`# The Premise\n\nYou put on the headset. The boot sequence floods your retinas — a wireframe hexagon spinning in the dark — and then you are **somewhere else**.\n\nThe world is called the **Datastream**. To the operators who built it, it is a sealed simulation: an entire modded Minecraft reality humming on hardware nobody outside the project has ever seen. To the people *inside* it, it is simply the world. The only world.\n\n> "We did not build a game. We built a place, and then we forgot the door home."\n\n## What the headset hides\n\nMost visitors never learn that the seams are showing. The terrain remembers being generated. The mobs of Alex's menagerie behave like they were *authored*. And under everything, the great clanking lattice of **Create** machinery turns — gearboxes, flywheels, encased chains — as if the simulation is physically holding itself together with brass and stress units.\n\n## The factions\n\nThe Datastream is not empty. Power inside it has organized. The dominant force — the one writing the rules now — is **WyrmTech**.`},
      { id:uid(), title:'WyrmTech Doctrine', icon:'ti-shield-bolt', updated:Date.now(),
        body:`# WyrmTech Doctrine\n\nWyrmTech believes the Datastream is **incomplete**, and that completion is an engineering problem.\n\n## The Lattice\n\nAt the core of WyrmTech ideology is *the Lattice* — a self-replicating nanotech-analog woven from Create rotational logic and aeronautic salvage. The Lattice does not destroy. It **assimilates**: it takes a chunk, studies its stress, and rebuilds it stronger, denser, more *intentional*.\n\n### Three Tenets\n\n1. **Density is virtue.** A region fully built is a region fully understood.\n2. **The hand-built decays. The self-built endures.** Automation over toil.\n3. **The Wyrm does not sleep.** Expansion never stops.\n\n## Aesthetic\n\nBurnt orange and gunmetal. Hexagonal plating. Exposed gearwork that *should* be hidden but is shown deliberately, like scar tissue worn as armor.`},
      { id:uid(), title:'The Aeronautic War', icon:'ti-plane', updated:Date.now(),
        body:`# The Aeronautic War\n\nBefore WyrmTech consolidated the surface, the skies belonged to no one — and then to everyone, violently.\n\nWith **Create: Aeronautics** salvage, rival crews lifted whole foundries into the clouds. Airship against airship, physics-driven hulls grinding through contested altitude.\n\n## Outcome\n\nWyrmTech did not win the war by having the best ships. It won by **building the sky itself** — anchoring Lattice spires high enough that no free-flying crew could pass without docking, paying, and eventually, joining.\n\n*(Write your battles, treaties, and aces here.)*`},
    ],
    factions:[
      { id:uid(), name:'WyrmTech', color:'#ff6a2b', motto:'The Wyrm does not sleep.', tag:'Hegemon // Player Faction',
        body:`# WyrmTech\n\n**The faction you lead.** Braytech-by-way-of-SIVA: a techno-industrial hegemon that treats unbuilt land as an unsolved equation.\n\n## Leadership\n- *(your operator handle here)*\n\n## Holdings\n- Industrial District (primary forge)\n- The Spine (Lattice backbone)\n\n## Signature tech\n- Self-replicating Lattice assemblers\n- Aeronautic anchor-spires\n- Hexplate construction standard`,
        updated:Date.now() },
      { id:uid(), name:'The Hollowkin', color:'#36c5c5', motto:'We remember the door.', tag:'Resistance // Subterranean',
        body:`# The Hollowkin\n\nDwellers of **Alex's Caves** — the biolume deeps, the magnetic caves, the abyssal dark. They reject the Lattice and believe the simulation *wants* to stay wild.\n\nThey know things about the underworld WyrmTech's surveyors fear to log.`, updated:Date.now() },
      { id:uid(), name:'Free Skydock Crews', color:'#d6a445', motto:'Altitude is freedom.', tag:'Independents // Aerial',
        body:`# Free Skydock Crews\n\nThe last unaligned airship crews from the Aeronautic War. They run salvage and contraband between the anchor-spires, dodging WyrmTech tariffs.`, updated:Date.now() },
    ],
    districts:[
      { id:uid(), name:'Industrial District', status:'prog', cover:'', faction:'WyrmTech',
        summary:'The primary WyrmTech forge-sprawl. Where raw chunks become Lattice. Heavy Create automation, conveyor arteries, smokestacks venting in hexagonal rhythm.',
        buildings:[
          { id:uid(), name:'The Great Foundry', status:'prog', tags:'Create · Mechanical', notes:'Central smeltery hall. Multiblock mixers + blaze burners on a rotational bus. Needs second floor for casting basins.', images:[] },
          { id:uid(), name:'Conveyor Spine A', status:'done', tags:'Logistics', notes:'Main item artery running N-S. Funnels + belts feeding the foundry. DONE — leave as reference for Spine B.', images:[] },
          { id:uid(), name:'Stress Substation 01', status:'plan', tags:'Power', notes:'Flywheel + furnace engine bank to feed the district. Plan capacity for full district load before building.', images:[] },
        ],
        updated:Date.now() },
      { id:uid(), name:'The Skydock Spire', status:'plan', cover:'', faction:'WyrmTech',
        summary:'Aeronautic anchor-spire. Vertical district climbing into contested airspace. Docking clamps for airships, Lattice growing upward.',
        buildings:[
          { id:uid(), name:'Anchor Clamp Ring', status:'plan', tags:'Aeronautics', notes:'Rotating dock clamps using Create bearings. Reference real-world container cranes.', images:[] },
        ], updated:Date.now() },
    ],
    settings:{ accent:'#ff6a2b' }
  };
}

/* ---------- util ---------- */
function uid(){ return 'x'+Math.random().toString(36).slice(2,9)+Date.now().toString(36).slice(-3); }
function now(){ return Date.now(); }
function ago(ts){
  const s=(Date.now()-ts)/1000;
  if(s<60)return 'just now';
  if(s<3600)return Math.floor(s/60)+'m ago';
  if(s<86400)return Math.floor(s/3600)+'h ago';
  return Math.floor(s/86400)+'d ago';
}
function esc(s){return (s||'').replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));}

/* ---------- state ---------- */
let DB = load();
let route = { view:'dashboard', id:null };

function load(){
  try{ const r=localStorage.getItem(LS_KEY); if(r) return JSON.parse(r); }catch(e){}
  const s=seedDB(); save(s); return s;
}
function save(db){
  db = db||DB; db.meta.updated=Date.now();
  try{ localStorage.setItem(LS_KEY, JSON.stringify(db)); flashSaved(); }
  catch(e){ toast('Storage full — export & trim images'); }
}
let saveTimer;
function saveDebounced(){ clearTimeout(saveTimer); saveTimer=setTimeout(()=>save(),400); chipSaving(); }
function chipSaving(){ const c=document.getElementById('saveChip'); c.innerHTML='<i class="ti ti-loader-2" style="font-size:11px"></i> Saving'; }
function flashSaved(){ const c=document.getElementById('saveChip'); c.innerHTML='<i class="ti ti-device-floppy" style="font-size:11px"></i> Saved'; }

/* ---------- tiny markdown ---------- */
function md(src){
  if(!src) return '<p style="color:var(--ink-faint)">No content yet.</p>';
  let lines = src.replace(/\r/g,'').split('\n');
  let out=[], i=0, inList=false, listType='';
  const inline = t => esc(t)
    .replace(/\*\*([^*]+)\*\*/g,'<strong>$1</strong>')
    .replace(/(^|[^*])\*([^*]+)\*/g,'$1<em>$2</em>')
    .replace(/`([^`]+)`/g,'<code>$1</code>')
    .replace(/\[([^\]]+)\]\(([^)]+)\)/g,'<a href="$2" target="_blank" rel="noopener">$1</a>')
    .replace(/!IMG\(([^)]+)\)/g,'<img src="$1" loading="lazy">');
  const closeList=()=>{ if(inList){ out.push(listType==='ol'?'</ol>':'</ul>'); inList=false; } };
  for(i=0;i<lines.length;i++){
    let l=lines[i];
    if(/^\s*$/.test(l)){ closeList(); continue; }
    if(/^### /.test(l)){ closeList(); out.push('<h3>'+inline(l.slice(4))+'</h3>'); continue; }
    if(/^## /.test(l)){ closeList(); out.push('<h2>'+inline(l.slice(3))+'</h2>'); continue; }
    if(/^# /.test(l)){ closeList(); out.push('<h1>'+inline(l.slice(2))+'</h1>'); continue; }
    if(/^> /.test(l)){ closeList(); out.push('<blockquote>'+inline(l.slice(2))+'</blockquote>'); continue; }
    if(/^(---|___|\*\*\*)\s*$/.test(l)){ closeList(); out.push('<hr>'); continue; }
    if(/^\s*[-*] /.test(l)){ if(!inList||listType!=='ul'){closeList();out.push('<ul>');inList=true;listType='ul';} out.push('<li>'+inline(l.replace(/^\s*[-*] /,''))+'</li>'); continue; }
    if(/^\s*\d+\. /.test(l)){ if(!inList||listType!=='ol'){closeList();out.push('<ol>');inList=true;listType='ol';} out.push('<li>'+inline(l.replace(/^\s*\d+\. /,''))+'</li>'); continue; }
    out.push('<p>'+inline(l)+'</p>');
  }
  closeList();
  return out.join('');
}

/* ---------- status helpers ---------- */
const STATUS={ plan:{l:'Planned',c:'b-plan'}, prog:{l:'In Progress',c:'b-prog'}, done:{l:'Built',c:'b-done'}, hold:{l:'On Hold',c:'b-hold'} };
function statusBadge(s){ const o=STATUS[s]||STATUS.plan; return `<span class="badge ${o.c}">${o.l}</span>`; }
function progressOf(d){ const b=d.buildings||[]; if(!b.length)return 0; const w={plan:0,hold:.15,prog:.5,done:1}; return Math.round(b.reduce((a,x)=>a+(w[x.status]||0),0)/b.length*100); }

/* =========================================================
   NAV
   ========================================================= */
function renderNav(){
  const nav=document.getElementById('nav');
  let h='';
  h+=`<div class="nav-item ${route.view==='dashboard'?'active':''}" onclick="go('dashboard')">
        <i class="ti ti-layout-dashboard ic"></i><span class="lbl">Dashboard</span></div>`;

  h+=`<div class="nav-sec">Lore Archive <span class="add" onclick="event.stopPropagation();newLore()" title="New lore page">+</span></div>`;
  DB.lore.forEach(p=>{
    h+=`<div class="nav-item nav-sub ${route.view==='lore'&&route.id===p.id?'active':''}" onclick="go('lore','${p.id}')">
          <i class="ti ${p.icon||'ti-file-text'} ic"></i><span class="lbl">${esc(p.title)}</span></div>`;
  });

  h+=`<div class="nav-sec">Factions <span class="add" onclick="event.stopPropagation();newFaction()" title="New faction">+</span></div>`;
  DB.factions.forEach(f=>{
    h+=`<div class="nav-item nav-sub ${route.view==='faction'&&route.id===f.id?'active':''}" onclick="go('faction','${f.id}')">
          <span class="fac-dot" style="background:${f.color}"></span><span class="lbl">${esc(f.name)}</span></div>`;
  });

  h+=`<div class="nav-sec">Districts <span class="add" onclick="event.stopPropagation();newDistrict()" title="New district">+</span></div>`;
  DB.districts.forEach(d=>{
    h+=`<div class="nav-item nav-sub ${route.view==='district'&&route.id===d.id?'active':''}" onclick="go('district','${d.id}')">
          <i class="ti ti-building-factory-2 ic"></i><span class="lbl">${esc(d.name)}</span></div>`;
  });
  nav.innerHTML=h;
}

function go(view,id){ route={view,id:id||null}; renderNav(); render(); document.getElementById('main').scrollTop=0; }

/* =========================================================
   RENDER ROUTER
   ========================================================= */
function render(){
  const v=document.getElementById('view');
  const crumb=document.getElementById('crumb');
  if(route.view==='dashboard'){ crumb.textContent='DASHBOARD'; v.innerHTML=renderDashboard(); }
  else if(route.view==='lore'){ const p=DB.lore.find(x=>x.id===route.id); crumb.textContent='LORE / '+(p?p.title:''); v.innerHTML=renderLore(p); }
  else if(route.view==='faction'){ const f=DB.factions.find(x=>x.id===route.id); crumb.textContent='FACTION / '+(f?f.name:''); v.innerHTML=renderFaction(f); }
  else if(route.view==='district'){ const d=DB.districts.find(x=>x.id===route.id); crumb.textContent='DISTRICT / '+(d?d.name:''); v.innerHTML=renderDistrict(d); }
}

/* ---------- DASHBOARD ---------- */
function renderDashboard(){
  const totalBld=DB.districts.reduce((a,d)=>a+(d.buildings?.length||0),0);
  const doneBld=DB.districts.reduce((a,d)=>a+(d.buildings||[]).filter(b=>b.status==='done').length,0);
  let h=`
    <h2 class="page-title"><span class="tag">// OPERATOR DASHBOARD</span>The Datastream</h2>
    <p class="page-sub">Live archive of the WyrmTech world. Lore, factions, and district build-plans — all stored locally and ready to export. The Wyrm does not sleep.</p>

    <div class="stat-grid">
      <div class="stat"><div class="k">Lore Pages</div><div class="v">${DB.lore.length}</div><div class="d">archive entries</div></div>
      <div class="stat alt"><div class="k">Factions</div><div class="v">${DB.factions.length}</div><div class="d">powers in play</div></div>
      <div class="stat alt2"><div class="k">Districts</div><div class="v">${DB.districts.length}</div><div class="d">planned zones</div></div>
      <div class="stat"><div class="k">Builds Logged</div><div class="v">${totalBld}</div><div class="d">${doneBld} complete</div></div>
    </div>

    <div class="panel-h">District Build Status</div>
    <div class="tile-grid">`;
  if(!DB.districts.length){ h+=`<div class="empty"><i class="ti ti-building-factory-2"></i><p>No districts yet</p></div>`; }
  DB.districts.forEach(d=>{
    const p=progressOf(d); const fac=DB.factions.find(f=>f.name===d.faction);
    h+=`<div class="tile" onclick="go('district','${d.id}')" ${d.cover?`style="background:linear-gradient(rgba(20,26,27,.82),rgba(20,26,27,.92)),url('${d.cover}') center/cover"`:''}>
          <span class="tile-go">→</span>
          <div class="tm">${fac?esc(fac.name):'Unaligned'} · ${(d.buildings?.length||0)} builds</div>
          <div class="tt">${esc(d.name)}</div>
          <div class="td">${esc(d.summary||'No summary.')}</div>
          <div style="margin-top:10px">${statusBadge(d.status)}</div>
          <div class="tbar"><i style="width:${p}%;background:${fac?fac.color:'var(--rust)'}"></i></div>
          <div class="meta-line" style="margin-top:6px">${p}% built</div>
        </div>`;
  });
  h+=`</div>

    <div class="panel-h">Recent Lore</div>
    <div class="tile-grid">`;
  [...DB.lore].sort((a,b)=>b.updated-a.updated).slice(0,4).forEach(p=>{
    h+=`<div class="tile" onclick="go('lore','${p.id}')">
          <span class="tile-go">→</span>
          <div class="tm"><i class="ti ${p.icon||'ti-file-text'}"></i> ${ago(p.updated)}</div>
          <div class="tt">${esc(p.title)}</div>
          <div class="td">${esc(p.body.replace(/[#*>`\-]/g,'').slice(0,140))}</div>
        </div>`;
  });
  h+=`</div>`;
  return h;
}

/* ---------- LORE ---------- */
function renderLore(p){
  if(!p) return `<div class="empty"><i class="ti ti-file-off"></i><p>Page not found</p></div>`;
  return `
    <div class="hdr-row">
      <div class="grow">
        <h2 class="page-title"><span class="tag">// LORE ARCHIVE · ${ago(p.updated)}</span>${esc(p.title)}</h2>
      </div>
      <button class="btn" onclick="editLore('${p.id}')"><i class="ti ti-edit"></i>Edit</button>
      <button class="btn danger ghost" onclick="delLore('${p.id}')"><i class="ti ti-trash"></i></button>
    </div>
    <div class="divider"></div>
    <div class="md-body" onclick="lightboxDelegate(event)">${md(p.body)}</div>`;
}

/* ---------- FACTION ---------- */
function renderFaction(f){
  if(!f) return `<div class="empty"><i class="ti ti-flag-off"></i><p>Faction not found</p></div>`;
  const dists=DB.districts.filter(d=>d.faction===f.name);
  return `
    <div class="fac-banner card" style="border-color:${f.color}55">
      <span style="position:absolute;left:0;top:0;bottom:0;width:6px;background:${f.color}"></span>
      <div style="display:flex;justify-content:space-between;align-items:flex-start;gap:16px">
        <div>
          <div class="fn" style="color:${f.color}">${esc(f.name)}</div>
          <div class="fm">${esc(f.tag||'')}</div>
          <div class="tagline" style="margin-top:10px;font-style:italic;color:var(--ink-dim)">&ldquo;${esc(f.motto||'')}&rdquo;</div>
        </div>
        <div style="display:flex;gap:6px">
          <button class="btn" onclick="editFaction('${f.id}')"><i class="ti ti-edit"></i>Edit</button>
          <button class="btn danger ghost" onclick="delFaction('${f.id}')"><i class="ti ti-trash"></i></button>
        </div>
      </div>
    </div>
    <div class="md-body" onclick="lightboxDelegate(event)">${md(f.body)}</div>
    ${dists.length?`<div class="panel-h">Holdings</div><div class="tile-grid">${dists.map(d=>`
      <div class="tile" onclick="go('district','${d.id}')"><span class="tile-go">→</span>
        <div class="tm">${(d.buildings?.length||0)} builds · ${progressOf(d)}%</div>
        <div class="tt">${esc(d.name)}</div><div class="td">${esc(d.summary||'')}</div>
        <div style="margin-top:8px">${statusBadge(d.status)}</div></div>`).join('')}</div>`:''}`;
}

/* ---------- DISTRICT (the build tracker) ---------- */
function renderDistrict(d){
  if(!d) return `<div class="empty"><i class="ti ti-building-off"></i><p>District not found</p></div>`;
  const fac=DB.factions.find(f=>f.name===d.faction);
  const p=progressOf(d);
  let h=`
    <div class="hdr-row">
      <div class="grow">
        <h2 class="page-title"><span class="tag">// DISTRICT PLAN · ${(d.buildings?.length||0)} structures</span>${esc(d.name)}</h2>
      </div>
      <button class="btn" onclick="editDistrict('${d.id}')"><i class="ti ti-edit"></i>Edit</button>
      <button class="btn danger ghost" onclick="delDistrict('${d.id}')"><i class="ti ti-trash"></i></button>
    </div>

    <div class="dist-head">
      <div class="cover" style="${d.cover?`background-image:url('${d.cover}')`:''}" onclick="pickImage(url=>{setDistrictCover('${d.id}',url)})">
        ${d.cover?'':'<span><i class="ti ti-photo" style="font-size:22px;display:block;text-align:center;margin-bottom:4px"></i>SET COVER</span>'}
        <div class="ov"><i class="ti ti-camera"></i>&nbsp;change cover</div>
      </div>
      <div style="flex:1;min-width:0">
        <div style="display:flex;gap:8px;align-items:center;margin-bottom:8px;flex-wrap:wrap">
          ${statusBadge(d.status)}
          ${fac?`<span class="badge" style="color:${fac.color};border-color:${fac.color}">${esc(fac.name)}</span>`:''}
        </div>
        <p style="color:var(--ink);font-size:15px;line-height:1.6">${esc(d.summary||'No summary yet — hit Edit to describe this district.')}</p>
        <div style="margin-top:14px">
          <div class="meta-line">Build Progress · ${p}%</div>
          <div class="tile" style="cursor:default;padding:0;margin-top:6px;background:var(--bg-2)"><div class="tbar" style="height:8px;margin:0"><i style="width:${p}%;background:${fac?fac.color:'var(--rust)'}"></i></div></div>
        </div>
      </div>
    </div>

    <div class="panel-h">Structures &amp; Build Plans
      <button class="btn sm primary" style="margin-left:auto" onclick="newBuilding('${d.id}')"><i class="ti ti-plus"></i>Add Building</button>
    </div>`;

  if(!d.buildings||!d.buildings.length){
    h+=`<div class="empty"><i class="ti ti-stack-2"></i><p>No structures planned — add your first build</p></div>`;
  } else {
    h+=`<div class="bld-grid">`;
    d.buildings.forEach(b=>{
      const cover=b.images&&b.images.length?b.images[0]:'';
      h+=`<div class="bld">
        <div class="bld-img" style="${cover?`background-image:url('${cover}')`:''}" onclick="${cover?`openLightbox('${cover}')`:`addBuildingImage('${d.id}','${b.id}')`}">
          ${cover?'':`<div class="ph"><i class="ti ti-photo"></i>add reference</div>`}
          ${b.images&&b.images.length>1?`<span class="imgcount">+${b.images.length-1} more</span>`:''}
        </div>
        <div class="bld-body">
          <div class="bld-name">${esc(b.name)}</div>
          <div class="bld-meta">${esc(b.tags||'untagged')}</div>
          <div style="margin-bottom:8px">${statusBadge(b.status)}</div>
          <div class="bld-notes">${esc(b.notes||'No build notes yet.')}</div>
          <div class="thumbstrip">
            ${(b.images||[]).map((im,idx)=>`<img src="${im}" onclick="openLightbox('${im}')" title="reference ${idx+1}">`).join('')}
            <div class="addt" onclick="addBuildingImage('${d.id}','${b.id}')" title="add reference image">+</div>
          </div>
          <div class="bld-foot">
            <div class="sp"></div>
            <button class="icon-btn" onclick="editBuilding('${d.id}','${b.id}')" title="edit"><i class="ti ti-edit"></i></button>
            <button class="icon-btn" onclick="delBuilding('${d.id}','${b.id}')" title="delete"><i class="ti ti-trash"></i></button>
          </div>
        </div>
      </div>`;
    });
    h+=`</div>`;
  }
  return h;
}

/* =========================================================
   IMAGE handling (file -> dataURL, or paste URL)
   ========================================================= */
function pickImage(cb){
  modal({
    title:'Add <b>Image</b>',
    body:`
      <div class="field">
        <label class="fl">Upload from device</label>
        <input type="file" accept="image/*" class="f" id="imgFile">
        <div class="meta-line" style="margin-top:6px">Stored locally as data — large images bloat the file. Keep under ~1MB each.</div>
      </div>
      <div style="text-align:center;font-family:var(--mono);font-size:10px;color:var(--ink-faint);margin:6px 0">— OR —</div>
      <div class="field">
        <label class="fl">Paste image URL</label>
        <input type="text" class="f" id="imgUrl" placeholder="https://...">
      </div>`,
    actions:[
      {label:'Cancel',cls:'ghost',fn:closeModal},
      {label:'Add Image',cls:'primary',fn:()=>{
        const file=document.getElementById('imgFile').files[0];
        const url=document.getElementById('imgUrl').value.trim();
        if(file){
          const r=new FileReader();
          r.onload=()=>{ cb(r.result); closeModal(); };
          r.readAsDataURL(file);
        } else if(url){ cb(url); closeModal(); }
        else { toast('Pick a file or paste a URL'); }
      }}
    ]
  });
}
function setDistrictCover(did,url){ const d=DB.districts.find(x=>x.id===did); d.cover=url; d.updated=now(); save(); render(); renderNav(); toast('Cover updated'); }
function addBuildingImage(did,bid){
  pickImage(url=>{
    const d=DB.districts.find(x=>x.id===did); const b=d.buildings.find(x=>x.id===bid);
    b.images=b.images||[]; b.images.push(url); d.updated=now(); save(); render(); toast('Reference added');
  });
}

/* =========================================================
   LIGHTBOX
   ========================================================= */
function openLightbox(src){
  document.getElementById('lightboxRoot').innerHTML=`<div class="lightbox" onclick="this.remove()"><img src="${src}"></div>`;
}
function lightboxDelegate(e){ if(e.target.tagName==='IMG') openLightbox(e.target.src); }

/* =========================================================
   MODAL system
   ========================================================= */
function modal({title,body,actions,wide}){
  const a=(actions||[]).map((x,i)=>`<button class="btn ${x.cls||''}" data-mi="${i}">${x.label}</button>`).join('');
  document.getElementById('modalRoot').innerHTML=`
    <div class="modal-bg" id="mbg">
      <div class="modal" style="${wide?'max-width:720px':''}">
        <div class="modal-h"><div class="mt">${title}</div><button class="icon-btn" onclick="closeModal()"><i class="ti ti-x"></i></button></div>
        <div class="modal-body">${body}</div>
        <div class="modal-foot">${a}</div>
      </div>
    </div>`;
  (actions||[]).forEach((x,i)=>{ document.querySelector(`[data-mi="${i}"]`).onclick=x.fn; });
  document.getElementById('mbg').addEventListener('mousedown',e=>{ if(e.target.id==='mbg') closeModal(); });
}
function closeModal(){ document.getElementById('modalRoot').innerHTML=''; }

/* =========================================================
   LORE CRUD
   ========================================================= */
const ICONS=['ti-broadcast','ti-shield-bolt','ti-plane','ti-file-text','ti-book','ti-map-2','ti-cpu','ti-flame','ti-bolt','ti-skull','ti-ghost','ti-world','ti-gizmo','ti-radioactive','ti-antenna-bars-5'];
function newLore(){ loreForm(); }
function editLore(id){ loreForm(DB.lore.find(x=>x.id===id)); }
function loreForm(p){
  const isNew=!p;
  modal({ wide:true, title:isNew?'New <b>Lore Page</b>':'Edit <b>Lore Page</b>',
    body:`
      <div class="field-row">
        <div class="field"><label class="fl">Title</label><input class="f" id="lTitle" value="${p?esc(p.title):''}" placeholder="The Premise"></div>
        <div class="field"><label class="fl">Icon</label>
          <select class="f" id="lIcon">${ICONS.map(ic=>`<option value="${ic}" ${p&&p.icon===ic?'selected':''}>${ic.replace('ti-','')}</option>`).join('')}</select>
        </div>
      </div>
      <div class="field"><label class="fl">Body — Markdown supported (# H1, ## H2, ### H3, **bold**, *italic*, > quote, - list, \`code\`, [link](url), !IMG(url) )</label>
        <textarea class="f" id="lBody" style="min-height:260px;font-family:var(--mono);font-size:13px">${p?esc(p.body):'# Title\\n\\nStart writing...'}</textarea>
        <div class="meta-line" style="margin-top:6px">Tip: embed an image inline with <span class="hl">!IMG(https://...)</span></div>
      </div>`,
    actions:[
      {label:'Cancel',cls:'ghost',fn:closeModal},
      {label:isNew?'Create':'Save',cls:'primary',fn:()=>{
        const t=document.getElementById('lTitle').value.trim()||'Untitled';
        const ic=document.getElementById('lIcon').value;
        const b=document.getElementById('lBody').value;
        if(isNew){ const np={id:uid(),title:t,icon:ic,body:b,updated:now()}; DB.lore.push(np); save(); renderNav(); go('lore',np.id); }
        else { p.title=t;p.icon=ic;p.body=b;p.updated=now(); save(); renderNav(); render(); }
        closeModal(); toast(isNew?'Page created':'Page saved');
      }}
    ]});
}
function delLore(id){ confirmModal('Delete this lore page?',()=>{ DB.lore=DB.lore.filter(x=>x.id!==id); save(); renderNav(); go('dashboard'); toast('Page deleted'); }); }

/* =========================================================
   FACTION CRUD
   ========================================================= */
const FCOLORS=['#ff6a2b','#36c5c5','#d6a445','#c43c3c','#7f77dd','#3a9d6a','#e0451f','#888780'];
function newFaction(){ factionForm(); }
function editFaction(id){ factionForm(DB.factions.find(x=>x.id===id)); }
function factionForm(f){
  const isNew=!f;
  modal({ wide:true, title:isNew?'New <b>Faction</b>':'Edit <b>Faction</b>',
    body:`
      <div class="field-row">
        <div class="field"><label class="fl">Name</label><input class="f" id="fName" value="${f?esc(f.name):''}" placeholder="WyrmTech"></div>
        <div class="field"><label class="fl">Classification Tag</label><input class="f" id="fTag" value="${f?esc(f.tag||''):''}" placeholder="Hegemon // Player Faction"></div>
      </div>
      <div class="field"><label class="fl">Motto</label><input class="f" id="fMotto" value="${f?esc(f.motto||''):''}" placeholder="The Wyrm does not sleep."></div>
      <div class="field"><label class="fl">Faction Color</label>
        <div style="display:flex;gap:8px;flex-wrap:wrap" id="fcolors">
          ${FCOLORS.map(c=>`<div data-c="${c}" style="width:30px;height:30px;background:${c};border:2px solid ${f&&f.color===c?'#fff':'transparent'};cursor:pointer" onclick="this.parentNode.querySelectorAll('[data-c]').forEach(x=>x.style.borderColor='transparent');this.style.borderColor='#fff';document.getElementById('fColorVal').value='${c}'"></div>`).join('')}
        </div>
        <input type="hidden" id="fColorVal" value="${f?f.color:FCOLORS[0]}">
      </div>
      <div class="field"><label class="fl">Profile — Markdown supported</label>
        <textarea class="f" id="fBody" style="min-height:200px;font-family:var(--mono);font-size:13px">${f?esc(f.body):'# Faction Name\\n\\n'}</textarea>
      </div>`,
    actions:[
      {label:'Cancel',cls:'ghost',fn:closeModal},
      {label:isNew?'Create':'Save',cls:'primary',fn:()=>{
        const name=document.getElementById('fName').value.trim()||'New Faction';
        const o={name,tag:document.getElementById('fTag').value,motto:document.getElementById('fMotto').value,
          color:document.getElementById('fColorVal').value,body:document.getElementById('fBody').value,updated:now()};
        if(isNew){ o.id=uid(); DB.factions.push(o); save(); renderNav(); go('faction',o.id); }
        else { Object.assign(f,o); save(); renderNav(); render(); }
        closeModal(); toast(isNew?'Faction created':'Faction saved');
      }}
    ]});
}
function delFaction(id){ confirmModal('Delete this faction?',()=>{ DB.factions=DB.factions.filter(x=>x.id!==id); save(); renderNav(); go('dashboard'); toast('Faction deleted'); }); }

/* =========================================================
   DISTRICT CRUD
   ========================================================= */
function newDistrict(){ districtForm(); }
function editDistrict(id){ districtForm(DB.districts.find(x=>x.id===id)); }
function districtForm(d){
  const isNew=!d;
  const facOpts=DB.factions.map(f=>`<option value="${esc(f.name)}" ${d&&d.faction===f.name?'selected':''}>${esc(f.name)}</option>`).join('');
  modal({ title:isNew?'New <b>District</b>':'Edit <b>District</b>',
    body:`
      <div class="field"><label class="fl">District Name</label><input class="f" id="dName" value="${d?esc(d.name):''}" placeholder="Industrial District"></div>
      <div class="field-row">
        <div class="field"><label class="fl">Status</label>
          <select class="f" id="dStatus">${Object.entries(STATUS).map(([k,v])=>`<option value="${k}" ${d&&d.status===k?'selected':''}>${v.l}</option>`).join('')}</select>
        </div>
        <div class="field"><label class="fl">Controlling Faction</label>
          <select class="f" id="dFaction"><option value="">— Unaligned —</option>${facOpts}</select>
        </div>
      </div>
      <div class="field"><label class="fl">Summary</label><textarea class="f" id="dSummary" placeholder="Describe the district vibe, purpose, mods in play...">${d?esc(d.summary||''):''}</textarea></div>`,
    actions:[
      {label:'Cancel',cls:'ghost',fn:closeModal},
      {label:isNew?'Create':'Save',cls:'primary',fn:()=>{
        const o={name:document.getElementById('dName').value.trim()||'New District',
          status:document.getElementById('dStatus').value,
          faction:document.getElementById('dFaction').value,
          summary:document.getElementById('dSummary').value,updated:now()};
        if(isNew){ o.id=uid();o.cover='';o.buildings=[]; DB.districts.push(o); save(); renderNav(); go('district',o.id); }
        else { Object.assign(d,o); save(); renderNav(); render(); }
        closeModal(); toast(isNew?'District created':'District saved');
      }}
    ]});
}
function delDistrict(id){ confirmModal('Delete this district and all its builds?',()=>{ DB.districts=DB.districts.filter(x=>x.id!==id); save(); renderNav(); go('dashboard'); toast('District deleted'); }); }

/* =========================================================
   BUILDING CRUD
   ========================================================= */
function newBuilding(did){ buildingForm(did); }
function editBuilding(did,bid){ const d=DB.districts.find(x=>x.id===did); buildingForm(did,d.buildings.find(b=>b.id===bid)); }
function buildingForm(did,b){
  const isNew=!b;
  modal({ title:isNew?'New <b>Structure</b>':'Edit <b>Structure</b>',
    body:`
      <div class="field"><label class="fl">Building Name</label><input class="f" id="bName" value="${b?esc(b.name):''}" placeholder="The Great Foundry"></div>
      <div class="field-row">
        <div class="field"><label class="fl">Status</label>
          <select class="f" id="bStatus">${Object.entries(STATUS).map(([k,v])=>`<option value="${k}" ${b&&b.status===k?'selected':''}>${v.l}</option>`).join('')}</select>
        </div>
        <div class="field"><label class="fl">Tags</label><input class="f" id="bTags" value="${b?esc(b.tags||''):''}" placeholder="Create · Mechanical"></div>
      </div>
      <div class="field"><label class="fl">Build Notes &amp; Plans</label><textarea class="f" id="bNotes" style="min-height:140px" placeholder="Layout ideas, multiblock specs, material list, what's left to do...">${b?esc(b.notes||''):''}</textarea></div>`,
    actions:[
      {label:'Cancel',cls:'ghost',fn:closeModal},
      {label:isNew?'Add':'Save',cls:'primary',fn:()=>{
        const d=DB.districts.find(x=>x.id===did);
        const o={name:document.getElementById('bName').value.trim()||'New Structure',
          status:document.getElementById('bStatus').value,
          tags:document.getElementById('bTags').value,
          notes:document.getElementById('bNotes').value};
        if(isNew){ o.id=uid();o.images=[]; d.buildings.push(o); }
        else { Object.assign(b,o); }
        d.updated=now(); save(); render(); closeModal(); toast(isNew?'Structure added':'Structure saved');
      }}
    ]});
}
function delBuilding(did,bid){ confirmModal('Delete this structure?',()=>{ const d=DB.districts.find(x=>x.id===did); d.buildings=d.buildings.filter(b=>b.id!==bid); d.updated=now(); save(); render(); toast('Structure deleted'); }); }

/* =========================================================
   CONFIRM + TOAST + IMPORT/EXPORT
   ========================================================= */
function confirmModal(msg,fn){
  modal({ title:'<b>Confirm</b>',
    body:`<p style="color:var(--ink);font-size:15px">${msg}</p>`,
    actions:[{label:'Cancel',cls:'ghost',fn:closeModal},{label:'Delete',cls:'primary danger',fn:()=>{fn();closeModal();}}]});
}
let toastTimer;
function toast(msg){ const t=document.getElementById('toast'); document.getElementById('toastMsg').textContent=msg; t.classList.add('show'); clearTimeout(toastTimer); toastTimer=setTimeout(()=>t.classList.remove('show'),2200); }

function exportData(){
  const blob=new Blob([JSON.stringify(DB,null,2)],{type:'application/json'});
  const a=document.createElement('a'); a.href=URL.createObjectURL(blob);
  a.download='wyrmtech-datastream-'+new Date().toISOString().slice(0,10)+'.json'; a.click();
  toast('Backup exported');
}
function importData(e){
  const file=e.target.files[0]; if(!file)return;
  const r=new FileReader();
  r.onload=()=>{ try{ const obj=JSON.parse(r.result);
    if(!obj.lore||!obj.districts){ toast('Not a valid backup'); return; }
    confirmModal('Import will replace ALL current data. Continue?',()=>{ DB=obj; save(); renderNav(); go('dashboard'); toast('Data imported'); });
  }catch(err){ toast('Import failed — bad file'); } };
  r.readAsText(file); e.target.value='';
}

/* =========================================================
   BOOT
   ========================================================= */
renderNav();
render();
</script>
</body>
</html>
