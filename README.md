<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>FWA Accomplishment Tracker</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@300;400;500;600&family=DM+Serif+Display&display=swap" rel="stylesheet" />
  <script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.8.2/jspdf.plugin.autotable.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
  <style>
    :root {
      --bg:#f7f6f2;--surface:#fff;--surface2:#f0ede6;
      --border:#e2ddd6;--border-strong:#c8c2b8;
      --text:#1a1816;--text-muted:#6b665f;--text-faint:#a09a92;
      --accent:#2d5016;--accent-light:#eaf2e0;
      --radius:10px;--radius-sm:6px;
      --shadow:0 1px 3px rgba(0,0,0,.06),0 4px 12px rgba(0,0,0,.04);
    }
    *{box-sizing:border-box;margin:0;padding:0;}
    body{font-family:'DM Sans',sans-serif;background:var(--bg);color:var(--text);min-height:100vh;font-size:14px;line-height:1.6;}

    /* LOGIN */
    .login-screen{min-height:100vh;display:flex;align-items:center;justify-content:center;background:var(--bg);}
    .login-box{background:var(--surface);border:1px solid var(--border);border-radius:16px;padding:2.5rem 2rem;width:100%;max-width:400px;box-shadow:var(--shadow);}
    .login-logo{display:flex;align-items:center;gap:10px;margin-bottom:1.75rem;}
    .login-logo-mark{width:36px;height:36px;background:var(--accent);border-radius:9px;display:flex;align-items:center;justify-content:center;}
    .login-logo-mark svg{width:18px;height:18px;fill:none;stroke:#fff;stroke-width:2;stroke-linecap:round;}
    .login-tabs{display:flex;gap:4px;margin-bottom:1.5rem;border-bottom:1px solid var(--border);}
    .login-tab{font-family:'DM Sans',sans-serif;font-size:13px;padding:8px 14px;border:none;background:none;color:var(--text-muted);cursor:pointer;border-bottom:2px solid transparent;margin-bottom:-1px;}
    .login-tab.active{color:var(--accent);border-bottom-color:var(--accent);font-weight:500;}
    .lfield{margin-bottom:14px;}
    .lfield label{font-size:11px;font-weight:500;color:var(--text-muted);display:block;margin-bottom:5px;}
    .lfield input{width:100%;font-family:'DM Sans',sans-serif;font-size:13px;padding:9px 12px;border:1px solid var(--border);border-radius:var(--radius-sm);background:var(--surface);color:var(--text);outline:none;transition:border-color .15s,box-shadow .15s;}
    .lfield input:focus{border-color:var(--accent);box-shadow:0 0 0 3px rgba(45,80,22,.08);}
    .lbtn{width:100%;font-family:'DM Sans',sans-serif;font-size:13px;font-weight:500;padding:10px;border-radius:var(--radius-sm);border:none;background:var(--accent);color:#fff;cursor:pointer;margin-top:4px;transition:background .15s;}
    .lbtn:hover{background:#234010;}
    .lmsg{font-size:12px;margin-top:10px;text-align:center;min-height:18px;}
    .lmsg.err{color:#c0392b;}.lmsg.ok{color:var(--accent);}

    /* APP */
    #app{display:none;}
    .site-header{background:var(--surface);border-bottom:1px solid var(--border);padding:0 2rem;display:flex;align-items:center;justify-content:space-between;height:60px;position:sticky;top:0;z-index:100;box-shadow:var(--shadow);}
    .logo{display:flex;align-items:center;gap:10px;}
    .logo-mark{width:32px;height:32px;background:var(--accent);border-radius:8px;display:flex;align-items:center;justify-content:center;flex-shrink:0;}
    .logo-mark svg{width:18px;height:18px;fill:none;stroke:#fff;stroke-width:2;stroke-linecap:round;}
    .logo-text{font-size:15px;font-weight:600;letter-spacing:-.02em;}
    .logo-sub{font-size:11px;color:var(--text-muted);font-weight:400;}
    .header-right{display:flex;align-items:center;gap:10px;}
    .user-pill{display:flex;align-items:center;gap:8px;padding:5px 12px 5px 6px;background:var(--surface2);border:1px solid var(--border);border-radius:99px;font-size:12px;color:var(--text-muted);}
    .user-avatar{width:24px;height:24px;border-radius:50%;background:var(--accent);display:flex;align-items:center;justify-content:center;font-size:11px;font-weight:600;color:#fff;flex-shrink:0;}
    .logout-btn{font-family:'DM Sans',sans-serif;font-size:12px;padding:5px 12px;border:1px solid var(--border);border-radius:99px;background:var(--surface);color:var(--text-muted);cursor:pointer;transition:all .15s;}
    .logout-btn:hover{background:#fee;color:#c0392b;border-color:#fcc;}

    /* LAYOUT */
    .layout{display:grid;grid-template-columns:220px 1fr;min-height:calc(100vh - 60px);}
    .sidebar{background:var(--surface);border-right:1px solid var(--border);padding:1.5rem 0;position:sticky;top:60px;height:calc(100vh - 60px);overflow-y:auto;}
    .sidebar-section{margin-bottom:1.5rem;}
    .sidebar-label{font-size:10px;font-weight:600;letter-spacing:.08em;text-transform:uppercase;color:var(--text-faint);padding:0 1.25rem;margin-bottom:4px;}
    .sidebar-item{display:flex;align-items:center;gap:10px;padding:8px 1.25rem;font-size:13px;color:var(--text-muted);cursor:pointer;border-left:2px solid transparent;transition:all .15s;}
    .sidebar-item:hover{background:var(--surface2);color:var(--text);}
    .sidebar-item.active{color:var(--accent);border-left-color:var(--accent);background:var(--accent-light);font-weight:500;}
    .sidebar-icon{width:16px;height:16px;flex-shrink:0;opacity:.7;}
    .sidebar-item.active .sidebar-icon{opacity:1;}
    .main{padding:2rem;max-width:860px;}
    .page{display:none;}.page.active{display:block;animation:fadeIn .2s ease;}
    @keyframes fadeIn{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:translateY(0)}}
    .page-header{margin-bottom:1.75rem;}
    .page-title{font-family:'DM Serif Display',serif;font-size:26px;font-weight:400;letter-spacing:-.02em;margin-bottom:4px;}
    .page-desc{font-size:13px;color:var(--text-muted);}

    /* CARDS */
    .card{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:1.25rem;margin-bottom:1rem;box-shadow:var(--shadow);}
    .card-title{font-size:11px;font-weight:600;letter-spacing:.07em;text-transform:uppercase;color:var(--text-faint);margin-bottom:1rem;padding-bottom:8px;border-bottom:1px solid var(--border);}

    /* FORMS */
    .form-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:12px;}
    .form-grid.three{grid-template-columns:1fr 1fr 1fr;}
    .form-grid.full{grid-template-columns:1fr;}
    .field{display:flex;flex-direction:column;}
    .field label{font-size:11px;font-weight:500;color:var(--text-muted);margin-bottom:5px;letter-spacing:.02em;}
    .field input[type="text"],.field select,.field textarea{font-family:'DM Sans',sans-serif;font-size:13px;padding:8px 11px;border:1px solid var(--border);border-radius:var(--radius-sm);background:var(--surface);color:var(--text);outline:none;width:100%;transition:border-color .15s,box-shadow .15s;}
    .field input:focus,.field select:focus,.field textarea:focus{border-color:var(--accent);box-shadow:0 0 0 3px rgba(45,80,22,.08);}
    .field textarea{resize:vertical;min-height:64px;line-height:1.5;}
    .day-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:8px;margin-bottom:4px;}
    .day-cell label{font-size:10px;font-weight:600;text-transform:uppercase;letter-spacing:.05em;color:var(--text-faint);text-align:center;display:block;margin-bottom:4px;}
    .day-cell select{font-size:11px;padding:5px 3px;text-align:center;}

    /* BUTTONS */
    .btn{font-family:'DM Sans',sans-serif;font-size:13px;font-weight:500;padding:8px 16px;border-radius:var(--radius-sm);border:1px solid var(--border);background:var(--surface);color:var(--text);cursor:pointer;transition:all .15s;display:inline-flex;align-items:center;gap:6px;}
    .btn:hover{background:var(--surface2);border-color:var(--border-strong);}
    .btn-primary{background:var(--accent);color:#fff;border-color:var(--accent);}
    .btn-primary:hover{background:#234010;border-color:#234010;}
    .btn-group{display:flex;gap:8px;flex-wrap:wrap;}

    /* UPLOAD */
    .upload-zone{border:1.5px dashed var(--border-strong);border-radius:var(--radius-sm);padding:16px;text-align:center;cursor:pointer;background:var(--surface2);transition:background .15s,border-color .15s;}
    .upload-zone:hover,.upload-zone.dragover{background:var(--accent-light);border-color:var(--accent);}
    .upload-zone input[type="file"]{display:none;}
    .upload-zone-text{font-size:12px;color:var(--text-muted);line-height:1.6;}
    .upload-zone-text strong{color:var(--text);}
    .thumb-row{display:flex;flex-wrap:wrap;gap:8px;margin-top:10px;}
    .thumb{position:relative;width:96px;height:64px;border-radius:var(--radius-sm);overflow:hidden;border:1px solid var(--border);}
    .thumb img{width:100%;height:100%;object-fit:cover;display:block;}
    .thumb-del{position:absolute;top:3px;right:3px;background:rgba(0,0,0,.6);color:#fff;border:none;border-radius:50%;width:18px;height:18px;font-size:11px;cursor:pointer;display:flex;align-items:center;justify-content:center;}
    .thumb-badge{position:absolute;bottom:3px;left:3px;font-size:9px;background:rgba(0,0,0,.45);color:#fff;padding:1px 5px;border-radius:3px;}

    /* ENTRIES */
    .entry-item{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius-sm);padding:10px 14px;margin-bottom:8px;display:flex;align-items:flex-start;gap:12px;transition:border-color .15s;animation:fadeIn .15s ease;}
    .entry-item:hover{border-color:var(--border-strong);}
    .entry-body{flex:1;min-width:0;}
    .entry-tags{display:flex;align-items:center;gap:6px;flex-wrap:wrap;margin-bottom:5px;}
    .entry-desc{font-size:14px;color:var(--text);}
    .entry-notes{font-size:12px;color:var(--text-muted);margin-top:3px;}
    .entry-imgs{display:flex;flex-wrap:wrap;gap:6px;margin-top:8px;}
    .entry-img{width:64px;height:44px;border-radius:4px;object-fit:cover;border:1px solid var(--border);cursor:pointer;}
    .entry-del{background:none;border:none;color:var(--text-faint);font-size:18px;cursor:pointer;padding:0 2px;line-height:1;flex-shrink:0;}
    .entry-del:hover{color:#c0392b;}

    /* BADGES */
    .badge{font-size:10px;font-weight:600;padding:2px 8px;border-radius:99px;letter-spacing:.02em;white-space:nowrap;}
    .badge-project{background:var(--accent-light);color:var(--accent);}
    .badge-ongoing{background:#fdf3d8;color:#7a5a0e;}
    .badge-completed{background:#e8f5e9;color:#2e7d32;}
    .badge-recurring{background:#e3f2fd;color:#1565c0;}
    .badge-notinit{background:var(--surface2);color:var(--text-muted);}
    .badge-date,.badge-photo{background:var(--surface2);color:var(--text-muted);}

    /* STATS */
    .stats-row{display:grid;grid-template-columns:repeat(5,1fr);gap:10px;margin-bottom:1.5rem;}
    .stat-card{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius-sm);padding:14px;box-shadow:var(--shadow);}
    .stat-val{font-size:24px;font-weight:600;color:var(--text);letter-spacing:-.02em;}
    .stat-lbl{font-size:11px;color:var(--text-muted);margin-top:2px;}

    /* SIG */
    .sig-fixed-box{background:var(--surface2);border-radius:var(--radius-sm);padding:10px 14px;}
    .sig-fixed-title{font-size:11px;font-weight:600;color:var(--text-muted);margin-bottom:4px;}
    .sig-fixed-name{font-size:13px;font-weight:600;color:var(--text);}
    .sig-fixed-role{font-size:12px;color:var(--text-muted);}
    .sig-fixed-note{font-size:11px;color:var(--text-faint);font-style:italic;margin-top:4px;}

    /* EXPORT */
    .export-note{font-size:12px;color:var(--text-muted);margin-bottom:12px;line-height:1.7;}
    .preview-wrap{background:var(--surface2);border:1px solid var(--border);border-radius:var(--radius-sm);padding:1rem;margin:1rem 0;overflow-x:auto;}
    .preview-table{width:100%;border-collapse:collapse;font-size:11px;}
    .preview-table th{background:#fef9e7;border:1px solid #ccc;padding:6px 8px;text-align:left;font-weight:600;}
    .preview-table td{border:1px solid #ddd;padding:5px 8px;vertical-align:top;}

    /* GROUPS */
    .proj-group{margin-bottom:1.5rem;}
    .proj-group-header{font-size:13px;font-weight:600;color:var(--text);display:flex;align-items:center;gap:8px;margin-bottom:8px;}
    .proj-count{font-size:11px;color:var(--text-muted);font-weight:400;}
    .empty-state{text-align:center;padding:3rem 0;color:var(--text-faint);font-size:13px;}

    /* LIGHTBOX */
    .lightbox{display:none;position:fixed;inset:0;background:rgba(0,0,0,.8);z-index:9999;align-items:center;justify-content:center;}
    .lightbox.open{display:flex;}
    .lightbox img{max-width:90vw;max-height:85vh;border-radius:var(--radius);object-fit:contain;}
    .lightbox-close{position:absolute;top:20px;right:24px;color:#fff;font-size:28px;background:none;border:none;cursor:pointer;}

    /* TEAM DELIVERABLES */
    .team-tabs{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:1.25rem;}
    .team-tab{font-family:'DM Sans',sans-serif;font-size:12px;font-weight:500;padding:6px 14px;border:1px solid var(--border);border-radius:99px;background:var(--surface);color:var(--text-muted);cursor:pointer;transition:all .15s;}
    .team-tab:hover{background:var(--surface2);}
    .team-tab.active{background:var(--accent);color:#fff;border-color:var(--accent);}
    .team-table-wrap{overflow-x:auto;border:1px solid var(--border);border-radius:var(--radius-sm);background:var(--surface);}
    .team-table{width:100%;border-collapse:collapse;font-size:13px;min-width:700px;}
    .team-table th{background:#f7f6f2;border-bottom:1px solid var(--border);padding:9px 12px;text-align:left;font-size:11px;font-weight:600;letter-spacing:.04em;color:var(--text-muted);white-space:nowrap;}
    .team-table td{border-bottom:1px solid var(--border);padding:8px 12px;vertical-align:middle;}
    .team-table tr:last-child td{border-bottom:none;}
    .team-table tr:hover td{background:var(--surface2);}
    .team-table input[type="text"],.team-table select{font-family:'DM Sans',sans-serif;font-size:12px;padding:5px 8px;border:1px solid transparent;border-radius:4px;background:transparent;color:var(--text);width:100%;outline:none;transition:border-color .15s,background .15s;}
    .team-table input[type="text"]:focus,.team-table select:focus{border-color:var(--accent);background:var(--surface);box-shadow:0 0 0 2px rgba(45,80,22,.07);}
    .team-table input[type="text"]:hover,.team-table select:hover{background:var(--surface);border-color:var(--border);}
    .del-row-btn{background:none;border:none;color:var(--text-faint);font-size:16px;cursor:pointer;padding:0 4px;line-height:1;}
    .del-row-btn:hover{color:#c0392b;}
    .add-row-btn{font-family:'DM Sans',sans-serif;font-size:12px;padding:6px 14px;border:1px dashed var(--border-strong);border-radius:var(--radius-sm);background:transparent;color:var(--text-muted);cursor:pointer;width:100%;margin-top:8px;transition:all .15s;}
    .add-row-btn:hover{background:var(--accent-light);border-color:var(--accent);color:var(--accent);}
    .status-pill{font-size:11px;font-weight:500;padding:3px 10px;border-radius:99px;white-space:nowrap;}
    .s-completed{background:#e8f5e9;color:#2e7d32;}
    .s-ongoing{background:#fdf3d8;color:#7a5a0e;}
    .s-notinit{background:var(--surface2);color:var(--text-muted);}
    .export-note{font-size:12px;color:var(--text-muted);margin-bottom:12px;line-height:1.7;}
    pre{white-space:pre-wrap;word-break:break-word;font-family:'Courier New',monospace;font-size:12px;background:var(--surface2);border:1px solid var(--border);border-radius:var(--radius-sm);padding:1rem;margin:1rem 0;max-height:360px;overflow-y:auto;color:var(--text);line-height:1.6;}
    .btn-group{display:flex;gap:8px;flex-wrap:wrap;}
    .header-nav{display:flex;gap:4px;align-items:center;}
    .hnav-btn{font-family:'DM Sans',sans-serif;font-size:12px;font-weight:500;padding:6px 12px;border:1px solid var(--border);border-radius:var(--radius-sm);background:var(--surface);color:var(--text-muted);cursor:pointer;transition:all .15s;white-space:nowrap;}
    .hnav-btn:hover{background:var(--surface2);color:var(--text);}
    .hnav-btn.active{background:var(--accent);color:#fff;border-color:var(--accent);}
    .user-label-text{}
    @media(max-width:900px){.user-label-text{display:none;}}
    @media(max-width:768px){
      .layout{grid-template-columns:1fr;}.sidebar{display:none;}
      .main{padding:1rem;}.stats-row{grid-template-columns:repeat(3,1fr);}
      .form-grid,.form-grid.three{grid-template-columns:1fr;}
      .day-grid{grid-template-columns:repeat(4,1fr);}
      .header-nav{gap:2px;}.hnav-btn{font-size:11px;padding:5px 8px;}
      .logout-btn{font-size:11px;padding:5px 8px;}
    }
    /* MODAL */
    .modal-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.45);z-index:9000;align-items:center;justify-content:center;}
    .modal-overlay.open{display:flex;}
    .modal-box{background:var(--surface);border:1px solid var(--border);border-radius:14px;padding:2rem;width:100%;max-width:420px;box-shadow:0 8px 40px rgba(0,0,0,.18);animation:fadeIn .18s ease;max-height:90vh;overflow-y:auto;}
    .modal-title{font-size:15px;font-weight:600;margin-bottom:4px;}
    .modal-desc{font-size:12px;color:var(--text-muted);margin-bottom:1.25rem;line-height:1.6;}
    .modal-footer{display:flex;gap:8px;margin-top:1.25rem;justify-content:flex-end;}
    /* Forgot password link */
    .forgot-link{font-size:11px;color:var(--accent);cursor:pointer;text-align:right;display:block;margin-top:6px;text-decoration:underline;}
    .forgot-link:hover{color:#234010;}
    /* EmailJS setup notice in admin */
    .emailjs-note{background:#fffbea;border:1px solid #f0d060;border-radius:var(--radius-sm);padding:10px 14px;font-size:12px;color:#7a5a0e;line-height:1.65;margin-bottom:12px;}
    /* UNDO TOAST */
    .undo-toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%) translateY(80px);background:#1a1816;color:#fff;font-size:13px;padding:10px 18px;border-radius:99px;display:flex;align-items:center;gap:12px;z-index:9999;transition:transform .25s ease;box-shadow:0 4px 20px rgba(0,0,0,.25);}
    .undo-toast.show{transform:translateX(-50%) translateY(0);}
    .undo-btn{background:var(--accent-light);color:var(--accent);border:none;border-radius:99px;padding:4px 12px;font-size:12px;font-weight:600;cursor:pointer;font-family:'DM Sans',sans-serif;}
    /* DASHBOARD */
    .dash-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:14px;margin-bottom:1.5rem;}
    .dash-card{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:1.25rem;box-shadow:var(--shadow);}
    .dash-card-title{font-size:11px;font-weight:600;letter-spacing:.07em;text-transform:uppercase;color:var(--text-faint);margin-bottom:1rem;padding-bottom:8px;border-bottom:1px solid var(--border);}
    .dash-bar-row{display:flex;align-items:center;gap:10px;margin-bottom:8px;}
    .dash-bar-label{font-size:12px;color:var(--text-muted);width:90px;flex-shrink:0;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
    .dash-bar-track{flex:1;height:8px;background:var(--surface2);border-radius:99px;overflow:hidden;}
    .dash-bar-fill{height:100%;border-radius:99px;background:var(--accent);transition:width .4s ease;}
    .dash-bar-count{font-size:11px;color:var(--text-muted);width:28px;text-align:right;flex-shrink:0;}
    .dash-week-row{display:flex;align-items:center;justify-content:space-between;padding:6px 0;border-bottom:1px solid var(--border);font-size:12px;}
    .dash-week-row:last-child{border-bottom:none;}
    /* OVERDUE badge */
    .badge-overdue{background:#fde8e8;color:#c0392b;}
    /* EDIT entry button */
    .entry-edit{background:none;border:none;color:var(--text-faint);font-size:13px;cursor:pointer;padding:0 4px;line-height:1;}
    .entry-edit:hover{color:var(--accent);}
    /* ACKNOWLEDGEMENTS / REACTIONS */
    .react-row{display:flex;align-items:center;gap:6px;flex-wrap:wrap;margin-top:8px;padding-top:8px;border-top:1px solid var(--border);}
    .react-btn{font-family:'DM Sans',sans-serif;font-size:12px;padding:3px 9px;border-radius:99px;border:1px solid var(--border);background:var(--surface);cursor:pointer;display:inline-flex;align-items:center;gap:4px;transition:all .15s;color:var(--text-muted);}
    .react-btn:hover{background:var(--accent-light);border-color:var(--accent);color:var(--accent);}
    .react-btn.reacted{background:var(--accent-light);border-color:var(--accent);color:var(--accent);font-weight:600;}
    .react-count{font-size:11px;font-weight:600;}
    .react-add{font-size:12px;padding:3px 9px;border-radius:99px;border:1px dashed var(--border-strong);background:transparent;cursor:pointer;color:var(--text-faint);transition:all .15s;}
    .react-add:hover{background:var(--accent-light);border-color:var(--accent);color:var(--accent);}
    .react-picker{display:none;position:absolute;background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:8px;box-shadow:var(--shadow);z-index:200;flex-wrap:wrap;gap:4px;width:200px;}
    .react-picker.open{display:flex;}
    .react-picker-btn{font-size:18px;cursor:pointer;padding:4px;border-radius:6px;border:none;background:none;transition:background .1s;}
    .react-picker-btn:hover{background:var(--surface2);}
    .react-wrap{position:relative;}
    /* KUDOS WALL */
    .kudos-card{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:1rem 1.25rem;margin-bottom:10px;box-shadow:var(--shadow);animation:fadeIn .2s ease;}
    .kudos-header{display:flex;align-items:center;gap:10px;margin-bottom:6px;}
    .kudos-avatar{width:32px;height:32px;border-radius:50%;background:var(--accent);display:flex;align-items:center;justify-content:center;font-size:13px;font-weight:700;color:#fff;flex-shrink:0;}
    .kudos-meta{font-size:11px;color:var(--text-faint);}
    .kudos-task{font-size:13px;color:var(--text);margin-bottom:6px;}
    .kudos-reactions{display:flex;gap:6px;flex-wrap:wrap;}
    .kudos-pill{font-size:12px;padding:2px 10px;border-radius:99px;background:var(--accent-light);color:var(--accent);border:1px solid #c5dba8;font-weight:500;}
    /* APPRECIATION SUMMARY (replaces leaderboard) */
    .appr-row{display:flex;align-items:center;gap:12px;padding:10px 0;border-bottom:1px solid var(--border);}
    .appr-row:last-child{border-bottom:none;}
    .appr-avatar{width:34px;height:34px;border-radius:50%;background:var(--accent);display:flex;align-items:center;justify-content:center;font-size:14px;font-weight:700;color:#fff;flex-shrink:0;}
    .appr-info{flex:1;}
    .appr-name{font-size:13px;font-weight:600;color:var(--text);}
    .appr-sub{font-size:11px;color:var(--text-muted);}
    .appr-emojis{font-size:16px;letter-spacing:2px;}
    /* Kudos nav tab */
    .kudos-tabs{display:flex;gap:6px;margin-bottom:1.25rem;border-bottom:1px solid var(--border);}
    .kudos-tab{font-family:'DM Sans',sans-serif;font-size:13px;padding:8px 16px;border:none;background:none;color:var(--text-muted);cursor:pointer;border-bottom:2px solid transparent;margin-bottom:-1px;transition:all .15s;}
    .kudos-tab.active{color:var(--accent);border-bottom-color:var(--accent);font-weight:500;}
    /* entry-item needs relative for picker */
    .entry-item{position:relative;}
  </style>
</head>
<body>

<!-- LOGIN -->
<div class="login-screen" id="loginScreen">
  <div class="login-box">
    <div class="login-logo">
      <div class="login-logo-mark">
        <svg viewBox="0 0 24 24"><path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2"/><rect x="9" y="3" width="6" height="4" rx="1"/><line x1="9" y1="12" x2="15" y2="12"/><line x1="9" y1="16" x2="13" y2="16"/></svg>
      </div>
      <div>
        <div style="font-size:15px;font-weight:600;letter-spacing:-.02em;">FWA Tracker</div>
        <div style="font-size:11px;color:var(--text-muted);" id="loginOrgSub">FWA Tracker</div>
      </div>
    </div>
    <div class="login-tabs">
      <button class="login-tab active" id="ltab-login" onclick="switchLoginTab('login')">Sign in</button>
      <button class="login-tab" id="ltab-register" onclick="switchLoginTab('register')">Create account</button>
      <button class="login-tab" id="ltab-forgot" onclick="switchLoginTab('forgot')">Forgot password</button>
    </div>
    <div id="lpane-login">
      <div class="lfield"><label>Username</label><input type="text" id="loginUser" placeholder="Enter your username" /></div>
      <div class="lfield"><label>Password</label><input type="password" id="loginPass" placeholder="Enter your password" onkeydown="if(event.key==='Enter')doLogin()" /></div>
      <button class="lbtn" onclick="doLogin()">Sign in</button>
      <span class="forgot-link" onclick="switchLoginTab('forgot')">Forgot your password?</span>
      <div class="lmsg" id="loginMsg"></div>
    </div>
    <div id="lpane-register" style="display:none;">
      <div class="lfield"><label>Full name</label><input type="text" id="regName" placeholder="e.g. Bea Valencia" /></div>
      <div class="lfield"><label>Email address <span style="color:#c0392b;">*</span></label><input type="text" id="regEmail" placeholder="e.g. bea@email.com" autocomplete="email" /></div>
      <div style="font-size:11px;color:var(--text-muted);margin:-8px 0 10px;line-height:1.5;">Your email is used to sync and restore your data across devices and to send you a copy of your credentials.</div>
      <div class="lfield"><label>Username</label><input type="text" id="regUser" placeholder="Choose a username" /></div>
      <div class="lfield"><label>Password</label><input type="password" id="regPass" placeholder="Choose a password (min 4 chars)" /></div>
      <div class="lfield"><label>Confirm password</label><input type="password" id="regPass2" placeholder="Repeat password" onkeydown="if(event.key==='Enter')doRegister()" /></div>
      <button class="lbtn" onclick="doRegister()">Create account</button>
      <div class="lmsg" id="registerMsg"></div>
    </div>
    <div id="lpane-forgot" style="display:none;">
      <div style="font-size:12px;color:var(--text-muted);margin-bottom:14px;line-height:1.6;">Enter your registered email address and we'll send your username and a temporary password to that address.</div>
      <div class="lfield"><label>Email address</label><input type="text" id="forgotEmail" placeholder="e.g. bea@email.com" onkeydown="if(event.key==='Enter')doForgotPassword()" /></div>
      <button class="lbtn" onclick="doForgotPassword()">Send reset email</button>
      <div class="lmsg" id="forgotMsg"></div>
    </div>
  </div>
</div>

<!-- APP -->
<div id="app">
  <header class="site-header">
    <div class="logo">
      <div class="logo-mark">
        <svg viewBox="0 0 24 24"><path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2"/><rect x="9" y="3" width="6" height="4" rx="1"/><line x1="9" y1="12" x2="15" y2="12"/><line x1="9" y1="16" x2="13" y2="16"/></svg>
      </div>
      <div>
        <div class="logo-text">FWA Tracker</div>
        <div class="logo-sub" id="appOrgSub">FWA Tracker</div>
      </div>
    </div>
    <div class="header-right">
      <div class="header-nav" id="headerNav">
        <button class="hnav-btn" onclick="showPage('dashboard')" id="hnav-dashboard">Dashboard</button>
        <button class="hnav-btn" onclick="showPage('add')" id="hnav-add">Add</button>
        <button class="hnav-btn" onclick="showPage('view')" id="hnav-view">Entries</button>
        <button class="hnav-btn" onclick="showPage('kudos')" id="hnav-kudos">🏅 Kudos</button>
        <button class="hnav-btn" onclick="showPage('team')" id="hnav-team">Team</button>
        <button class="hnav-btn" onclick="showPage('export')" id="hnav-export">PDF</button>
        <button class="hnav-btn" onclick="showPage('teamexport')" id="hnav-teamexport">Sheets</button>
      </div>
      <div class="user-pill">
        <div class="user-avatar" id="userAvatar">?</div>
        <span id="userLabel" class="user-label-text">—</span>
        <span id="userEmailPill" style="font-size:10px;color:var(--text-faint);max-width:160px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;" class="user-label-text"></span>
      </div>
      <span id="syncBadge" style="font-size:11px;transition:opacity .5s;opacity:0;"></span>
      <button class="logout-btn" onclick="doLogout()">Sign out</button>
    </div>
  </header>

  <div class="layout">
    <aside class="sidebar">
      <div class="sidebar-section">
        <div class="sidebar-label">Tracker</div>
        <div class="sidebar-item" onclick="showPage('dashboard')" id="nav-dashboard">
          <svg class="sidebar-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="7" height="9"/><rect x="14" y="3" width="7" height="5"/><rect x="14" y="12" width="7" height="9"/><rect x="3" y="16" width="7" height="5"/></svg>
          Dashboard
        </div>
        <div class="sidebar-item active" onclick="showPage('add')" id="nav-add">
          <svg class="sidebar-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/></svg>
          Add deliverable
        </div>
        <div class="sidebar-item" onclick="showPage('kudos')" id="nav-kudos">
          <svg class="sidebar-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/></svg>
          Kudos Wall
        </div>
        <div class="sidebar-item" onclick="showPage('view')" id="nav-view">
          <svg class="sidebar-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="7" height="7"/><rect x="14" y="3" width="7" height="7"/><rect x="3" y="14" width="7" height="7"/><rect x="14" y="14" width="7" height="7"/></svg>
          All entries
        </div>
      </div>
      <div class="sidebar-section">
        <div class="sidebar-label">Team</div>
        <div class="sidebar-item" onclick="showPage('team')" id="nav-team">
          <svg class="sidebar-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M17 21v-2a4 4 0 00-4-4H5a4 4 0 00-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 00-3-3.87"/><path d="M16 3.13a4 4 0 010 7.75"/></svg>
          Team deliverables
        </div>
      </div>
      <div class="sidebar-section">
        <div class="sidebar-label">Export</div>
        <div class="sidebar-item" onclick="showPage('export')" id="nav-export">
          <svg class="sidebar-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M14 2H6a2 2 0 00-2 2v16a2 2 0 002 2h12a2 2 0 002-2V8z"/><polyline points="14 2 14 8 20 8"/><line x1="12" y1="12" x2="12" y2="18"/><line x1="9" y1="15" x2="15" y2="15"/></svg>
          PDF (FWA format)
        </div>
        <div class="sidebar-item" onclick="showPage('teamexport')" id="nav-teamexport">
          <svg class="sidebar-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="18" height="18" rx="2"/><line x1="3" y1="9" x2="21" y2="9"/><line x1="3" y1="15" x2="21" y2="15"/><line x1="9" y1="3" x2="9" y2="21"/></svg>
          Team → Excel
        </div>
      </div>
      <div class="sidebar-section">
        <div class="sidebar-label">Account</div>
        <div class="sidebar-item" onclick="showPage('profile')" id="nav-profile">
          <svg class="sidebar-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M20 21v-2a4 4 0 00-4-4H8a4 4 0 00-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>
          My Profile
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

        <!-- Motivational banner -->
        <div id="motiveBanner" style="background:var(--accent-light);border:1px solid #c5dba8;border-radius:var(--radius);padding:12px 18px;margin-bottom:1.25rem;display:flex;align-items:center;gap:12px;">
          <span style="font-size:20px;">💪</span>
          <span id="motiveText" style="font-size:13px;color:var(--accent);font-weight:500;line-height:1.5;"></span>
        </div>
        <div class="card">
          <div class="card-title">Report header</div>
          <div class="form-grid">
            <div class="field"><label>Name</label><input type="text" id="hName" placeholder="Full name" /></div>
            <div class="field"><label>Office / Unit</label><input type="text" id="hOffice" placeholder="e.g. OVPDx" /></div>
          </div>
          <div class="form-grid full" style="margin-bottom:14px;">
            <div class="field"><label>For the period of</label>
              <select id="hPeriod" onchange="if(document.getElementById('page-view').classList.contains('active'))renderView()">
                <option value="">Select week...</option>
              </select>
            </div>
          </div>
          <div class="card-title" style="margin-top:4px;">Work arrangement (per day)</div>
          <div class="day-grid">
            <div class="day-cell"><label>Mon</label><select id="dMon"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>SURP</option><option>ITDC</option><option>Rest Day</option></select></div>
            <div class="day-cell"><label>Tue</label><select id="dTue"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>SURP</option><option>ITDC</option><option>Rest Day</option></select></div>
            <div class="day-cell"><label>Wed</label><select id="dWed"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>SURP</option><option>ITDC</option><option>Rest Day</option></select></div>
            <div class="day-cell"><label>Thu</label><select id="dThu"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>SURP</option><option>ITDC</option><option>Rest Day</option></select></div>
            <div class="day-cell"><label>Fri</label><select id="dFri"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>SURP</option><option>ITDC</option><option>Rest Day</option></select></div>
            <div class="day-cell"><label>Sat</label><select id="dSat"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>SURP</option><option>ITDC</option><option>Rest Day</option></select></div>
            <div class="day-cell"><label>Sun</label><select id="dSun"><option value="">—</option><option>WFH</option><option>Office</option><option>Field</option><option>SURP</option><option>ITDC</option><option>Rest Day</option></select></div>
          </div>
        </div>

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
            <label>Verification photos — portrait images are auto-cropped to landscape (4:3)</label>
            <div class="upload-zone" id="uploadZone" onclick="document.getElementById('fileInput').click()" ondragover="onDragOver(event)" ondragleave="onDragLeave(event)" ondrop="onDrop(event)">
              <input type="file" id="fileInput" accept="image/*" multiple onchange="onFileChange(event)" />
              <div class="upload-zone-text">Click to upload or drag &amp; drop · <strong>PNG, JPG, WEBP</strong> · Multiple allowed · Auto-cropped to 4:3 landscape</div>
            </div>
            <div class="thumb-row" id="thumbRow"></div>
          </div>
          <button class="btn btn-primary" onclick="addEntry()">+ Add entry</button>
        </div>

        <div id="recent-list"></div>

        <div class="card">
          <div class="card-title">Signature block</div>
          <div class="form-grid" style="margin-bottom:12px;">
            <div class="field"><label>Submitted by (name)</label><input type="text" id="sigSubmitted" placeholder="e.g. Juan dela Cruz" /></div>
            <div class="field"><label>Submitted by (position)</label><input type="text" id="sigSubmittedPos" placeholder="e.g. Administrative Aide" /></div>
          </div>
          <div class="form-grid" style="margin-bottom:12px;">
            <div class="field"><label>Reviewed by (name)</label><input type="text" id="sigReviewed" placeholder="e.g. Maria Santos" /></div>
            <div class="field"><label>Reviewed by (position)</label><input type="text" id="sigReviewedPos" placeholder="e.g. Project Development Officer" /></div>
          </div>
          <div class="sig-fixed-box">
            <div class="sig-fixed-title">Approved by — fixed (set in Admin config)</div>
            <div class="sig-fixed-name" id="sigApprovedName">—</div>
            <div class="sig-fixed-role" id="sigApprovedRole"></div>
            <div class="sig-fixed-note">Editable by admin in Settings.</div>
          </div>
        </div>
      </div>

      <!-- VIEW PAGE -->
      <div class="page" id="page-view">
        <div class="page-header">
          <div class="page-title">All entries</div>
          <div class="page-desc" id="viewDesc">Entries for the current period.</div>
        </div>
        <div class="stats-row" id="stats"></div>
        <div id="view-list"></div>
      </div>

      <!-- EXPORT PAGE -->
      <div class="page" id="page-export">
        <div class="page-header">
          <div class="page-title">Export</div>
          <div class="page-desc">Generate your official UP FWA Accomplishment Report PDF.</div>
        </div>
        <div class="card">
          <div class="export-note">Generates the official UP FWA Accomplishment Report — work arrangement table, activity log with verification photos inside the Remarks column, and signature block with positions.</div>
          <div class="preview-wrap" id="pdf-preview-table"><div class="empty-state">No entries yet for this period.</div></div>
          <div class="btn-group">
            <button class="btn btn-primary" onclick="exportPDF()">⬇ Download PDF</button>
            <button class="btn" onclick="buildPDFPreview()">Refresh preview</button>
          </div>
        </div>
      </div>

      <!-- TEAM DELIVERABLES PAGE -->
      <div class="page" id="page-team">
        <div class="page-header">
          <div class="page-title">Team deliverables</div>
          <div class="page-desc">Log deliverables per team and person. Summarized by person for export.</div>
        </div>
        <div class="card">
          <div class="card-title">Week / period</div>
          <div class="form-grid full" style="margin-bottom:0;">
            <div class="field"><label>For the period of</label>
              <select id="tPeriod" onchange="renderTeamTables()">
                <option value="">Select week...</option>
              </select>
            </div>
          </div>
        </div>

        <div class="team-tabs" id="teamTabs"></div>

        <!-- Add row form -->
        <div class="card" id="teamAddForm">
          <div class="card-title" id="teamAddTitle">Add entry</div>
          <div class="form-grid">
            <div class="field"><label>Person name</label>
              <select id="tPerson">
                <option value="">Select person</option>
              </select>
            </div>
            <div class="field"><label>Project</label><input type="text" id="tProject" placeholder="e.g. Dx Labs '25" /></div>
          </div>
          <div class="form-grid full" style="margin-bottom:12px;">
            <div class="field"><label>Target deliverable</label><input type="text" id="tDeliverable" placeholder="Describe the deliverable..." /></div>
          </div>
          <div class="form-grid three" style="margin-bottom:12px;">
            <div class="field"><label>Nature of Task</label>
              <select id="tNature">
                <option value="">Select nature...</option>
                <option value="Strategy-based">Strategy-based</option>
                <option value="Project-based">Project-based</option>
                <option value="Routine-based">Routine-based</option>
              </select>
            </div>
            <div class="field"><label>Status</label>
              <select id="tStatus">
                <option value="Completed">Completed</option>
                <option value="Ongoing Progress" selected>Ongoing Progress</option>
                <option value="Not Initiated">Not Initiated</option>
              </select>
            </div>
            <div class="field"><label>Due date (optional)</label>
              <input type="date" id="tDueDate" />
            </div>
          </div>
          <div class="form-grid" style="margin-bottom:12px;">
            <div class="field"><label>Assignees</label>
              <select id="tAssignees">
                <option value="">Select assignee</option>
              </select>
            </div>
            <div class="field"><label>MOV (Mode of Verification)</label>
              <input type="text" id="tMov" placeholder="e.g. Minutes, Report, Screenshot..." />
            </div>
          </div>
          <button class="btn btn-primary" onclick="addTeamRow()">+ Add entry</button>
        </div>

        <!-- Summary per person -->
        <div id="teamTableArea"></div>
      </div>

      <!-- TEAM EXPORT PAGE -->
      <div class="page" id="page-teamexport">
        <div class="page-header">
          <div class="page-title">Team → Excel</div>
          <div class="page-desc">Download team deliverables as an Excel file (.xlsx) — one sheet per team, each with person columns side by side.</div>
        </div>
        <div class="card">
          <div class="card-title">Export period</div>
          <div class="form-grid full" style="margin-bottom:0;">
            <div class="field"><label>Period</label>
              <select id="tPeriodExport">
                <option value="">Select week...</option>
              </select>
            </div>
          </div>
          <div style="margin-top:12px;font-size:12px;color:var(--text-muted);">Each team gets its own sheet tab. Within each sheet, person names appear as column headers side by side with their Project, Target Deliverables, Status, and Assignees listed below.</div>
        </div>
        <div class="card">
          <div style="margin-bottom:14px;" id="exportPreviewArea">
            <div class="empty-state" style="padding:1.5rem 0;">Click "Download Excel" to preview and export.</div>
          </div>
          <div class="btn-group">
            <button class="btn btn-primary" onclick="exportExcel()">⬇ Download Excel (.xlsx)</button>
            <button class="btn" onclick="previewExport()">Preview</button>
          </div>
        </div>
      </div>
      <!-- DASHBOARD PAGE -->
      <div class="page" id="page-dashboard">
        <div class="page-header">
          <div class="page-title">Dashboard</div>
          <div class="page-desc">Overview of your activity across all weeks.</div>
        </div>
        <div class="stats-row" id="dash-totals"></div>
        <div class="dash-grid">
          <div class="dash-card">
            <div class="dash-card-title">Entries by project</div>
            <div id="dash-by-project"><div class="empty-state" style="padding:1rem 0;">No data yet.</div></div>
          </div>
          <div class="dash-card">
            <div class="dash-card-title">Status breakdown</div>
            <div id="dash-by-status"></div>
          </div>
        </div>
        <div class="dash-card" style="margin-bottom:1rem;">
          <div class="dash-card-title">Weekly activity (last 8 weeks)</div>
          <div id="dash-by-week"></div>
        </div>
      </div>

      <!-- KUDOS WALL PAGE -->
      <div class="page" id="page-kudos">
        <div class="page-header">
          <div class="page-title">Kudos Wall</div>
          <div class="page-desc">Every task you complete moves the team forward. Your work matters — keep going! React to entries to show your teammates you see their effort.</div>
        </div>
        <div class="kudos-tabs">
          <button class="kudos-tab active" id="ktab-wall" onclick="switchKudosTab('wall')">🏅 Recognition Wall</button>
          <button class="kudos-tab" id="ktab-appreciation" onclick="switchKudosTab('appreciation')">💛 Team Appreciation</button>
        </div>
        <!-- Wall -->
        <div id="kudos-wall-pane">
          <div class="card" style="margin-bottom:1rem;">
            <div class="card-title">Filter by week</div>
            <div class="field" style="max-width:320px;">
              <select id="kudosPeriod" onchange="renderKudosWall()">
                <option value="">All weeks</option>
              </select>
            </div>
          </div>
          <div id="kudos-wall-list"></div>
        </div>
        <!-- Team Appreciation summary — no ranking, just a warm overview -->
        <div id="kudos-appreciation-pane" style="display:none;">
          <div class="card" style="margin-bottom:1rem;">
            <div class="card-title">Your team shows up, every single week ✨</div>
            <div style="font-size:12px;color:var(--text-muted);margin-bottom:1rem;line-height:1.65;">Progress isn't always loud. Every entry here is proof someone showed up and delivered. That counts — and so does every reaction below.</div>
            <div id="kudos-appr-list"></div>
          </div>
          <div class="card">
            <div class="card-title">Most-used reactions across the team</div>
            <div style="font-size:12px;color:var(--text-muted);margin-bottom:1rem;">How the team has been showing appreciation.</div>
            <div id="kudos-emoji-breakdown"></div>
          </div>
        </div>
      </div>

      <!-- PROFILE PAGE -->
      <div class="page" id="page-profile">
        <div class="page-header">
          <div class="page-title">My Profile</div>
          <div class="page-desc">Update your display name and change your password.</div>
        </div>
        <div class="card">
          <div class="card-title">Account info</div>
          <div style="font-size:12px;color:var(--text-muted);margin-bottom:14px;">Logged in as: <strong id="profileUsername"></strong> &nbsp;·&nbsp; <span id="profileEmail"></span></div>
          <div class="form-grid full" style="margin-bottom:12px;">
            <div class="field"><label>Display name</label><input type="text" id="profileName" placeholder="Your full name" /></div>
          </div>
          <button class="btn btn-primary" onclick="saveProfileName()">Save name</button>
          <div id="profileNameMsg" style="font-size:12px;margin-top:8px;min-height:16px;"></div>
        </div>
        <div class="card">
          <div class="card-title">Change password</div>
          <div class="form-grid full" style="margin-bottom:12px;">
            <div class="field"><label>Current password</label><input type="password" id="profileOldPass" placeholder="Enter current password" /></div>
          </div>
          <div class="form-grid" style="margin-bottom:12px;">
            <div class="field"><label>New password</label><input type="password" id="profileNewPass" placeholder="Min 4 characters" /></div>
            <div class="field"><label>Confirm new password</label><input type="password" id="profileNewPass2" placeholder="Repeat new password" /></div>
          </div>
          <button class="btn btn-primary" onclick="saveProfilePassword()">Change password</button>
          <div id="profilePassMsg" style="font-size:12px;margin-top:8px;min-height:16px;"></div>
        </div>
      </div>

      <!-- hidden TSV element kept for compatibility -->
      <pre id="team-tsv" style="display:none;"></pre>

    </main>
  </div>
</div>

<!-- Emoji Picker Modal -->
<div class="modal-overlay" id="emojiPickerModal">
  <div class="modal-box" style="max-width:320px;">
    <div class="modal-title">React to this entry</div>
    <div class="modal-desc" id="emojiPickerDesc"></div>
    <div style="display:grid;grid-template-columns:repeat(5,1fr);gap:6px;margin-bottom:1rem;">
      <button class="react-picker-btn" style="font-size:22px;" onclick="pickEmoji('👏')">👏</button>
      <button class="react-picker-btn" style="font-size:22px;" onclick="pickEmoji('🔥')">🔥</button>
      <button class="react-picker-btn" style="font-size:22px;" onclick="pickEmoji('💪')">💪</button>
      <button class="react-picker-btn" style="font-size:22px;" onclick="pickEmoji('⭐')">⭐</button>
      <button class="react-picker-btn" style="font-size:22px;" onclick="pickEmoji('🎉')">🎉</button>
      <button class="react-picker-btn" style="font-size:22px;" onclick="pickEmoji('❤️')">❤️</button>
      <button class="react-picker-btn" style="font-size:22px;" onclick="pickEmoji('🙌')">🙌</button>
      <button class="react-picker-btn" style="font-size:22px;" onclick="pickEmoji('💡')">💡</button>
      <button class="react-picker-btn" style="font-size:22px;" onclick="pickEmoji('🚀')">🚀</button>
      <button class="react-picker-btn" style="font-size:22px;" onclick="pickEmoji('✅')">✅</button>
    </div>
    <div style="font-size:11px;color:var(--text-faint);margin-bottom:12px;">Click again to remove your reaction.</div>
    <div class="modal-footer">
      <button class="btn" onclick="closeEmojiPicker()">Cancel</button>
    </div>
  </div>
</div>

<!-- Undo Toast -->
<div class="undo-toast" id="undoToast">
  <span id="undoMsg">Entry deleted.</span>
  <button class="undo-btn" onclick="undoDelete()">Undo</button>
</div>

<!-- Edit Entry Modal -->
<div class="modal-overlay" id="editEntryModal">
  <div class="modal-box">
    <div class="modal-title">Edit entry</div>
    <input type="hidden" id="editEntryId" />
    <div class="form-grid" style="margin-bottom:10px;">
      <div class="field"><label>Date</label><input type="text" id="editDate" placeholder="e.g. March 17" /></div>
      <div class="field"><label>Project</label><input type="text" id="editProject" /></div>
    </div>
    <div class="field" style="margin-bottom:10px;"><label>Activity / Task</label><input type="text" id="editDesc" /></div>
    <div class="form-grid" style="margin-bottom:10px;">
      <div class="field"><label>Status</label>
        <select id="editStatus">
          <option value="ongoing">O – Ongoing/In-Process</option>
          <option value="completed">C – Completed</option>
          <option value="recurring">R – Recurring</option>
          <option value="notinit">Not initiated</option>
        </select>
      </div>
      <div class="field"><label>Remarks</label><input type="text" id="editNotes" placeholder="Link, verification..." /></div>
    </div>
    <div class="modal-footer">
      <button class="btn" onclick="closeEditModal()">Cancel</button>
      <button class="btn btn-primary" onclick="saveEditEntry()">Save changes</button>
    </div>
  </div>
</div>

<!-- Generic Modal -->
<div class="modal-overlay" id="modalOverlay">
  <div class="modal-box">
    <div class="modal-title" id="modalTitle"></div>
    <div class="modal-desc" id="modalDesc"></div>
    <div id="modalBody"></div>
    <div class="modal-footer" id="modalFooter">
      <button class="btn btn-primary" onclick="closeModal()">OK</button>
    </div>
  </div>
</div>

<!-- Lightbox -->
<div class="lightbox" id="lightbox" onclick="closeLightbox()">
  <button class="lightbox-close" onclick="closeLightbox()">×</button>
  <img id="lightboxImg" src="" alt="Verification photo" />
</div>

<script>
// ── AUTH ──────────────────────────────────
function getUsers(){return JSON.parse(localStorage.getItem('fwa_users')||'{}');}
function saveUsers(u){localStorage.setItem('fwa_users',JSON.stringify(u));}
function getCurrentUser(){return localStorage.getItem('fwa_current_user')||null;}
function setCurrentUser(u){localStorage.setItem('fwa_current_user',u);}

function isValidEmail(email){ return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email.trim()); }

// Cloud storage keys — tied to email so data is portable across devices/browsers
function emailKey(email, suffix){ return 'fwa_'+suffix+'__'+email.toLowerCase().trim(); }

function switchLoginTab(tab){
  ['login','register','forgot'].forEach(t=>{
    document.getElementById('ltab-'+t).classList.toggle('active',t===tab);
    document.getElementById('lpane-'+t).style.display=t===tab?'':'none';
  });
}

// ── MODAL ─────────────────────────────────
function showModal(title, desc, bodyHTML='', footerHTML=''){
  document.getElementById('modalTitle').textContent = title;
  document.getElementById('modalDesc').textContent = desc;
  document.getElementById('modalBody').innerHTML = bodyHTML;
  document.getElementById('modalFooter').innerHTML = footerHTML||'<button class="btn btn-primary" onclick="closeModal()">OK</button>';
  document.getElementById('modalOverlay').classList.add('open');
}
function closeModal(){ document.getElementById('modalOverlay').classList.remove('open'); }
document.addEventListener('keydown', e=>{ if(e.key==='Escape') closeModal(); });

// ── EMAILJS ───────────────────────────────
function getEjsConfig(){
  return {
    publicKey: APP_CONFIG.ejsPublicKey||'',
    serviceId: APP_CONFIG.ejsService||'',
    templateId: APP_CONFIG.ejsTemplate||''
  };
}

async function sendEmail({toEmail, toName, subject, username, password, message}){
  const cfg = getEjsConfig();
  if(!cfg.publicKey||!cfg.serviceId||!cfg.templateId){
    console.warn('EmailJS not configured — skipping email send.');
    return { ok: false, reason: 'not_configured' };
  }
  try {
    emailjs.init({ publicKey: cfg.publicKey });
    await emailjs.send(cfg.serviceId, cfg.templateId, {
      to_email: toEmail,
      to_name: toName||toEmail,
      subject: subject||'FWA Tracker',
      username: username||'',
      password: password||'',
      message: message||''
    });
    return { ok: true };
  } catch(err){
    console.error('EmailJS error:', err);
    return { ok: false, reason: err };
  }
}

function genTempPassword(len=10){
  const chars='ABCDEFGHJKMNPQRSTUVWXYZabcdefghjkmnpqrstuvwxyz23456789!@#';
  let p='';
  for(let i=0;i<len;i++) p+=chars[Math.floor(Math.random()*chars.length)];
  return p;
}

async function doLogin(){
  const user=document.getElementById('loginUser').value.trim();
  const pass=document.getElementById('loginPass').value;
  const msg=document.getElementById('loginMsg');
  if(!user||!pass){msg.className='lmsg err';msg.textContent='Please fill in all fields.';return;}
  const users=getUsers();
  if(!users[user]){msg.className='lmsg err';msg.textContent='Username not found.';return;}
  if(users[user].password!==btoa(pass)){msg.className='lmsg err';msg.textContent='Incorrect password.';return;}
  setCurrentUser(user);
  msg.className='lmsg ok';msg.textContent='Signing in and restoring your data…';
  await launchApp(user,users[user].name,users[user].email);
}

async function doRegister(){
  const name=document.getElementById('regName').value.trim();
  const email=document.getElementById('regEmail').value.trim().toLowerCase();
  const user=document.getElementById('regUser').value.trim();
  const pass=document.getElementById('regPass').value;
  const pass2=document.getElementById('regPass2').value;
  const msg=document.getElementById('registerMsg');
  if(!name||!email||!user||!pass||!pass2){msg.className='lmsg err';msg.textContent='Please fill in all fields.';return;}
  if(!isValidEmail(email)){msg.className='lmsg err';msg.textContent='Please enter a valid email address.';return;}
  if(pass!==pass2){msg.className='lmsg err';msg.textContent='Passwords do not match.';return;}
  if(pass.length<4){msg.className='lmsg err';msg.textContent='Password must be at least 4 characters.';return;}
  const users=getUsers();
  if(users[user]){msg.className='lmsg err';msg.textContent='Username already taken.';return;}
  const existingUser = Object.keys(users).find(u=>users[u].email===email);
  if(existingUser){msg.className='lmsg err';msg.textContent='An account with this email already exists.';return;}
  users[user]={name, email, password:btoa(pass)};
  saveUsers(users);
  msg.className='lmsg ok';msg.textContent='Account created! Sending welcome email…';

  // Always show credentials in a modal so user always has a copy
  showModal(
    '🎉 Account created!',
    'Please save your credentials below. A copy will also be sent to your email if EmailJS is configured.',
    `<div style="background:var(--surface2);border:1px solid var(--border);border-radius:8px;padding:14px 16px;font-size:13px;line-height:2;">
      <div><span style="color:var(--text-muted);width:90px;display:inline-block;">Full name</span> <strong>${escHtmlEntry(name)}</strong></div>
      <div><span style="color:var(--text-muted);width:90px;display:inline-block;">Username</span> <strong>${escHtmlEntry(user)}</strong></div>
      <div><span style="color:var(--text-muted);width:90px;display:inline-block;">Password</span> <strong style="font-family:monospace;font-size:14px;color:var(--accent);">${escHtmlEntry(pass)}</strong></div>
      <div><span style="color:var(--text-muted);width:90px;display:inline-block;">Email</span> <strong>${escHtmlEntry(email)}</strong></div>
    </div>
    <div style="font-size:11px;color:var(--text-faint);margin-top:10px;">⚠ Screenshot or copy these credentials before closing.</div>`,
    `<button class="btn btn-primary" onclick="closeModal()">Got it, I've saved them</button>`
  );

  // Also attempt to send via EmailJS (bonus — works if configured)
  const emailResult = await sendEmail({
    toEmail: email,
    toName: name,
    subject: 'FWA Tracker — Your account credentials',
    username: user,
    password: pass,
    message: `Hi ${name},\n\nYour FWA Tracker account has been created.\n\nUsername: ${user}\nPassword: ${pass}\nEmail: ${email}\n\nPlease keep this email for your records. You can use these credentials to sign in and your data will be automatically restored via your email.\n\nFWA Tracker`
  });

  if(emailResult.ok){
    msg.textContent='Account created! Credentials also sent to your email.';
  } else if(emailResult.reason==='not_configured'){
    msg.textContent='Account created! (To enable email sending, configure EmailJS credentials in the source code.)';
  } else {
    msg.textContent='Account created! (Email delivery failed — check EmailJS settings.)';
  }

  setTimeout(async()=>{setCurrentUser(user);await launchApp(user,name,email);},1200);
}

function doLogout(){
  localStorage.removeItem('fwa_current_user');
  document.getElementById('app').style.display='none';
  document.getElementById('loginScreen').style.display='flex';
  ['loginUser','loginPass'].forEach(id=>document.getElementById(id).value='');
  document.getElementById('loginMsg').textContent='';
  entries=[];pendingImages=[];
}

async function doForgotPassword(){
  const email = document.getElementById('forgotEmail').value.trim().toLowerCase();
  const msg = document.getElementById('forgotMsg');
  if(!email){ msg.className='lmsg err'; msg.textContent='Please enter your email address.'; return; }
  if(!isValidEmail(email)){ msg.className='lmsg err'; msg.textContent='Please enter a valid email address.'; return; }

  const users = getUsers();
  const username = Object.keys(users).find(u => users[u].email === email);
  if(!username){
    // Don't reveal whether the email exists — show same message for security
    msg.className='lmsg ok';
    msg.textContent='If that email is registered, a reset email has been sent.';
    return;
  }

  msg.className='lmsg ok'; msg.textContent='Sending reset email…';

  // Generate a new temporary password
  const tempPass = genTempPassword();
  users[username].password = btoa(tempPass);
  saveUsers(users);

  const result = await sendEmail({
    toEmail: email,
    toName: users[username].name||username,
    subject: 'FWA Tracker — Password Reset',
    username: username,
    password: tempPass,
    message: `Hi ${users[username].name||username},\n\nA password reset was requested for your FWA Tracker account.\n\nUsername: ${username}\nTemporary password: ${tempPass}\n\nPlease sign in with this temporary password and change it as soon as possible.\n\nIf you did not request this reset, please contact your admin.\n\nOVPDx FWA Tracker`
  });

  if(result.ok){
    msg.className='lmsg ok';
    msg.textContent='Reset email sent! Check your inbox.';
  } else if(result.reason==='not_configured'){
    // EmailJS not set up — show temp password in a modal as fallback
    showModal(
      'Password Reset',
      'EmailJS is not configured, so the email could not be sent. Share this temporary password securely:',
      `<div style="background:var(--surface2);border:1px solid var(--border);border-radius:6px;padding:10px 14px;font-size:13px;margin-bottom:4px;">
        <div style="margin-bottom:6px;"><strong>Username:</strong> ${username}</div>
        <div><strong>Temporary password:</strong> <span style="font-family:monospace;font-size:14px;color:var(--accent);">${tempPass}</span></div>
       </div>
       <div style="font-size:11px;color:var(--text-muted);margin-top:8px;">To enable email sending, configure EmailJS credentials in the source code.</div>`
    );
    msg.className='lmsg ok'; msg.textContent='Temporary password set. See the popup for credentials.';
  } else {
    msg.className='lmsg err'; msg.textContent='Could not send email. Check EmailJS configuration.';
  }
}

async function launchApp(username, fullname, email){
  document.getElementById('loginScreen').style.display='none';
  document.getElementById('app').style.display='block';
  document.getElementById('userLabel').textContent=fullname||username;
  document.getElementById('userAvatar').textContent=(fullname||username).charAt(0).toUpperCase();
  // Show email pill in header if available
  const emailPill = document.getElementById('userEmailPill');
  if(emailPill) emailPill.textContent = email||'';

  await loadAppConfig();

  // Load entries — prefer email-keyed cloud storage for cross-device restore
  entries = await loadEntriesByEmail(username, email);
  await loadReactions();

  const users=getUsers();
  if(users[username]&&users[username].name) document.getElementById('hName').value=users[username].name;
  generateWeekOptions();
  document.getElementById('hnav-add').classList.add('active');
  renderRecent();
  initMotive();
}

window.addEventListener('DOMContentLoaded',async ()=>{
  const u=getCurrentUser();
  if(u){const users=getUsers();if(users[u]){await launchApp(u,users[u].name,users[u].email);return;}}
  document.getElementById('loginScreen').style.display='flex';
  document.getElementById('hPeriod').addEventListener('change',()=>{if(document.getElementById('page-view').classList.contains('active'))renderView();});
});

// ── WEEK DROPDOWN ─────────────────────────
function generateWeekOptions() {
  const MONTHS = ['January','February','March','April','May','June','July','August','September','October','November','December'];
  const options = [];

  // Generate Mon–Fri weeks from Jan 2026 to Dec 2027
  const start = new Date(2026, 0, 1); // Jan 1 2026
  const end   = new Date(2027, 11, 31); // Dec 31 2027

  // Find first Monday on or after start
  let d = new Date(start);
  const day = d.getDay();
  if (day !== 1) d.setDate(d.getDate() + ((1 - day + 7) % 7));

  while (d <= end) {
    const mon = new Date(d);
    const fri = new Date(d); fri.setDate(fri.getDate() + 4);

    const monMonth = MONTHS[mon.getMonth()];
    const friMonth = MONTHS[fri.getMonth()];
    const monDay   = mon.getDate();
    const friDay   = fri.getDate();
    const year     = fri.getFullYear();

    let label;
    if (mon.getMonth() === fri.getMonth()) {
      label = `${monMonth} ${monDay}–${friDay}, ${year}`;
    } else {
      label = `${monMonth} ${monDay} – ${friMonth} ${friDay}, ${year}`;
    }
    options.push(label);
    d.setDate(d.getDate() + 7);
  }

  // Populate all period selects
  ['hPeriod','tPeriod','tPeriodExport','kudosPeriod'].forEach(id => {
    const sel = document.getElementById(id);
    if (!sel) return;
    const current = sel.value;
    sel.innerHTML = '<option value="">Select week...</option>' +
      options.map(o => `<option value="${o}"${o===current?' selected':''}>${o}</option>`).join('');
  });

  // Auto-select current week if available
  autoSelectCurrentWeek();
}

function autoSelectCurrentWeek() {
  const today = new Date();
  const MONTHS = ['January','February','March','April','May','June','July','August','September','October','November','December'];
  // Find Monday of current week
  const d = new Date(today);
  const day = d.getDay();
  const diff = (day === 0) ? -6 : 1 - day;
  d.setDate(d.getDate() + diff);
  const mon = new Date(d);
  const fri = new Date(d); fri.setDate(fri.getDate() + 4);
  let label;
  if (mon.getMonth() === fri.getMonth()) {
    label = `${MONTHS[mon.getMonth()]} ${mon.getDate()}–${fri.getDate()}, ${fri.getFullYear()}`;
  } else {
    label = `${MONTHS[mon.getMonth()]} ${mon.getDate()} – ${MONTHS[fri.getMonth()]} ${fri.getDate()}, ${fri.getFullYear()}`;
  }
  ['hPeriod','tPeriod','tPeriodExport','kudosPeriod'].forEach(id => {
    const sel = document.getElementById(id);
    if (!sel) return;
    // Only auto-select if not already set
    if (!sel.value) {
      for (const opt of sel.options) {
        if (opt.value === label) { sel.value = label; break; }
      }
    }
  });
}
let entries=[],pendingImages=[];
const SL={ongoing:'Ongoing / In-Process',completed:'Completed',recurring:'Recurring',notinit:'Not initiated'};
const SC={ongoing:'O',completed:'C',recurring:'R',notinit:'N'};
const STATUS_ORDER=['completed','ongoing','recurring','notinit'];

// ── CLOUD STORAGE (cross-device sync via window.storage) ──────────────
async function save(){
  const u=getCurrentUser();
  if(!u) return;
  const users=getUsers();
  const email=(users[u]&&users[u].email)||null;
  const json=JSON.stringify(entries);
  // Save to username key (legacy fallback)
  localStorage.setItem('fwa_entries_'+u, json);
  try { await window.storage.set('fwa_entries_'+u, json); } catch(e) {}
  // Also save to email key (primary cross-device key)
  if(email){
    const ekey=emailKey(email,'entries');
    localStorage.setItem(ekey, json);
    try { await window.storage.set(ekey, json); showSyncBadge(true); } catch(e) { showSyncBadge(false); }
  }
}

async function loadEntriesByEmail(username, email) {
  // 1. Try email-keyed cloud storage first (most authoritative — cross-device)
  if(email){
    const ekey = emailKey(email,'entries');
    try {
      const res = await window.storage.get(ekey);
      if(res && res.value){ showSyncBadge(true); return JSON.parse(res.value); }
    } catch(e){}
    // 2. Try email-keyed localStorage
    const local = localStorage.getItem(emailKey(email,'entries'));
    if(local) return JSON.parse(local);
  }
  // 3. Fall back to username-keyed cloud
  try {
    const res = await window.storage.get('fwa_entries_'+username);
    if(res && res.value) return JSON.parse(res.value);
  } catch(e){}
  // 4. Final fallback: username-keyed localStorage
  return JSON.parse(localStorage.getItem('fwa_entries_'+username)||'[]');
}

// Keep old loadEntries as alias for backward compat
async function loadEntries(username){ return loadEntriesByEmail(username, null); }

async function saveTeamDataCloud() {
  localStorage.setItem('fwa_team_data', JSON.stringify(teamData));
  try { await window.storage.set('fwa_team_data', JSON.stringify(teamData)); } catch(e) {}
}

async function loadTeamDataCloud() {
  try {
    const res = await window.storage.get('fwa_team_data');
    if (res && res.value) { teamData = JSON.parse(res.value); return; }
  } catch(e) {}
  teamData = JSON.parse(localStorage.getItem('fwa_team_data') || '{}');
}

// Show a small sync indicator
function showSyncBadge(ok) {
  let badge = document.getElementById('syncBadge');
  if (!badge) return;
  badge.textContent = ok ? '✓ Synced' : '⚠ Saved locally';
  badge.style.color = ok ? 'var(--accent)' : '#b8860b';
  badge.style.opacity = '1';
  clearTimeout(badge._t);
  badge._t = setTimeout(()=>{ badge.style.opacity='0'; }, 2500);
}
function getPeriod(){return document.getElementById('hPeriod').value.trim()||'(Period not set)';}
function getHeader(){
  return{
    name:document.getElementById('hName').value.trim(),
    office:document.getElementById('hOffice').value.trim(),
    period:document.getElementById('hPeriod').value.trim(),
    submitted:document.getElementById('sigSubmitted').value.trim(),
    submittedPos:document.getElementById('sigSubmittedPos').value.trim(),
    reviewed:document.getElementById('sigReviewed').value.trim(),
    reviewedPos:document.getElementById('sigReviewedPos').value.trim(),
    days:{Mon:document.getElementById('dMon').value,Tue:document.getElementById('dTue').value,Wed:document.getElementById('dWed').value,Thu:document.getElementById('dThu').value,Fri:document.getElementById('dFri').value,Sat:document.getElementById('dSat').value,Sun:document.getElementById('dSun').value}
  };
}

// ── IMAGE PROCESSING (portrait → 4:3 landscape crop) ──
function processImage(file){
  return new Promise(resolve=>{
    const reader=new FileReader();
    reader.onload=ev=>{
      const img=new Image();
      img.onload=()=>{
        const W=img.width,H=img.height;
        const TARGET=4/3;
        let sx,sy,sw,sh;
        // Always crop to 4:3 from center regardless of orientation
        if(W/H>=TARGET){
          // wider than 4:3 — crop sides
          sh=H;sw=Math.round(H*TARGET);
          sx=Math.round((W-sw)/2);sy=0;
        } else {
          // taller than 4:3 (portrait) — crop top/bottom
          sw=W;sh=Math.round(W/TARGET);
          sx=0;sy=Math.round((H-sh)/2);
        }
        const outW=Math.min(sw,1200),outH=Math.round(outW/TARGET);
        const canvas=document.createElement('canvas');
        canvas.width=outW;canvas.height=outH;
        canvas.getContext('2d').drawImage(img,sx,sy,sw,sh,0,0,outW,outH);
        resolve({dataUrl:canvas.toDataURL('image/jpeg',0.88),name:file.name});
      };
      img.src=ev.target.result;
    };
    reader.readAsDataURL(file);
  });
}

function onDragOver(e){e.preventDefault();document.getElementById('uploadZone').classList.add('dragover');}
function onDragLeave(e){document.getElementById('uploadZone').classList.remove('dragover');}
function onDrop(e){e.preventDefault();document.getElementById('uploadZone').classList.remove('dragover');handleFiles(e.dataTransfer.files);}
function onFileChange(e){handleFiles(e.target.files);e.target.value='';}
async function handleFiles(files){
  for(const file of Array.from(files)){
    if(!file.type.startsWith('image/'))continue;
    const p=await processImage(file);
    pendingImages.push(p);renderThumbs();
  }
}
function renderThumbs(){
  document.getElementById('thumbRow').innerHTML=pendingImages.map((img,i)=>`
    <div class="thumb">
      <img src="${img.dataUrl}" onclick="openLightbox('${img.dataUrl}')" />
      <button class="thumb-del" onclick="removePending(${i})">×</button>
      <span class="thumb-badge">4:3</span>
    </div>`).join('');
}
function removePending(i){pendingImages.splice(i,1);renderThumbs();}
function openLightbox(src){document.getElementById('lightboxImg').src=src;document.getElementById('lightbox').classList.add('open');}
function closeLightbox(){document.getElementById('lightbox').classList.remove('open');}

// ── ENTRIES ──────────────────────────────
async function addEntry(){
  const desc=document.getElementById('fDesc').value.trim(),project=document.getElementById('fProject').value.trim(),
        status=document.getElementById('fStatus').value,notes=document.getElementById('fNotes').value.trim(),
        date=document.getElementById('fDate').value.trim();
  if(!desc){alert('Please describe the activity/task.');return;}
  if(!project){alert('Please enter a project name.');return;}
  entries.push({id:Date.now(),project,desc,status,notes,date,period:getPeriod(),owner:getCurrentUser(),images:pendingImages.map(i=>({dataUrl:i.dataUrl,name:i.name}))});
  await save();
  showSyncBadge(true);
  ['fDesc','fNotes','fProject','fDate'].forEach(id=>document.getElementById(id).value='');
  pendingImages=[];renderThumbs();renderRecent();
}
async function deleteEntry(id){
  const deleted = entries.find(e=>e.id===id);
  entries=entries.filter(e=>e.id!==id);
  await save();
  showSyncBadge(true);
  renderRecent();
  renderView();
  if(deleted) showUndoToast(deleted);
}

// ── UNDO DELETE ───────────────────────────
let _undoEntry = null, _undoTimer = null;
function showUndoToast(entry){
  _undoEntry = entry;
  clearTimeout(_undoTimer);
  const toast = document.getElementById('undoToast');
  toast.classList.add('show');
  _undoTimer = setTimeout(()=>{ toast.classList.remove('show'); _undoEntry=null; }, 5000);
}
async function undoDelete(){
  if(!_undoEntry) return;
  entries.push(_undoEntry);
  entries.sort((a,b)=>a.id-b.id);
  _undoEntry = null;
  clearTimeout(_undoTimer);
  document.getElementById('undoToast').classList.remove('show');
  await save();
  renderRecent(); renderView();
}

// ── EDIT ENTRY ────────────────────────────
function openEditModal(id){
  const e = entries.find(x=>x.id===id);
  if(!e) return;
  document.getElementById('editEntryId').value = id;
  document.getElementById('editDate').value    = e.date||'';
  document.getElementById('editProject').value = e.project||'';
  document.getElementById('editDesc').value    = e.desc||'';
  document.getElementById('editStatus').value  = e.status||'ongoing';
  document.getElementById('editNotes').value   = e.notes||'';
  document.getElementById('editEntryModal').classList.add('open');
}
function closeEditModal(){ document.getElementById('editEntryModal').classList.remove('open'); }
async function saveEditEntry(){
  const id = parseInt(document.getElementById('editEntryId').value);
  const idx = entries.findIndex(x=>x.id===id);
  if(idx<0) return;
  entries[idx] = {
    ...entries[idx],
    date:    document.getElementById('editDate').value.trim(),
    project: document.getElementById('editProject').value.trim(),
    desc:    document.getElementById('editDesc').value.trim(),
    status:  document.getElementById('editStatus').value,
    notes:   document.getElementById('editNotes').value.trim()
  };
  await save(); showSyncBadge(true);
  closeEditModal(); renderRecent(); renderView();
}

function badgeClass(s){return{ongoing:'badge-ongoing',completed:'badge-completed',recurring:'badge-recurring',notinit:'badge-notinit'}[s]||'badge-notinit';}

function itemHTML(e){
  const ic=(e.images||[]).length;
  const ib=ic?`<span class="badge badge-photo">${ic} photo${ic>1?'s':''}</span>`:'';
  const th=(e.images||[]).map(img=>`<img class="entry-img" src="${img.dataUrl}" onclick="openLightbox('${img.dataUrl}')" />`).join('');
  const reactRow = reactionRowHTML(e.id).replace('<div class="react-row"','<div class="react-row" id="react-row-'+e.id+'"');
  return `<div class="entry-item">
    <div class="entry-body">
      <div class="entry-tags">
        <span class="badge badge-project">${escHtmlEntry(e.project)}</span>
        <span class="badge ${badgeClass(e.status)}">${SC[e.status]} – ${SL[e.status]}</span>
        ${e.date?`<span class="badge badge-date">${e.date}</span>`:''}
        ${ib}
        <span style="font-size:11px;color:var(--text-faint);">${e.period}</span>
      </div>
      <div class="entry-desc">${escHtmlEntry(e.desc)}</div>
      ${e.notes?`<div class="entry-notes">${escHtmlEntry(e.notes)}</div>`:''}
      ${th?`<div class="entry-imgs">${th}</div>`:''}
      ${reactRow}
    </div>
    <div style="display:flex;gap:4px;flex-shrink:0;">
      <button class="entry-edit" onclick="openEditModal(${e.id})" title="Edit">✏</button>
      <button class="entry-del" onclick="deleteEntry(${e.id})" title="Delete">×</button>
    </div>
  </div>`;
}
function escHtmlEntry(s){ return String(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }

// ── ACKNOWLEDGEMENTS / REACTIONS ──────────
// reactions stored globally, keyed by entryId
// structure: { [entryId]: { [emoji]: [ usernames... ] } }
let reactions = {};
let _emojiTargetId = null;

async function loadReactions(){
  try {
    const res = await window.storage.get('fwa_reactions');
    if(res && res.value) reactions = JSON.parse(res.value);
  } catch(e){
    reactions = JSON.parse(localStorage.getItem('fwa_reactions')||'{}');
  }
}
async function saveReactions(){
  const json = JSON.stringify(reactions);
  localStorage.setItem('fwa_reactions', json);
  try { await window.storage.set('fwa_reactions', json); } catch(e){}
}

function getReactionsForEntry(entryId){
  return reactions[entryId] || {};
}

function reactionRowHTML(entryId){
  const me = getCurrentUser();
  const r = getReactionsForEntry(entryId);
  const emojis = Object.keys(r).filter(em => r[em] && r[em].length > 0);
  const pills = emojis.map(em => {
    const users = r[em];
    const iMine = users.includes(me);
    const names = users.join(', ');
    return `<button class="react-btn${iMine?' reacted':''}" onclick="toggleReaction(${entryId},'${em}')" title="${escHtmlEntry(names)}">${em} <span class="react-count">${users.length}</span></button>`;
  }).join('');
  return `<div class="react-row">
    ${pills}
    <button class="react-add" onclick="openEmojiPicker(${entryId})" title="Add reaction">+ React</button>
  </div>`;
}

function openEmojiPicker(entryId){
  _emojiTargetId = entryId;
  const entry = entries.find(e=>e.id===entryId);
  document.getElementById('emojiPickerDesc').textContent = entry ? `"${entry.desc.slice(0,60)}${entry.desc.length>60?'…':''}"` : '';
  document.getElementById('emojiPickerModal').classList.add('open');
}
function closeEmojiPicker(){ document.getElementById('emojiPickerModal').classList.remove('open'); _emojiTargetId=null; }

async function pickEmoji(emoji){
  if(_emojiTargetId===null) return;
  await toggleReaction(_emojiTargetId, emoji);
  closeEmojiPicker();
}

async function toggleReaction(entryId, emoji){
  const me = getCurrentUser();
  if(!me) return;
  if(!reactions[entryId]) reactions[entryId] = {};
  if(!reactions[entryId][emoji]) reactions[entryId][emoji] = [];
  const idx = reactions[entryId][emoji].indexOf(me);
  if(idx>=0){
    reactions[entryId][emoji].splice(idx,1);
    if(!reactions[entryId][emoji].length) delete reactions[entryId][emoji];
  } else {
    reactions[entryId][emoji].push(me);
  }
  await saveReactions();
  // Refresh just the reaction row in place if visible
  const rowEl = document.getElementById('react-row-'+entryId);
  if(rowEl) rowEl.outerHTML = reactionRowHTML(entryId).replace('<div class="react-row"','<div class="react-row" id="react-row-'+entryId+'"');
  // Also update kudos wall if open
  const kudosPage = document.getElementById('page-kudos');
  if(kudosPage && kudosPage.classList.contains('active')) renderKudosWall();
}

// ── KUDOS WALL ────────────────────────────
function switchKudosTab(tab){
  document.getElementById('kudos-wall-pane').style.display          = tab==='wall'?'':'none';
  document.getElementById('kudos-appreciation-pane').style.display  = tab==='appreciation'?'':'none';
  document.getElementById('ktab-wall').classList.toggle('active', tab==='wall');
  document.getElementById('ktab-appreciation').classList.toggle('active', tab==='appreciation');
  if(tab==='appreciation') renderKudosAppreciation();
  else renderKudosWall();
}

function renderKudosWall(){
  const sel = document.getElementById('kudosPeriod');
  const filterPeriod = sel ? sel.value : '';
  // Only entries that have at least one reaction, OR all entries if no filter
  let pool = filterPeriod ? entries.filter(e=>e.period===filterPeriod) : [...entries];
  // Sort: most reacted first
  pool = pool.sort((a,b)=>{
    const ra = Object.values(reactions[a.id]||{}).reduce((s,u)=>s+u.length,0);
    const rb = Object.values(reactions[b.id]||{}).reduce((s,u)=>s+u.length,0);
    return rb-ra;
  });
  const list = document.getElementById('kudos-wall-list');
  if(!pool.length){ list.innerHTML='<div class="empty-state">No entries for this period.</div>'; return; }
  list.innerHTML = pool.map(e=>{
    const r = reactions[e.id]||{};
    const totalReacts = Object.values(r).reduce((s,u)=>s+u.length,0);
    const pills = Object.keys(r).filter(em=>r[em].length>0).map(em=>`<span class="kudos-pill">${em} ${r[em].length}</span>`).join('');
    const me = getCurrentUser();
    const iMine = Object.values(r).some(u=>u.includes(me));
    return `<div class="kudos-card">
      <div class="kudos-header">
        <div class="kudos-avatar">${(e.project||'?').charAt(0).toUpperCase()}</div>
        <div>
          <div style="font-size:13px;font-weight:600;color:var(--text);">${escHtmlEntry(e.project)}</div>
          <div class="kudos-meta">${e.period}${e.date?' · '+e.date:''} ${totalReacts>0?'· '+totalReacts+' reaction'+(totalReacts>1?'s':''):''}</div>
        </div>
      </div>
      <div class="kudos-task">${escHtmlEntry(e.desc)}</div>
      ${pills?`<div class="kudos-reactions">${pills}</div>`:'<div style="font-size:11px;color:var(--text-faint);">No reactions yet — be the first!</div>'}
      <div style="margin-top:10px;display:flex;gap:6px;flex-wrap:wrap;" id="react-row-${e.id}">
        ${Object.keys(r).filter(em=>r[em].length>0).map(em=>{
          const users=r[em]; const iM=users.includes(me);
          return `<button class="react-btn${iM?' reacted':''}" onclick="toggleReaction(${e.id},'${em}')" title="${users.join(', ')}">${em} <span class="react-count">${users.length}</span></button>`;
        }).join('')}
        <button class="react-add" onclick="openEmojiPicker(${e.id})">+ React</button>
      </div>
    </div>`;
  }).join('');
}

function renderKudosAppreciation(){
  // Gather all reactions per person — no ranking, just a warm summary
  const personMap = {}; // username → { totalReacts, emojiCounts: {emoji: count} }
  entries.forEach(e => {
    const owner = e.owner || getCurrentUser();
    const r = reactions[e.id] || {};
    Object.entries(r).forEach(([em, users]) => {
      if(!users.length) return;
      if(!personMap[owner]) personMap[owner] = { totalReacts: 0, emojiCounts: {} };
      personMap[owner].totalReacts += users.length;
      personMap[owner].emojiCounts[em] = (personMap[owner].emojiCounts[em]||0) + users.length;
    });
  });

  const people = Object.entries(personMap);
  // Shuffle so no implied order — appreciation is not a competition
  for(let i = people.length-1; i>0; i--){
    const j = Math.floor(Math.random()*(i+1));
    [people[i],people[j]] = [people[j],people[i]];
  }

  const apprList = document.getElementById('kudos-appr-list');
  if(!people.length){
    apprList.innerHTML='<div class="empty-state" style="padding:1rem 0;">No reactions yet — head to the Recognition Wall and appreciate someone\'s work! 🌟</div>';
  } else {
    const users = getUsers();
    apprList.innerHTML = people.map(([u, data])=>{
      const displayName = (users[u]&&users[u].name)||u;
      const topEmojis = Object.entries(data.emojiCounts).sort((a,b)=>b[1]-a[1]).slice(0,5).map(([em])=>em).join(' ');
      return `<div class="appr-row">
        <div class="appr-avatar">${displayName.charAt(0).toUpperCase()}</div>
        <div class="appr-info">
          <div class="appr-name">${escHtmlEntry(displayName)}</div>
          <div class="appr-sub">${data.totalReacts} reaction${data.totalReacts!==1?'s':''} received across their entries</div>
        </div>
        <div class="appr-emojis">${topEmojis}</div>
      </div>`;
    }).join('');
  }

  // Emoji breakdown — team-wide
  const emojiMap = {};
  entries.forEach(e => {
    const r = reactions[e.id]||{};
    Object.entries(r).forEach(([em,users])=>{ if(users.length) emojiMap[em]=(emojiMap[em]||0)+users.length; });
  });
  const emojiRanked = Object.entries(emojiMap).sort((a,b)=>b[1]-a[1]);
  const maxE = emojiRanked[0]?emojiRanked[0][1]:1;
  const breakdown = document.getElementById('kudos-emoji-breakdown');
  breakdown.innerHTML = emojiRanked.length
    ? emojiRanked.map(([em,c])=>`
        <div class="dash-bar-row">
          <div class="dash-bar-label" style="width:44px;font-size:18px;">${em}</div>
          <div class="dash-bar-track"><div class="dash-bar-fill" style="width:${Math.round(c/maxE*100)}%"></div></div>
          <div class="dash-bar-count">${c}</div>
        </div>`).join('')
    : '<div class="empty-state" style="padding:1rem 0;">No reactions yet.</div>';
}

function getPeriodEntries(){return entries.filter(e=>e.period===getPeriod());}

function renderRecent(){
  const el=document.getElementById('recent-list'),recent=[...entries].reverse().slice(0,6);
  el.innerHTML=recent.length
    ?`<div style="font-size:11px;font-weight:600;letter-spacing:.06em;text-transform:uppercase;color:var(--text-faint);margin-bottom:8px;">Recent entries</div>`+recent.map(itemHTML).join('')
    :'<div class="empty-state">No entries yet. Add one above.</div>';
}

// ── MOTIVATIONAL BANNER ───────────────────
const MOTIVE_MESSAGES = [
  'Every task you complete moves the team forward. Your work matters — keep going!',
  'Progress isn\'t always loud. Every entry here is proof you showed up and delivered. That counts.',
  'Every task you complete moves the team forward. Keep showing up — it makes a difference!',
  'Progress isn\'t always visible right away, but every effort you log brings the team closer to the goal.',
  'Your work matters. Every deliverable you complete is a step forward for the whole team.',
  'Showing up consistently is one of the most powerful things you can do. Thank you for being here.',
  'Great teams are built on consistent effort — just like yours. Keep it going!',
  'Behind every deliverable is a person who chose to show up. That person is you. 🙌',
];
let _motiveIdx = Math.floor(Math.random() * MOTIVE_MESSAGES.length);
function rotateMotive() {
  const el = document.getElementById('motiveText');
  if (!el) return;
  _motiveIdx = (_motiveIdx + 1) % MOTIVE_MESSAGES.length;
  el.style.opacity = '0';
  setTimeout(() => {
    el.textContent = MOTIVE_MESSAGES[_motiveIdx];
    el.style.transition = 'opacity .5s';
    el.style.opacity = '1';
  }, 300);
}
function initMotive() {
  const el = document.getElementById('motiveText');
  if (el) el.textContent = MOTIVE_MESSAGES[_motiveIdx];
  setInterval(rotateMotive, 8000);
}

function renderView(){
  const we=getPeriodEntries(),sEl=document.getElementById('stats'),lEl=document.getElementById('view-list');
  document.getElementById('viewDesc').textContent='Showing entries for: '+getPeriod();
  sEl.innerHTML=`
    <div class="stat-card"><div class="stat-val">${we.length}</div><div class="stat-lbl">Total</div></div>
    <div class="stat-card"><div class="stat-val">${we.filter(e=>e.status==='completed').length}</div><div class="stat-lbl">Completed</div></div>
    <div class="stat-card"><div class="stat-val">${we.filter(e=>e.status==='ongoing').length}</div><div class="stat-lbl">Ongoing</div></div>
    <div class="stat-card"><div class="stat-val">${we.filter(e=>e.status==='recurring').length}</div><div class="stat-lbl">Recurring</div></div>
    <div class="stat-card"><div class="stat-val">${we.filter(e=>e.status==='notinit').length}</div><div class="stat-lbl">Not initiated</div></div>`;
  if(!we.length){lEl.innerHTML='<div class="empty-state">No entries for this period label.</div>';return;}
  lEl.innerHTML=[...new Set(we.map(e=>e.project))].map(proj=>{
    const pi=we.filter(e=>e.project===proj);
    return `<div class="proj-group"><div class="proj-group-header">${proj} <span class="proj-count">${pi.length} task${pi.length>1?'s':''}</span></div>${pi.map(itemHTML).join('')}</div>`;
  }).join('');
}

// ── PDF PREVIEW ───────────────────────────
function buildPDFPreview(){
  const we=getPeriodEntries(),h=getHeader(),el=document.getElementById('pdf-preview-table');
  if(!we.length){el.innerHTML='<div class="empty-state">No entries for this period.</div>';return;}
  let html=`<div style="font-size:12px;color:var(--text-muted);margin-bottom:10px;font-weight:500;">${h.name||'(Name)'} · ${h.office||'(Office)'} · ${h.period||'(Period)'}</div>`;
  html+=`<table class="preview-table"><thead><tr>
    <th style="width:70px;">Date</th><th>Activity / Task</th>
    <th style="width:30px;text-align:center;">O</th><th style="width:30px;text-align:center;">C</th><th style="width:30px;text-align:center;">R</th>
    <th>Remarks &amp; Photos</th>
  </tr></thead><tbody>`;
  we.forEach(e=>{
    const imgs=(e.images||[]);
    const th=imgs.map(img=>`<img src="${img.dataUrl}" style="width:60px;height:45px;object-fit:cover;border-radius:3px;margin:2px;cursor:pointer;" onclick="openLightbox('${img.dataUrl}')" />`).join('');
    html+=`<tr>
      <td>${e.date||''}</td>
      <td>${e.project?'<span style="color:#888;font-size:10px;">['+e.project+']</span><br>':''}${e.desc}</td>
      <td style="text-align:center;">${e.status==='ongoing'?'x':''}</td>
      <td style="text-align:center;">${e.status==='completed'?'x':''}</td>
      <td style="text-align:center;">${e.status==='recurring'?'x':''}</td>
      <td>${e.notes||''}${th?'<div style="margin-top:4px;">'+th+'</div>':''}</td>
    </tr>`;
  });
  html+=`</tbody></table>
  <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:16px;margin-top:16px;font-size:11px;">
    <div><div style="color:var(--text-muted);margin-bottom:18px;">Submitted by:</div><div style="border-top:1px solid var(--text);padding-top:4px;font-weight:600;">${h.submitted||'___________________'}</div><div style="color:var(--text-muted);font-size:10px;">${h.submittedPos||''}</div></div>
    <div><div style="color:var(--text-muted);margin-bottom:18px;">Reviewed by:</div><div style="border-top:1px solid var(--text);padding-top:4px;font-weight:600;">${h.reviewed||'___________________'}</div><div style="color:var(--text-muted);font-size:10px;">${h.reviewedPos||''}</div></div>
    <div><div style="color:var(--text-muted);margin-bottom:18px;">Approved by:</div><div style="border-top:1px solid var(--text);padding-top:4px;font-weight:600;">${APP_CONFIG.approverName||'Peter A. Sy'}</div><div style="color:var(--text-muted);font-size:10px;">${APP_CONFIG.approverRole||'Vice President for Digital Transformation'}</div></div>
  </div>`;
  el.innerHTML=html;
}

// ── PDF EXPORT ────────────────────────────
async function exportPDF(){
  if(typeof window.jspdf==='undefined'){alert('PDF library loading, please try again.');return;}
  const{jsPDF}=window.jspdf,we=getPeriodEntries(),h=getHeader();
  if(!we.length){alert('No entries for this period.');return;}
  const doc=new jsPDF({orientation:'portrait',unit:'mm',format:'a4'});
  const pw=210,ml=15,mr=15,cW=pw-ml-mr;let y=18;

  doc.setFont('helvetica','bold');doc.setFontSize(11);
  doc.text(APP_CONFIG.univ||'UNIVERSITY OF THE PHILIPPINES',pw/2,y,{align:'center'});y+=5;
  doc.setFont('helvetica','normal');doc.setFontSize(9);
  doc.text(APP_CONFIG.officeHeader||'',pw/2,y,{align:'center'});y+=10;
  doc.setFontSize(10);
  doc.text('Name',ml,y);doc.line(ml+10,y+.5,ml+70,y+.5);doc.text(h.name,ml+12,y);
  doc.text('Office/Unit',ml+80,y);doc.line(ml+97,y+.5,ml+cW,y+.5);doc.text(h.office,ml+99,y);y+=10;
  doc.setFont('helvetica','bold');doc.setFontSize(11);
  doc.text('FLEXIBLE WORK ARRANGEMENT (FWA) ACCOMPLISHMENT REPORT',pw/2,y,{align:'center'});y+=5;
  doc.setFont('helvetica','normal');doc.setFontSize(10);
  const pl='For the Period of ';
  doc.text(pl,pw/2-30,y);doc.line(pw/2-30+doc.getTextWidth(pl),y+.5,pw/2+35,y+.5);
  doc.text(h.period,pw/2-30+doc.getTextWidth(pl)+2,y);y+=8;

  const days=['Mon','Tue','Wed','Thu','Fri','Sat','Sun'],dW=cW/7;
  doc.setFillColor(254,249,231);doc.rect(ml,y,cW,7,'F');
  doc.setDrawColor(150,150,150);doc.rect(ml,y,cW,14);
  doc.setFont('helvetica','bold');doc.setFontSize(9);doc.text('Work Arrangement*',pw/2,y+4,{align:'center'});
  doc.setFont('helvetica','italic');doc.setFontSize(7.5);doc.text("(as indicated in the Office/Unit's Regular Weekly FWA)",pw/2,y+6.5,{align:'center'});
  y+=7;
  days.forEach((d,i)=>{
    const x=ml+i*dW;doc.setDrawColor(150,150,150);doc.line(x,y,x,y+7);
    doc.setFont('helvetica','bold');doc.setFontSize(8);doc.text(d,x+dW/2,y+3.5,{align:'center'});
    doc.setFont('helvetica','normal');doc.setFontSize(7);doc.text(h.days[d]||'',x+dW/2,y+6.2,{align:'center'});
  });
  y+=10;

  const COL={date:22,task:68,o:10,c:10,r:10,rem:60};
  const IW=38,IH=28,IP=2;

  async function loadImg(u){return new Promise(r=>{const i=new Image();i.onload=()=>r(i);i.onerror=()=>r(null);i.src=u;});}
  const eid=[];
  for(const e of we){const ld=[];for(const img of(e.images||[])){const el=await loadImg(img.dataUrl);if(el)ld.push({dataUrl:img.dataUrl});}eid.push(ld);}

  const wks=[...new Set(we.map(e=>e.period))];
  const tb=[],rm=[];
  wks.forEach(wk=>{
    const wi=we.filter(e=>e.period===wk);
    tb.push([{content:`Week of ${wk}`,colSpan:6,styles:{fillColor:[254,230,150],textColor:[0,0,0],fontStyle:'bold',fontSize:8,cellPadding:3}}]);rm.push(-1);
    wi.forEach(e=>{
      tb.push([e.date||'',(e.project?'['+e.project+']\n':'')+e.desc,e.status==='ongoing'?'x':'',e.status==='completed'?'x':'',e.status==='recurring'?'x':'',e.notes||'']);
      rm.push(we.indexOf(e));
    });
  });

  doc.autoTable({
    startY:y,margin:{left:ml,right:mr},
    head:[[
      {content:'DATE\n(optional)',styles:{fillColor:[254,249,231],textColor:[0,0,0],fontStyle:'bold',halign:'center',fontSize:7}},
      {content:'ACTIVITY/ TASK',styles:{fillColor:[254,249,231],textColor:[0,0,0],fontStyle:'bold',halign:'center',fontSize:7}},
      {content:'O',styles:{fillColor:[254,249,231],textColor:[0,0,0],fontStyle:'bold',halign:'center',fontSize:7}},
      {content:'C',styles:{fillColor:[254,249,231],textColor:[0,0,0],fontStyle:'bold',halign:'center',fontSize:7}},
      {content:'R',styles:{fillColor:[254,249,231],textColor:[0,0,0],fontStyle:'bold',halign:'center',fontSize:7}},
      {content:'REMARKS\n(mode of verification / link to output)',styles:{fillColor:[254,249,231],textColor:[0,0,0],fontStyle:'bold',halign:'center',fontSize:7}}
    ]],
    body:tb,
    columnStyles:{0:{cellWidth:COL.date,fontSize:7,valign:'top'},1:{cellWidth:COL.task,fontSize:7,valign:'top'},2:{cellWidth:COL.o,halign:'center',fontSize:8,valign:'top'},3:{cellWidth:COL.c,halign:'center',fontSize:8,valign:'top'},4:{cellWidth:COL.r,halign:'center',fontSize:8,valign:'top'},5:{cellWidth:COL.rem,fontSize:7,valign:'top'}},
    styles:{lineColor:[180,180,180],lineWidth:.3,cellPadding:2,overflow:'linebreak'},
    headStyles:{lineColor:[180,180,180],lineWidth:.3},theme:'grid',
    didDrawCell:function(data){
      if(data.section!=='body'||data.column.index!==5)return;
      const bi=data.row.index;if(bi<0||bi>=rm.length)return;
      const ei=rm[bi];if(ei<0)return;
      const imgs=eid[ei];if(!imgs||!imgs.length)return;
      const cx=data.cell.x,cy=data.cell.y,cw=data.cell.width;
      const tl=(data.cell.text&&data.cell.text.length)?data.cell.text.length:1;
      let ix=cx+IP,iy=cy+tl*4+3;
      imgs.forEach(img=>{
        if(ix+IW>cx+cw-IP){ix=cx+IP;iy+=IH+IP;}
        try{doc.addImage(img.dataUrl,'JPEG',ix,iy,IW,IH);}catch(e2){}
        ix+=IW+IP;
      });
    },
    didParseCell:function(data){
      if(data.section!=='body')return;
      const bi=data.row.index;if(bi<0||bi>=rm.length)return;
      const ei=rm[bi];if(ei<0)return;
      const imgs=eid[ei];if(!imgs||!imgs.length)return;
      const ipr=Math.max(1,Math.floor(COL.rem/(IW+IP)));
      const ir=Math.ceil(imgs.length/ipr);
      data.cell.styles.minCellHeight=ir*(IH+IP)+16;
    }
  });

  let fy=doc.lastAutoTable.finalY+5;
  doc.setFontSize(7.5);doc.setFont('helvetica','italic');
  doc.text('* Work from Home, Satellite Office or Another Fixed Place within the Philippines',ml,fy);
  fy+=12;
  if(fy+40>280){doc.addPage();fy=20;}
  const colW2=cW/3;
  [{label:'Submitted by:',name:h.submitted||'',pos:h.submittedPos||''},
   {label:'Reviewed by:',name:h.reviewed||'',pos:h.reviewedPos||''},
   {label:'Approved by:',name:APP_CONFIG.approverName||'Peter A. Sy',pos:APP_CONFIG.approverRole||'Vice President for Digital Transformation'}
  ].forEach((col,i)=>{
    const x=ml+i*colW2;
    doc.setFont('helvetica','normal');doc.setFontSize(9);doc.text(col.label,x,fy);
    doc.line(x,fy+16,x+colW2-6,fy+16);
    doc.setFont('helvetica','bold');doc.setFontSize(9);doc.text(col.name,x,fy+21);
    doc.setFont('helvetica','normal');doc.setFontSize(8);
    if(col.pos)doc.text(col.pos,x,fy+26);
  });

  doc.save(`FWA_${(h.name||'Report').replace(/\s+/g,'_')}_${(h.period||'Period').replace(/[^a-z0-9]/gi,'_')}.pdf`);
}

// ── APP CONFIG (hardcoded) ────────────────
const APP_CONFIG = {
  org:          'OVPDx · UP System',
  univ:         'UNIVERSITY OF THE PHILIPPINES',
  officeHeader: 'Office of the Vice President for Digital Transformation',
  approverName: 'Peter A. Sy',
  approverRole: 'Vice President for Digital Transformation',
  ejsPublicKey: '',
  ejsService:   '',
  ejsTemplate:  ''
};

const TEAMS = ['Admin Team','Communications Team','Project Team','Research Team','Management Team'];
const TEAM_MEMBERS = {
  'Admin Team':          ['John Mark Paya','Paula Beatrize Valencia','Rozhelle Yu'],
  'Communications Team': ['Marianne Laron','Eileen Rudi'],
  'Project Team':        ['John Paul Cristobal','Duane Burdeos','Keith Andrei Layson'],
  'Research Team':       ['Katheryn Hidalgo','Veronica Consolacion'],
  'Management Team':     ['Marisha Beloro','Kristofferson Dela Cruz','Regine Pustadan']
};
const STATUSES = ['Completed','Ongoing Progress','Not Initiated'];

function loadAppConfig() { applyConfig(); }

function applyConfig() {
  const loginSub = document.getElementById('loginOrgSub');
  const appSub   = document.getElementById('appOrgSub');
  if (loginSub) loginSub.textContent = APP_CONFIG.org;
  if (appSub)   appSub.textContent   = APP_CONFIG.org;
  const an = document.getElementById('sigApprovedName');
  const ar = document.getElementById('sigApprovedRole');
  if (an) an.textContent = APP_CONFIG.approverName;
  if (ar) ar.textContent = APP_CONFIG.approverRole;
  // activeTeam init
  if (!activeTeam || !TEAMS.includes(activeTeam)) activeTeam = TEAMS[0];
}
let activeTeam = null;
// teamData: { period: { teamName: [{id, person, project, deliverable, status, assignees}] } }
let teamData = {};

function saveTeamData() { saveTeamDataCloud(); }
function loadTeamData() {
  // sync load from localStorage as immediate fallback; cloud loaded separately
  teamData = JSON.parse(localStorage.getItem('fwa_team_data') || '{}');
}
function getTPeriod() { return document.getElementById('tPeriod').value.trim() || '(Period not set)'; }

function ensureTeamPeriod(period, team) {
  if (!teamData[period]) teamData[period] = {};
  if (!teamData[period][team]) teamData[period][team] = [];
}

function updatePersonDropdown() {
  const members = TEAM_MEMBERS[activeTeam] || [];
  const opts = '<option value="">Select person</option>' + members.map(m => `<option value="${m}">${m}</option>`).join('');
  document.getElementById('tPerson').innerHTML = opts;
  const aOpts = '<option value="">Select assignee</option>' + members.map(m => `<option value="${m}">${m}</option>`).join('');
  document.getElementById('tAssignees').innerHTML = aOpts;
}

async function addTeamRow() {
  const period = getTPeriod();
  const person = document.getElementById('tPerson').value.trim();
  const project = document.getElementById('tProject').value.trim();
  const deliverable = document.getElementById('tDeliverable').value.trim();
  const nature = document.getElementById('tNature').value;
  const status = document.getElementById('tStatus').value;
  const assignees = document.getElementById('tAssignees').value.trim();
  const dueDate = document.getElementById('tDueDate').value;
  const mov = document.getElementById('tMov').value.trim();
  if (!person) { alert('Please select a person.'); return; }
  if (!deliverable) { alert('Please describe the deliverable.'); return; }
  ensureTeamPeriod(period, activeTeam);
  teamData[period][activeTeam].push({ id: Date.now(), person, project, deliverable, nature, status, assignees, dueDate, mov });
  await saveTeamDataCloud();
  showSyncBadge(true);
  ['tProject','tDeliverable','tMov'].forEach(id => document.getElementById(id).value = '');
  document.getElementById('tNature').value = '';
  document.getElementById('tStatus').value = 'Ongoing Progress';
  document.getElementById('tAssignees').value = '';
  document.getElementById('tDueDate').value = '';
  renderTeamTabs();
  renderTeamTables();
}

async function deleteTeamRow(team, id) {
  const period = getTPeriod();
  if (!teamData[period] || !teamData[period][team]) return;
  teamData[period][team] = teamData[period][team].filter(r => r.id !== id);
  await saveTeamDataCloud();
  showSyncBadge(true);
  renderTeamTabs();
  renderTeamTables();
}

function renderTeamTabs() {
  if (!TEAMS.length) {
    document.getElementById('teamTabs').innerHTML = '';
    document.getElementById('teamAddTitle').textContent = 'Add entry';
    return;
  }
  if (!activeTeam || !TEAMS.includes(activeTeam)) activeTeam = TEAMS[0];
  document.getElementById('teamTabs').innerHTML = TEAMS.map(t => {
    const period = getTPeriod();
    const count = (teamData[period] && teamData[period][t]) ? teamData[period][t].length : 0;
    return `<button class="team-tab${t===activeTeam?' active':''}" onclick="switchTeam('${t}')">${t}${count?' <span style="font-size:10px;opacity:.7;">('+count+')</span>':''}</button>`;
  }).join('');
  document.getElementById('teamAddTitle').textContent = `Add entry — ${activeTeam}`;
}

function switchTeam(team) { activeTeam = team; renderTeamTabs(); updatePersonDropdown(); renderTeamTables(); }

function statusBadge(s) {
  if (s==='Completed') return `<span style="background:#e8f5e9;color:#2e7d32;font-size:10px;font-weight:600;padding:2px 8px;border-radius:99px;">${s}</span>`;
  if (s==='Ongoing Progress') return `<span style="background:#fdf3d8;color:#7a5a0e;font-size:10px;font-weight:600;padding:2px 8px;border-radius:99px;">${s}</span>`;
  return `<span style="background:var(--surface2);color:var(--text-muted);font-size:10px;font-weight:600;padding:2px 8px;border-radius:99px;">${s}</span>`;
}

function natureBadge(n) {
  if (n==='Strategy-based') return `<span style="background:#e8eaf6;color:#3949ab;font-size:10px;font-weight:600;padding:2px 8px;border-radius:99px;">Strategy</span>`;
  if (n==='Project-based')  return `<span style="background:#e0f2f1;color:#00695c;font-size:10px;font-weight:600;padding:2px 8px;border-radius:99px;">Project</span>`;
  if (n==='Routine-based')  return `<span style="background:#fff3e0;color:#e65100;font-size:10px;font-weight:600;padding:2px 8px;border-radius:99px;">Routine</span>`;
  return `<span style="color:var(--text-faint);">—</span>`;
}

function renderTeamTables() {
  const period = getTPeriod();
  ensureTeamPeriod(period, activeTeam);
  const rows = teamData[period][activeTeam] || [];

  if (!rows.length) {
    document.getElementById('teamTableArea').innerHTML = `<div class="empty-state">No entries yet for ${activeTeam}. Add one above.</div>`;
    return;
  }

  // Group by person
  const people = [...new Set(rows.map(r => r.person))];

  let html = '';
  people.forEach(person => {
    const pRows = rows.filter(r => r.person === person);
    html += `<div class="card" style="padding:0;overflow:hidden;margin-bottom:1rem;">
      <div style="padding:10px 16px;border-bottom:1px solid var(--border);display:flex;align-items:center;justify-content:space-between;background:var(--surface2);">
        <div style="display:flex;align-items:center;gap:10px;">
          <div style="width:28px;height:28px;border-radius:50%;background:var(--accent);display:flex;align-items:center;justify-content:center;font-size:12px;font-weight:600;color:#fff;flex-shrink:0;">${person.charAt(0).toUpperCase()}</div>
          <span style="font-size:13px;font-weight:600;color:var(--text);">${escHtml(person)}</span>
        </div>
        <span style="font-size:11px;color:var(--text-muted);">${pRows.length} deliverable${pRows.length!==1?'s':''}</span>
      </div>
      <div class="team-table-wrap" style="border:none;border-radius:0;">
        <table class="team-table">
          <thead><tr>
            <th style="width:110px;">Project</th>
            <th>Target Deliverable</th>
            <th style="width:90px;">Nature</th>
            <th style="width:150px;">Status</th>
            <th style="width:90px;">Due Date</th>
            <th style="width:110px;">Assignees</th>
            <th style="width:140px;">MOV</th>
            <th style="width:32px;"></th>
          </tr></thead>
          <tbody>`;
    pRows.forEach(row => {
      const today = new Date(); today.setHours(0,0,0,0);
      const due = row.dueDate ? new Date(row.dueDate) : null;
      const isOverdue = due && due < today && row.status !== 'Completed';
      const dueTxt = row.dueDate
        ? `<span class="badge ${isOverdue?'badge-overdue':'badge-date'}">${isOverdue?'⚠ ':''}${row.dueDate}</span>`
        : '<span style="color:var(--text-faint);">—</span>';
      html += `<tr id="trow-${row.id}">
        <td style="font-size:12px;">${escHtml(row.project)||'<span style="color:var(--text-faint);">—</span>'}</td>
        <td style="font-size:12px;">${escHtml(row.deliverable)}</td>
        <td>${natureBadge(row.nature)}</td>
        <td id="tstat-${row.id}">${statusBadgeWithEdit(row.status, row.id)}</td>
        <td>${dueTxt}</td>
        <td style="font-size:12px;">${escHtml(row.assignees)||'<span style="color:var(--text-faint);">—</span>'}</td>
        <td style="font-size:12px;">${escHtml(row.mov)||'<span style="color:var(--text-faint);">—</span>'}</td>
        <td><button class="del-row-btn" onclick="deleteTeamRow('${activeTeam}',${row.id})">×</button></td>
      </tr>`;
    });
    html += `</tbody></table></div></div>`;
  });

  // Summary stats
  const total = rows.length;
  const done = rows.filter(r=>r.status==='Completed').length;
  const ongoing = rows.filter(r=>r.status==='Ongoing Progress').length;
  const notinit = rows.filter(r=>r.status==='Not Initiated').length;
  html = `<div style="display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:1rem;">
    <div class="stat-card"><div class="stat-val">${total}</div><div class="stat-lbl">Total</div></div>
    <div class="stat-card"><div class="stat-val">${done}</div><div class="stat-lbl">Completed</div></div>
    <div class="stat-card"><div class="stat-val">${ongoing}</div><div class="stat-lbl">Ongoing</div></div>
    <div class="stat-card"><div class="stat-val">${notinit}</div><div class="stat-lbl">Not initiated</div></div>
  </div>` + html;

  document.getElementById('teamTableArea').innerHTML = html;
}

function statusBadgeWithEdit(s, rowId) {
  return `<div style="display:flex;align-items:center;gap:6px;">
    ${statusBadge(s)}
    <button onclick="toggleStatusEdit(${rowId})" title="Edit status" style="background:none;border:1px solid var(--border);border-radius:4px;padding:2px 6px;font-size:10px;color:var(--text-muted);cursor:pointer;white-space:nowrap;transition:all .15s;" onmouseover="this.style.background='var(--accent-light)';this.style.borderColor='var(--accent)';this.style.color='var(--accent)'" onmouseout="this.style.background='none';this.style.borderColor='var(--border)';this.style.color='var(--text-muted)'">✏ Edit</button>
  </div>`;
}

function toggleStatusEdit(rowId) {
  const cell = document.getElementById('tstat-' + rowId);
  if (!cell) return;
  // Find current status
  const period = getTPeriod();
  let rowRef = null;
  for (const t of Object.keys(teamData[period]||{})) {
    const found = (teamData[period][t]||[]).find(r=>r.id===rowId);
    if (found) { rowRef = found; break; }
  }
  if (!rowRef) return;

  cell.innerHTML = `<div style="display:flex;align-items:center;gap:6px;">
    <select id="sedit-${rowId}" style="font-family:'DM Sans',sans-serif;font-size:12px;padding:4px 8px;border:1px solid var(--accent);border-radius:6px;background:var(--surface);color:var(--text);outline:none;box-shadow:0 0 0 2px rgba(45,80,22,.1);">
      ${STATUSES.map(s=>`<option value="${s}"${s===rowRef.status?' selected':''}>${s}</option>`).join('')}
    </select>
    <button onclick="confirmStatusEdit(${rowId})" style="background:var(--accent);color:#fff;border:none;border-radius:4px;padding:4px 10px;font-size:11px;font-weight:600;cursor:pointer;">Save</button>
    <button onclick="renderTeamTables()" style="background:none;border:1px solid var(--border);border-radius:4px;padding:4px 8px;font-size:11px;cursor:pointer;color:var(--text-muted);">Cancel</button>
  </div>`;
  document.getElementById('sedit-'+rowId).focus();
}

async function confirmStatusEdit(rowId) {
  const sel = document.getElementById('sedit-'+rowId);
  if (!sel) return;
  const newStatus = sel.value;
  const period = getTPeriod();
  let updated = false;
  for (const t of Object.keys(teamData[period]||{})) {
    const row = (teamData[period][t]||[]).find(r=>r.id===rowId);
    if (row) { row.status = newStatus; updated = true; break; }
  }
  if (updated) {
    await saveTeamDataCloud();
    showSyncBadge(true);
  }
  renderTeamTabs();
  renderTeamTables();
}

function escHtml(s) { return String(s||'').replace(/&/g,'&amp;').replace(/"/g,'&quot;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }

// Build TSV — person names as column headers side by side (matching screenshot layout)
function buildTeamTSV() {
  const period = getTPeriod();

  // Collect all persons across all teams in fixed order
  const allPersonBlocks = [];
  TEAMS.forEach(team => {
    const members = TEAM_MEMBERS[team] || [];
    members.forEach(person => {
      const rows = (teamData[period] && teamData[period][team])
        ? teamData[period][team].filter(r => r.person === person)
        : [];
      allPersonBlocks.push({ person, team, rows });
    });
  });

  if (!allPersonBlocks.length) {
    document.getElementById('team-tsv').textContent = 'No entries yet.';
    return '';
  }

  const lines = [];

  // Row 1: person names as headers — each person gets 6 cols, separated by blank col
  const headerR1 = [];
  allPersonBlocks.forEach((block, idx) => {
    headerR1.push(`"${block.person}"`, '""', '""', '""', '""', '""');
    if (idx < allPersonBlocks.length - 1) headerR1.push('""');
  });
  lines.push(headerR1.join('\t'));

  // Row 2: column sub-headers under each person
  const headerR2 = [];
  allPersonBlocks.forEach((block, idx) => {
    headerR2.push('"Project"', '"Target Deliverables"', '"Nature of Task"', '"Status"', '"Assignees"', '"MOV"');
    if (idx < allPersonBlocks.length - 1) headerR2.push('""');
  });
  lines.push(headerR2.join('\t'));

  // Data rows — expand to max row count across all persons
  const maxRows = Math.max(...allPersonBlocks.map(b => b.rows.length), 0);
  for (let i = 0; i < maxRows; i++) {
    const row = [];
    allPersonBlocks.forEach((block, idx) => {
      const r = block.rows[i];
      if (r) {
        row.push(
          `"${(r.project||'').replace(/"/g,'""')}"`,
          `"${(r.deliverable||'').replace(/"/g,'""')}"`,
          `"${(r.nature||'').replace(/"/g,'""')}"`,
          `"${(r.status||'').replace(/"/g,'""')}"`,
          `"${(r.assignees||'').replace(/"/g,'""')}"`,
          `"${(r.mov||'').replace(/"/g,'""')}"`
        );
      } else {
        row.push('""','""','""','""','""','""');
      }
      if (idx < allPersonBlocks.length - 1) row.push('""');
    });
    lines.push(row.join('\t'));
  }

  const tsv = lines.join('\n');
  document.getElementById('team-tsv').textContent = tsv;
  return tsv;
}

// ── EXCEL EXPORT — one sheet per team ────
function getExportPeriod() {
  const v = document.getElementById('tPeriodExport').value.trim();
  return v || getTPeriod();
}

function buildTeamSheetData(team, period) {
  const members = TEAM_MEMBERS[team] || [];
  const teamRows = (teamData[period] && teamData[period][team]) ? teamData[period][team] : [];

  // Each member gets 6 cols: Project, Target Deliverables, Nature of Task, Status, Assignees, MOV
  const maxRows = Math.max(...members.map(m => teamRows.filter(r => r.person === m).length), 0);
  const sheetData = [];

  // Row 1 — person names (span 6 cols each)
  const nameRow = [];
  members.forEach((m, idx) => {
    nameRow.push(m, '', '', '', '', '');
    if (idx < members.length - 1) nameRow.push('');
  });
  sheetData.push(nameRow);

  // Row 2 — sub-headers
  const subRow = [];
  members.forEach((m, idx) => {
    subRow.push('Project', 'Target Deliverables', 'Nature of Task', 'Status', 'Assignees', 'MOV');
    if (idx < members.length - 1) subRow.push('');
  });
  sheetData.push(subRow);

  // Data rows
  for (let i = 0; i < maxRows; i++) {
    const row = [];
    members.forEach((m, idx) => {
      const mRows = teamRows.filter(r => r.person === m);
      const r = mRows[i];
      if (r) {
        row.push(r.project||'', r.deliverable||'', r.nature||'', r.status||'', r.assignees||'', r.mov||'');
      } else {
        row.push('', '', '', '', '', '');
      }
      if (idx < members.length - 1) row.push('');
    });
    sheetData.push(row);
  }

  return sheetData;
}

function previewExport() {
  const period = getExportPeriod();
  loadTeamData();
  let html = '';
  TEAMS.forEach(team => {
    const members = TEAM_MEMBERS[team] || [];
    const rows = (teamData[period] && teamData[period][team]) ? teamData[period][team] : [];
    const total = rows.length;
    const people = members.filter(m => rows.some(r => r.person === m));
    html += `<div style="margin-bottom:10px;display:flex;align-items:center;justify-content:space-between;padding:8px 12px;background:var(--surface2);border-radius:var(--radius-sm);">
      <div>
        <span style="font-size:13px;font-weight:600;color:var(--text);">${team}</span>
        <span style="font-size:11px;color:var(--text-muted);margin-left:8px;">${people.length} person${people.length!==1?'s':''} · ${total} entr${total!==1?'ies':'y'}</span>
      </div>
      <span style="font-size:11px;color:var(--accent);font-weight:500;">1 sheet tab</span>
    </div>`;
  });
  html += `<div style="font-size:12px;color:var(--text-muted);margin-top:8px;">Period: <strong>${period}</strong> · ${TEAMS.length} sheet tabs total</div>`;
  document.getElementById('exportPreviewArea').innerHTML = html;
}

function exportExcel() {
  if (typeof XLSX === 'undefined') { alert('Excel library loading, please try again in a moment.'); return; }
  const period = getExportPeriod();
  loadTeamData();

  const wb = XLSX.utils.book_new();

  TEAMS.forEach(team => {
    const sheetData = buildTeamSheetData(team, period);
    const ws = XLSX.utils.aoa_to_sheet(sheetData);

    // Style: bold the first two header rows by setting cell styles
    const members = TEAM_MEMBERS[team] || [];
    const numCols = members.length * 4 + Math.max(0, members.length - 1);

    // Set column widths (6 data cols + 1 sep per member)
    ws['!cols'] = [];
    members.forEach((m, idx) => {
      ws['!cols'].push({wch:20},{wch:36},{wch:16},{wch:18},{wch:20},{wch:28});
      if (idx < members.length - 1) ws['!cols'].push({wch:3});
    });

    // Merge cells for person name headers (each name spans 6 cols)
    if (!ws['!merges']) ws['!merges'] = [];
    members.forEach((m, idx) => {
      const startCol = idx * 7; // 6 data cols + 1 sep
      ws['!merges'].push({
        s: {r:0, c:startCol},
        e: {r:0, c:startCol+5}
      });
    });

    XLSX.utils.book_append_sheet(wb, ws, team.substring(0,31));
  });

  const filename = `TeamDeliverables_${period.replace(/[^a-z0-9]/gi,'_')}.xlsx`;
  XLSX.writeFile(wb, filename);
  previewExport();
}

async function showPage(page){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.sidebar-item').forEach(i=>i.classList.remove('active'));
  document.querySelectorAll('.hnav-btn').forEach(i=>i.classList.remove('active'));
  document.getElementById('page-'+page).classList.add('active');
  const n=document.getElementById('nav-'+page);if(n)n.classList.add('active');
  const hn=document.getElementById('hnav-'+page);if(hn)hn.classList.add('active');
  if(page==='dashboard') renderDashboard();
  if(page==='kudos'){ await loadReactions(); switchKudosTab('wall'); }
  if(page==='view')renderView();
  if(page==='export')buildPDFPreview();
  if(page==='team'){
    await loadTeamDataCloud();
    renderTeamTabs();updatePersonDropdown();renderTeamTables();
  }
  if(page==='teamexport'){
    await loadTeamDataCloud();
    const tp = document.getElementById('tPeriod').value;
    if (tp) document.getElementById('tPeriodExport').value = tp;
    previewExport();
  }
  if(page==='profile'){ loadProfilePage(); }
}

// ── DASHBOARD ─────────────────────────────
function renderDashboard(){
  const all = entries;
  const total = all.length;
  const done  = all.filter(e=>e.status==='completed').length;
  const ongoing = all.filter(e=>e.status==='ongoing').length;
  const recurring = all.filter(e=>e.status==='recurring').length;
  const notinit = all.filter(e=>e.status==='notinit').length;

  document.getElementById('dash-totals').innerHTML=`
    <div class="stat-card"><div class="stat-val">${total}</div><div class="stat-lbl">All-time entries</div></div>
    <div class="stat-card"><div class="stat-val">${done}</div><div class="stat-lbl">Completed</div></div>
    <div class="stat-card"><div class="stat-val">${ongoing}</div><div class="stat-lbl">Ongoing</div></div>
    <div class="stat-card"><div class="stat-val">${recurring}</div><div class="stat-lbl">Recurring</div></div>
    <div class="stat-card"><div class="stat-val">${notinit}</div><div class="stat-lbl">Not initiated</div></div>`;

  // By project
  const projectMap = {};
  all.forEach(e=>{ projectMap[e.project]=(projectMap[e.project]||0)+1; });
  const projects = Object.entries(projectMap).sort((a,b)=>b[1]-a[1]).slice(0,8);
  const maxP = projects[0]?projects[0][1]:1;
  document.getElementById('dash-by-project').innerHTML = projects.length
    ? projects.map(([p,c])=>`
        <div class="dash-bar-row">
          <div class="dash-bar-label" title="${escHtmlEntry(p)}">${escHtmlEntry(p)}</div>
          <div class="dash-bar-track"><div class="dash-bar-fill" style="width:${Math.round(c/maxP*100)}%"></div></div>
          <div class="dash-bar-count">${c}</div>
        </div>`).join('')
    : '<div class="empty-state" style="padding:1rem 0;">No data yet.</div>';

  // By status
  const statuses = [
    {key:'completed',label:'Completed',count:done,color:'#4caf50'},
    {key:'ongoing',label:'Ongoing',count:ongoing,color:'#f0a500'},
    {key:'recurring',label:'Recurring',count:recurring,color:'#1565c0'},
    {key:'notinit',label:'Not initiated',count:notinit,color:'#a09a92'},
  ];
  const maxS = Math.max(...statuses.map(s=>s.count),1);
  document.getElementById('dash-by-status').innerHTML = statuses.map(s=>`
    <div class="dash-bar-row">
      <div class="dash-bar-label">${s.label}</div>
      <div class="dash-bar-track"><div class="dash-bar-fill" style="width:${Math.round(s.count/maxS*100)}%;background:${s.color}"></div></div>
      <div class="dash-bar-count">${s.count}</div>
    </div>`).join('');

  // By week — last 8 unique periods with entries
  const weekMap = {};
  all.forEach(e=>{ if(e.period) weekMap[e.period]=(weekMap[e.period]||0)+1; });
  const weeks = Object.entries(weekMap).slice(-8);
  const maxW = Math.max(...weeks.map(w=>w[1]),1);
  document.getElementById('dash-by-week').innerHTML = weeks.length
    ? weeks.map(([w,c])=>`
        <div class="dash-week-row">
          <span style="color:var(--text-muted);flex:1;">${w}</span>
          <div style="display:flex;align-items:center;gap:8px;">
            <div style="width:120px;height:6px;background:var(--surface2);border-radius:99px;overflow:hidden;">
              <div style="height:100%;width:${Math.round(c/maxW*100)}%;background:var(--accent);border-radius:99px;"></div>
            </div>
            <span style="font-size:12px;color:var(--text-muted);width:24px;text-align:right;">${c}</span>
          </div>
        </div>`).join('')
    : '<div class="empty-state" style="padding:1rem 0;">No entries yet.</div>';
}

// ── PROFILE PAGE ──────────────────────────
function loadProfilePage(){
  const u = getCurrentUser();
  const users = getUsers();
  if(!u||!users[u]) return;
  document.getElementById('profileUsername').textContent = u;
  document.getElementById('profileEmail').textContent = users[u].email||'(no email)';
  document.getElementById('profileName').value = users[u].name||'';
  document.getElementById('profileNameMsg').textContent='';
  document.getElementById('profilePassMsg').textContent='';
  ['profileOldPass','profileNewPass','profileNewPass2'].forEach(id=>document.getElementById(id).value='');
}

function saveProfileName(){
  const u = getCurrentUser();
  const users = getUsers();
  const name = document.getElementById('profileName').value.trim();
  const msg = document.getElementById('profileNameMsg');
  if(!name){ msg.style.color='#c0392b'; msg.textContent='Name cannot be empty.'; return; }
  users[u].name = name;
  saveUsers(users);
  document.getElementById('userLabel').textContent = name;
  document.getElementById('userAvatar').textContent = name.charAt(0).toUpperCase();
  msg.style.color='var(--accent)'; msg.textContent='✓ Name updated.';
  setTimeout(()=>msg.textContent='',3000);
}

function saveProfilePassword(){
  const u = getCurrentUser();
  const users = getUsers();
  const oldPass = document.getElementById('profileOldPass').value;
  const newPass = document.getElementById('profileNewPass').value;
  const newPass2 = document.getElementById('profileNewPass2').value;
  const msg = document.getElementById('profilePassMsg');
  if(!oldPass||!newPass||!newPass2){ msg.style.color='#c0392b'; msg.textContent='Please fill in all fields.'; return; }
  if(users[u].password!==btoa(oldPass)){ msg.style.color='#c0392b'; msg.textContent='Current password is incorrect.'; return; }
  if(newPass!==newPass2){ msg.style.color='#c0392b'; msg.textContent='New passwords do not match.'; return; }
  if(newPass.length<4){ msg.style.color='#c0392b'; msg.textContent='Password must be at least 4 characters.'; return; }
  users[u].password = btoa(newPass);
  saveUsers(users);
  ['profileOldPass','profileNewPass','profileNewPass2'].forEach(id=>document.getElementById(id).value='');
  msg.style.color='var(--accent)'; msg.textContent='✓ Password changed successfully.';
  setTimeout(()=>msg.textContent='',3000);
}
</script>
</body>
</html>
