<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>OVPDx FWA Accomplishment Tracker</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;500;600&family=DM+Serif+Display&display=swap" rel="stylesheet" />
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.8.2/jspdf.plugin.autotable.min.js"></script>
  <style>
    :root {
      --bg: #f7f6f2;
      --surface: #ffffff;
      --surface2: #f0ede6;
      --border: #e2ddd6;
      --border-strong: #c8c2b8;
      --text: #1a1816;
      --text-muted: #6b665f;
      --text-faint: #a09a92;
      --accent: #2d5016;
      --accent-light: #eaf2e0;
      --gold: #8b6914;
      --gold-light: #fdf3d8;
      --radius: 10px;
      --radius-sm: 6px;
      --shadow: 0 1px 3px rgba(0,0,0,0.06), 0 4px 12px rgba(0,0,0,0.04);
      --shadow-md: 0 2px 8px rgba(0,0,0,0.08), 0 8px 24px rgba(0,0,0,0.06);
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: 'DM Sans', sans-serif;
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      font-size: 14px;
      line-height: 1.6;
    }

    /* ── Header ── */
    .site-header {
      background: var(--surface);
      border-bottom: 1px solid var(--border);
      padding: 0 2rem;
      display: flex;
      align-items: center;
      justify-content: space-between;
      height: 60px;
      position: sticky;
      top: 0;
      z-index: 100;
      box-shadow: var(--shadow);
    }
    .logo {
      display: flex;
      align-items: center;
      gap: 10px;
    }
    .logo-mark {
      width: 32px;
      height: 32px;
      background: var(--accent);
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-shrink: 0;
    }
    .logo-mark svg { width: 18px; height: 18px; fill: none; stroke: #fff; stroke-width: 2; stroke-linecap: round; }
    .logo-text { font-size: 15px; font-weight: 600; letter-spacing: -0.02em; }
    .logo-sub { font-size: 11px; color: var(--text-muted); font-weight: 400; }
    .header-right { display: flex; align-items: center; gap: 8px; }

    /* ── Layout ── */
    .layout { display: grid; grid-template-columns: 220px 1fr; min-height: calc(100vh - 60px); }

    /* ── Sidebar ── */
    .sidebar {
      background: var(--surface);
      border-right: 1px solid var(--border);
      padding: 1.5rem 0;
      position: sticky;
      top: 60px;
      height: calc(100vh - 60px);
      overflow-y: auto;
    }
    .sidebar-section { margin-bottom: 1.5rem; }
    .sidebar-label {
      font-size: 10px;
      font-weight: 600;
      letter-spacing: 0.08em;
      text-transform: uppercase;
      color: var(--text-faint);
      padding: 0 1.25rem;
      margin-bottom: 4px;
    }
    .sidebar-item {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 8px 1.25rem;
      font-size: 13px;
      color: var(--text-muted);
      cursor: pointer;
      border-left: 2px solid transparent;
      transition: all 0.15s;
      text-decoration: none;
    }
    .sidebar-item:hover { background: var(--surface2); color: var(--text); }
    .sidebar-item.active { color: var(--accent); border-left-color: var(--accent); background: var(--accent-light); font-weight: 500; }
    .sidebar-icon { width: 16px; height: 16px; flex-shrink: 0; opacity: 0.7; }
    .sidebar-item.active .sidebar-icon { opacity: 1; }

    /* ── Main content ── */
    .main { padding: 2rem; max-width: 860px; }
    .page { display: none; }
    .page.active { display: block; }

    /* ── Page header ── */
    .page-header { margin-bottom: 1.75rem; }
    .page-title { font-family: 'DM Serif Display', serif; font-size: 26px; font-weight: 400; letter-spacing: -0.02em; margin-bottom: 4px; }
    .page-desc { font-size: 13px; color: var(--text-muted); }

    /* ── Cards ── */
    .card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1.25rem;
      margin-bottom: 1rem;
      box-shadow: var(--shadow);
    }
    .card-title {
      font-size: 11px;
      font-weight: 600;
      letter-spacing: 0.07em;
      text-transform: uppercase;
      color: var(--text-faint);
      margin-bottom: 1rem;
      padding-bottom: 8px;
      border-bottom: 1px solid var(--border);
    }

    /* ── Forms ── */
    .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-bottom: 12px; }
    .form-grid.three { grid-template-columns: 1fr 1fr 1fr; }
    .form-grid.full { grid-template-columns: 1fr; }
    .field { display: flex; flex-direction: column; }
    .field label { font-size: 11px; font-weight: 500; color: var(--text-muted); margin-bottom: 5px; letter-spacing: 0.02em; }
    .field input[type="text"],
    .field select,
    .field textarea {
      font-family: 'DM Sans', sans-serif;
      font-size: 13px;
      padding: 8px 11px;
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      background: var(--surface);
      color: var(--text);
      transition: border-color 0.15s, box-shadow 0.15s;
      outline: none;
      width: 100%;
    }
    .field input:focus, .field select:focus, .field textarea:focus {
      border-color: var(--accent);
      box-shadow: 0 0 0 3px rgba(45,80,22,0.08);
    }
    .field textarea { resize: vertical; min-height: 64px; line-height: 1.5; }

    /* ── Day grid ── */
    .day-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: 8px; margin-bottom: 4px; }
    .day-cell label { font-size: 10px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.05em; color: var(--text-faint); text-align: center; display: block; margin-bottom: 4px; }
    .day-cell select { font-size: 11px; padding: 5px 3px; text-align: center; }

    /* ── Buttons ── */
    .btn {
      font-family: 'DM Sans', sans-serif;
      font-size: 13px;
      font-weight: 500;
      padding: 8px 16px;
      border-radius: var(--radius-sm);
      border: 1px solid var(--border);
      background: var(--surface);
      color: var(--text);
      cursor: pointer;
      transition: all 0.15s;
      display: inline-flex;
      align-items: center;
      gap: 6px;
    }
    .btn:hover { background: var(--surface2); border-color: var(--border-strong); }
    .btn-primary {
      background: var(--accent);
      color: #fff;
      border-color: var(--accent);
    }
    .btn-primary:hover { background: #234010; border-color: #234010; }
    .btn-sm { font-size: 12px; padding: 5px 12px; }
    .btn-group { display: flex; gap: 8px; flex-wrap: wrap; }

    /* ── Upload zone ── */
    .upload-zone {
      border: 1.5px dashed var(--border-strong);
      border-radius: var(--radius-sm);
      padding: 16px;
      text-align: center;
      cursor: pointer;
      background: var(--surface2);
      transition: background 0.15s, border-color 0.15s;
    }
    .upload-zone:hover, .upload-zone.dragover { background: var(--accent-light); border-color: var(--accent); }
    .upload-zone input[type="file"] { display: none; }
    .upload-zone-text { font-size: 12px; color: var(--text-muted); line-height: 1.6; }
    .upload-zone-text strong { color: var(--text); }
    .thumb-row { display: flex; flex-wrap: wrap; gap: 8px; margin-top: 10px; }
    .thumb {
      position: relative;
      width: 68px; height: 68px;
      border-radius: var(--radius-sm);
      overflow: hidden;
      border: 1px solid var(--border);
    }
    .thumb img { width: 100%; height: 100%; object-fit: cover; display: block; }
    .thumb-del {
      position: absolute; top: 3px; right: 3px;
      background: rgba(0,0,0,0.6); color: #fff;
      border: none; border-radius: 50%;
      width: 18px; height: 18px; font-size: 11px;
      cursor: pointer; display: flex; align-items: center; justify-content: center;
    }

    /* ── Entry items ── */
    .entry-item {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 10px 14px;
      margin-bottom: 8px;
      display: flex;
      align-items: flex-start;
      gap: 12px;
      transition: border-color 0.15s;
    }
    .entry-item:hover { border-color: var(--border-strong); }
    .entry-body { flex: 1; min-width: 0; }
    .entry-tags { display: flex; align-items: center; gap: 6px; flex-wrap: wrap; margin-bottom: 5px; }
    .entry-desc { font-size: 14px; color: var(--text); }
    .entry-notes { font-size: 12px; color: var(--text-muted); margin-top: 3px; }
    .entry-imgs { display: flex; flex-wrap: wrap; gap: 6px; margin-top: 8px; }
    .entry-img { width: 44px; height: 44px; border-radius: 4px; object-fit: cover; border: 1px solid var(--border); cursor: pointer; }
    .entry-del { background: none; border: none; color: var(--text-faint); font-size: 18px; cursor: pointer; padding: 0 2px; line-height: 1; flex-shrink: 0; }
    .entry-del:hover { color: #c0392b; }

    /* ── Badges ── */
    .badge {
      font-size: 10px;
      font-weight: 600;
      padding: 2px 8px;
      border-radius: 99px;
      letter-spacing: 0.02em;
      white-space: nowrap;
    }
    .badge-project { background: var(--accent-light); color: var(--accent); }
    .badge-ongoing { background: #fdf3d8; color: #7a5a0e; }
    .badge-completed { background: #e8f5e9; color: #2e7d32; }
    .badge-recurring { background: #e3f2fd; color: #1565c0; }
    .badge-notinit { background: var(--surface2); color: var(--text-muted); }
    .badge-date { background: var(--surface2); color: var(--text-muted); }
    .badge-photo { background: var(--surface2); color: var(--text-muted); }

    /* ── Stats ── */
    .stats-row { display: grid; grid-template-columns: repeat(5, 1fr); gap: 10px; margin-bottom: 1.5rem; }
    .stat-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 14px;
      box-shadow: var(--shadow);
    }
    .stat-val { font-size: 24px; font-weight: 600; color: var(--text); letter-spacing: -0.02em; }
    .stat-lbl { font-size: 11px; color: var(--text-muted); margin-top: 2px; }

    /* ── Sig preview ── */
    .sig-fixed-box {
      background: var(--surface2);
      border-radius: var(--radius-sm);
      padding: 10px 14px;
    }
    .sig-fixed-title { font-size: 11px; font-weight: 600; color: var(--text-muted); margin-bottom: 4px; }
    .sig-fixed-name { font-size: 13px; font-weight: 600; color: var(--text); }
    .sig-fixed-role { font-size: 12px; color: var(--text-muted); }
    .sig-fixed-note { font-size: 11px; color: var(--text-faint); font-style: italic; margin-top: 4px; }

    /* ── Export ── */
    .export-tabs { display: flex; gap: 6px; margin-bottom: 1rem; flex-wrap: wrap; }
    .ex-tab {
      font-family: 'DM Sans', sans-serif;
      font-size: 12px; font-weight: 500;
      padding: 5px 14px;
      border: 1px solid var(--border);
      border-radius: 99px;
      background: var(--surface);
      color: var(--text-muted);
      cursor: pointer;
      transition: all 0.15s;
    }
    .ex-tab:hover { background: var(--surface2); }
    .ex-tab.active { background: var(--accent); color: #fff; border-color: var(--accent); }
    .export-note { font-size: 12px; color: var(--text-muted); margin-bottom: 12px; line-height: 1.7; }
    pre {
      white-space: pre-wrap; word-break: break-word;
      font-family: 'Courier New', monospace;
      font-size: 12px;
      background: var(--surface2);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 1rem;
      margin: 1rem 0;
      max-height: 360px;
      overflow-y: auto;
      color: var(--text);
      line-height: 1.6;
    }
    .preview-wrap {
      background: var(--surface2);
      border: 1px solid var(--border);
      border-radius: var(--radius-sm);
      padding: 1rem;
      margin: 1rem 0;
    }
    .preview-table { width: 100%; border-collapse: collapse; font-size: 11px; }
    .preview-table th {
      background: #fef9e7;
      border: 1px solid #ccc;
      padding: 6px 8px;
      text-align: left;
      font-weight: 600;
    }
    .preview-table td { border: 1px solid #ddd; padding: 5px 8px; vertical-align: top; }
    .preview-table tr.group-row td { background: #fef9e7; font-weight: 600; }

    /* ── Project group ── */
    .proj-group { margin-bottom: 1.5rem; }
    .proj-group-header {
      font-size: 13px; font-weight: 600;
      color: var(--text);
      display: flex; align-items: center; gap: 8px;
      margin-bottom: 8px;
    }
    .proj-count { font-size: 11px; color: var(--text-muted); font-weight: 400; }

    /* ── Empty state ── */
    .empty-state { text-align: center; padding: 3rem 0; color: var(--text-faint); font-size: 13px; }

    /* ── Lightbox ── */
    .lightbox {
      display: none; position: fixed; inset: 0;
      background: rgba(0,0,0,0.8);
      z-index: 9999;
      align-items: center; justify-content: center;
    }
    .lightbox.open { display: flex; }
    .lightbox img { max-width: 90vw; max-height: 85vh; border-radius: var(--radius); object-fit: contain; }
    .lightbox-close {
      position: absolute; top: 20px; right: 24px;
      color: #fff; font-size: 28px;
      background: none; border: none; cursor: pointer; line-height: 1;
    }

    /* ── Responsive ── */
    @media (max-width: 768px) {
      .layout { grid-template-columns: 1fr; }
      .sidebar { display: none; }
      .main { padding: 1rem; }
      .stats-row { grid-template-columns: repeat(3, 1fr); }
      .form-grid { grid-template-columns: 1fr; }
      .form-grid.three { grid-template-columns: 1fr; }
      .day-grid { grid-template-columns: repeat(4, 1fr); }
    }

    /* ── Animations ── */
    @keyframes fadeIn { from { opacity:0; transform: translateY(6px); } to { opacity:1; transform: translateY(0); } }
    .page.active { animation: fadeIn 0.2s ease; }
    .entry-item { animation: fadeIn 0.15s ease; }
  </style>
</head>
<body>

<header class="site-header">
  <div class="logo">
    <div class="logo-mark">
      <svg viewBox="0 0 24 24"><path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2"/><rect x="9" y="3" width="6" height="4" rx="1"/><line x1="9" y1="12" x2="15" y2="12"/><line x1="9" y1="16" x2="13" y2="16"/></svg>
    </div>
    <div>
      <div class="logo-text">FWA Tracker</div>
      <div class="logo-sub">OVPDx · UP System</div>
    </div>
  </div>
  <div class="header-right">
    <button class="btn btn-sm" onclick="showPage('export'); switchExTab('pdf')">Export / PDF ↗</button>
  </div>
</header>

<div class="layout">
  <aside class="sidebar">
    <div class="sidebar-section">
      <div class="sidebar-label">Tracker</div>
      <div class="sidebar-item active" onclick="showPage('add')" id="nav-add">
        <svg class="sidebar-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/></svg>
        Add deliverable
      </div>
      <div class="sidebar-item" onclick="showPage('view')" id="nav-view">
        <svg class="sidebar-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="7" height="7"/><rect x="14" y="3" width="7" height="7"/><rect x="3" y="14" width="7" height="7"/><rect x="14" y="14" width="7" height="7"/></svg>
        All entries
      </div>
    </div>
    <div class="sidebar-section">
      <div class="sidebar-label">Export</div>
      <div class="sidebar-item" onclick="showPage('export'); switchExTab('pdf')" id="nav-export">
        <svg class="sidebar-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="12" y1="12" x2="12" y2="18"/><line x1="9" y1="15" x2="15" y2="15"/></svg>
        PDF (FWA format)
      </div>
      <div class="sidebar-item" onclick="showPage('export'); switchExTab('sheets')" id="nav-sheets">
        <svg class="sidebar-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="18" height="18" rx="2"/><line x1="3" y1="9" x2="21" y2="9"/><line x1="3" y1="15" x2="21" y2="15"/><line x1="9" y1="3" x2="9" y2="21"/></svg>
        Google Sheets
      </div>
      <div class="sidebar-item" onclick="showPage('export'); switchExTab('docs')" id="nav-docs">
        <svg class="sidebar-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="16" y1="13" x2="8" y2="13"/><line x1="16" y1="17" x2="8" y2="17"/><polyline points="10 9 9 9 8 9"/></svg>
        Google Docs
      </div>
    </div>
  </aside>

  <main class="main">

    <!-- ADD PAGE -->
    <div class="page active" id="page-add">
      <div class="page-header">
        <div class="page-title">Add deliverable</div>
        <div class="page-desc">Fill in the report details, log your tasks, then sign off at the bottom.</div>
      </div>

      <!-- Report header -->
      <div class="card">
        <div class="card-title">Report header</div>
        <div class="form-grid">
          <div class="field"><label>Name</label><input type="text" id="hName" placeholder="Full name" /></div>
          <div class="field"><label>Office / Unit</label><input type="text" id="hOffice" placeholder="e.g. OVPDx" /></div>
        </div>
        <div class="form-grid full" style="margin-bottom:14px;">
          <div class="field"><label>For the period of</label><input type="text" id="hPeriod" placeholder="e.g. March 17–21, 2025" /></div>
        </div>
        <div class="card-title" style="margin-top:4px;">Work arrangement (per day)</div>
        <div class="day-grid">
          <div class="day-cell"><label>Mon</label><select id="dMon"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>Rest Day</option></select></div>
          <div class="day-cell"><label>Tue</label><select id="dTue"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>Rest Day</option></select></div>
          <div class="day-cell"><label>Wed</label><select id="dWed"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>Rest Day</option></select></div>
          <div class="day-cell"><label>Thu</label><select id="dThu"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>Rest Day</option></select></div>
          <div class="day-cell"><label>Fri</label><select id="dFri"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>Rest Day</option></select></div>
          <div class="day-cell"><label>Sat</label><select id="dSat"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>Rest Day</option></select></div>
          <div class="day-cell"><label>Sun</label><select id="dSun"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>Rest Day</option></select></div>
        </div>
      </div>

      <!-- Entry form -->
      <div class="card">
        <div class="card-title">Add activity / task</div>
        <div class="form-grid">
          <div class="field"><label>Date (optional)</label><input type="text" id="fDate" placeholder="e.g. March 17" /></div>
          <div class="field"><label>Project name</label><input type="text" id="fProject" placeholder="e.g. Dx Labs AI Workshop" /></div>
        </div>
        <div class="form-grid full" style="margin-bottom:12px;">
          <div class="field"><label>Activity / Task</label><input type="text" id="fDesc" placeholder="Describe the activity or task..." /></div>
        </div>
        <div class="form-grid three" style="margin-bottom:12px;">
          <div class="field"><label>Status</label>
            <select id="fStatus">
              <option value="ongoing">O – Ongoing/In-Process</option>
              <option value="completed">C – Completed</option>
              <option value="recurring">R – Recurring</option>
              <option value="notinit">Not initiated</option>
            </select>
          </div>
          <div class="field" style="grid-column:span 2;"><label>Remarks / mode of verification</label><input type="text" id="fNotes" placeholder="Link to output, mode of verification..." /></div>
        </div>
        <div class="field" style="margin-bottom:14px;">
          <label>Verification photos (optional)</label>
          <div class="upload-zone" id="uploadZone" onclick="document.getElementById('fileInput').click()" ondragover="onDragOver(event)" ondragleave="onDragLeave(event)" ondrop="onDrop(event)">
            <input type="file" id="fileInput" accept="image/*" multiple onchange="onFileChange(event)" />
            <div class="upload-zone-text">Click to upload or drag &amp; drop images<br><strong>PNG, JPG, WEBP</strong> — multiple allowed</div>
          </div>
          <div class="thumb-row" id="thumbRow"></div>
        </div>
        <button class="btn btn-primary" onclick="addEntry()">+ Add entry</button>
      </div>

      <!-- Recent entries -->
      <div id="recent-list"></div>

      <!-- Signature block -->
      <div class="card">
        <div class="card-title">Signature block</div>
        <div class="form-grid" style="margin-bottom:12px;">
          <div class="field"><label>Submitted by (name)</label><input type="text" id="sigSubmitted" placeholder="e.g. Juan dela Cruz" /></div>
          <div class="field"><label>Reviewed by (name)</label><input type="text" id="sigReviewed" placeholder="e.g. Maria Santos" /></div>
        </div>
        <div class="sig-fixed-box">
          <div class="sig-fixed-title">Approved by — fixed</div>
          <div class="sig-fixed-name">Peter A. Sy</div>
          <div class="sig-fixed-role">Vice President for Digital Transformation</div>
          <div class="sig-fixed-note">This section is pre-filled and cannot be edited.</div>
        </div>
      </div>
    </div>

    <!-- VIEW PAGE -->
    <div class="page" id="page-view">
      <div class="page-header">
        <div class="page-title">All entries</div>
        <div class="page-desc">Entries for the current period label set in the report header.</div>
      </div>
      <div class="stats-row" id="stats"></div>
      <div id="view-list"></div>
    </div>

    <!-- EXPORT PAGE -->
    <div class="page" id="page-export">
      <div class="page-header">
        <div class="page-title">Export</div>
        <div class="page-desc">Generate your FWA Accomplishment Report or export data to other formats.</div>
      </div>
      <div class="export-tabs">
        <button class="ex-tab active" id="etab-pdf" onclick="switchExTab('pdf')">PDF (FWA format)</button>
        <button class="ex-tab" id="etab-sheets" onclick="switchExTab('sheets')">Google Sheets</button>
        <button class="ex-tab" id="etab-docs" onclick="switchExTab('docs')">Google Docs</button>
      </div>

      <div id="epane-pdf">
        <div class="card">
          <div class="export-note">Generates the official UP FWA Accomplishment Report as a downloadable PDF — including work arrangement table, activity log, and signature block. Fill in all header and signature fields first.</div>
          <div class="preview-wrap" id="pdf-preview-table"><div class="empty-state">No entries yet for this period.</div></div>
          <div class="btn-group">
            <button class="btn btn-primary" onclick="exportPDF()">⬇ Download PDF</button>
            <button class="btn" onclick="buildPDFPreview()">Refresh preview</button>
          </div>
        </div>
      </div>

      <div id="epane-sheets" style="display:none;">
        <div class="card">
          <div class="export-note">Tab-separated values (TSV). Open Google Sheets → paste with <strong>Ctrl+Shift+V</strong> (unformatted paste) so each value lands in its own cell. Or download the .tsv file and import via <strong>File → Import</strong>.</div>
          <pre id="export-tsv"></pre>
          <div class="btn-group">
            <button class="btn btn-primary" onclick="copyEl('export-tsv')">Copy for Sheets</button>
            <button class="btn" onclick="downloadTSV()">⬇ Download .tsv</button>
          </div>
        </div>
      </div>

      <div id="epane-docs" style="display:none;">
        <div class="card">
          <div class="export-note">Formatted for Google Docs. Copy → paste into your Doc. Apply <strong>Heading 2</strong> to project names and <strong>Heading 3</strong> to status groups after pasting.</div>
          <pre id="export-docs"></pre>
          <div class="btn-group">
            <button class="btn btn-primary" onclick="copyEl('export-docs')">Copy for Docs</button>
          </div>
        </div>
      </div>
    </div>

  </main>
</div>

<!-- Lightbox -->
<div class="lightbox" id="lightbox" onclick="closeLightbox()">
  <button class="lightbox-close" onclick="closeLightbox()">×</button>
  <img id="lightboxImg" src="" alt="Verification photo" />
</div>

<script>
  let entries = JSON.parse(localStorage.getItem('ovpdx_fwa_v7') || '[]');
  let pendingImages = [];
  const SL = { ongoing:'Ongoing / In-Process', completed:'Completed', recurring:'Recurring', notinit:'Not initiated' };
  const SC = { ongoing:'O', completed:'C', recurring:'R', notinit:'N' };
  const STATUS_ORDER = ['completed','ongoing','recurring','notinit'];

  function save() { localStorage.setItem('ovpdx_fwa_v7', JSON.stringify(entries)); }
  function getPeriod() { return document.getElementById('hPeriod').value.trim() || '(Period not set)'; }
  function getHeader() {
    return {
      name: document.getElementById('hName').value.trim(),
      office: document.getElementById('hOffice').value.trim(),
      period: document.getElementById('hPeriod').value.trim(),
      submitted: document.getElementById('sigSubmitted').value.trim(),
      reviewed: document.getElementById('sigReviewed').value.trim(),
      days: {
        Mon:document.getElementById('dMon').value, Tue:document.getElementById('dTue').value,
        Wed:document.getElementById('dWed').value, Thu:document.getElementById('dThu').value,
        Fri:document.getElementById('dFri').value, Sat:document.getElementById('dSat').value,
        Sun:document.getElementById('dSun').value
      }
    };
  }

  // Image handling
  function onDragOver(e) { e.preventDefault(); document.getElementById('uploadZone').classList.add('dragover'); }
  function onDragLeave(e) { document.getElementById('uploadZone').classList.remove('dragover'); }
  function onDrop(e) { e.preventDefault(); document.getElementById('uploadZone').classList.remove('dragover'); handleFiles(e.dataTransfer.files); }
  function onFileChange(e) { handleFiles(e.target.files); e.target.value=''; }
  function handleFiles(files) {
    Array.from(files).forEach(file => {
      if (!file.type.startsWith('image/')) return;
      const reader = new FileReader();
      reader.onload = ev => { pendingImages.push({ dataUrl:ev.target.result, name:file.name }); renderThumbs(); };
      reader.readAsDataURL(file);
    });
  }
  function renderThumbs() {
    document.getElementById('thumbRow').innerHTML = pendingImages.map((img,i) => `
      <div class="thumb">
        <img src="${img.dataUrl}" title="${img.name}" onclick="openLightbox('${img.dataUrl}')" />
        <button class="thumb-del" onclick="removePending(${i})">×</button>
      </div>`).join('');
  }
  function removePending(i) { pendingImages.splice(i,1); renderThumbs(); }
  function openLightbox(src) { document.getElementById('lightboxImg').src=src; document.getElementById('lightbox').classList.add('open'); }
  function closeLightbox() { document.getElementById('lightbox').classList.remove('open'); }

  // Add entry
  function addEntry() {
    const desc=document.getElementById('fDesc').value.trim(), project=document.getElementById('fProject').value.trim(),
          status=document.getElementById('fStatus').value, notes=document.getElementById('fNotes').value.trim(),
          date=document.getElementById('fDate').value.trim();
    if (!desc) { alert('Please describe the activity/task.'); return; }
    if (!project) { alert('Please enter a project name.'); return; }
    entries.push({ id:Date.now(), project, desc, status, notes, date, period:getPeriod(), images:pendingImages.map(i=>({dataUrl:i.dataUrl,name:i.name})) });
    save();
    ['fDesc','fNotes','fProject','fDate'].forEach(id=>document.getElementById(id).value='');
    pendingImages=[]; renderThumbs(); renderRecent();
  }
  function deleteEntry(id) { entries=entries.filter(e=>e.id!==id); save(); renderRecent(); renderView(); }

  function badgeClass(s) { return {ongoing:'badge-ongoing',completed:'badge-completed',recurring:'badge-recurring',notinit:'badge-notinit'}[s]||'badge-notinit'; }

  function itemHTML(e) {
    const imgCount=(e.images||[]).length;
    const imgBadge=imgCount?`<span class="badge badge-photo">${imgCount} photo${imgCount>1?'s':''}</span>`:'';
    const thumbs=(e.images||[]).map(img=>`<img class="entry-img" src="${img.dataUrl}" title="${img.name}" onclick="openLightbox('${img.dataUrl}')" />`).join('');
    return `<div class="entry-item">
      <div class="entry-body">
        <div class="entry-tags">
          <span class="badge badge-project">${e.project}</span>
          <span class="badge ${badgeClass(e.status)}">${SC[e.status]} – ${SL[e.status]}</span>
          ${e.date?`<span class="badge badge-date">${e.date}</span>`:''}
          ${imgBadge}
          <span style="font-size:11px;color:var(--text-faint);">${e.period}</span>
        </div>
        <div class="entry-desc">${e.desc}</div>
        ${e.notes?`<div class="entry-notes">${e.notes}</div>`:''}
        ${thumbs?`<div class="entry-imgs">${thumbs}</div>`:''}
      </div>
      <button class="entry-del" onclick="deleteEntry(${e.id})">×</button>
    </div>`;
  }

  function getPeriodEntries() { return entries.filter(e=>e.period===getPeriod()); }

  function renderRecent() {
    const el=document.getElementById('recent-list'), recent=[...entries].reverse().slice(0,6);
    el.innerHTML=recent.length
      ?`<div style="font-size:11px;font-weight:600;letter-spacing:0.06em;text-transform:uppercase;color:var(--text-faint);margin-bottom:8px;">Recent entries</div>`+recent.map(itemHTML).join('')
      :'<div class="empty-state">No entries yet. Add one above.</div>';
  }

  function renderView() {
    const we=getPeriodEntries(), statsEl=document.getElementById('stats'), listEl=document.getElementById('view-list');
    statsEl.innerHTML=`
      <div class="stat-card"><div class="stat-val">${we.length}</div><div class="stat-lbl">Total</div></div>
      <div class="stat-card"><div class="stat-val">${we.filter(e=>e.status==='completed').length}</div><div class="stat-lbl">Completed</div></div>
      <div class="stat-card"><div class="stat-val">${we.filter(e=>e.status==='ongoing').length}</div><div class="stat-lbl">Ongoing</div></div>
      <div class="stat-card"><div class="stat-val">${we.filter(e=>e.status==='recurring').length}</div><div class="stat-lbl">Recurring</div></div>
      <div class="stat-card"><div class="stat-val">${we.filter(e=>e.status==='notinit').length}</div><div class="stat-lbl">Not initiated</div></div>`;
    if (!we.length) { listEl.innerHTML='<div class="empty-state">No entries for this period label.</div>'; return; }
    const projects=[...new Set(we.map(e=>e.project))];
    listEl.innerHTML=projects.map(proj=>{
      const pItems=we.filter(e=>e.project===proj);
      return `<div class="proj-group">
        <div class="proj-group-header">${proj} <span class="proj-count">${pItems.length} task${pItems.length>1?'s':''}</span></div>
        ${pItems.map(itemHTML).join('')}
      </div>`;
    }).join('');
  }

  function buildPDFPreview() {
    const we=getPeriodEntries(), h=getHeader();
    const el=document.getElementById('pdf-preview-table');
    if (!we.length) { el.innerHTML='<div class="empty-state">No entries for this period.</div>'; return; }
    let html=`<div style="font-size:12px;color:var(--text-muted);margin-bottom:10px;font-weight:500;">${h.name||'(Name)'} · ${h.office||'(Office)'} · ${h.period||'(Period)'}</div>`;
    html+=`<table class="preview-table">
      <thead><tr>
        <th style="width:70px;">Date</th>
        <th>Activity / Task</th>
        <th style="width:30px;text-align:center;">O</th>
        <th style="width:30px;text-align:center;">C</th>
        <th style="width:30px;text-align:center;">R</th>
        <th>Remarks</th>
        <th style="width:60px;text-align:center;">Photos</th>
      </tr></thead><tbody>`;
    we.forEach(e=>{
      const imgs=(e.images||[]);
      const thumbsHtml=imgs.map(img=>`<img src="${img.dataUrl}" style="width:28px;height:28px;object-fit:cover;border-radius:3px;margin:1px;cursor:pointer;" onclick="openLightbox('${img.dataUrl}')" />`).join('');
      html+=`<tr>
        <td>${e.date||''}</td>
        <td>${e.project?'<span style="color:#888;font-size:10px;">['+e.project+']</span> ':''}${e.desc}</td>
        <td style="text-align:center;">${e.status==='ongoing'?'✓':''}</td>
        <td style="text-align:center;">${e.status==='completed'?'✓':''}</td>
        <td style="text-align:center;">${e.status==='recurring'?'✓':''}</td>
        <td>${e.notes||''}</td>
        <td style="text-align:center;">${thumbsHtml||'—'}</td>
      </tr>`;
    });
    html+=`</tbody></table>`;
    html+=`<div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:16px;margin-top:16px;font-size:11px;">
      <div><div style="color:var(--text-muted);margin-bottom:18px;">Submitted by:</div><div style="border-top:1px solid var(--text);padding-top:4px;font-weight:600;">${h.submitted||'___________________'}</div></div>
      <div><div style="color:var(--text-muted);margin-bottom:18px;">Reviewed by:</div><div style="border-top:1px solid var(--text);padding-top:4px;font-weight:600;">${h.reviewed||'___________________'}</div></div>
      <div><div style="color:var(--text-muted);margin-bottom:18px;">Approved by:</div><div style="border-top:1px solid var(--text);padding-top:4px;font-weight:600;">Peter A. Sy</div><div style="color:var(--text-muted);">Vice President for Digital Transformation</div></div>
    </div>`;
    el.innerHTML=html;
  }

  async function exportPDF() {
    if (typeof window.jspdf==='undefined') { alert('PDF library loading, please try again.'); return; }
    const {jsPDF}=window.jspdf, we=getPeriodEntries(), h=getHeader();
    if (!we.length) { alert('No entries for this period.'); return; }
    const doc=new jsPDF({orientation:'portrait',unit:'mm',format:'a4'});
    const pw=210,ml=15,mr=15,contentW=pw-ml-mr;
    let y=18;
    doc.setFont('helvetica','bold'); doc.setFontSize(11);
    doc.text('UNIVERSITY OF THE PHILIPPINES',pw/2,y,{align:'center'}); y+=5;
    doc.setFontSize(10); doc.text('<CU>',pw/2,y,{align:'center'}); y+=10;
    doc.setFont('helvetica','normal'); doc.setFontSize(10);
    doc.text('Name',ml,y); doc.line(ml+10,y+0.5,ml+70,y+0.5); doc.text(h.name,ml+12,y);
    doc.text('Office/Unit',ml+80,y); doc.line(ml+97,y+0.5,ml+contentW,y+0.5); doc.text(h.office,ml+99,y);
    y+=10;
    doc.setFont('helvetica','bold'); doc.setFontSize(11);
    doc.text('FLEXIBLE WORK ARRANGEMENT (FWA) ACCOMPLISHMENT REPORT',pw/2,y,{align:'center'}); y+=5;
    doc.setFont('helvetica','normal'); doc.setFontSize(10);
    const pLabel='For the Period of ';
    doc.text(pLabel,pw/2-30,y); doc.line(pw/2-30+doc.getTextWidth(pLabel),y+0.5,pw/2+35,y+0.5);
    doc.text(h.period,pw/2-30+doc.getTextWidth(pLabel)+2,y); y+=8;
    const days=['Mon','Tue','Wed','Thu','Fri','Sat','Sun'],cellW=contentW/7;
    doc.setFillColor(254,249,231); doc.rect(ml,y,contentW,7,'F');
    doc.setDrawColor(150,150,150); doc.rect(ml,y,contentW,14);
    doc.setFont('helvetica','bold'); doc.setFontSize(9); doc.text('Work Arrangement*',pw/2,y+4,{align:'center'});
    doc.setFont('helvetica','italic'); doc.setFontSize(7.5); doc.text("(as indicated in the Office/Unit's Regular Weekly FWA)",pw/2,y+6.5,{align:'center'});
    y+=7;
    days.forEach((d,i)=>{
      const x=ml+i*cellW;
      doc.setDrawColor(150,150,150); doc.line(x,y,x,y+7);
      doc.setFont('helvetica','bold'); doc.setFontSize(8); doc.text(d,x+cellW/2,y+3.5,{align:'center'});
      doc.setFont('helvetica','normal'); doc.setFontSize(7); doc.text(h.days[d]||'',x+cellW/2,y+6.2,{align:'center'});
    });
    y+=10;
    const tableRows=we.map(e=>[
      e.date||'', (e.project?'['+e.project+'] ':'')+e.desc,
      e.status==='ongoing'?'✓':'', e.status==='completed'?'✓':'', e.status==='recurring'?'✓':'',
      e.notes+((e.images||[]).length?(e.notes?' | ':'')+'[see photo(s)]':'')
    ]);
    doc.autoTable({
      startY:y, margin:{left:ml,right:mr},
      head:[[
        {content:'DATE\n(optional)',styles:{fillColor:[254,249,231],textColor:[0,0,0],fontStyle:'bold',halign:'center'}},
        {content:'ACTIVITY/ TASK',styles:{fillColor:[254,249,231],textColor:[0,0,0],fontStyle:'bold',halign:'center'}},
        {content:'O',styles:{fillColor:[254,249,231],textColor:[0,0,0],fontStyle:'bold',halign:'center'}},
        {content:'C',styles:{fillColor:[254,249,231],textColor:[0,0,0],fontStyle:'bold',halign:'center'}},
        {content:'R',styles:{fillColor:[254,249,231],textColor:[0,0,0],fontStyle:'bold',halign:'center'}},
        {content:'REMARKS\n(MUST include mode of verification)',styles:{fillColor:[254,249,231],textColor:[0,0,0],fontStyle:'bold',halign:'center'}}
      ]],
      body:tableRows,
      columnStyles:{0:{cellWidth:22,fontSize:8},1:{cellWidth:78,fontSize:8},2:{cellWidth:10,halign:'center',fontSize:9},3:{cellWidth:10,halign:'center',fontSize:9},4:{cellWidth:10,halign:'center',fontSize:9},5:{cellWidth:55,fontSize:8}},
      styles:{lineColor:[180,180,180],lineWidth:0.3,cellPadding:2.5},
      headStyles:{lineColor:[180,180,180],lineWidth:0.3}, theme:'grid'
    });
    let finalY=doc.lastAutoTable.finalY+5;
    doc.setFontSize(7.5); doc.setFont('helvetica','italic');
    doc.text('* Work from Home, Satellite Office or Another Fixed Place within the Philippines',ml,finalY);
    finalY+=12;
    if (finalY+35>280) { doc.addPage(); finalY=20; }
    const colW=contentW/3;
    [{label:'Submitted by:',name:h.submitted||''},{label:'Reviewed by:',name:h.reviewed||''},{label:'Approved by:',name:'Peter A. Sy',title:'Vice President for Digital Transformation'}].forEach((col,i)=>{
      const x=ml+i*colW;
      doc.setFont('helvetica','normal'); doc.setFontSize(9); doc.text(col.label,x,finalY);
      doc.line(x,finalY+14,x+colW-6,finalY+14);
      doc.setFont('helvetica','bold'); doc.setFontSize(9); doc.text(col.name,x,finalY+19);
      if (col.title) { doc.setFont('helvetica','normal'); doc.setFontSize(8); doc.text(col.title,x,finalY+24); }
    });
    const allImgEntries=we.filter(e=>(e.images||[]).length>0);
    if (allImgEntries.length>0) {
      doc.addPage(); let py=18;
      doc.setFont('helvetica','bold'); doc.setFontSize(11); doc.text('VERIFICATION PHOTOS',pw/2,py,{align:'center'}); py+=3;
      doc.setFont('helvetica','normal'); doc.setFontSize(9); doc.text(`${h.period} | ${h.name} | ${h.office}`,pw/2,py+4,{align:'center'}); py+=12;
      for (const e of allImgEntries) {
        if (py>250) { doc.addPage(); py=18; }
        doc.setFont('helvetica','bold'); doc.setFontSize(9); doc.text(`${e.desc}${e.date?' ('+e.date+')':''}`,ml,py); py+=5;
        if (e.notes) { doc.setFont('helvetica','italic'); doc.setFontSize(8); doc.text('Remarks: '+e.notes,ml,py); py+=5; }
        let imgX=ml;
        for (const img of (e.images||[])) {
          try {
            const imgW=55,imgH=42;
            if (imgX+imgW>pw-mr) { imgX=ml; py+=imgH+6; }
            if (py+imgH>270) { doc.addPage(); py=18; imgX=ml; }
            doc.addImage(img.dataUrl,'JPEG',imgX,py,imgW,imgH); imgX+=imgW+6;
          } catch(err) {}
        }
        py+=48; doc.setDrawColor(200,200,200); doc.line(ml,py-4,pw-mr,py-4);
      }
    }
    doc.save(`FWA_${(h.name||'Report').replace(/\s+/g,'_')}_${(h.period||'Period').replace(/[^a-z0-9]/gi,'_')}.pdf`);
  }

  function buildSheets() {
    const we=getPeriodEntries(),h=getHeader();
    const headers=['Period','Name','Office','Date','Project','Activity/Task','Status','O','C','R','Remarks','Photos'];
    const rows=[headers,...we.map(e=>[e.period,h.name||'',h.office||'',e.date||'',e.project,e.desc,SL[e.status],e.status==='ongoing'?'✓':'',e.status==='completed'?'✓':'',e.status==='recurring'?'✓':'',e.notes||'',(e.images||[]).length>0?`[${(e.images||[]).length} photo(s)]`:'' ])];
    document.getElementById('export-tsv').textContent=rows.map(r=>r.map(c=>`"${String(c).replace(/"/g,'""')}"`).join('\t')).join('\n');
  }

  function buildDocs() {
    const we=getPeriodEntries(),h=getHeader();
    if (!we.length) { document.getElementById('export-docs').textContent='No entries.'; return; }
    let t=`## FWA ACCOMPLISHMENT REPORT — ${(h.period||'').toUpperCase()}\nName: ${h.name||''} | Office/Unit: ${h.office||''}\n\n`;
    t+='Work Arrangement: '+['Mon','Tue','Wed','Thu','Fri','Sat','Sun'].map(d=>`${d}: ${h.days[d]||'—'}`).join('  ')+'\n\n';
    const projects=[...new Set(we.map(e=>e.project))];
    projects.forEach(proj=>{
      t+=`### ${proj}\n`;
      STATUS_ORDER.forEach(s=>{
        const g=we.filter(e=>e.project===proj&&e.status===s); if(!g.length) return;
        t+=`${SL[s]}:\n`;
        g.forEach(e=>{ t+=`• ${e.desc}${e.date?' ('+e.date+')':''}${e.notes?'\n  Remarks: '+e.notes:''}${(e.images||[]).length?'\n  ['+e.images.length+' verification photo(s)]':''}\n`; });
        t+='\n';
      });
    });
    t+=`\n──────────────────────────────\nSubmitted by: ${h.submitted||'___________'}     Reviewed by: ${h.reviewed||'___________'}     Approved by: Peter A. Sy, Vice President for Digital Transformation`;
    document.getElementById('export-docs').textContent=t;
  }

  function downloadTSV() {
    const text=document.getElementById('export-tsv').textContent,h=getHeader();
    const blob=new Blob([text],{type:'text/tab-separated-values'});
    const a=document.createElement('a'); a.href=URL.createObjectURL(blob);
    a.download=`FWA_${(h.period||'Period').replace(/[^a-z0-9]/gi,'_')}.tsv`; a.click();
  }

  function copyEl(id) {
    navigator.clipboard.writeText(document.getElementById(id).textContent).then(()=>{
      const b=event.target,orig=b.textContent; b.textContent='Copied!'; setTimeout(()=>b.textContent=orig,1800);
    });
  }

  function switchExTab(tab) {
    ['pdf','sheets','docs'].forEach(t=>{
      document.getElementById('etab-'+t).classList.toggle('active',t===tab);
      document.getElementById('epane-'+t).style.display=t===tab?'':'none';
    });
    if(tab==='pdf') buildPDFPreview();
    if(tab==='sheets') buildSheets();
    if(tab==='docs') buildDocs();
  }

  function showPage(page) {
    document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
    document.querySelectorAll('.sidebar-item').forEach(i=>i.classList.remove('active'));
    document.getElementById('page-'+page).classList.add('active');
    const navEl=document.getElementById('nav-'+page);
    if (navEl) navEl.classList.add('active');
    if(page==='view') renderView();
    if(page==='export') buildPDFPreview();
  }

  document.getElementById('hPeriod').addEventListener('input',()=>{ if(document.getElementById('page-view').classList.contains('active')) renderView(); });
  renderRecent();
</script>
</body>
</html>
