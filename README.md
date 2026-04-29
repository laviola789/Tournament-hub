<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>La Viola Fleur de Lis • Tournament Hub</title>
  <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-database-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-storage-compat.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@600;800&family=Montserrat:wght@500;700&display=swap" rel="stylesheet">

  <style>
    :root {
      --primary: #8b5cf6;
      --primary-dark: #7c3aed;
      --accent: #a78bfa;
      --bg: #ffffff;
      --card: #faf5ff;
      --border: #ddd6fe;
      --text: #1f2937;
      --text-light: #6b7280;
    }

    * { margin:0; padding:0; box-sizing:border-box; }
    body {
      font-family: 'Segoe UI', system-ui, sans-serif;
      background: var(--bg);
      color: var(--text);
      line-height: 1.6;
      overflow-x: hidden;
    }

    .header {
      background: linear-gradient(135deg, #8b5cf6, #a78bfa);
      box-shadow: 0 4px 12px rgba(139, 92, 246, 0.2);
      position: sticky;
      top: 0;
      z-index: 100;
      padding: 1rem 0;
    }

    nav {
      max-width: 1280px;
      margin: 0 auto;
      display: flex;
      justify-content: center;
      gap: 1rem;
      flex-wrap: wrap;
    }

    nav button {
      padding: 12px 28px;
      background: rgba(255,255,255,0.2);
      color: white;
      border: 2px solid rgba(255,255,255,0.3);
      border-radius: 9999px;
      cursor: pointer;
      transition: all 0.3s ease;
      font-weight: 600;
    }

    nav button:hover, nav button.active {
      background: white;
      color: var(--primary);
      border-color: white;
      transform: translateY(-2px);
    }

    .container {
      max-width: 1280px;
      margin: 2rem auto;
      padding: 0 1.5rem;
    }

    h1 {
      font-size: 2.8rem;
      background: linear-gradient(90deg, #8b5cf6, #a78bfa);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      margin-bottom: 0.5rem;
      text-align: center;
    }

    h2, h3 { color: var(--primary); }

    .card {
      background: var(--card);
      border: 2px solid var(--border);
      border-radius: 16px;
      padding: 2rem;
      box-shadow: 0 10px 30px rgba(139,92,246,0.1);
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      margin-bottom: 1.5rem;
    }

    .card:hover {
      transform: translateY(-4px);
      box-shadow: 0 20px 40px rgba(139,92,246,0.15);
    }

    .section { display: none; }
    .section.active { display: block; }

    .upload-area {
      border: 2px dashed var(--primary);
      border-radius: 12px;
      padding: 2rem;
      text-align: center;
      background: rgba(139,92,246,0.05);
      transition: all 0.3s;
    }

    .preview {
      max-width: 240px;
      border-radius: 12px;
      border: 3px solid var(--primary);
      margin: 1rem auto;
      display: none;
      box-shadow: 0 0 20px rgba(139,92,246,0.3);
    }

    table {
      width: 100%;
      border-collapse: collapse;
      background: white;
      border-radius: 12px;
      overflow: hidden;
    }
    th {
      background: var(--primary);
      padding: 16px;
      text-align: left;
      color: white;
      font-weight: 600;
    }
    td {
      padding: 14px 16px;
      border-bottom: 1px solid var(--border);
    }
    tr:hover { background: var(--card); }

    input, select, textarea {
      width: 100%;
      padding: 14px;
      background: white;
      border: 2px solid var(--border);
      border-radius: 8px;
      color: var(--text);
      margin: 8px 0;
      transition: border-color 0.3s;
      font-family: inherit;
    }
    input:focus, select:focus, textarea:focus {
      outline: none;
      border-color: var(--primary);
    }

    button {
      padding: 12px 24px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-weight: 600;
      transition: all 0.3s;
    }

    .btn-primary { background: var(--primary); color: white; }
    .btn-primary:hover { background: var(--primary-dark); transform: scale(1.03); }
    .btn-success { background: #10b981; color: white; }
    .btn-success:hover { background: #059669; }
    .btn-danger { background: #ef4444; color: white; }
    .btn-danger:hover { background: #dc2626; }
    .btn-warning { background: #f59e0b; color: white; }
    .btn-warning:hover { background: #d97706; }
    .btn-premium { background: linear-gradient(135deg, #d946ef, #a855f7); color: white; }
    .btn-premium:hover { transform: scale(1.03); box-shadow: 0 0 20px rgba(217,70,239,0.5); }

    .url-display {
      background: white;
      padding: 14px;
      border-radius: 8px;
      font-size: 0.95rem;
      word-break: break-all;
      margin: 12px 0;
      border-left: 4px solid var(--primary);
    }

    .squad-form { display: none; }
    .squad-form.active { display: block; }

    .player-slot {
      background: white;
      padding: 12px;
      border: 2px solid var(--border);
      border-radius: 8px;
      margin: 8px 0;
      display: flex;
      align-items: center;
      gap: 10px;
    }
    .player-slot.key-player {
      border-color: var(--primary);
      background: rgba(139,92,246,0.05);
    }
    .player-slot.vpn-player {
      border-color: #10b981;
      background: rgba(16,185,129,0.05);
    }
    .player-slot.nonvpn-player {
      border-color: #ef4444;
      background: rgba(239,68,68,0.05);
    }
    .player-slot label {
      min-width: 140px;
      font-weight: 600;
      color: var(--primary);
    }
    .player-slot select { margin: 0; }

    .fixture-item {
      background: white;
      border: 2px solid var(--border);
      border-radius: 12px;
      padding: 1.5rem;
      margin: 1rem 0;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 1rem;
    }
    .fixture-teams { font-size: 1.2rem; font-weight: 600; color: var(--primary); }
    .fixture-date { color: var(--text-light); font-size: 0.95rem; }

    .fixture-generator-box {
      background: white;
      border: 2px solid var(--border);
      border-radius: 12px;
      padding: 1.5rem;
      margin: 1rem 0;
    }

    .fixture-generator-output {
      background: #1f2937;
      color: #f9fafb;
      padding: 1.5rem;
      border-radius: 8px;
      font-family: 'Courier New', monospace;
      font-size: 0.9rem;
      white-space: pre-wrap;
      word-break: break-word;
      margin: 1rem 0;
      border: 2px solid var(--primary);
      max-height: 500px;
      overflow-y: auto;
    }

    .fixture-generator-btn-group {
      display: flex;
      gap: 1rem;
      flex-wrap: wrap;
      margin: 1rem 0;
    }

    .fixture-generator-disabled {
      opacity: 0.5;
      cursor: not-allowed;
    }

    .fixture-status {
      padding: 12px;
      border-radius: 8px;
      margin: 1rem 0;
      font-weight: 600;
    }

    .fixture-status.warning {
      background: #fef3c7;
      border-left: 4px solid #f59e0b;
      color: #92400e;
    }

    .fixture-status.success {
      background: #d1fae5;
      border-left: 4px solid #10b981;
      color: #065f46;
    }

    /* ===================== SCORECARD STYLES ===================== */
    .sc-wrapper {
      display: flex;
      flex-direction: column;
      gap: 2rem;
    }

    .scorecard-dark {
      background: #2c2c2c;
      border-radius: 15px;
      padding: 30px;
      box-shadow: 0 0 20px rgba(139,92,246,0.4);
      border: 2px solid #8b5cf6;
      font-family: 'Montserrat', sans-serif;
    }

    .scorecard-light {
      background: #ffffff;
      border-radius: 15px;
      padding: 30px;
      box-shadow: 0 0 20px rgba(139,92,246,0.2);
      border: 2px solid #a78bfa;
      font-family: 'Montserrat', sans-serif;
    }

    .sc-title-container {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 15px;
      margin-bottom: 10px;
    }

    .sc-tournament-logo {
      width: 80px; height: 80px;
      border-radius: 50%;
      border: 3px solid #a78bfa;
      object-fit: cover;
      box-shadow: 0 0 15px rgba(139,92,246,0.5);
    }

    .scorecard-dark .sc-title {
      font-size: 36px;
      font-family: 'Orbitron', sans-serif;
      color: #ffffff;
      text-shadow: 0 0 10px rgba(139,92,246,0.7);
      border: 3px solid #a78bfa;
      padding: 10px 20px;
      border-radius: 10px;
      background: #1a1a1a;
    }

    .scorecard-light .sc-title {
      font-size: 36px;
      font-family: 'Orbitron', sans-serif;
      color: #1f2937;
      border: 3px solid #a78bfa;
      padding: 10px 20px;
      border-radius: 10px;
      background: #faf5ff;
    }

    .scorecard-dark .sc-date {
      font-size: 20px;
      font-family: 'Orbitron', sans-serif;
      color: #a78bfa;
      text-align: center;
      margin-bottom: 25px;
    }

    .scorecard-light .sc-date {
      font-size: 20px;
      font-family: 'Orbitron', sans-serif;
      color: #8b5cf6;
      text-align: center;
      margin-bottom: 25px;
    }

    .sc-teams {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
      gap: 10px;
    }

    .scorecard-dark .sc-team-panel {
      flex: 1;
      text-align: center;
      font-size: 22px;
      font-weight: 800;
      padding: 15px;
      border-radius: 10px;
      border: 3px solid #a78bfa;
      font-family: 'Orbitron', sans-serif;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
      color: #ffffff;
      background: #1a1a1a;
    }

    .scorecard-light .sc-team-panel {
      flex: 1;
      text-align: center;
      font-size: 22px;
      font-weight: 800;
      padding: 15px;
      border-radius: 10px;
      border: 3px solid #a78bfa;
      font-family: 'Orbitron', sans-serif;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
      color: #1f2937;
      background: #faf5ff;
    }

    .sc-team-score {
      width: 80px; height: 50px;
      background: #8b5cf6;
      color: #ffffff;
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: 'Orbitron', sans-serif;
      font-size: 24px;
      font-weight: 700;
      border: 3px solid #a78bfa;
      flex-shrink: 0;
    }

    .sc-team-logo {
      width: 60px; height: 60px;
      border-radius: 50%;
      object-fit: cover;
      border: 3px solid #a78bfa;
    }

    .sc-matches { display: flex; flex-direction: column; gap: 10px; }

    .sc-match-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 5px 0;
      gap: 10px;
    }

    .sc-player-container {
      flex: 1;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 8px;
      border: 2px solid #a78bfa;
      border-radius: 5px;
      height: 42px;
      box-sizing: border-box;
      position: relative;
    }

    .scorecard-dark .sc-player-container { background: #1a1a1a; }
    .scorecard-light .sc-player-container { background: #faf5ff; }

    .scorecard-dark .sc-player-name {
      font-family: 'Orbitron', sans-serif;
      font-size: 18px;
      font-weight: 600;
      color: #ffffff;
      font-style: italic;
      text-align: center;
      width: 100%;
    }

    .scorecard-light .sc-player-name {
      font-family: 'Orbitron', sans-serif;
      font-size: 18px;
      font-weight: 600;
      color: #1f2937;
      font-style: italic;
      text-align: center;
      width: 100%;
    }

    .sc-motm-player { border: 3px solid #10b981 !important; }

    .sc-motm-player::before {
      content: '👑';
      position: absolute;
      top: -18px;
      left: 50%;
      transform: translateX(-50%);
      font-size: 14px;
    }

    .scorecard-dark .sc-motm-player .sc-player-name {
      color: #a78bfa;
      font-style: normal;
      font-weight: 800;
    }

    .scorecard-light .sc-motm-player .sc-player-name {
      color: #7c3aed;
      font-style: normal;
      font-weight: 800;
    }

    .sc-score-box {
      width: 90px; height: 40px;
      background: #8b5cf6;
      color: #ffffff;
      border-radius: 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: 'Orbitron', sans-serif;
      font-size: 18px;
      margin: 0 10px;
      border: 2px solid #a78bfa;
      flex-shrink: 0;
    }

    .sc-results {
      margin-top: 20px;
      text-align: center;
      font-family: 'Orbitron', sans-serif;
    }

    .scorecard-dark .sc-winner {
      color: #a78bfa;
      font-size: 22px;
      font-weight: 800;
    }

    .scorecard-light .sc-winner {
      color: #7c3aed;
      font-size: 22px;
      font-weight: 800;
    }

    .scorecard-dark .sc-motm-text {
      color: #10b981;
      font-size: 18px;
      margin-top: 8px;
    }

    .scorecard-light .sc-motm-text {
      color: #059669;
      font-size: 18px;
      margin-top: 8px;
    }

    .sc-paste-area {
      width: 100%;
      height: 180px;
      padding: 14px;
      background: #1f2937;
      border: 2px solid var(--border);
      border-radius: 8px;
      color: #f9fafb;
      font-family: 'Montserrat', sans-serif;
      resize: vertical;
      margin: 8px 0;
    }
    .sc-paste-area:focus { outline: none; border-color: var(--primary); }

    .sc-btn-row {
      display: flex;
      gap: 1rem;
      flex-wrap: wrap;
      margin-top: 1rem;
    }

    .sc-label {
      font-weight: 700;
      color: var(--primary);
      margin-top: 1rem;
      display: block;
    }

    /* Archive */
    .archive-item {
      background: white;
      border: 2px solid var(--border);
      border-radius: 10px;
      padding: 12px 16px;
      margin: 8px 0;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 8px;
    }
    .archive-info { font-size: 0.9rem; color: var(--text-light); }
    .archive-teams { font-weight: 700; color: var(--primary); }

    /* Toast */
    .toast {
      position: fixed;
      bottom: 24px;
      right: 24px;
      padding: 12px 20px;
      border-radius: 10px;
      font-weight: 700;
      font-size: 0.9rem;
      z-index: 9999;
      display: none;
      animation: slideIn 0.3s ease;
    }
    .toast.success { background: #8b5cf6; color: white; }
    .toast.error { background: #ef4444; color: white; }
    @keyframes slideIn {
      from { transform: translateY(20px); opacity: 0; }
      to { transform: translateY(0); opacity: 1; }
    }

    /* Password visibility badge */
    .pass-badge {
      display: inline-block;
      background: #f3f4f6;
      border: 1px solid #d1d5db;
      padding: 3px 10px;
      border-radius: 6px;
      font-family: monospace;
      font-size: 0.9rem;
      color: #374151;
      cursor: default;
      user-select: all;
    }

    /* Captain login team select */
    .captain-login-box {
      background: var(--card);
      border: 2px solid var(--border);
      border-radius: 16px;
      padding: 2rem;
      margin-bottom: 1.5rem;
    }

    .team-select-option {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    /* Premium Badge & Lock */
    .premium-badge {
      display: inline-block;
      background: linear-gradient(135deg, #d946ef, #a855f7);
      color: white;
      padding: 6px 12px;
      border-radius: 20px;
      font-size: 0.75rem;
      font-weight: 700;
      letter-spacing: 0.5px;
      margin-left: 8px;
    }

    .premium-lock-overlay {
      background: linear-gradient(135deg, rgba(217, 70, 239, 0.15), rgba(168, 85, 247, 0.15));
      border: 2px dashed #d946ef;
      border-radius: 12px;
      padding: 2rem;
      text-align: center;
      margin: 1.5rem 0;
    }

    .premium-lock-overlay h4 {
      color: #d946ef;
      margin-bottom: 0.5rem;
      font-size: 1.2rem;
    }

    .premium-lock-overlay p {
      color: #a855f7;
      font-weight: 600;
      margin-bottom: 1rem;
    }

    .premium-lock-overlay .btn-premium {
      display: inline-block;
      padding: 10px 20px;
      cursor: pointer;
    }

  </style>
</head>
<body>
  <div class="header">
    <nav>
      <button onclick="showSection('viewer')" class="active" id="nav-viewer">Viewer</button>
      <button onclick="showSection('scorecard')" id="nav-scorecard">Scorecard</button>
      <button onclick="showSection('admin')" id="nav-admin">Admin</button>
      <button onclick="showSection('captain')" id="nav-captain">Captain</button>
    </nav>
  </div>

  <div class="container">

    <!-- ===== VIEWER ===== -->
    <div id="viewer" class="section active">
      <h1>La Viola Fleur de Lis</h1>
      <p style="text-align:center; font-size:1.3rem; margin-bottom:2rem; color:#8b5cf6;">Tournament Hub</p>
      <div id="viewer-teams"></div>
      <div class="card">
        <h2 style="text-align:center; margin-bottom:1.5rem;">Fixtures</h2>
        <div id="viewer-fixtures"></div>
      </div>
    </div>

    <!-- ===== SCORECARD ===== -->
    <div id="scorecard" class="section">
      <div class="card" style="max-width:1100px; margin:0 auto;">
        <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:1.5rem;flex-wrap:wrap;gap:1rem;">
          <div>
            <h2 style="margin:0;">⚽ Scorecard Generator</h2>
            <p style="color:var(--text-light); margin:0.5rem 0 0;">Create professional match scorecards</p>
          </div>
          <span class="premium-badge" style="font-size:0.9rem;padding:8px 16px;">PREMIUM</span>
        </div>

        <div style="background:linear-gradient(135deg, rgba(217, 70, 239, 0.1), rgba(168, 85, 247, 0.1));border-radius:12px;padding:1.5rem;margin-bottom:1.5rem;">
          <p style="color:var(--text);margin:0;font-weight:600;margin-bottom:1rem;">📋 Match Input Format</p>
          <div style="background:white;padding:1rem;border-radius:8px;border-left:4px solid #8b5cf6;font-family:monospace;font-size:0.9rem;color:#374151;line-height:1.6;">
            Team1 ⚒️ Team2<br>
            Player1 (2)🆚(1) Player2<br>
            Player3👑 (3)🆚(0) Player4
          </div>
        </div>

        <div style="margin-bottom:1.5rem;">
          <span class="sc-label">🎮 Match Data</span>
          <textarea class="sc-paste-area" id="scPasteText" placeholder="Example:&#10;Team1 ⚒️ Team2&#10;Player1 (2)🆚(1) Player2&#10;Player3👑 (3)🆚(0) Player4" style="height:200px;"></textarea>

          <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));gap:1rem;margin-bottom:1rem;">
            <div>
              <span class="sc-label">🏆 Tournament Name</span>
              <input type="text" id="scNameInput" placeholder="e.g. La Viola Cup 2026">
            </div>
            <div>
              <span class="sc-label">📅 Tournament Stage / Date</span>
              <input type="text" id="scStageInput" placeholder="e.g. Group Stage or 28 April 2026">
            </div>
            <div>
              <span class="sc-label">🖼️ Logo URL (optional)</span>
              <input type="text" id="scLogoInput" placeholder="https://...">
            </div>
          </div>

          <div class="sc-btn-row" style="justify-content:center;">
            <button class="btn-primary" onclick="generateScorecard()" style="padding:14px 32px;font-size:1rem;">⚡ Generate Scorecards</button>
          </div>
        </div>

        <div class="sc-wrapper">
          <div>
            <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:1rem;flex-wrap:wrap;gap:1rem;">
              <h3 style="margin:0;">🌑 Dark Version</h3>
              <button class="btn-success" onclick="downloadScorecard('sc-dark')" style="padding:10px 20px;">⬇ Download PNG</button>
            </div>
            <div class="scorecard-dark" id="sc-dark">
              <div class="sc-title-container">
                <img src="https://i.ibb.co/QmTqf2K/default-logo.png" class="sc-tournament-logo" id="sc-dark-logo" onerror="this.style.display='none'">
                <div class="sc-title" id="sc-dark-name">La Viola Cup</div>
              </div>
              <div class="sc-date" id="sc-dark-stage">Group Stage</div>
              <div class="sc-teams">
                <div class="sc-team-panel" id="sc-dark-t1">Team 1</div>
                <div class="sc-team-score" id="sc-dark-s1">0</div>
                <div class="sc-team-score" id="sc-dark-s2">0</div>
                <div class="sc-team-panel" id="sc-dark-t2">Team 2</div>
              </div>
              <div class="sc-matches" id="sc-dark-matches"></div>
              <div class="sc-results">
                <div class="sc-winner" id="sc-dark-winner">Winner: -</div>
                <div class="sc-motm-text" id="sc-dark-motm">Man of the Match: -</div>
              </div>
            </div>
          </div>

          <div>
            <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:1rem;flex-wrap:wrap;gap:1rem;">
              <h3 style="margin:0;">☀️ Light Version</h3>
              <button class="btn-success" onclick="downloadScorecard('sc-light')" style="padding:10px 20px;">⬇ Download PNG</button>
            </div>
            <div class="scorecard-light" id="sc-light">
              <div class="sc-title-container">
                <img src="https://i.ibb.co/QmTqf2K/default-logo.png" class="sc-tournament-logo" id="sc-light-logo" onerror="this.style.display='none'">
                <div class="sc-title" id="sc-light-name">La Viola Cup</div>
              </div>
              <div class="sc-date" id="sc-light-stage">Group Stage</div>
              <div class="sc-teams">
                <div class="sc-team-panel" id="sc-light-t1">Team 1</div>
                <div class="sc-team-score" id="sc-light-s1">0</div>
                <div class="sc-team-score" id="sc-light-s2">0</div>
                <div class="sc-team-panel" id="sc-light-t2">Team 2</div>
              </div>
              <div class="sc-matches" id="sc-light-matches"></div>
              <div class="sc-results">
                <div class="sc-winner" id="sc-light-winner">Winner: -</div>
                <div class="sc-motm-text" id="sc-light-motm">Man of the Match: -</div>
              </div>
            </div>
          </div>
        </div>

        <hr style="margin:2rem 0; border-color:var(--border);">

        <div style="background:linear-gradient(135deg, rgba(217, 70, 239, 0.1), rgba(168, 85, 247, 0.1));border-radius:12px;padding:1.5rem;">
          <div style="display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:1rem;margin-bottom:1rem;">
            <h3 style="margin:0;">📁 Scorecard Archive</h3>
            <span style="background:#10b981;color:white;padding:6px 12px;border-radius:20px;font-weight:700;font-size:0.85rem;" id="archive-count">0 Saved</span>
          </div>
          <div id="sc-archive-list" style="margin-top:1rem;"></div>
        </div>
      </div>
    </div>

    <!-- ===== ADMIN ===== -->
    <div id="admin" class="section">
      <div class="card" style="max-width:900px; margin:0 auto;">
        <h2 style="text-align:center; margin-bottom:1.5rem;">Admin Panel</h2>

        <div id="admin-login">
          <input type="password" id="adminPass" placeholder="Admin Password">
          <button class="btn-primary" style="width:100%" onclick="adminLogin()">Login as Admin</button>
        </div>

        <div id="admin-content" style="display:none;">
          <div style="display:flex; gap:1rem; margin-bottom:1.5rem; flex-wrap:wrap;">
            <button class="btn-primary" onclick="showAdminTab(1)">Team Registration</button>
            <button class="btn-primary" onclick="showAdminTab(2)">Fixture Creator</button>
            <button class="btn-primary" onclick="showAdminTab(3)">Scorecard Config</button>
            <button class="btn-primary" onclick="showAdminTab(4)">Fixture Generator</button>
          </div>

          <!-- Tab 1: Team Registration -->
          <div id="admin-tab1">
            <h3>Register New Team</h3>
            <input type="text" id="teamName" placeholder="Team Name">
            <input type="text" id="captainName" placeholder="Captain Name">
            <input type="text" id="teamPassword" placeholder="Team Password">

            <div class="upload-area">
              <p style="margin-bottom:1rem; font-weight:600;">Team Logo</p>
              <input type="file" id="teamLogoInput" accept="image/*">
              <button class="btn-success" onclick="uploadTeamLogo()" style="margin-top:10px;">1. Upload Logo</button>
              <div class="url-display" id="teamLogoUrlDisplay">URL will appear here...</div>
              <input type="text" id="teamLogoUrl" placeholder="2. Paste URL here">
              <button class="btn-primary" onclick="setTeamLogoPreview()">3. Set Logo & Preview</button>
              <img id="teamLogoPreview" class="preview">
            </div>

            <button class="btn-success" style="width:100%; margin-top:1rem;" onclick="registerTeam()">Register Team</button>

            <hr style="margin:2rem 0; border-color:var(--border);">
            <h3>Registered Teams <span style="font-size:0.85rem; color:var(--text-light);">(Admin can see all passwords)</span></h3>
            <div id="admin-teams-list"></div>
          </div>

          <!-- Tab 2: Fixture Creator -->
          <div id="admin-tab2" style="display:none;">
            <h3>Create New Fixture</h3>
            <select id="fixtureTeam1"><option value="">Select Team 1</option></select>
            <select id="fixtureTeam2"><option value="">Select Team 2</option></select>
            <input type="date" id="fixtureDate">
            <input type="time" id="fixtureTime">
            <select id="fixtureFormat">
              <option value="">Select Format</option>
              <option value="10v10">10v10</option>
              <option value="12v12">12v12</option>
              <option value="8v8">8v8</option>
              <option value="6v6">6v6</option>
            </select>
            <input type="text" id="fixtureVenue" placeholder="Venue (optional)">
            <button class="btn-success" style="width:100%; margin-top:1rem;" onclick="createFixture()">Create Fixture</button>

            <hr style="margin:2rem 0; border-color:var(--border);">
            <h3>All Fixtures</h3>
            <div id="admin-fixtures-list"></div>
          </div>

          <!-- Tab 3: Scorecard Config -->
          <div id="admin-tab3" style="display:none;">
            <h3>Scorecard Settings</h3>
            <p style="color:var(--text-light); margin-bottom:1rem;">Set default tournament info for scorecards</p>

            <label style="font-weight:600; color:var(--primary);">Tournament Name</label>
            <input type="text" id="scConfigName" placeholder="e.g. La Viola Cup 2026">

            <label style="font-weight:600; color:var(--primary);">Tournament Stage</label>
            <input type="text" id="scConfigStage" placeholder="e.g. Group Stage">

            <label style="font-weight:600; color:var(--primary);">Tournament Logo URL</label>
            <input type="text" id="scConfigLogoUrl" placeholder="https://...">

            <div class="upload-area" style="margin-top:0.5rem;">
              <p style="font-weight:600; margin-bottom:0.5rem;">Or Upload Logo</p>
              <input type="file" id="scConfigLogoFile" accept="image/*">
              <button class="btn-success" onclick="uploadScConfigLogo()" style="margin-top:8px;">Upload Logo</button>
              <div class="url-display" id="scConfigLogoDisplay">URL will appear here...</div>
            </div>

            <button class="btn-primary" style="width:100%; margin-top:1rem;" onclick="saveScConfig()">Save Scorecard Config</button>

            <hr style="margin:2rem 0; border-color:var(--border);">
            <h3>Current Config</h3>
            <div id="scConfigDisplay" style="background:white; padding:1rem; border-radius:8px; border:2px solid var(--border);"></div>
          </div>

          <!-- Tab 4: Fixture Generator -->
          <div id="admin-tab4" style="display:none;">
            <h3>🚀 Fixture Generator</h3>
            <p style="color:var(--text-light); margin-bottom:1rem;">Auto-generate fixture format from squad submissions</p>
            <div id="fg-fixtures-list"></div>
          </div>
        </div>
      </div>
    </div>

    <!-- ===== CAPTAIN ===== -->
    <div id="captain" class="section">
      <div class="card" style="max-width:900px; margin:0 auto;">
        <h2 style="text-align:center;">Captain Panel</h2>

        <!-- Captain Login Box -->
        <div id="captain-login-area" style="margin-top:1.5rem;">
          <div class="captain-login-box">
            <h3 style="margin-bottom:1rem; text-align:center;">🔐 Login</h3>
            <label style="font-weight:600; color:var(--primary); display:block; margin-bottom:4px;">Select Your Team</label>
            <select id="captainTeamSelect" style="margin-bottom:0.5rem;">
              <option value="">— Choose Team —</option>
            </select>
            <label style="font-weight:600; color:var(--primary); display:block; margin-bottom:4px;">Team Password</label>
            <input type="password" id="teamPasswordLogin" placeholder="Enter team password" style="margin-bottom:1rem;">
            <button class="btn-primary" style="width:100%" onclick="captainLogin()">Login as Captain</button>
          </div>
        </div>

        <div id="captain-content" style="display:none; margin-top:2rem;">
          <h3 id="currentTeamName" style="text-align:center; color:var(--primary); font-size:1.5rem;"></h3>

          <div style="display:flex; gap:1rem; margin:1.5rem 0; flex-wrap:wrap; justify-content:center;">
            <button class="btn-primary" onclick="showCaptainTab(1)">Transfer Window</button>
            <button class="btn-premium" onclick="showCaptainTab(2)">Squad Info <span class="premium-badge">PREMIUM</span></button>
            <button class="btn-premium" onclick="showCaptainTab(3)">Squad Submit <span class="premium-badge">PREMIUM</span></button>
            <button class="btn-warning" onclick="showCaptainTab(4)">🔑 Change Password</button>
          </div>

          <!-- Tab 1: Transfer Window -->
          <div id="captain-tab1">
            <div class="card">
              <h3>➕ Add New Player</h3>
              <p style="color:var(--text-light); margin-bottom:1rem;">📌 Only Name is required. Add other details later if needed.</p>

              <input type="text" id="playerName" placeholder="Player Name *REQUIRED" style="border-color: #ef4444;">

              <div style="background:#f0f9ff; padding:1rem; border-radius:8px; margin:1rem 0;">
                <p style="font-weight:600; color:var(--primary); margin-bottom:0.5rem;">Optional Info (can add later):</p>
                <input type="text" id="playerUid" placeholder="User ID (optional)">
                <input type="text" id="playerDevice" placeholder="Device Name (optional)">
              </div>

              <button class="btn-success" style="width:100%" onclick="addPlayer()">✅ Add Player (Name Only)</button>
            </div>

            <hr style="margin:2rem 0; border-color:var(--border);">

            <div class="card">
              <h3>✏️ Quick Edit Player Info</h3>
              <p style="color:var(--text-light); margin-bottom:1rem;">Click a player below to add missing info:</p>
              <div id="captain-edit-players-list"></div>
            </div>
          </div>

          <!-- Tab 2: Squad Info (PREMIUM) -->
          <div id="captain-tab2" style="display:none;">
            <div class="card">
              <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:1.5rem;flex-wrap:wrap;gap:1rem;">
                <h3 style="margin:0;">👥 Player Roster</h3>
                <span class="premium-badge">PREMIUM</span>
              </div>
              
              <div style="background:linear-gradient(135deg, rgba(217, 70, 239, 0.1), rgba(168, 85, 247, 0.1));border-radius:12px;padding:1.5rem;margin-bottom:1.5rem;">
                <p style="color:var(--text);margin:0;font-weight:600;margin-bottom:0.5rem;">📊 Squad Overview</p>
                <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:1rem;margin-top:1rem;" id="squad-stats">
                  <div style="background:white;padding:1rem;border-radius:8px;border-left:4px solid #8b5cf6;">
                    <div style="font-size:24px;font-weight:800;color:#8b5cf6;" id="stat-total">0</div>
                    <div style="font-size:0.85rem;color:#6b7280;">Total Players</div>
                  </div>
                  <div style="background:white;padding:1rem;border-radius:8px;border-left:4px solid #10b981;">
                    <div style="font-size:24px;font-weight:800;color:#10b981;" id="stat-complete">0</div>
                    <div style="font-size:0.85rem;color:#6b7280;">Complete Info</div>
                  </div>
                  <div style="background:white;padding:1rem;border-radius:8px;border-left:4px solid #f59e0b;">
                    <div style="font-size:24px;font-weight:800;color:#f59e0b;" id="stat-incomplete">0</div>
                    <div style="font-size:0.85rem;color:#6b7280;">Incomplete</div>
                  </div>
                </div>
              </div>

              <h4 style="color:var(--primary);margin-bottom:1rem;">📋 All Players</h4>
              <div style="overflow-x:auto;">
                <table id="playerTable">
                  <thead><tr><th>Name</th><th>UID</th><th>Device</th><th>Status</th><th>Action</th></tr></thead>
                  <tbody></tbody>
                </table>
              </div>

              <div style="margin-top:1.5rem;display:flex;gap:1rem;flex-wrap:wrap;">
                <button class="btn-success" onclick="downloadPlayerInfoJPG()">📥 Download Roster JPG</button>
                <button class="btn-primary" onclick="exportPlayerInfo()">📋 Export as Text</button>
              </div>
            </div>
          </div>



          <!-- Tab 3: Squad Submit (PREMIUM) -->
          <div id="captain-tab3" style="display:none;">
            <div class="card">
              <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:1.5rem;flex-wrap:wrap;gap:1rem;">
                <h3 style="margin:0;">🎯 Squad Submission</h3>
                <span class="premium-badge">PREMIUM</span>
              </div>

              <div style="background:linear-gradient(135deg, rgba(217, 70, 239, 0.1), rgba(168, 85, 247, 0.1));border-radius:12px;padding:1.5rem;margin-bottom:1.5rem;">
                <p style="color:var(--text);margin:0;font-weight:600;margin-bottom:1rem;">📌 Select Tournament Format</p>
                <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(120px,1fr));gap:1rem;" id="format-buttons">
                  <button class="btn-primary" onclick="selectSquadFormat('10v10')" style="padding:12px;font-weight:700;border:3px solid transparent;" id="btn-10v10">10v10</button>
                  <button class="btn-primary" onclick="selectSquadFormat('12v12')" style="padding:12px;font-weight:700;border:3px solid transparent;" id="btn-12v12">12v12</button>
                  <button class="btn-primary" onclick="selectSquadFormat('8v8')" style="padding:12px;font-weight:700;border:3px solid transparent;" id="btn-8v8">8v8</button>
                  <button class="btn-primary" onclick="selectSquadFormat('6v6')" style="padding:12px;font-weight:700;border:3px solid transparent;" id="btn-6v6">6v6</button>
                </div>
              </div>

              <div id="squad-10v10" class="squad-form">
                <h4 style="color:var(--primary); margin-bottom:0.5rem;text-align:center;font-size:1.3rem;">⚽ 10v10 Squad Submission</h4>
                <p style="text-align:center;color:#6b7280;margin-bottom:1.5rem;">Click on each slot to select a player</p>
                <div id="squad-10v10-slots" style="background:white;padding:1.5rem;border-radius:12px;border:2px solid var(--border);"></div>
                <button class="btn-success" style="width:100%; margin-top:1.5rem;padding:14px;font-size:1rem;" onclick="saveSquad('10v10')">✅ Submit 10v10 Squad</button>
                <div id="squad-10v10-status" style="margin-top:1rem;padding:12px;border-radius:8px;display:none;"></div>
              </div>

              <div id="squad-12v12" class="squad-form">
                <h4 style="color:var(--primary); margin-bottom:0.5rem;text-align:center;font-size:1.3rem;">⚽ 12v12 Squad Submission</h4>
                <p style="text-align:center;color:#6b7280;margin-bottom:1.5rem;">Click on each slot to select a player</p>
                <div id="squad-12v12-slots" style="background:white;padding:1.5rem;border-radius:12px;border:2px solid var(--border);"></div>
                <button class="btn-success" style="width:100%; margin-top:1.5rem;padding:14px;font-size:1rem;" onclick="saveSquad('12v12')">✅ Submit 12v12 Squad</button>
                <div id="squad-12v12-status" style="margin-top:1rem;padding:12px;border-radius:8px;display:none;"></div>
              </div>

              <div id="squad-8v8" class="squad-form">
                <h4 style="color:var(--primary); margin-bottom:0.5rem;text-align:center;font-size:1.3rem;">⚽ 8v8 Squad Submission</h4>
                <p style="text-align:center;color:#6b7280;margin-bottom:1.5rem;">Click on each slot to select a player</p>
                <div id="squad-8v8-slots" style="background:white;padding:1.5rem;border-radius:12px;border:2px solid var(--border);"></div>
                <button class="btn-success" style="width:100%; margin-top:1.5rem;padding:14px;font-size:1rem;" onclick="saveSquad('8v8')">✅ Submit 8v8 Squad</button>
                <div id="squad-8v8-status" style="margin-top:1rem;padding:12px;border-radius:8px;display:none;"></div>
              </div>

              <div id="squad-6v6" class="squad-form">
                <h4 style="color:var(--primary); margin-bottom:0.5rem;text-align:center;font-size:1.3rem;">⚽ 6v6 Squad Submission</h4>
                <p style="text-align:center;color:#6b7280;margin-bottom:1.5rem;">Click on each slot to select a player</p>
                <div id="squad-6v6-slots" style="background:white;padding:1.5rem;border-radius:12px;border:2px solid var(--border);"></div>
                <button class="btn-success" style="width:100%; margin-top:1.5rem;padding:14px;font-size:1rem;" onclick="saveSquad('6v6')">✅ Submit 6v6 Squad</button>
                <div id="squad-6v6-status" style="margin-top:1rem;padding:12px;border-radius:8px;display:none;"></div>
              </div>
            </div>
          </div>

          <!-- Tab 4: Change Password -->
          <div id="captain-tab4" style="display:none;">
            <div class="card" style="max-width:500px; margin:0 auto;">
              <h3 style="text-align:center; margin-bottom:1.5rem;">🔑 Change Team Password</h3>
              <p style="color:var(--text-light); margin-bottom:1rem; text-align:center;">Only team captain should change the password. Share new password with admin.</p>

              <label style="font-weight:600; color:var(--primary); display:block; margin-bottom:4px;">Current Password</label>
              <input type="password" id="currentPassInput" placeholder="Enter current password">

              <label style="font-weight:600; color:var(--primary); display:block; margin-bottom:4px; margin-top:0.5rem;">New Password</label>
              <input type="password" id="newPassInput" placeholder="Enter new password">

              <label style="font-weight:600; color:var(--primary); display:block; margin-bottom:4px; margin-top:0.5rem;">Confirm New Password</label>
              <input type="password" id="confirmPassInput" placeholder="Confirm new password">

              <button class="btn-warning" style="width:100%; margin-top:1.5rem;" onclick="changeTeamPassword()">🔑 Change Password</button>
            </div>
          </div>

        </div>
      </div>
    </div>
  </div>

  <!-- Hidden container for team info JPG rendering -->
  <div id="team-info-render-zone" style="position:fixed;left:-9999px;top:0;pointer-events:none;z-index:-1;"></div>

  <div class="toast" id="toast"></div>

  <script>
    // ====== FIREBASE ======
    let db, storage;
    const ADMIN_PASSWORD = "*laviola#";
    let currentTeamId = null;
    let teamLogoUrl = '';
    let playerPhotoUrl = '';
    let allPlayers = [];
    let scArchives = [];
    let scConfig = { name: 'La Viola Cup', stage: 'Group Stage', logo: '' };
    let teamLogoMap = {};
    let isPremium = false;

    window.addEventListener('load', function () {
      if (typeof firebase === 'undefined') {
        alert('Firebase failed to load. Please refresh.');
        return;
      }
      const firebaseConfig = {
        apiKey: "AIzaSyCtXOQb7E2fAX_EZmBXZHgH5Jhaq4h9J20",
        authDomain: "llfc-f9f99.firebaseapp.com",
        databaseURL: "https://llfc-f9f99-default-rtdb.firebaseio.com",
        projectId: "llfc-f9f99",
        storageBucket: "llfc-f9f99.firebasestorage.app",
        messagingSenderId: "373649276495",
        appId: "1:373649276495:web:5668ec4c50f43de6b3343a"
      };
      try {
        firebase.initializeApp(firebaseConfig);
        db = firebase.database();
        storage = firebase.storage();
        showSection('viewer');
        loadScConfig();
        loadScArchives();
      } catch (e) {
        alert('Firebase error: ' + e.message);
      }
    });

    function checkFirebase() {
      if (!db || !storage) { showToast('Firebase is still loading...', 'error'); return false; }
      return true;
    }

    function showToast(msg, type = 'success') {
      const t = document.getElementById('toast');
      t.textContent = msg;
      t.className = 'toast ' + type;
      t.style.display = 'block';
      setTimeout(() => t.style.display = 'none', 3000);
    }

    function upgradeToPremium() {
      showToast('Premium feature coming soon!', 'error');
    }

    function selectSquadFormat(format) {
      // Hide all squad forms
      document.querySelectorAll('.squad-form').forEach(f => f.classList.remove('active'));
      // Show selected format
      document.getElementById(`squad-${format}`).classList.add('active');
      // Update button styling
      document.querySelectorAll('#format-buttons button').forEach(btn => {
        btn.style.borderColor = 'transparent';
        btn.style.transform = 'scale(1)';
      });
      document.getElementById(`btn-${format}`).style.borderColor = '#a78bfa';
      document.getElementById(`btn-${format}`).style.transform = 'scale(1.05)';
    }

    function updateSquadStats() {
      const total = allPlayers.length;
      const complete = allPlayers.filter(p => p.uid !== 'TBD' && p.device !== 'TBD').length;
      const incomplete = total - complete;
      
      document.getElementById('stat-total').textContent = total;
      document.getElementById('stat-complete').textContent = complete;
      document.getElementById('stat-incomplete').textContent = incomplete;
    }

    function downloadPlayerInfoJPG() {
      const table = document.getElementById('playerTable');
      if (!table || !table.querySelector('tbody tr')) return showToast('No players to download', 'error');
      html2canvas(table, { scale: 2, backgroundColor: '#ffffff' }).then(canvas => {
        const link = document.createElement('a');
        link.download = `squad-roster-${new Date().toISOString().split('T')[0]}.jpg`;
        link.href = canvas.toDataURL('image/jpeg', 0.95);
        link.click();
        showToast('✅ Roster downloaded!');
      }).catch(e => showToast('Download failed: ' + e.message, 'error'));
    }

    function exportPlayerInfo() {
      if (allPlayers.length === 0) return showToast('No players to export', 'error');
      let text = '📋 PLAYER ROSTER EXPORT\n';
      text += '=' .repeat(50) + '\n\n';
      allPlayers.forEach((p, i) => {
        text += `${i+1}. ${p.name}\n`;
        text += `   UID: ${p.uid}\n`;
        text += `   Device: ${p.device}\n`;
        text += `   Status: ${p.uid !== 'TBD' && p.device !== 'TBD' ? '✅ Complete' : '⚠️ Incomplete'}\n\n`;
      });
      text += '=' .repeat(50) + '\n';
      text += `Total Players: ${allPlayers.length}\n`;
      text += `Complete: ${allPlayers.filter(p => p.uid !== 'TBD' && p.device !== 'TBD').length}\n`;
      
      const blob = new Blob([text], { type: 'text/plain' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = `squad-roster-${new Date().toISOString().split('T')[0]}.txt`;
      a.click();
      showToast('✅ Exported as text!');
    }

    // ====== SQUAD CONFIGS ======
    const squadConfigs = {
      '10v10': [
        {label:'First Day 🔑', key:true,  category:'firstday', vpn:null},
        {label:'First Day 🔑', key:true,  category:'firstday', vpn:null},
        {label:'First Day',    key:false, category:'firstday', vpn:null},
        {label:'First Day',    key:false, category:'firstday', vpn:null},
        {label:'Star 🔑',      key:true,  category:'star',     vpn:null},
        {label:'Star',         key:false, category:'star',     vpn:null},
        {label:'Last Day VPN 🔑',  key:true,  category:'lastday', vpn:true},
        {label:'Last Day VPN 🔑',  key:true,  category:'lastday', vpn:true},
        {label:'Last Day VPN 🔑',  key:true,  category:'lastday', vpn:true},
        {label:'Last Day Non-VPN', key:false, category:'lastday', vpn:false},
        {label:'Last Day Non-VPN', key:false, category:'lastday', vpn:false},
        {label:'Last Day Non-VPN', key:false, category:'lastday', vpn:false},
        {label:'Captain',      key:false, category:'captain',  vpn:null}
      ],
      '12v12': [
        {label:'First Day 🔑', key:true,  category:'firstday', vpn:null},
        {label:'First Day 🔑', key:true,  category:'firstday', vpn:null},
        {label:'First Day',    key:false, category:'firstday', vpn:null},
        {label:'First Day',    key:false, category:'firstday', vpn:null},
        {label:'Star 🔑',      key:true,  category:'star',     vpn:null},
        {label:'Star',         key:false, category:'star',     vpn:null},
        {label:'Last Day VPN 🔑',  key:true,  category:'lastday', vpn:true},
        {label:'Last Day VPN 🔑',  key:true,  category:'lastday', vpn:true},
        {label:'Last Day VPN 🔑',  key:true,  category:'lastday', vpn:true},
        {label:'Last Day Non-VPN', key:false, category:'lastday', vpn:false},
        {label:'Last Day Non-VPN', key:false, category:'lastday', vpn:false},
        {label:'Last Day Non-VPN', key:false, category:'lastday', vpn:false},
        {label:'Captain',      key:false, category:'captain',  vpn:null}
      ],
      '8v8': [
        {label:'First Day 🔑', key:true,  category:'firstday', vpn:null},
        {label:'First Day',    key:false, category:'firstday', vpn:null},
        {label:'Star 🔑',      key:true,  category:'star',     vpn:null},
        {label:'Star',         key:false, category:'star',     vpn:null},
        {label:'Last Day VPN 🔑',  key:true,  category:'lastday', vpn:true},
        {label:'Last Day VPN 🔑',  key:true,  category:'lastday', vpn:true},
        {label:'Last Day Non-VPN', key:false, category:'lastday', vpn:false},
        {label:'Last Day Non-VPN', key:false, category:'lastday', vpn:false},
        {label:'Captain',      key:false, category:'captain',  vpn:null}
      ],
      '6v6': [
        {label:'Star 🔑',      key:true,  category:'star',     vpn:null},
        {label:'Star',         key:false, category:'star',     vpn:null},
        {label:'Last Day VPN 🔑',  key:true,  category:'lastday', vpn:true},
        {label:'Last Day VPN 🔑',  key:true,  category:'lastday', vpn:true},
        {label:'Last Day Non-VPN', key:false, category:'lastday', vpn:false},
        {label:'Last Day Non-VPN', key:false, category:'lastday', vpn:false},
        {label:'Captain',      key:false, category:'captain',  vpn:null}
      ]
    };

    // ====== SLOT CATEGORIZATION HELPERS ======
    function getPlayersByCategory(squad, category, vpn = null) {
      return squad.filter(p => {
        if (!p.slot) return false;
        const slot = p.slot.toLowerCase();
        if (category === 'firstday') {
          if (slot.includes('first day') || slot.includes('first day')) {
            if (vpn === null) return true;
          }
          return false;
        }
        if (category === 'firstday_key') {
          return (slot.includes('first day 🔑') || slot.includes('first day 🔑')) && p.isKey;
        }
        if (category === 'firstday_normal') {
          return slot.includes('first day') && !p.isKey && !slot.includes('🔑');
        }
        if (category === 'star_key') {
          return slot.includes('star 🔑') || (slot.includes('star') && p.isKey && !slot.includes('last'));
        }
        if (category === 'star_normal') {
          return (slot === 'star') || (slot.includes('star') && !p.isKey && !slot.includes('🔑') && !slot.includes('last'));
        }
        if (category === 'lastday_vpn') {
          return (slot.includes('vpn') && slot.includes('🔑')) ||
                 (slot.includes('last day 🔑') && !slot.includes('non-vpn')) ||
                 (slot.includes('last day 🔑') && !slot.includes('non'));
        }
        if (category === 'lastday_nonvpn') {
          return slot.includes('non-vpn') ||
                 (slot.includes('last day') && !p.isKey && !slot.includes('🔑'));
        }
        if (category === 'captain') {
          return slot.includes('captain');
        }
        return false;
      });
    }

    // ====== NAV ======
    function showSection(section) {
      document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
      document.getElementById(section).classList.add('active');
      document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
      document.getElementById('nav-' + section).classList.add('active');
      if (section === 'viewer') { loadViewerData(); loadViewerFixtures(); }
      if (section === 'admin') loadAdminTeams();
      if (section === 'scorecard') { loadScArchives(); loadTeamLogosForSc(); }
      if (section === 'captain') { loadTeamsForCaptainLogin(); }
    }

    function showAdminTab(tab) {
      for (let i = 1; i <= 4; i++) {
        document.getElementById('admin-tab' + i).style.display = tab === i ? 'block' : 'none';
      }
      if (tab === 2) { loadTeamsForFixtures(); loadAdminFixtures(); }
      if (tab === 3) renderScConfigDisplay();
      if (tab === 4) loadFixtureGeneratorData();
    }

    function showCaptainTab(tab) {
      for (let i = 1; i <= 4; i++) {
        document.getElementById('captain-tab' + i).style.display = tab === i ? 'block' : 'none';
      }
      if (tab === 1) loadPlayersForEdit();
      if (tab === 2) {
        loadPlayersForEdit();
        updateSquadStats();
      }
      if (tab === 3) {
        loadPlayersForEdit();
        updateSquadStats();
        initializeSquadForms();
      }
    }

    // ====== UPLOAD HELPERS ======
    function uploadTeamLogo() {
      if (!storage) return showToast('Firebase loading...', 'error');
      const file = document.getElementById('teamLogoInput').files[0];
      if (!file) return showToast('Select logo first', 'error');
      const ref = storage.ref('team-logos/' + Date.now() + '_' + file.name);
      ref.put(file).then(s => s.ref.getDownloadURL()).then(url => {
        teamLogoUrl = url;
        document.getElementById('teamLogoUrlDisplay').innerHTML = `✅ URL Ready:<br><strong>${url}</strong>`;
        document.getElementById('teamLogoUrl').value = url;
      });
    }

    function setTeamLogoPreview() {
      const url = document.getElementById('teamLogoUrl').value.trim();
      if (!url) return showToast('Paste URL first', 'error');
      teamLogoUrl = url;
      const preview = document.getElementById('teamLogoPreview');
      preview.src = url; preview.style.display = 'block';
    }

    function uploadScConfigLogo() {
      if (!checkFirebase()) return;
      const file = document.getElementById('scConfigLogoFile').files[0];
      if (!file) return showToast('Select file first', 'error');
      const ref = storage.ref('sc-logos/' + Date.now() + '_' + file.name);
      ref.put(file).then(s => s.ref.getDownloadURL()).then(url => {
        document.getElementById('scConfigLogoDisplay').innerHTML = `✅ URL Ready:<br><strong>${url}</strong>`;
        document.getElementById('scConfigLogoUrl').value = url;
        showToast('Logo uploaded!');
      });
    }

    // ====== ADMIN ======
    function adminLogin() {
      const pass = document.getElementById('adminPass').value;
      if (pass === ADMIN_PASSWORD) {
        document.getElementById('admin-login').style.display = 'none';
        document.getElementById('admin-content').style.display = 'block';
        showAdminTab(1);
      } else {
        showToast('Incorrect password', 'error');
      }
    }

    function registerTeam() {
      if (!checkFirebase()) return;
      const name = document.getElementById('teamName').value.trim();
      const captain = document.getElementById('captainName').value.trim();
      const password = document.getElementById('teamPassword').value.trim();
      if (!name || !captain || !password) return showToast('Fill all fields', 'error');
      if (!teamLogoUrl) return showToast('Upload and set team logo', 'error');

      const teamId = 'team_' + Date.now();
      db.ref('teams/' + teamId).set({ name, captain, password, logo: teamLogoUrl, createdAt: Date.now() })
        .then(() => {
          showToast('Team registered!');
          document.getElementById('teamName').value = '';
          document.getElementById('captainName').value = '';
          document.getElementById('teamPassword').value = '';
          document.getElementById('teamLogoUrl').value = '';
          teamLogoUrl = '';
          document.getElementById('teamLogoPreview').style.display = 'none';
          loadAdminTeams();
        });
    }

    function loadAdminTeams() {
      if (!checkFirebase()) return;
      db.ref('teams').once('value', snapshot => {
        const teams = snapshot.val() || {};
        let html = `<table>
          <thead>
            <tr>
              <th>Logo</th>
              <th>Team</th>
              <th>Captain</th>
              <th>🔑 Password</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody>`;
        Object.entries(teams).forEach(([id, team]) => {
          html += `<tr>
            <td><img src="${team.logo}" style="width:40px;height:40px;border-radius:50%;object-fit:cover;"></td>
            <td><strong>${team.name}</strong></td>
            <td>${team.captain}</td>
            <td>
              <span class="pass-badge" title="Click to select">${team.password || '—'}</span>
              <button class="btn-warning" style="padding:6px 10px;font-size:0.8rem;margin-left:6px;" onclick="adminResetPassword('${id}','${team.name}')">Reset</button>
            </td>
            <td><button class="btn-danger" onclick="deleteTeam('${id}')">Delete</button></td>
          </tr>`;
        });
        html += '</tbody></table>';
        document.getElementById('admin-teams-list').innerHTML = html;
      });
    }

    function adminResetPassword(teamId, teamName) {
      const newPass = prompt(`Reset password for "${teamName}":\nEnter new password:`);
      if (newPass === null || newPass.trim() === '') return;
      db.ref('teams/' + teamId + '/password').set(newPass.trim())
        .then(() => { showToast(`Password reset for ${teamName}`); loadAdminTeams(); });
    }

    function deleteTeam(teamId) {
      if (!checkFirebase()) return;
      if (confirm('Delete this team?')) {
        db.ref('teams/' + teamId).remove().then(() => { showToast('Team deleted'); loadAdminTeams(); });
      }
    }

    function loadTeamsForFixtures() {
      if (!checkFirebase()) return;
      db.ref('teams').once('value', snapshot => {
        const teams = snapshot.val() || {};
        let options = '<option value="">Select Team</option>';
        Object.entries(teams).forEach(([id, team]) => {
          options += `<option value="${id}">${team.name}</option>`;
        });
        document.getElementById('fixtureTeam1').innerHTML = options;
        document.getElementById('fixtureTeam2').innerHTML = options;
      });
    }

    function createFixture() {
      if (!checkFirebase()) return;
      const t1 = document.getElementById('fixtureTeam1').value;
      const t2 = document.getElementById('fixtureTeam2').value;
      const date = document.getElementById('fixtureDate').value;
      const time = document.getElementById('fixtureTime').value;
      const format = document.getElementById('fixtureFormat').value;
      const venue = document.getElementById('fixtureVenue').value.trim();
      if (!t1 || !t2 || !date || !time || !format) return showToast('Fill all required fields', 'error');
      if (t1 === t2) return showToast('Select different teams', 'error');

      db.ref('fixtures/fixture_' + Date.now()).set({ team1: t1, team2: t2, date, time, format, venue, createdAt: Date.now() })
        .then(() => {
          showToast('Fixture created!');
          document.getElementById('fixtureTeam1').value = '';
          document.getElementById('fixtureTeam2').value = '';
          document.getElementById('fixtureDate').value = '';
          document.getElementById('fixtureTime').value = '';
          document.getElementById('fixtureFormat').value = '';
          document.getElementById('fixtureVenue').value = '';
          loadAdminFixtures();
        });
    }

    function loadAdminFixtures() {
      if (!checkFirebase()) return;
      Promise.all([db.ref('fixtures').once('value'), db.ref('teams').once('value')]).then(([fs, ts]) => {
        const fixtures = fs.val() || {}, teams = ts.val() || {};
        let html = '';
        Object.entries(fixtures).forEach(([id, f]) => {
          const t1 = teams[f.team1]?.name || 'Unknown', t2 = teams[f.team2]?.name || 'Unknown';
          html += `<div class="fixture-item">
            <div>
              <div class="fixture-teams">${t1} vs ${t2}</div>
              <div class="fixture-date">${f.date} at ${f.time} • ${f.format}</div>
              ${f.venue ? `<div class="fixture-date">Venue: ${f.venue}</div>` : ''}
            </div>
            <button class="btn-danger" onclick="deleteFixture('${id}')">Delete</button>
          </div>`;
        });
        document.getElementById('admin-fixtures-list').innerHTML = html || '<p style="text-align:center;color:#6b7280;">No fixtures yet</p>';
      });
    }

    function deleteFixture(fixtureId) {
      if (!checkFirebase()) return;
      if (confirm('Delete fixture?')) {
        db.ref('fixtures/' + fixtureId).remove().then(() => { showToast('Fixture deleted'); loadAdminFixtures(); });
      }
    }

    function saveScConfig() {
      if (!checkFirebase()) return;
      const name = document.getElementById('scConfigName').value.trim();
      const stage = document.getElementById('scConfigStage').value.trim();
      const logo = document.getElementById('scConfigLogoUrl').value.trim();
      if (!name || !stage) return showToast('Enter name and stage', 'error');
      const config = { name, stage, logo, updatedAt: Date.now() };
      db.ref('scConfig').set(config).then(() => {
        scConfig = config;
        showToast('Scorecard config saved!');
        renderScConfigDisplay();
        document.getElementById('scConfigName').value = '';
        document.getElementById('scConfigStage').value = '';
        document.getElementById('scConfigLogoUrl').value = '';
      });
    }

    function loadScConfig() {
      if (!checkFirebase()) return;
      db.ref('scConfig').once('value', snap => {
        const data = snap.val();
        if (data) scConfig = data;
      });
    }

    function renderScConfigDisplay() {
      const d = document.getElementById('scConfigDisplay');
      if (!d) return;
      d.innerHTML = `
        ${scConfig.logo ? `<img src="${scConfig.logo}" style="width:50px;height:50px;border-radius:50%;object-fit:cover;vertical-align:middle;margin-right:10px;">` : ''}
        <strong>${scConfig.name}</strong> — ${scConfig.stage}
      `;
    }

    // ====== FIXTURE GENERATOR (Admin Tab 4) ======
    function loadFixtureGeneratorData() {
      if (!checkFirebase()) return;
      Promise.all([db.ref('fixtures').once('value'), db.ref('teams').once('value')]).then(([fs, ts]) => {
        const fixtures = fs.val() || {}, teams = ts.val() || {};
        let html = '';
        Object.entries(fixtures).forEach(([fixtureId, fixture]) => {
          const t1 = teams[fixture.team1], t2 = teams[fixture.team2];
          if (!t1 || !t2) return;

          const canGenerate = checkGeneratorAvailability(fixture.date);
          const statusMsg = canGenerate
            ? '✅ Ready to generate'
            : '⏳ Available from ' + getAvailableTime(fixture.date);

          html += `<div class="fixture-generator-box">
            <div style="margin-bottom:1rem;">
              <div class="fixture-teams">${t1.name} vs ${t2.name}</div>
              <div class="fixture-date">${fixture.date} at ${fixture.time} • ${fixture.format}</div>
              <div class="fixture-status ${canGenerate ? 'success' : 'warning'}">${statusMsg}</div>
            </div>
            <div class="fixture-generator-btn-group">
              <button class="btn-primary ${!canGenerate ? 'fixture-generator-disabled' : ''}"
                onclick="generateFixtureFormat('${fixtureId}','${fixture.team1}','${fixture.team2}','${fixture.date}','${fixture.format}')"
                ${!canGenerate ? 'disabled' : ''}>🚀 Generate Fixture</button>
            </div>
            <div id="fg-output-${fixtureId}"></div>
          </div>`;
        });
        document.getElementById('fg-fixtures-list').innerHTML = html || '<p style="text-align:center;color:#6b7280;">No fixtures found</p>';
      });
    }

    function checkGeneratorAvailability(fixtureDate) {
      const now = new Date();
      const fixture = new Date(fixtureDate);
      const dayBefore = new Date(fixture);
      dayBefore.setDate(dayBefore.getDate() - 1);
      dayBefore.setHours(23, 59, 0, 0);
      return now >= dayBefore;
    }

    function getAvailableTime(fixtureDate) {
      const fixture = new Date(fixtureDate);
      const dayBefore = new Date(fixture);
      dayBefore.setDate(dayBefore.getDate() - 1);
      dayBefore.setHours(23, 59, 0, 0);
      return dayBefore.toLocaleString('en-GB', { day: '2-digit', month: 'short', hour: '2-digit', minute: '2-digit' });
    }

    function generateFixtureFormat(fixtureId, team1Id, team2Id, fixtureDate, format) {
      if (!checkFirebase()) return;

      Promise.all([
        db.ref('teams/' + team1Id).once('value'),
        db.ref('teams/' + team2Id).once('value')
      ]).then(([t1Snap, t2Snap]) => {
        const team1 = t1Snap.val();
        const team2 = t2Snap.val();
        if (!team1 || !team2) return showToast('Error: Cannot fetch team data', 'error');

        Promise.all([
          db.ref('teams/' + team1Id + '/squads/' + format).once('value'),
          db.ref('teams/' + team2Id + '/squads/' + format).once('value')
        ]).then(([squad1Snap, squad2Snap]) => {
          const squad1Data = squad1Snap.val();
          const squad2Data = squad2Snap.val();
          if (!squad1Data || !squad2Data) return showToast('Error: Squad submission not found for this format', 'error');

          const squad1 = squad1Data.squad;
          const squad2 = squad2Data.squad;

          const fixtureText = buildFixtureText(team1.name, team2.name, squad1, squad2, fixtureDate, format);

          window[`fgData_${fixtureId}`] = { team1, team2, squad1, squad2, fixtureDate, format };

          const outputDiv = document.getElementById(`fg-output-${fixtureId}`);
          outputDiv.innerHTML = `
            <div style="margin-top:1.5rem;">
              <div class="fixture-generator-output">${escapeHtml(fixtureText)}</div>
              <div style="display:flex;gap:1rem;flex-wrap:wrap;margin-top:1rem;">
                <button class="btn-success" onclick="copyFixtureText('${fixtureId}')">📋 Copy to Clipboard</button>
                <button class="btn-primary" onclick="downloadTeamInfoJPG('${fixtureId}', 1)">⬇ Home Info JPG</button>
                <button class="btn-primary" onclick="downloadTeamInfoJPG('${fixtureId}', 2)">⬇ Away Info JPG</button>
              </div>
            </div>
          `;

          window[`fixture_text_${fixtureId}`] = fixtureText;
          showToast('Fixture generated!');
        });
      });
    }

    function buildFixtureText(team1Name, team2Name, squad1, squad2, fixtureDate, format) {
      const date = new Date(fixtureDate);

      const t1_fdKey    = getPlayersByCategory(squad1, 'firstday_key');
      const t1_fdNorm   = getPlayersByCategory(squad1, 'firstday_normal');
      const t1_starKey  = getPlayersByCategory(squad1, 'star_key');
      const t1_starNorm = getPlayersByCategory(squad1, 'star_normal');
      const t1_ldVPN    = getPlayersByCategory(squad1, 'lastday_vpn');
      const t1_ldNon    = getPlayersByCategory(squad1, 'lastday_nonvpn');
      const t1_capt     = getPlayersByCategory(squad1, 'captain')[0];

      const t2_fdKey    = getPlayersByCategory(squad2, 'firstday_key');
      const t2_fdNorm   = getPlayersByCategory(squad2, 'firstday_normal');
      const t2_starKey  = getPlayersByCategory(squad2, 'star_key');
      const t2_starNorm = getPlayersByCategory(squad2, 'star_normal');
      const t2_ldVPN    = getPlayersByCategory(squad2, 'lastday_vpn');
      const t2_ldNon    = getPlayersByCategory(squad2, 'lastday_nonvpn');
      const t2_capt     = getPlayersByCategory(squad2, 'captain')[0];

      const fdDate = new Date(date);
      fdDate.setDate(fdDate.getDate() + 1);
      fdDate.setHours(0, 0, 0, 0);

      const starDate = new Date(date);
      starDate.setDate(starDate.getDate() + 1);
      starDate.setHours(23, 0, 0, 0);

      const ldDate = new Date(date);
      ldDate.setDate(ldDate.getDate() + 2);
      ldDate.setHours(1, 20, 0, 0);

      const fmtFD   = formatDateTime(fdDate);
      const fmtStar = formatDateTime(starDate);
      const fmtLD   = formatDateTime(ldDate);

      const firstDayMatches = buildMatchups(t1_fdKey, t1_fdNorm, t2_fdKey, t2_fdNorm, false);
      const starMatches     = buildMatchups(t1_starKey, t1_starNorm, t2_starKey, t2_starNorm, false);
      const lastDayMatches  = buildLastDayMatchups(t1_ldVPN, t1_ldNon, t2_ldVPN, t2_ldNon);

      return `🔥 𝗧𝗵𝗿𝗶𝗹𝗹𝗲𝗿 𝗟𝗼𝗮𝗱𝗶𝗻𝗴…

🏆 𝗟𝗩𝗟 𝗜𝗡𝗧𝗥𝗔 𝗕𝗜𝗗 2026

𝗚𝗥𝗢𝗨𝗣 𝗦𝗧𝗔𝗚𝗘

${team1Name} ⚒️ ${team2Name}

🚨 𝗙𝗶𝗿𝘀𝘁 𝗗𝗮𝘆: ${fmtFD}

${firstDayMatches}

⭐ 𝗦𝘁𝗮𝗿: ${fmtStar}

${starMatches}

⛔ 𝗟𝗮𝘀𝘁 𝗗𝗮𝘆: ${fmtLD}

${lastDayMatches}

𝗣𝗢𝗜𝗡𝗧𝗦:
${team1Name}: 
${team2Name}: 

ROOM SETTING: 8 MINUTES NORMAL

𝗥𝗲𝗳𝗲𝗿𝗲𝗲: ${t1_capt?.playerName || 'TBD'} / ${t2_capt?.playerName || 'TBD'}`;
    }

    function buildMatchups(keyPlayers1, normalPlayers1, keyPlayers2, normalPlayers2) {
      const matches = [];
      const len = Math.max(keyPlayers1.length, normalPlayers2.length);
      for (let i = 0; i < len; i++) {
        const p1 = keyPlayers1[i], p2 = normalPlayers2[i];
        if (p1 && p2) matches.push(`${p1.playerName} 🔑 🆚 ${p2.playerName}`);
      }
      const len2 = Math.max(normalPlayers1.length, keyPlayers2.length);
      for (let i = 0; i < len2; i++) {
        const p1 = normalPlayers1[i], p2 = keyPlayers2[i];
        if (p1 && p2) matches.push(`${p1.playerName} 🆚 🔑 ${p2.playerName}`);
      }
      return matches.join('\n') || '(No matches)';
    }

    function buildLastDayMatchups(vpn1, nonVpn1, vpn2, nonVpn2) {
      const matches = [];
      const len1 = Math.max(vpn1.length, nonVpn2.length);
      for (let i = 0; i < len1; i++) {
        const p1 = vpn1[i], p2 = nonVpn2[i];
        if (p1 && p2) matches.push(`${p1.playerName} 🔑(VPN) 🆚 ${p2.playerName} (Non-VPN)`);
      }
      const len2 = Math.max(nonVpn1.length, vpn2.length);
      for (let i = 0; i < len2; i++) {
        const p1 = nonVpn1[i], p2 = vpn2[i];
        if (p1 && p2) matches.push(`${p1.playerName} (Non-VPN) 🆚 ${p2.playerName} 🔑(VPN)`);
      }
      return matches.join('\n') || '(No matches)';
    }

    function formatDateTime(date) {
      const d = date.getDate().toString().padStart(2,'0');
      const m = (date.getMonth()+1).toString().padStart(2,'0');
      const h = date.getHours().toString().padStart(2,'0');
      const min = date.getMinutes().toString().padStart(2,'0');
      return `${d}/${m} | ${h}:${min}`;
    }

    function escapeHtml(text) {
      const map = {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#039;'};
      return text.replace(/[&<>"']/g, m => map[m]);
    }

    function copyFixtureText(fixtureId) {
      const text = window[`fixture_text_${fixtureId}`];
      if (!text) return showToast('No text to copy', 'error');
      navigator.clipboard.writeText(text).then(() => showToast('✅ Copied!')).catch(() => showToast('Copy failed', 'error'));
    }

    // ====== TEAM INFO JPG ======
    async function downloadTeamInfoJPG(fixtureId, teamNum) {
      const data = window[`fgData_${fixtureId}`];
      if (!data) return showToast('Generate fixture first', 'error');

      const isHome = teamNum === 1;
      const myTeam     = isHome ? data.team1 : data.team2;
      const opponent   = isHome ? data.team2 : data.team1;
      const mySquad    = isHome ? data.squad1 : data.squad2;
      const side       = isHome ? 'HOME' : 'AWAY';

      showToast(`Generating ${side} info card...`);

      const zone = document.getElementById('team-info-render-zone');
      zone.innerHTML = buildTeamInfoCardHTML(
        myTeam.name, myTeam.logo, mySquad, opponent.name, side, data.fixtureDate, data.format
      );

      const el = zone.firstElementChild;
      if (!el) return showToast('Render error', 'error');

      await document.fonts.ready;
      const imgs = el.querySelectorAll('img');
      await Promise.all(Array.from(imgs).map(img => new Promise(res => {
        if (img.complete) return res();
        img.onload = res; img.onerror = res;
      })));
      await new Promise(r => setTimeout(r, 300));

      try {
        const canvas = await html2canvas(el, {
          scale: 2.5,
          useCORS: true,
          backgroundColor: '#0a0514',
          logging: false
        });
        const link = document.createElement('a');
        link.download = `${myTeam.name.replace(/\s+/g,'_')}_${side}_squad.jpg`;
        link.href = canvas.toDataURL('image/jpeg', 0.97);
        link.click();
        showToast(`✅ ${side} info downloaded!`);
      } catch(e) {
        showToast('Download failed: ' + e.message, 'error');
      }
    }

    function buildTeamInfoCardHTML(teamName, teamLogo, squadData, opponentName, side, fixtureDate, format) {
      const fdKey   = getPlayersByCategory(squadData, 'firstday_key');
      const fdNorm  = getPlayersByCategory(squadData, 'firstday_normal');
      const stKey   = getPlayersByCategory(squadData, 'star_key');
      const stNorm  = getPlayersByCategory(squadData, 'star_normal');
      const ldVPN   = getPlayersByCategory(squadData, 'lastday_vpn');
      const ldNon   = getPlayersByCategory(squadData, 'lastday_nonvpn');
      const captain = getPlayersByCategory(squadData, 'captain')[0];

      const formDate = fixtureDate ? new Date(fixtureDate).toLocaleDateString('en-GB',{day:'2-digit',month:'short',year:'numeric'}) : '';
      const sideColor = side === 'HOME' ? '#22c55e' : '#f97316';

      const playerBox = (p, accent, bg) =>
        `<div style="background:${bg};border:2px solid ${accent};border-radius:8px;padding:7px 12px;margin-bottom:6px;display:flex;align-items:center;gap:8px;">
          <span style="color:${accent};font-family:'Orbitron',sans-serif;font-size:13px;font-weight:600;word-break:break-word;">${p.playerName}</span>
        </div>`;

      const keyBox = (p, accent) => playerBox(p, accent, `rgba(${hexToRgb(accent)},0.15)`);
      const normBox = (p, accent) => playerBox(p, `${accent}99`, `rgba(${hexToRgb(accent)},0.07)`);

      function hexToRgb(hex) {
        const r = parseInt(hex.slice(1,3),16);
        const g = parseInt(hex.slice(3,5),16);
        const b = parseInt(hex.slice(5,7),16);
        return `${r},${g},${b}`;
      }

      return `<div style="
        width:580px;
        background:linear-gradient(145deg,#0a0514 0%,#150a2e 40%,#0c0a1e 80%,#080415 100%);
        padding:28px;
        border-radius:20px;
        border:3px solid #8b5cf6;
        font-family:'Montserrat',sans-serif;
        box-sizing:border-box;
        box-shadow:0 0 60px rgba(139,92,246,0.4);
      ">
        <!-- TOP STRIPE -->
        <div style="height:4px;background:linear-gradient(90deg,#8b5cf6,${sideColor},#8b5cf6);border-radius:4px;margin-bottom:20px;"></div>

        <!-- HEADER -->
        <div style="display:flex;align-items:center;gap:18px;margin-bottom:20px;padding-bottom:18px;border-bottom:1px solid rgba(139,92,246,0.35);">
          ${teamLogo
            ? `<img src="${teamLogo}" style="width:88px;height:88px;border-radius:50%;border:3px solid #a78bfa;object-fit:cover;box-shadow:0 0 25px rgba(139,92,246,0.7);">`
            : `<div style="width:88px;height:88px;border-radius:50%;border:3px solid #a78bfa;background:linear-gradient(135deg,#2d1b69,#1e1b4b);display:flex;align-items:center;justify-content:center;font-size:36px;">⚽</div>`}
          <div style="flex:1;min-width:0;">
            <div style="font-family:'Orbitron',sans-serif;font-size:20px;color:#ffffff;font-weight:800;letter-spacing:0.5px;margin-bottom:6px;line-height:1.2;">${teamName}</div>
            <div style="display:inline-block;background:${sideColor}22;border:1.5px solid ${sideColor};color:${sideColor};padding:3px 12px;border-radius:20px;font-size:11px;font-weight:700;letter-spacing:1px;margin-bottom:6px;">${side}</div>
            <div style="color:#7c7ca0;font-size:12px;">vs <span style="color:#a78bfa;font-weight:600;">${opponentName}</span>${formDate ? ` • ${formDate}` : ''}${format ? ` • ${format}` : ''}</div>
          </div>
          <div style="text-align:center;flex-shrink:0;">
            <div style="font-size:22px;">🏆</div>
            <div style="color:rgba(167,139,250,0.5);font-size:9px;font-weight:700;letter-spacing:2px;margin-top:2px;">LA VIOLA</div>
          </div>
        </div>

        <!-- FIRST DAY -->
        ${fdKey.length > 0 || fdNorm.length > 0 ? `
        <div style="margin-bottom:16px;">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px;">
            <div style="width:3px;height:18px;background:#f59e0b;border-radius:2px;"></div>
            <span style="color:#f59e0b;font-size:12px;font-weight:800;letter-spacing:1.5px;font-family:'Orbitron',sans-serif;">🚨 FIRST DAY</span>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;">
            <div>
              ${fdKey.length > 0 ? `<div style="color:#f59e0b88;font-size:10px;font-weight:700;letter-spacing:1px;margin-bottom:4px;">🔑 KEY</div>${fdKey.map(p => keyBox(p,'#f59e0b')).join('')}` : ''}
            </div>
            <div>
              ${fdNorm.length > 0 ? `<div style="color:#c4b5fd88;font-size:10px;font-weight:700;letter-spacing:1px;margin-bottom:4px;">NORMAL</div>${fdNorm.map(p => normBox(p,'#c4b5fd')).join('')}` : ''}
            </div>
          </div>
        </div>` : ''}

        <!-- STAR -->
        ${stKey.length > 0 || stNorm.length > 0 ? `
        <div style="margin-bottom:16px;">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px;">
            <div style="width:3px;height:18px;background:#fbbf24;border-radius:2px;"></div>
            <span style="color:#fbbf24;font-size:12px;font-weight:800;letter-spacing:1.5px;font-family:'Orbitron',sans-serif;">⭐ STAR</span>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;">
            <div>
              ${stKey.length > 0 ? `<div style="color:#fbbf2488;font-size:10px;font-weight:700;letter-spacing:1px;margin-bottom:4px;">🔑 KEY</div>${stKey.map(p => keyBox(p,'#fbbf24')).join('')}` : ''}
            </div>
            <div>
              ${stNorm.length > 0 ? `<div style="color:#fde68a88;font-size:10px;font-weight:700;letter-spacing:1px;margin-bottom:4px;">NORMAL</div>${stNorm.map(p => normBox(p,'#fde68a')).join('')}` : ''}
            </div>
          </div>
        </div>` : ''}

        <!-- LAST DAY -->
        ${ldVPN.length > 0 || ldNon.length > 0 ? `
        <div style="margin-bottom:16px;">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:10px;">
            <div style="width:3px;height:18px;background:#ef4444;border-radius:2px;"></div>
            <span style="color:#ef4444;font-size:12px;font-weight:800;letter-spacing:1.5px;font-family:'Orbitron',sans-serif;">⛔ LAST DAY</span>
          </div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;">
            <div>
              <div style="text-align:center;background:rgba(139,92,246,0.25);border:1px solid #8b5cf6;border-radius:6px;padding:5px;margin-bottom:8px;color:#a78bfa;font-size:10px;font-weight:800;letter-spacing:1px;">🔑 VPN</div>
              ${ldVPN.map(p => keyBox(p,'#8b5cf6')).join('')}
            </div>
            <div>
              <div style="text-align:center;background:rgba(239,68,68,0.2);border:1px solid #ef4444;border-radius:6px;padding:5px;margin-bottom:8px;color:#fca5a5;font-size:10px;font-weight:800;letter-spacing:1px;">Non-VPN</div>
              ${ldNon.map(p => keyBox(p,'#ef4444')).join('')}
            </div>
          </div>
        </div>` : ''}

        <!-- FOOTER -->
        <div style="border-top:1px solid rgba(139,92,246,0.25);padding-top:12px;margin-top:4px;display:flex;justify-content:space-between;align-items:center;">
          ${captain ? `<span style="color:#a78bfa;font-size:12px;font-weight:600;font-family:'Orbitron',sans-serif;">👑 <span style="color:#ffffff;">${captain.playerName}</span></span>` : '<span></span>'}
          <span style="color:rgba(167,139,250,0.35);font-size:9px;letter-spacing:2px;font-weight:700;font-family:'Orbitron',sans-serif;">LA VIOLA FLEUR DE LIS</span>
        </div>
        <!-- BOTTOM STRIPE -->
        <div style="height:3px;background:linear-gradient(90deg,transparent,#8b5cf6,transparent);border-radius:4px;margin-top:12px;"></div>
      </div>`;
    }

    // ====== CAPTAIN ======
    function loadTeamsForCaptainLogin() {
      if (!checkFirebase()) return;
      db.ref('teams').once('value', snap => {
        const teams = snap.val() || {};
        let opts = '<option value="">— Choose Team —</option>';
        Object.entries(teams).forEach(([id, t]) => {
          opts += `<option value="${id}">${t.name}</option>`;
        });
        const sel = document.getElementById('captainTeamSelect');
        if (sel) sel.innerHTML = opts;
      });
    }

    function captainLogin() {
      if (!checkFirebase()) return;
      const teamId   = document.getElementById('captainTeamSelect').value;
      const password = document.getElementById('teamPasswordLogin').value.trim();
      if (!teamId)   return showToast('Select your team', 'error');
      if (!password) return showToast('Enter team password', 'error');

      db.ref('teams/' + teamId).once('value', snap => {
        const team = snap.val();
        if (team && team.password === password) {
          currentTeamId = teamId;
          document.getElementById('currentTeamName').textContent = team.name;
          document.getElementById('captain-login-area').style.display = 'none';
          document.getElementById('captain-content').style.display = 'block';
          showCaptainTab(1);
        } else {
          showToast('Invalid password', 'error');
        }
      });
    }

    function changeTeamPassword() {
      if (!checkFirebase() || !currentTeamId) return showToast('Not logged in', 'error');
      const currentPass = document.getElementById('currentPassInput').value.trim();
      const newPass     = document.getElementById('newPassInput').value.trim();
      const confirmPass = document.getElementById('confirmPassInput').value.trim();

      if (!currentPass || !newPass || !confirmPass) return showToast('Fill all fields', 'error');
      if (newPass !== confirmPass) return showToast('New passwords do not match', 'error');
      if (newPass.length < 4) return showToast('Password too short (min 4 chars)', 'error');

      db.ref('teams/' + currentTeamId + '/password').once('value', snap => {
        if (snap.val() !== currentPass) return showToast('Current password is wrong', 'error');
        db.ref('teams/' + currentTeamId + '/password').set(newPass).then(() => {
          showToast('✅ Password changed! Share new password with admin.');
          document.getElementById('currentPassInput').value = '';
          document.getElementById('newPassInput').value = '';
          document.getElementById('confirmPassInput').value = '';
        });
      });
    }

    function addPlayer() {
      if (!checkFirebase()) return;
      if (!currentTeamId) return showToast('Login first', 'error');
      const name = document.getElementById('playerName').value.trim();
      if (!name) return showToast('Player Name is required', 'error');

      const uid    = document.getElementById('playerUid').value.trim() || 'TBD';
      const device = document.getElementById('playerDevice').value.trim() || 'TBD';

      db.ref('teams/' + currentTeamId + '/players/player_' + Date.now()).set({ name, uid, device, addedAt: Date.now() })
        .then(() => {
          showToast('✅ Player added!');
          document.getElementById('playerName').value = '';
          document.getElementById('playerUid').value = '';
          document.getElementById('playerDevice').value = '';
          loadPlayersForEdit();
        });
    }

    function loadPlayersForEdit() {
      if (!checkFirebase() || !currentTeamId) return;
      db.ref('teams/' + currentTeamId + '/players').once('value', snapshot => {
        const players = snapshot.val() || {};
        allPlayers = Object.entries(players).map(([id, p]) => ({ id, ...p }));

        updateSquadStats();

        let html = '';
        allPlayers.forEach(p => {
          const isComplete = p.uid !== 'TBD' && p.device !== 'TBD';
          html += `<tr>
            <td><strong>${p.name}</strong></td>
            <td style="color:${p.uid === 'TBD' ? '#f59e0b' : '#10b981'};font-weight:600;">${p.uid === 'TBD' ? '⚠️ TBD' : p.uid}</td>
            <td style="color:${p.device === 'TBD' ? '#f59e0b' : '#10b981'};font-weight:600;">${p.device === 'TBD' ? '⚠️ TBD' : p.device}</td>
            <td>${isComplete ? '<span style="color:#10b981;font-weight:700;">✅ Complete</span>' : '<span style="color:#f59e0b;font-weight:700;">⚠️ Incomplete</span>'}</td>
            <td>
              <button class="btn-primary" style="padding:8px 12px;font-size:0.85rem;" onclick="showEditPlayerModal('${p.id}')">✏️ Edit</button>
              <button class="btn-danger"  style="padding:8px 12px;font-size:0.85rem;" onclick="deletePlayer('${p.id}')">🗑️</button>
            </td>
          </tr>`;
        });
        if (allPlayers.length === 0) {
          html = '<tr><td colspan="5" style="text-align:center;padding:2rem;color:#6b7280;">No players yet. Add from Transfer Window.</td></tr>';
        }
        document.getElementById('playerTable').querySelector('tbody').innerHTML = html;

        let editHtml = '<div style="display:grid;grid-template-columns:repeat(auto-fill,minmax(180px,1fr));gap:1rem;">';
        allPlayers.forEach(p => {
          const isComplete = p.uid !== 'TBD' && p.device !== 'TBD';
          editHtml += `<div style="background:white;border:3px solid ${isComplete ? '#10b981' : '#f59e0b'};border-radius:10px;padding:1.2rem;cursor:pointer;transition:all 0.3s;hover:box-shadow:0 8px 16px rgba(0,0,0,0.1);" onclick="showEditPlayerModal('${p.id}')">
            <div style="font-size:36px;text-align:center;margin-bottom:0.8rem;">👤</div>
            <div style="font-weight:700;margin-bottom:0.8rem;text-align:center;color:var(--primary);">${p.name}</div>
            <div style="font-size:0.85rem;color:${p.uid==='TBD'?'#f59e0b':'#10b981'};margin-bottom:0.5rem;display:flex;align-items:center;gap:0.5rem;">
              <span style="font-size:1rem;">📱</span> ${p.uid==='TBD'?'⚠️ No UID':p.uid}
            </div>
            <div style="font-size:0.85rem;color:${p.device==='TBD'?'#f59e0b':'#10b981'};margin-bottom:0.8rem;display:flex;align-items:center;gap:0.5rem;">
              <span style="font-size:1rem;">📲</span> ${p.device==='TBD'?'⚠️ No Device':p.device}
            </div>
            <div style="text-align:center;padding:8px;border-radius:6px;background:${isComplete?'#d1fae5':'#fef3c7'};color:${isComplete?'#065f46':'#92400e'};font-weight:700;font-size:0.8rem;">${isComplete?'✅ READY':'⚠️ INCOMPLETE'}</div>
            <button class="btn-primary" style="width:100%;margin-top:0.8rem;padding:8px;font-size:0.8rem;" onclick="event.stopPropagation();showEditPlayerModal('${p.id}')">✏️ EDIT</button>
          </div>`;
        });
        editHtml += '</div>';
        document.getElementById('captain-edit-players-list').innerHTML = editHtml || '<p style="text-align:center;color:#6b7280;font-size:1.1rem;">No players yet.</p>';
      });
    }

    function showEditPlayerModal(playerId) {
      const player = allPlayers.find(p => p.id === playerId);
      if (!player) return;
      const newName   = prompt('Player Name:',   player.name);
      if (newName === null) return;
      const newUid    = prompt('User ID:',        player.uid === 'TBD' ? '' : player.uid);
      if (newUid === null) return;
      const newDevice = prompt('Device Name:',    player.device === 'TBD' ? '' : player.device);
      if (newDevice === null) return;
      db.ref('teams/' + currentTeamId + '/players/' + playerId).update({
        name: newName.trim(),
        uid: newUid.trim() || 'TBD',
        device: newDevice.trim() || 'TBD'
      }).then(() => { showToast('✅ Player updated!'); loadPlayersForEdit(); });
    }

    function deletePlayer(playerId) {
      if (!checkFirebase()) return;
      if (confirm('Delete this player?')) {
        db.ref('teams/' + currentTeamId + '/players/' + playerId).remove()
          .then(() => { showToast('Player deleted'); loadPlayersForEdit(); });
      }
    }

    function initializeSquadForms() {
      loadPlayersForEdit();
      setTimeout(() => {
        Object.keys(squadConfigs).forEach(format => createSquadSlots(format));
        loadSavedSquads();
        // Show first format by default
        selectSquadFormat('10v10');
      }, 300);
    }

    function createSquadSlots(format) {
      const container = document.getElementById(`squad-${format}-slots`);
      const config = squadConfigs[format];
      let prevCategory = '';
      let html = '';

      config.forEach((slot, index) => {
        if (slot.category !== prevCategory) {
          const catLabels = {firstday:'🚨 First Day', star:'⭐ Star', lastday:'⛔ Last Day', captain:'👑 Captain'};
          const catColors = {firstday:'#f59e0b', star:'#fbbf24', lastday:'#ef4444', captain:'#8b5cf6'};
          html += `<div style="background:${catColors[slot.category]}15;border-left:4px solid ${catColors[slot.category]};padding:10px 12px;border-radius:6px;margin:16px 0 8px;font-weight:800;color:${catColors[slot.category]};font-size:1rem;">${catLabels[slot.category]||slot.category}</div>`;
          prevCategory = slot.category;
        }
        const slotClass = slot.vpn === true ? 'player-slot vpn-player' : slot.vpn === false ? 'player-slot nonvpn-player' : slot.key ? 'player-slot key-player' : 'player-slot';
        const vpnLabel = slot.vpn === true ? ' 🔐 VPN' : slot.vpn === false ? ' 🌐 Non-VPN' : '';
        html += `<div class="${slotClass}" style="margin:10px 0;padding:14px;border-radius:10px;display:flex;align-items:center;gap:12px;flex-wrap:wrap;">
          <label style="min-width:180px;font-weight:700;color:var(--primary);">${slot.label}${vpnLabel}</label>
          <select id="${format}-slot-${index}" style="flex:1;min-width:180px;">
            <option value="">👈 Select Player</option>
            ${allPlayers.map(p => `<option value="${p.id}" style="color:${p.uid === 'TBD' || p.device === 'TBD' ? '#f59e0b' : '#10b981'}">${p.name} ${p.uid === 'TBD' || p.device === 'TBD' ? '⚠️' : '✅'}</option>`).join('')}
          </select>
        </div>`;
      });
      container.innerHTML = html || '<p style="text-align:center;color:#6b7280;">No players available. Add players in Transfer Window first.</p>';
    }

    function showSquadForm(format) {
      document.querySelectorAll('.squad-form').forEach(f => f.classList.remove('active'));
      if (format) document.getElementById(`squad-${format}`).classList.add('active');
    }

    function saveSquad(format) {
      if (!currentTeamId) return showToast('Login first', 'error');
      const config = squadConfigs[format];
      const squad = [];
      const missing = [];
      
      for (let i = 0; i < config.length; i++) {
        const playerId = document.getElementById(`${format}-slot-${i}`).value;
        if (!playerId) {
          missing.push(config[i].label);
          continue;
        }
        const player = allPlayers.find(p => p.id === playerId);
        if (!player) continue;
        
        squad.push({
          slot:       config[i].label,
          playerId,
          playerName: player.name,
          isKey:      config[i].key,
          category:   config[i].category,
          vpn:        config[i].vpn
        });
      }
      
      if (missing.length > 0) {
        const statusDiv = document.getElementById(`squad-${format}-status`);
        statusDiv.innerHTML = `<div style="background:#fee2e2;border:2px solid #ef4444;color:#991b1b;padding:14px;border-radius:8px;font-weight:600;">❌ Missing ${missing.length} slot(s): ${missing.join(', ')}</div>`;
        statusDiv.style.display = 'block';
        showToast(`Select players for: ${missing.join(', ')}`, 'error');
        return;
      }
      
      if (squad.length !== config.length) {
        showToast('All slots must be filled', 'error');
        return;
      }

      db.ref('teams/' + currentTeamId + '/squads/' + format).set({ squad, updatedAt: Date.now(), submittedAt: new Date().toISOString() })
        .then(() => {
          const statusDiv = document.getElementById(`squad-${format}-status`);
          statusDiv.innerHTML = `<div style="background:#dcfce7;border:2px solid #16a34a;color:#166534;padding:14px;border-radius:8px;font-weight:600;">✅ ${format} Squad Submitted Successfully!</div>`;
          statusDiv.style.display = 'block';
          showToast(`✅ ${format} squad submitted!`);
          setTimeout(() => {
            statusDiv.style.display = 'none';
          }, 5000);
        })
        .catch(e => {
          showToast('Error saving squad: ' + e.message, 'error');
        });
    }

    function loadSavedSquads() {
      if (!currentTeamId) return;
      db.ref('teams/' + currentTeamId + '/squads').once('value', snapshot => {
        const squads = snapshot.val() || {};
        Object.entries(squads).forEach(([format, data]) => {
          data.squad.forEach((slot, index) => {
            const el = document.getElementById(`${format}-slot-${index}`);
            if (el) el.value = slot.playerId;
          });
        });
      });
    }

    // ====== VIEWER ======
    function loadViewerData() {
      if (!checkFirebase()) return;
      db.ref('teams').once('value', snapshot => {
        const teams = snapshot.val() || {};
        let html = '';
        Object.entries(teams).forEach(([id, team]) => {
          const playerCount = Object.keys(team.players || {}).length;
          html += `<div class="card">
            <div style="display:flex;align-items:center;gap:1.5rem;">
              <img src="${team.logo}" style="width:80px;height:80px;border-radius:50%;object-fit:cover;border:3px solid var(--primary);">
              <div>
                <h2 style="margin:0;">${team.name}</h2>
                <p style="color:var(--text-light);">Captain: ${team.captain}</p>
                <p style="color:var(--text-light);">Players: ${playerCount}</p>
              </div>
            </div>
          </div>`;
        });
        document.getElementById('viewer-teams').innerHTML = html || '<p style="text-align:center;color:#6b7280;">No teams yet</p>';
      });
    }

    function loadViewerFixtures() {
      if (!checkFirebase()) return;
      Promise.all([db.ref('fixtures').once('value'), db.ref('teams').once('value')]).then(([fs, ts]) => {
        const fixtures = fs.val() || {}, teams = ts.val() || {};
        let html = '';
        Object.entries(fixtures).forEach(([id, f]) => {
          const t1 = teams[f.team1], t2 = teams[f.team2];
          if (t1 && t2) {
            html += `<div class="fixture-item">
              <div style="display:flex;align-items:center;gap:1rem;">
                <img src="${t1.logo}" style="width:50px;height:50px;border-radius:50%;object-fit:cover;">
                <div class="fixture-teams">${t1.name} vs ${t2.name}</div>
                <img src="${t2.logo}" style="width:50px;height:50px;border-radius:50%;object-fit:cover;">
              </div>
              <div>
                <div class="fixture-date">${f.date} at ${f.time}</div>
                <div class="fixture-date">${f.format}</div>
                ${f.venue ? `<div class="fixture-date">Venue: ${f.venue}</div>` : ''}
              </div>
            </div>`;
          }
        });
        document.getElementById('viewer-fixtures').innerHTML = html || '<p style="text-align:center;color:#6b7280;">No fixtures yet</p>';
      });
    }

    // ====== SCORECARD ======
    function loadTeamLogosForSc() {
      if (!checkFirebase()) return;
      db.ref('teams').once('value', snap => {
        const teams = snap.val() || {};
        teamLogoMap = {};
        Object.values(teams).forEach(t => { teamLogoMap[t.name] = t.logo; });
      });
    }

    function cleanName(name) {
      return name ? name.replace(/[👑🔑@]/g, '').trim() : '';
    }

    function generateScorecard() {
      const text = document.getElementById('scPasteText').value.trim();
      if (!text) return showToast('📋 Paste match data first', 'error');

      const lines = text.split('\n').map(l => l.trim()).filter(l => l);
      let team1 = '', team2 = '';
      for (const line of lines) {
        const sep = line.includes('⚒️') ? '⚒️' : line.includes('🆚') ? '🆚' : line.includes('|') ? '|' : null;
        if (sep && !line.match(/\d+.*🆚.*\d+/)) {
          const parts = line.split(sep).map(t => t.trim());
          if (parts.length >= 2) { team1 = parts[0]; team2 = parts[1]; break; }
        }
      }

      if (!team1 || !team2) return showToast('❌ Cannot find team names. Use format: Team1 ⚒️ Team2', 'error');

      const matchRegex = /(.+?)\s*\(?(\d+)\)?\s*🆚\s*\(?(\d+)\)?\s*(.+)/;
      const matches = [];
      let motmPlayer = '';

      for (const line of lines) {
        const m = line.match(matchRegex);
        if (m) {
          const [, p1, s1, s2, p2] = m;
          matches.push([p1.trim(), s1, s2, p2.trim()]);
          if (p1.includes('👑')) motmPlayer = cleanName(p1);
          else if (p2.includes('👑')) motmPlayer = cleanName(p2);
        }
      }

      if (matches.length === 0) return showToast('❌ No matches found. Use: Player1 (X)🆚(Y) Player2', 'error');
      if (!motmPlayer) return showToast('❌ No Man of the Match (👑) found', 'error');

      let t1pts = 0, t2pts = 0;
      matches.forEach(([, s1, s2]) => {
        if (parseInt(s1) > parseInt(s2)) t1pts += 3;
        else if (parseInt(s2) > parseInt(s1)) t2pts += 3;
        else { t1pts++; t2pts++; }
      });

      const stageInput = document.getElementById('scStageInput').value.trim();
      const logoInput  = document.getElementById('scLogoInput').value.trim();
      const nameInput  = document.getElementById('scNameInput').value.trim();

      const stage = stageInput || scConfig.stage || 'Group Stage';
      const logo  = logoInput  || scConfig.logo  || '';
      const tName = nameInput  || scConfig.name  || 'La Viola Cup';

      const t1logo = teamLogoMap[team1] || '';
      const t2logo = teamLogoMap[team2] || '';

      ['dark','light'].forEach(theme => {
        const t1Panel = document.getElementById(`sc-${theme}-t1`);
        const t2Panel = document.getElementById(`sc-${theme}-t2`);
        t1Panel.innerHTML = `${t1logo ? `<img src="${t1logo}" class="sc-team-logo">` : ''}${team1}`;
        t2Panel.innerHTML = `${t2logo ? `<img src="${t2logo}" class="sc-team-logo">` : ''}${team2}`;
        document.getElementById(`sc-${theme}-s1`).textContent = t1pts;
        document.getElementById(`sc-${theme}-s2`).textContent = t2pts;
        document.getElementById(`sc-${theme}-name`).textContent = tName;
        document.getElementById(`sc-${theme}-stage`).textContent = stage;
        const logoEl = document.getElementById(`sc-${theme}-logo`);
        if (logo) { logoEl.src = logo; logoEl.style.display = 'block'; } else logoEl.style.display = 'none';

        const matchesDiv = document.getElementById(`sc-${theme}-matches`);
        matchesDiv.innerHTML = '';
        matches.forEach(([p1, s1, s2, p2]) => {
          const cp1 = cleanName(p1), cp2 = cleanName(p2);
          const mc1 = p1.includes('👑') ? 'sc-motm-player' : '';
          const mc2 = p2.includes('👑') ? 'sc-motm-player' : '';
          matchesDiv.innerHTML += `
            <div class="sc-match-row">
              <div class="sc-player-container ${mc1}"><span class="sc-player-name">${cp1}</span></div>
              <div class="sc-score-box">${s1} - ${s2}</div>
              <div class="sc-player-container ${mc2}"><span class="sc-player-name">${cp2}</span></div>
            </div>`;
        });

        const winText = t1pts > t2pts ? `${team1} wins! 🎉` : t2pts > t1pts ? `${team2} wins! 🎉` : 'Draw! 🤝';
        document.getElementById(`sc-${theme}-winner`).textContent = `Winner: ${winText}`;
        document.getElementById(`sc-${theme}-motm`).textContent   = `Man of the Match: 👑 ${motmPlayer}`;
      });

      saveScArchive(team1, team2, t1pts, t2pts, matches, motmPlayer, stage, tName, logo);
      showToast('✅ Scorecards generated successfully!');
    }

    function saveScArchive(team1, team2, t1pts, t2pts, matches, motm, stage, tName, logo) {
      if (!checkFirebase()) return;
      const id = 'sc_' + Date.now();
      const data = { id, team1, team2, t1pts, t2pts, matches, motm, stage, tName, logo,
        inputText: document.getElementById('scPasteText').value, timestamp: new Date().toISOString() };
      db.ref('scArchives/' + id).set(data).then(() => loadScArchives());
    }

    function loadScArchives() {
      if (!checkFirebase()) return;
      db.ref('scArchives').once('value', snap => {
        const data = snap.val() || {};
        scArchives = Object.values(data).sort((a, b) => b.timestamp > a.timestamp ? 1 : -1);
        renderScArchives();
      });
    }

    function renderScArchives() {
      const list = document.getElementById('sc-archive-list');
      if (!list) return;
      
      // Update archive count
      const countEl = document.getElementById('archive-count');
      if (countEl) countEl.innerHTML = `${scArchives.length} Saved`;
      
      if (scArchives.length === 0) { 
        list.innerHTML = '<p style="text-align:center;color:#6b7280;padding:2rem;">No archives yet. Generate your first scorecard!</p>'; 
        return; 
      }
      
      list.innerHTML = scArchives.map((a, i) => {
        const timestamp = new Date(a.timestamp);
        const timeStr = timestamp.toLocaleString('en-GB',{day:'2-digit',month:'short',year:'numeric',hour:'2-digit',minute:'2-digit'});
        const winner = a.t1pts > a.t2pts ? a.team1 : a.t2pts > a.t1pts ? a.team2 : 'Draw';
        
        return `
          <div style="background:white;border:2px solid var(--border);border-radius:12px;padding:1.2rem;margin:1rem 0;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:1rem;transition:all 0.3s;">
            <div style="flex:1;min-width:250px;">
              <div style="display:flex;align-items:center;gap:1rem;margin-bottom:0.5rem;">
                <div>
                  <div class="archive-teams" style="font-size:1.1rem;">${a.team1} <span style="color:var(--text-light);">vs</span> ${a.team2}</div>
                  <div class="archive-info" style="margin-top:0.25rem;">📊 ${a.t1pts} - ${a.t2pts}</div>
                </div>
                <div style="font-size:28px;font-weight:800;color:var(--primary);">${winner === 'Draw' ? '🤝' : '🏆'}</div>
              </div>
              <div class="archive-info">🏆 ${a.tName} • ${a.stage}</div>
              <div class="archive-info">👑 MOTM: <strong>${a.motm}</strong></div>
              <div class="archive-info">📅 ${timeStr}</div>
            </div>
            <div style="display:flex;gap:0.5rem;flex-wrap:wrap;">
              <button class="btn-primary" style="padding:8px 14px;font-size:0.85rem;" onclick="loadScArchiveItem(${i})">📂 Load</button>
              <button class="btn-danger" style="padding:8px 14px;font-size:0.85rem;" onclick="deleteScArchive('${a.id}')">🗑️ Delete</button>
            </div>
          </div>
        `;
      }).join('');
    }

    function loadScArchiveItem(index) {
      const a = scArchives[index];
      document.getElementById('scPasteText').value  = a.inputText || '';
      document.getElementById('scStageInput').value = a.stage || '';
      document.getElementById('scLogoInput').value  = a.logo  || '';
      document.getElementById('scNameInput').value  = a.tName || '';
      generateScorecard();
      showToast('Archive loaded!');
    }

    function deleteScArchive(id) {
      if (!checkFirebase()) return;
      if (confirm('Delete this archive?')) {
        db.ref('scArchives/' + id).remove().then(() => { showToast('Archive deleted'); loadScArchives(); });
      }
    }

    async function downloadScorecard(id) {
      const el = document.getElementById(id);
      try {
        const imgs = el.querySelectorAll('img');
        await Promise.all(Array.from(imgs).map(img => new Promise(res => {
          if (img.complete) { res(); return; }
          img.onload = res; img.onerror = res;
        })));
        const canvas = await html2canvas(el, {
          backgroundColor: id === 'sc-dark' ? '#2c2c2c' : '#ffffff',
          scale: 2, useCORS: true
        });
        const link = document.createElement('a');
        link.download = `scorecard_${id}_${Date.now()}.png`;
        link.href = canvas.toDataURL('image/png');
        link.click();
        showToast('Downloaded!');
      } catch(e) {
        showToast('Download failed: ' + e.message, 'error');
      }
    }
  </script>
</body>
</html>
