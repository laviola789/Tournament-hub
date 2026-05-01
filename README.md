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
      opacity: 1;
      transition: opacity 0.3s ease;
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

    /* ===================== BADGE STYLES ===================== */
    .badge {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 24px;
      height: 24px;
      border-radius: 50%;
      font-size: 12px;
      font-weight: 700;
      margin-right: 6px;
      vertical-align: middle;
      flex-shrink: 0;
    }

    .badge-primary { background: var(--primary); color: white; }
    .badge-success { background: #10b981; color: white; }
    .badge-warning { background: #f59e0b; color: white; }
    .badge-danger { background: #ef4444; color: white; }
    .badge-info { background: #3b82f6; color: white; }

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

    /* ===================== ADMIN TAB SYSTEM ===================== */
    .admin-tabs {
      display: flex;
      gap: 0;
      margin-bottom: 2rem;
      background: white;
      border: 2px solid var(--border);
      border-radius: 12px;
      overflow: hidden;
      flex-wrap: wrap;
    }

    .admin-tab-btn {
      flex: 1;
      min-width: 140px;
      padding: 14px 10px;
      background: white;
      color: var(--text-light);
      border: none;
      border-radius: 0;
      cursor: pointer;
      font-weight: 700;
      font-size: 0.9rem;
      transition: all 0.25s;
      border-right: 1px solid var(--border);
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 6px;
    }

    .admin-tab-btn:last-child { border-right: none; }

    .admin-tab-btn:hover {
      background: var(--card);
      color: var(--primary);
      transform: none;
    }

    .admin-tab-btn.active {
      background: var(--primary);
      color: white;
      transform: none;
    }

    /* ===================== FORM GRID ===================== */
    .form-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 1rem;
    }

    .form-field label {
      display: block;
      font-weight: 700;
      color: var(--primary);
      margin-bottom: 4px;
      font-size: 0.9rem;
    }

    .form-field input,
    .form-field select {
      margin: 0;
    }

    .form-field.required label::after {
      content: ' *';
      color: #ef4444;
    }

    .optional-tag {
      display: inline-block;
      background: #e5e7eb;
      color: #6b7280;
      padding: 2px 8px;
      border-radius: 20px;
      font-size: 0.75rem;
      font-weight: 600;
      margin-left: 6px;
      vertical-align: middle;
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

    /* Captain login */
    .captain-login-box {
      background: var(--card);
      border: 2px solid var(--border);
      border-radius: 16px;
      padding: 2rem;
      margin-bottom: 1.5rem;
    }

    /* Premium Badge */
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

    /* ===================== PLAYER CARD ===================== */
    .player-card {
      background: white;
      border: 2px solid var(--border);
      border-radius: 14px;
      padding: 1.2rem;
      transition: all 0.3s;
      position: relative;
      overflow: hidden;
    }

    .player-card::before {
      content: '';
      position: absolute;
      top: 0; left: 0; right: 0;
      height: 4px;
      background: var(--border);
      transition: background 0.3s;
    }

    .player-card.complete::before { background: #10b981; }
    .player-card.incomplete::before { background: #f59e0b; }

    .player-card:hover {
      box-shadow: 0 8px 24px rgba(139,92,246,0.15);
      transform: translateY(-2px);
    }

    .player-card .pcard-name {
      font-size: 1rem;
      font-weight: 800;
      color: var(--text);
      margin-bottom: 0.8rem;
    }

    .player-card .pcard-field {
      display: flex;
      align-items: center;
      gap: 6px;
      font-size: 0.82rem;
      margin-bottom: 4px;
      color: var(--text-light);
    }

    .pcard-field .icon { font-size: 1rem; }
    .pcard-field .val-complete { color: #10b981; font-weight: 600; }
    .pcard-field .val-missing { color: #f59e0b; font-weight: 600; }

    .pcard-status {
      margin-top: 0.8rem;
      padding: 6px 10px;
      border-radius: 6px;
      font-size: 0.8rem;
      font-weight: 700;
      text-align: center;
    }

    .pcard-status.complete { background: #d1fae5; color: #065f46; }
    .pcard-status.incomplete { background: #fef3c7; color: #92400e; }

    .pcard-actions {
      display: flex;
      gap: 6px;
      margin-top: 0.8rem;
    }

    .pcard-actions button {
      flex: 1;
      padding: 8px;
      font-size: 0.8rem;
      border-radius: 8px;
    }

    /* ===================== EDIT MODAL ===================== */
    .modal-overlay {
      position: fixed;
      inset: 0;
      background: rgba(0,0,0,0.55);
      z-index: 1000;
      display: none;
      align-items: center;
      justify-content: center;
      padding: 1rem;
    }

    .modal-overlay.open {
      display: flex;
    }

    .modal-box {
      background: white;
      border-radius: 20px;
      padding: 2rem;
      max-width: 460px;
      width: 100%;
      border: 2px solid var(--border);
      box-shadow: 0 25px 60px rgba(139,92,246,0.25);
      animation: modalPop 0.25s ease;
    }

    @keyframes modalPop {
      from { transform: scale(0.92) translateY(20px); opacity: 0; }
      to { transform: scale(1) translateY(0); opacity: 1; }
    }

    .modal-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 1.5rem;
    }

    .modal-close {
      width: 36px; height: 36px;
      background: #f3f4f6;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      font-size: 1.1rem;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: background 0.2s;
    }

    .modal-close:hover { background: #e5e7eb; transform: none; }

    .modal-field {
      margin-bottom: 1rem;
    }

    .modal-field label {
      display: block;
      font-weight: 700;
      color: var(--primary);
      margin-bottom: 4px;
      font-size: 0.9rem;
    }

    .modal-field input {
      margin: 0;
    }

    .modal-field .field-hint {
      font-size: 0.78rem;
      color: var(--text-light);
      margin-top: 3px;
    }

    .modal-actions {
      display: flex;
      gap: 1rem;
      margin-top: 1.5rem;
    }

    /* squad format buttons */
    .format-pill {
      flex: 1;
      padding: 12px;
      font-weight: 700;
      border: 3px solid transparent;
      border-radius: 10px;
      background: var(--primary);
      color: white;
      cursor: pointer;
      transition: all 0.25s;
    }

    .format-pill.active {
      border-color: white;
      box-shadow: 0 0 0 3px var(--primary);
      transform: scale(1.05);
    }

    /* ===================== SQUAD INFO PROFESSIONAL ===================== */
    .roster-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 1rem;
    }

    .stat-pill {
      background: white;
      border-radius: 12px;
      padding: 1rem 1.2rem;
      border-left: 4px solid var(--primary);
    }

    .stat-pill .stat-num {
      font-size: 28px;
      font-weight: 800;
    }

    /* ===================== POINT TABLE ===================== */
    .pt-container {
      background: white;
      border-radius: 16px;
      overflow: hidden;
      border: 2px solid var(--border);
      box-shadow: 0 10px 30px rgba(139,92,246,0.1);
    }

    .pt-header {
      background: linear-gradient(135deg, #8b5cf6, #a78bfa);
      padding: 1.5rem;
      color: white;
      text-align: center;
      font-size: 1.3rem;
      font-weight: 800;
      letter-spacing: 1px;
    }

    .pt-table {
      width: 100%;
      border-collapse: collapse;
    }

    .pt-thead th {
      background: #f3f4f6;
      padding: 1rem;
      text-align: left;
      font-weight: 700;
      color: var(--text);
      border-bottom: 2px solid var(--border);
      font-size: 0.9rem;
    }

    .pt-tbody td {
      padding: 0.9rem 1rem;
      border-bottom: 1px solid var(--border);
      color: var(--text);
    }

    .pt-tbody tr:hover {
      background: var(--card);
    }

    .pt-rank {
      font-weight: 800;
      text-align: center;
      width: 50px;
    }

    .pt-medal {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 32px;
      height: 32px;
      border-radius: 50%;
      font-weight: 800;
      color: white;
      font-size: 1.2rem;
    }

    .pt-medal.gold {
      background: linear-gradient(135deg, #fbbf24, #f59e0b);
      box-shadow: 0 0 15px rgba(251, 191, 36, 0.4);
    }

    .pt-medal.silver {
      background: linear-gradient(135deg, #e5e7eb, #d1d5db);
      box-shadow: 0 0 15px rgba(209, 213, 219, 0.4);
      color: #374151;
    }

    .pt-medal.bronze {
      background: linear-gradient(135deg, #f97316, #ea580c);
      box-shadow: 0 0 15px rgba(249, 115, 22, 0.4);
    }

    .pt-team {
      display: flex;
      align-items: center;
      gap: 10px;
      font-weight: 700;
    }

    .pt-team-logo {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid var(--border);
    }

    .pt-stats {
      text-align: center;
      font-weight: 600;
    }

    .pt-points {
      font-size: 1.2rem;
      font-weight: 800;
      color: var(--primary);
      text-align: center;
    }

    /* ===================== POINT TABLE RENDER (JPG) ===================== */
    .pt-render-wrapper {
      background: linear-gradient(135deg, #0f172a, #1e293b);
      padding: 40px;
      border-radius: 20px;
      font-family: 'Orbitron', 'Montserrat', sans-serif;
      box-shadow: 0 0 60px rgba(139,92,246,0.4);
    }

    .pt-render-title {
      text-align: center;
      color: white;
      margin-bottom: 10px;
      font-size: 32px;
      font-weight: 800;
      letter-spacing: 2px;
      font-family: 'Orbitron', sans-serif;
      text-shadow: 0 0 20px rgba(139,92,246,0.6);
    }

    .pt-render-subtitle {
      text-align: center;
      color: #a78bfa;
      margin-bottom: 30px;
      font-size: 14px;
      letter-spacing: 3px;
      font-family: 'Orbitron', sans-serif;
    }

    .pt-render-table {
      width: 100%;
      border-collapse: collapse;
      background: rgba(255,255,255,0.95);
      border-radius: 15px;
      overflow: hidden;
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
    }

    .pt-render-table th {
      background: linear-gradient(135deg, #8b5cf6, #a78bfa);
      color: white;
      padding: 16px 12px;
      text-align: left;
      font-weight: 800;
      font-size: 13px;
      letter-spacing: 1px;
      border-bottom: 3px solid #7c3aed;
    }

    .pt-render-table td {
      padding: 14px 12px;
      border-bottom: 1px solid #e5e7eb;
      color: #1f2937;
      font-weight: 500;
    }

    .pt-render-table tr:last-child td {
      border-bottom: none;
    }

    .pt-render-table tr:hover {
      background: #f9fafb;
    }

    .pt-render-rank {
      font-weight: 900;
      font-size: 16px;
      text-align: center;
      width: 40px;
    }

    .pt-render-medal {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 36px;
      height: 36px;
      border-radius: 50%;
      font-weight: 800;
      font-size: 18px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.2);
    }

    .pt-render-medal.gold {
      background: linear-gradient(135deg, #fbbf24, #f59e0b);
      color: white;
    }

    .pt-render-medal.silver {
      background: linear-gradient(135deg, #e5e7eb, #d1d5db);
      color: #374151;
    }

    .pt-render-medal.bronze {
      background: linear-gradient(135deg, #f97316, #ea580c);
      color: white;
    }

    .pt-render-team {
      display: flex;
      align-items: center;
      gap: 12px;
      font-weight: 700;
      color: #1f2937;
    }

    .pt-render-logo {
      width: 44px;
      height: 44px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid #a78bfa;
      box-shadow: 0 4px 10px rgba(139,92,246,0.3);
    }

    .pt-render-stat {
      text-align: center;
      font-weight: 600;
      font-size: 14px;
    }

    .pt-render-points {
      font-size: 16px;
      font-weight: 900;
      color: #8b5cf6;
      text-align: center;
    }

    .pt-render-footer {
      text-align: center;
      margin-top: 30px;
      padding-top: 20px;
      border-top: 2px solid rgba(139,92,246,0.3);
      color: #a78bfa;
      font-size: 12px;
      letter-spacing: 2px;
      font-family: 'Orbitron', sans-serif;
    }

    /* ===================== SCORE SUBMIT MODAL ===================== */
    .score-submit-box {
      background: white;
      border: 2px solid var(--border);
      border-radius: 14px;
      padding: 1.5rem;
      margin: 1rem 0;
    }

    .score-input-group {
      display: flex;
      align-items: center;
      gap: 1rem;
      margin: 1.2rem 0;
    }

    .score-input-group .team-badge {
      flex: 1;
      background: var(--primary);
      color: white;
      padding: 12px 16px;
      border-radius: 10px;
      font-weight: 700;
      text-align: center;
    }

    .score-input-group input {
      width: 100px;
      padding: 12px;
      font-size: 1.3rem;
      text-align: center;
      font-weight: 700;
      margin: 0;
    }

    .match-vs {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 1rem;
      margin: 1.5rem 0;
      padding: 1.5rem;
      background: var(--card);
      border-radius: 10px;
    }

    .match-vs .team {
      flex: 1;
      text-align: center;
    }

    .match-vs .team-name {
      font-weight: 800;
      font-size: 1.1rem;
      color: var(--primary);
      margin-bottom: 0.5rem;
    }

    .match-vs .team-logo {
      width: 60px;
      height: 60px;
      border-radius: 50%;
      object-fit: cover;
      border: 3px solid var(--primary);
      margin: 0 auto 0.5rem;
      display: block;
    }

    .vs-separator {
      font-size: 1.5rem;
      font-weight: 800;
      color: #ef4444;
    }

  </style>
</head>
<body>
  <div class="header">
    <nav>
      <button onclick="showSection('viewer')" class="active" id="nav-viewer"><span class="badge badge-primary">🏆</span> Viewer</button>
      <button onclick="showSection('scorecard')" id="nav-scorecard"><span class="badge badge-info">⚽</span> Scorecard</button>
      <button onclick="showSection('admin')" id="nav-admin"><span class="badge badge-warning">⚙️</span> Admin</button>
      <button onclick="showSection('captain')" id="nav-captain"><span class="badge badge-success">🎖️</span> Captain</button>
    </nav>
  </div>

  <!-- ===== EDIT PLAYER MODAL ===== -->
  <div class="modal-overlay" id="edit-modal">
    <div class="modal-box">
      <div class="modal-header">
        <h3 style="margin:0;">✏️ Edit Player</h3>
        <button class="modal-close" onclick="closeEditModal()">✕</button>
      </div>

      <div id="modal-player-badge" style="background:var(--card);border-radius:10px;padding:10px 14px;margin-bottom:1.2rem;display:flex;align-items:center;gap:10px;border:2px solid var(--border);">
        <span style="font-size:2rem;">👤</span>
        <div>
          <div style="font-weight:800;color:var(--primary);" id="modal-player-name-display">Player Name</div>
          <div style="font-size:0.8rem;color:var(--text-light);">Editing player info</div>
        </div>
      </div>

      <div class="modal-field">
        <label>👤 Player Name <span style="color:#ef4444;">*</span></label>
        <input type="text" id="modal-name" placeholder="Enter player name">
      </div>

      <div class="modal-field">
        <label>📱 User ID (UID)</label>
        <input type="text" id="modal-uid" placeholder="Enter UID or leave blank">
        <div class="field-hint">Leave blank to mark as TBD</div>
      </div>

      <div class="modal-field">
        <label>📲 Device Name</label>
        <input type="text" id="modal-device" placeholder="e.g. Samsung S24 Ultra">
        <div class="field-hint">Leave blank to mark as TBD</div>
      </div>

      <input type="hidden" id="modal-player-id">

      <div class="modal-actions">
        <button class="btn-success" style="flex:2;" onclick="saveEditPlayer()">✅ Save Changes</button>
        <button class="btn-danger" style="flex:1;" onclick="deletePlayerFromModal()">🗑️ Delete</button>
        <button style="flex:1;background:#f3f4f6;color:var(--text);" onclick="closeEditModal()">Cancel</button>
      </div>
    </div>
  </div>

  <div class="container">

    <!-- ===== VIEWER ===== -->
    <div id="viewer" class="section active">
      <h1>La Viola Fleur de Lis</h1>
      <p style="text-align:center; font-size:1.3rem; margin-bottom:2rem; color:#8b5cf6;">Intra Bid Tournament 2026</p>
      
      <div class="card">
        <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:1.5rem;flex-wrap:wrap;gap:1rem;">
          <h2 style="margin:0;"><span class="badge badge-primary">🏆</span> Point Table</h2>
          <button class="btn-success" onclick="downloadPointTableJPG()"><span class="badge badge-success">📥</span> Download JPG</button>
        </div>
        <div id="points-table-view"></div>
      </div>

      <div class="card">
        <h2 style="text-align:center; margin-bottom:1.5rem;"><span class="badge badge-info">👥</span> Teams</h2>
        <div id="viewer-teams"></div>
      </div>

      <div class="card">
        <h2 style="text-align:center; margin-bottom:1.5rem;"><span class="badge badge-warning">📅</span> Recent Matches</h2>
        <div id="viewer-fixtures"></div>
      </div>
    </div>

    <!-- ===== SCORECARD ===== -->
    <div id="scorecard" class="section">
      <div class="card" style="max-width:1100px; margin:0 auto;">
        <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:1.5rem;flex-wrap:wrap;gap:1rem;">
          <div>
            <h2 style="margin:0;"><span class="badge badge-info">⚽</span> Scorecard Generator</h2>
            <p style="color:var(--text-light); margin:0.5rem 0 0;">Create professional match scorecards</p>
          </div>
          <span class="premium-badge" style="font-size:0.9rem;padding:8px 16px;">PREMIUM</span>
        </div>

        <div style="background:linear-gradient(135deg, rgba(217, 70, 239, 0.1), rgba(168, 85, 247, 0.1));border-radius:12px;padding:1.5rem;margin-bottom:1.5rem;">
          <p style="color:var(--text);margin:0;font-weight:600;margin-bottom:1rem;"><span class="badge badge-warning">📋</span> Match Input Format</p>
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
      <div class="card" style="max-width:1000px; margin:0 auto;">

        <div style="text-align:center;margin-bottom:1.5rem;">
          <h2 style="margin:0;"><span class="badge badge-warning">⚙️</span> Admin Panel</h2>
          <p style="color:var(--text-light);margin-top:0.4rem;">Manage teams, fixtures and scorecard settings</p>
        </div>

        <div id="admin-login">
          <div style="max-width:380px;margin:0 auto;background:var(--card);border-radius:14px;padding:2rem;border:2px solid var(--border);">
            <div style="text-align:center;font-size:3rem;margin-bottom:1rem;">🔐</div>
            <input type="password" id="adminPass" placeholder="Enter Admin Password" style="margin-bottom:1rem;">
            <button class="btn-primary" style="width:100%;padding:14px;font-size:1rem;" onclick="adminLogin()">Login</button>
          </div>
        </div>

        <div id="admin-content" style="display:none;">

          <!-- Tab Bar -->
          <div class="admin-tabs">
            <button class="admin-tab-btn active" onclick="showAdminTab(1)" id="atab-1">👥 Teams</button>
            <button class="admin-tab-btn" onclick="showAdminTab(2)" id="atab-2">📅 Fixtures</button>
            <button class="admin-tab-btn" onclick="showAdminTab(3)" id="atab-3">📊 Results</button>
            <button class="admin-tab-btn" onclick="showAdminTab(4)" id="atab-4">🎨 SC Config</button>
            <button class="admin-tab-btn" onclick="showAdminTab(5)" id="atab-5">🚀 Generator</button>
          </div>

          <!-- ── Tab 1: Team Registration ── -->
          <div id="admin-tab1">
            <div style="background:var(--card);border-radius:14px;padding:1.5rem;border:2px solid var(--border);margin-bottom:1.5rem;">
              <h3 style="margin:0 0 1.2rem;">➕ Register New Team</h3>

              <div class="form-grid">
                <div class="form-field required">
                  <label>Team Name</label>
                  <input type="text" id="teamName" placeholder="e.g. La Viola FC">
                </div>
                <div class="form-field required">
                  <label>Captain Name</label>
                  <input type="text" id="captainName" placeholder="e.g. Alex Johnson">
                </div>
                <div class="form-field required">
                  <label>Team Password</label>
                  <input type="text" id="teamPassword" placeholder="Create a password">
                </div>
              </div>

              <div style="margin-top:1rem;">
                <div class="upload-area">
                  <p style="margin-bottom:1rem; font-weight:600;">🖼️ Team Logo</p>
                  <input type="file" id="teamLogoInput" accept="image/*">
                  <div style="display:flex;gap:0.8rem;margin-top:10px;flex-wrap:wrap;justify-content:center;">
                    <button class="btn-success" onclick="uploadTeamLogo()">1. Upload</button>
                    <button class="btn-primary" onclick="setTeamLogoPreview()">3. Preview</button>
                  </div>
                  <div class="url-display" id="teamLogoUrlDisplay" style="margin-top:0.8rem;">URL will appear after upload…</div>
                  <input type="text" id="teamLogoUrl" placeholder="2. Paste URL (or auto-filled after upload)">
                  <img id="teamLogoPreview" class="preview">
                </div>
              </div>

              <button class="btn-success" style="width:100%; margin-top:1.2rem;padding:14px;font-size:1rem;" onclick="registerTeam()">✅ Register Team</button>
            </div>

            <div>
              <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:1rem;flex-wrap:wrap;gap:0.5rem;">
                <h3 style="margin:0;">📋 Registered Teams</h3>
                <span style="font-size:0.85rem;color:var(--text-light);">Admin can see all passwords</span>
              </div>
              <div id="admin-teams-list"></div>
            </div>
          </div>

          <!-- ── Tab 2: Fixture Creator ── -->
          <div id="admin-tab2" style="display:none;">
            <div style="background:var(--card);border-radius:14px;padding:1.5rem;border:2px solid var(--border);margin-bottom:1.5rem;">
              <h3 style="margin:0 0 1.2rem;">➕ Create New Fixture</h3>

              <div class="form-grid">
                <div class="form-field required">
                  <label>Home Team</label>
                  <select id="fixtureTeam1"><option value="">Select Team 1</option></select>
                </div>
                <div class="form-field required">
                  <label>Away Team</label>
                  <select id="fixtureTeam2"><option value="">Select Team 2</option></select>
                </div>
                <div class="form-field required">
                  <label>Match Date</label>
                  <input type="date" id="fixtureDate">
                </div>
                <div class="form-field required">
                  <label>Format</label>
                  <select id="fixtureFormat">
                    <option value="">Select Format</option>
                    <option value="10v10">10v10</option>
                    <option value="12v12">12v12</option>
                    <option value="8v8">8v8</option>
                    <option value="6v6">6v6</option>
                  </select>
                </div>
                <div class="form-field">
                  <label>Venue <span class="optional-tag">Optional</span></label>
                  <input type="text" id="fixtureVenue" placeholder="e.g. Stadium A">
                </div>
              </div>

              <button class="btn-success" style="width:100%; margin-top:1.2rem;padding:14px;font-size:1rem;" onclick="createFixture()">📅 Create Fixture</button>
            </div>

            <div>
              <h3 style="margin-bottom:1rem;">📋 All Fixtures</h3>
              <div id="admin-fixtures-list"></div>
            </div>
          </div>

          <!-- ── Tab 3: Results Submission ── -->
          <div id="admin-tab3" style="display:none;">
            <div style="background:var(--card);border-radius:14px;padding:1.2rem;border:2px solid var(--border);margin-bottom:1.5rem;display:flex;align-items:flex-start;gap:12px;">
              <div style="font-size:2rem;flex-shrink:0;">ℹ️</div>
              <div>
                <div style="font-weight:700;color:var(--text);margin-bottom:4px;">Submit Match Results</div>
                <div style="color:var(--text-light);font-size:0.9rem;">Enter scores for completed fixtures. Point table will automatically update.</div>
              </div>
            </div>
            <div id="admin-results-list"></div>
          </div>

          <!-- ── Tab 4: Scorecard Config ── -->
          <div id="admin-tab4" style="display:none;">
            <div style="background:var(--card);border-radius:14px;padding:1.5rem;border:2px solid var(--border);margin-bottom:1.5rem;">
              <h3 style="margin:0 0 0.5rem;">🎨 Scorecard Settings</h3>
              <p style="color:var(--text-light);margin-bottom:1.2rem;font-size:0.9rem;">Set default tournament info used in scorecard generator</p>

              <div class="form-grid">
                <div class="form-field required">
                  <label>Tournament Name</label>
                  <input type="text" id="scConfigName" placeholder="e.g. La Viola Cup 2026">
                </div>
                <div class="form-field required">
                  <label>Tournament Stage</label>
                  <input type="text" id="scConfigStage" placeholder="e.g. Group Stage">
                </div>
                <div class="form-field">
                  <label>Logo URL <span class="optional-tag">Optional</span></label>
                  <input type="text" id="scConfigLogoUrl" placeholder="https://...">
                </div>
              </div>

              <div class="upload-area" style="margin-top:1rem;">
                <p style="font-weight:600; margin-bottom:0.5rem;">Or Upload Logo</p>
                <input type="file" id="scConfigLogoFile" accept="image/*">
                <button class="btn-success" onclick="uploadScConfigLogo()" style="margin-top:8px;">Upload Logo</button>
                <div class="url-display" id="scConfigLogoDisplay">URL will appear here…</div>
              </div>

              <button class="btn-primary" style="width:100%; margin-top:1.2rem;padding:14px;" onclick="saveScConfig()">💾 Save Config</button>
            </div>

            <div style="background:linear-gradient(135deg,rgba(139,92,246,0.07),rgba(167,139,250,0.07));border-radius:14px;padding:1.5rem;border:2px solid var(--border);margin-bottom:1.5rem;">
              <h3 style="margin:0 0 1rem;">⏰ Match Deadlines</h3>
              <p style="color:var(--text-light);margin-bottom:1.2rem;font-size:0.9rem;">Set times for First Day, Star, and Last Day. These will apply to all fixtures.</p>

              <div class="form-grid">
                <div class="form-field required">
                  <label>🚨 First Day Time</label>
                  <input type="time" id="timingFirstDay" placeholder="00:00">
                </div>
                <div class="form-field required">
                  <label>⭐ Star Time</label>
                  <input type="time" id="timingStar" placeholder="23:00">
                </div>
                <div class="form-field required">
                  <label>⛔ Last Day Time</label>
                  <input type="time" id="timingLastDay" placeholder="01:20">
                </div>
              </div>

              <button class="btn-success" style="width:100%; margin-top:1rem;padding:14px;" onclick="saveFixtureTimings()">⏰ Save Timings</button>
            </div>

            <div>
              <h3 style="margin-bottom:1rem;">📌 Current Config</h3>
              <div id="scConfigDisplay" style="background:white; padding:1rem; border-radius:8px; border:2px solid var(--border);margin-bottom:1.5rem;"></div>
              <div id="timingsDisplay" style="background:white; padding:1rem; border-radius:8px; border:2px solid var(--border);"></div>
            </div>
          </div>

          <!-- ── Tab 5: Fixture Generator ── -->
          <div id="admin-tab5" style="display:none;">
            <div style="background:var(--card);border-radius:14px;padding:1.2rem;border:2px solid var(--border);margin-bottom:1.5rem;display:flex;align-items:flex-start;gap:12px;">
              <div style="font-size:2rem;flex-shrink:0;">ℹ️</div>
              <div>
                <div style="font-weight:700;color:var(--text);margin-bottom:4px;">How the Generator Works</div>
                <div style="color:var(--text-light);font-size:0.9rem;">Fixtures are available to generate once squad submissions are in. They automatically <strong>disappear 24 hours after the match date</strong>. April 23 match → removed on April 24.</div>
              </div>
            </div>
            <div id="fg-fixtures-list"></div>
          </div>

        </div>
      </div>
    </div>

    <!-- ===== CAPTAIN ===== -->
    <div id="captain" class="section">
      <div class="card" style="max-width:960px; margin:0 auto;">
        <div style="text-align:center;margin-bottom:1.5rem;">
          <h2 style="margin:0;"><span class="badge badge-success">🎖️</span> Captain Panel</h2>
        </div>

        <!-- Captain Login -->
        <div id="captain-login-area">
          <div class="captain-login-box" style="max-width:400px;margin:0 auto;">
            <div style="text-align:center;font-size:3rem;margin-bottom:1rem;">🔐</div>
            <h3 style="margin-bottom:1.2rem;text-align:center;">Login to your Team</h3>
            <label style="font-weight:700;color:var(--primary);display:block;margin-bottom:4px;">Select Your Team</label>
            <select id="captainTeamSelect" style="margin-bottom:1rem;"><option value="">— Choose Team —</option></select>
            <label style="font-weight:700;color:var(--primary);display:block;margin-bottom:4px;">Team Password</label>
            <input type="password" id="teamPasswordLogin" placeholder="Enter team password" style="margin-bottom:1.2rem;">
            <button class="btn-primary" style="width:100%;padding:14px;font-size:1rem;" onclick="captainLogin()">Login</button>
          </div>
        </div>

        <div id="captain-content" style="display:none; margin-top:2rem;">
          <div style="text-align:center;margin-bottom:1.5rem;">
            <div id="currentTeamName" style="font-size:1.6rem;font-weight:800;color:var(--primary);"></div>
            <div style="color:var(--text-light);font-size:0.9rem;">Team Captain Panel</div>
          </div>

          <!-- Captain Tab Bar -->
          <div class="admin-tabs" style="margin-bottom:1.5rem;">
            <button class="admin-tab-btn active" onclick="showCaptainTab(1)" id="ctab-1">🔄 Transfers</button>
            <button class="admin-tab-btn" onclick="showCaptainTab(2)" id="ctab-2">👥 Squad Info</button>
            <button class="admin-tab-btn" onclick="showCaptainTab(3)" id="ctab-3">🎯 Submit Squad</button>
            <button class="admin-tab-btn" onclick="showCaptainTab(4)" id="ctab-4">🔑 Password</button>
          </div>

          <!-- Tab 1: Transfer Window -->
          <div id="captain-tab1">
            <div style="background:var(--card);border-radius:14px;padding:1.5rem;border:2px solid var(--border);margin-bottom:1.5rem;">
              <h3 style="margin:0 0 0.5rem;">➕ Add New Player</h3>
              <p style="color:var(--text-light);font-size:0.88rem;margin-bottom:1.2rem;">Only the player <strong>Name</strong> is required. UID and Device can be added later.</p>

              <div class="form-grid">
                <div class="form-field required" style="grid-column:1/-1;">
                  <label>Player Name</label>
                  <input type="text" id="playerName" placeholder="Full in-game name">
                </div>
                <div class="form-field">
                  <label>User ID (UID) <span class="optional-tag">Optional</span></label>
                  <input type="text" id="playerUid" placeholder="In-game UID">
                </div>
                <div class="form-field">
                  <label>Device Name <span class="optional-tag">Optional</span></label>
                  <input type="text" id="playerDevice" placeholder="e.g. Samsung S24">
                </div>
              </div>

              <button class="btn-success" style="width:100%;margin-top:1.2rem;padding:14px;" onclick="addPlayer()">✅ Add Player</button>
            </div>

            <div>
              <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:1rem;flex-wrap:wrap;gap:0.5rem;">
                <h3 style="margin:0;">✏️ Players — Quick Edit</h3>
                <span style="font-size:0.85rem;color:var(--text-light);">Tap any card to edit</span>
              </div>
              <div id="captain-edit-players-list"></div>
            </div>
          </div>

          <!-- Tab 2: Squad Info -->
          <div id="captain-tab2" style="display:none;">
            <div style="background:linear-gradient(135deg,rgba(139,92,246,0.07),rgba(167,139,250,0.07));border-radius:14px;padding:1.5rem;border:2px solid var(--border);margin-bottom:1.5rem;">
              <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:1.2rem;flex-wrap:wrap;gap:1rem;">
                <h3 style="margin:0;">👥 Player Roster</h3>
                <span class="premium-badge">PREMIUM</span>
              </div>

              <!-- Stats Row -->
              <div style="display:grid;grid-template-columns:repeat(3,1fr);gap:1rem;margin-bottom:1.5rem;" id="squad-stats">
                <div class="stat-pill" style="border-left-color:#8b5cf6;">
                  <div class="stat-num" id="stat-total" style="color:#8b5cf6;">0</div>
                  <div class="stat-label">Total Players</div>
                </div>
                <div class="stat-pill" style="border-left-color:#10b981;">
                  <div class="stat-num" id="stat-complete" style="color:#10b981;">0</div>
                  <div class="stat-label">Complete Info</div>
                </div>
                <div class="stat-pill" style="border-left-color:#f59e0b;">
                  <div class="stat-num" id="stat-incomplete" style="color:#f59e0b;">0</div>
                  <div class="stat-label">Need Update</div>
                </div>
              </div>

              <!-- Progress Bar -->
              <div style="margin-bottom:1.5rem;">
                <div style="display:flex;justify-content:space-between;font-size:0.82rem;color:var(--text-light);margin-bottom:6px;">
                  <span>Squad Completion</span>
                  <span id="completion-pct">0%</span>
                </div>
                <div style="background:#e5e7eb;border-radius:999px;height:10px;overflow:hidden;">
                  <div id="completion-bar" style="height:100%;background:linear-gradient(90deg,#8b5cf6,#10b981);border-radius:999px;width:0%;transition:width 0.6s ease;"></div>
                </div>
              </div>

              <!-- Filter Tabs -->
              <div style="display:flex;gap:0.5rem;margin-bottom:1rem;flex-wrap:wrap;">
                <button onclick="filterRoster('all')" id="rf-all" style="padding:7px 16px;border-radius:20px;font-size:0.82rem;font-weight:700;background:var(--primary);color:white;border:2px solid var(--primary);">All</button>
                <button onclick="filterRoster('complete')" id="rf-complete" style="padding:7px 16px;border-radius:20px;font-size:0.82rem;font-weight:700;background:white;color:#10b981;border:2px solid #10b981;">✅ Complete</button>
                <button onclick="filterRoster('incomplete')" id="rf-incomplete" style="padding:7px 16px;border-radius:20px;font-size:0.82rem;font-weight:700;background:white;color:#f59e0b;border:2px solid #f59e0b;">⚠️ Incomplete</button>
              </div>

              <!-- Roster Grid -->
              <div class="roster-grid" id="roster-grid-view"></div>

              <!-- Export Buttons -->
              <div style="margin-top:1.5rem;display:flex;gap:1rem;flex-wrap:wrap;">
                <button class="btn-success" onclick="downloadPlayerInfoJPG()">📥 Download JPG</button>
                <button class="btn-primary" onclick="exportPlayerInfo()">📋 Export Text</button>
              </div>

              <!-- Hidden table for JPG export -->
              <div style="overflow:hidden;height:0;pointer-events:none;">
                <table id="playerTable">
                  <thead><tr><th>Name</th><th>UID</th><th>Device</th><th>Status</th></tr></thead>
                  <tbody></tbody>
                </table>
              </div>
            </div>
          </div>

          <!-- Tab 3: Squad Submit -->
          <div id="captain-tab3" style="display:none;">
            <div style="background:var(--card);border-radius:14px;padding:1.5rem;border:2px solid var(--border);">
              <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:1.5rem;flex-wrap:wrap;gap:1rem;">
                <h3 style="margin:0;">🎯 Squad Submission</h3>
                <span class="premium-badge">PREMIUM</span>
              </div>

              <div style="margin-bottom:1.5rem;">
                <p style="font-weight:700;color:var(--primary);margin-bottom:1rem;">📌 Select Format</p>
                <div style="display:flex;gap:0.8rem;flex-wrap:wrap;" id="format-buttons">
                  <button class="format-pill" onclick="selectSquadFormat('10v10')" id="btn-10v10">10v10</button>
                  <button class="format-pill" onclick="selectSquadFormat('12v12')" id="btn-12v12">12v12</button>
                  <button class="format-pill" onclick="selectSquadFormat('8v8')" id="btn-8v8">8v8</button>
                  <button class="format-pill" onclick="selectSquadFormat('6v6')" id="btn-6v6">6v6</button>
                </div>
              </div>

              <div id="squad-10v10" class="squad-form">
                <h4 style="color:var(--primary);margin-bottom:0.5rem;text-align:center;font-size:1.2rem;">⚽ 10v10 Squad</h4>
                <p style="text-align:center;color:#6b7280;margin-bottom:1.2rem;font-size:0.88rem;">Select player for each slot</p>
                <div id="squad-10v10-slots" style="background:white;padding:1.2rem;border-radius:12px;border:2px solid var(--border);"></div>
                <button class="btn-success" style="width:100%;margin-top:1.2rem;padding:14px;font-size:1rem;" onclick="saveSquad('10v10')">✅ Submit 10v10 Squad</button>
                <div id="squad-10v10-status" style="margin-top:1rem;display:none;"></div>
              </div>
              <div id="squad-12v12" class="squad-form">
                <h4 style="color:var(--primary);margin-bottom:0.5rem;text-align:center;font-size:1.2rem;">⚽ 12v12 Squad</h4>
                <p style="text-align:center;color:#6b7280;margin-bottom:1.2rem;font-size:0.88rem;">Select player for each slot</p>
                <div id="squad-12v12-slots" style="background:white;padding:1.2rem;border-radius:12px;border:2px solid var(--border);"></div>
                <button class="btn-success" style="width:100%;margin-top:1.2rem;padding:14px;font-size:1rem;" onclick="saveSquad('12v12')">✅ Submit 12v12 Squad</button>
                <div id="squad-12v12-status" style="margin-top:1rem;display:none;"></div>
              </div>
              <div id="squad-8v8" class="squad-form">
                <h4 style="color:var(--primary);margin-bottom:0.5rem;text-align:center;font-size:1.2rem;">⚽ 8v8 Squad</h4>
                <p style="text-align:center;color:#6b7280;margin-bottom:1.2rem;font-size:0.88rem;">Select player for each slot</p>
                <div id="squad-8v8-slots" style="background:white;padding:1.2rem;border-radius:12px;border:2px solid var(--border);"></div>
                <button class="btn-success" style="width:100%;margin-top:1.2rem;padding:14px;font-size:1rem;" onclick="saveSquad('8v8')">✅ Submit 8v8 Squad</button>
                <div id="squad-8v8-status" style="margin-top:1rem;display:none;"></div>
              </div>
              <div id="squad-6v6" class="squad-form">
                <h4 style="color:var(--primary);margin-bottom:0.5rem;text-align:center;font-size:1.2rem;">⚽ 6v6 Squad</h4>
                <p style="text-align:center;color:#6b7280;margin-bottom:1.2rem;font-size:0.88rem;">Select player for each slot</p>
                <div id="squad-6v6-slots" style="background:white;padding:1.2rem;border-radius:12px;border:2px solid var(--border);"></div>
                <button class="btn-success" style="width:100%;margin-top:1.2rem;padding:14px;font-size:1rem;" onclick="saveSquad('6v6')">✅ Submit 6v6 Squad</button>
                <div id="squad-6v6-status" style="margin-top:1rem;display:none;"></div>
              </div>
            </div>
          </div>

          <!-- Tab 4: Change Password -->
          <div id="captain-tab4" style="display:none;">
            <div style="max-width:440px;margin:0 auto;background:var(--card);border-radius:14px;padding:1.5rem;border:2px solid var(--border);">
              <h3 style="text-align:center;margin-bottom:0.5rem;">🔑 Change Password</h3>
              <p style="color:var(--text-light);text-align:center;font-size:0.88rem;margin-bottom:1.5rem;">Share the new password with your admin after changing.</p>

              <div class="form-field required" style="margin-bottom:1rem;">
                <label>Current Password</label>
                <input type="password" id="currentPassInput" placeholder="Enter current password">
              </div>
              <div class="form-field required" style="margin-bottom:1rem;">
                <label>New Password</label>
                <input type="password" id="newPassInput" placeholder="Enter new password">
              </div>
              <div class="form-field required" style="margin-bottom:1.5rem;">
                <label>Confirm New Password</label>
                <input type="password" id="confirmPassInput" placeholder="Confirm new password">
              </div>

              <button class="btn-warning" style="width:100%;padding:14px;" onclick="changeTeamPassword()">🔑 Update Password</button>
            </div>
          </div>

        </div>
      </div>
    </div>

  </div>

  <!-- Hidden render zones for JPG exports -->
  <div id="pt-render-zone" style="position:fixed;left:-9999px;top:0;pointer-events:none;z-index:-1;"></div>
  <div id="team-info-render-zone" style="position:fixed;left:-9999px;top:0;pointer-events:none;z-index:-1;"></div>
  <div class="toast" id="toast"></div>

  <script>
    let db, storage;
    const ADMIN_PASSWORD = "*laviola#";
    let currentTeamId = null;
    let teamLogoUrl = '';
    let allPlayers = [];
    let scArchives = [];
    let scConfig = { name: 'La Viola Cup', stage: 'Group Stage', logo: '' };
    let fixtureTimings = { firstDayTime: '00:00', starTime: '23:00', lastDayTime: '01:20' };
    let teamLogoMap = {};
    let rosterFilter = 'all';
    let standings = {};

    function checkFirebase() {
      if (!db || !storage) { showToast('Firebase is still loading…', 'error'); return false; }
      return true;
    }

    function showToast(msg, type = 'success') {
      try {
        const t = document.getElementById('toast');
        if (t) {
          t.textContent = msg;
          t.className = 'toast ' + type;
          t.style.display = 'block';
          setTimeout(() => t.style.display = 'none', 3000);
        }
      } catch(e) {
        console.log('Toast:', msg);
      }
    }

    // ====== EDIT MODAL ======
    function showEditPlayerModal(playerId) {
      const player = allPlayers.find(p => p.id === playerId);
      if (!player) return;
      document.getElementById('modal-player-id').value = playerId;
      document.getElementById('modal-player-name-display').textContent = player.name;
      document.getElementById('modal-name').value = player.name;
      document.getElementById('modal-uid').value = player.uid === 'TBD' ? '' : player.uid;
      document.getElementById('modal-device').value = player.device === 'TBD' ? '' : player.device;
      document.getElementById('edit-modal').classList.add('open');
    }

    function closeEditModal() {
      document.getElementById('edit-modal').classList.remove('open');
    }

    // Close modal on overlay click
    document.addEventListener('DOMContentLoaded', () => {
      document.getElementById('edit-modal').addEventListener('click', function(e) {
        if (e.target === this) closeEditModal();
      });
    });

    function saveEditPlayer() {
      const playerId = document.getElementById('modal-player-id').value;
      const name   = document.getElementById('modal-name').value.trim();
      const uid    = document.getElementById('modal-uid').value.trim() || 'TBD';
      const device = document.getElementById('modal-device').value.trim() || 'TBD';
      if (!name) return showToast('Player name is required', 'error');

      db.ref('teams/' + currentTeamId + '/players/' + playerId).update({ name, uid, device })
        .then(() => {
          showToast('✅ Player updated!');
          closeEditModal();
          loadPlayersForEdit();
        });
    }

    function deletePlayerFromModal() {
      const playerId = document.getElementById('modal-player-id').value;
      if (!checkFirebase()) return;
      if (!confirm('Delete this player permanently?')) return;
      db.ref('teams/' + currentTeamId + '/players/' + playerId).remove()
        .then(() => { showToast('Player deleted'); closeEditModal(); loadPlayersForEdit(); });
    }

    // ====== ROSTER FILTER ======
    function filterRoster(filter) {
      rosterFilter = filter;
      ['all','complete','incomplete'].forEach(f => {
        const btn = document.getElementById('rf-' + f);
        if (f === filter) {
          btn.style.background = 'var(--primary)'; btn.style.color = 'white'; btn.style.borderColor = 'var(--primary)';
        } else {
          btn.style.background = 'white';
          if (f === 'complete') { btn.style.color = '#10b981'; btn.style.borderColor = '#10b981'; }
          else if (f === 'incomplete') { btn.style.color = '#f59e0b'; btn.style.borderColor = '#f59e0b'; }
          else { btn.style.color = 'var(--text-light)'; btn.style.borderColor = 'var(--border)'; }
        }
      });
      renderRosterGrid();
    }

    function renderRosterGrid() {
      const container = document.getElementById('roster-grid-view');
      if (!container) return;

      const filtered = rosterFilter === 'all' ? allPlayers
        : rosterFilter === 'complete' ? allPlayers.filter(p => p.uid !== 'TBD' && p.device !== 'TBD')
        : allPlayers.filter(p => p.uid === 'TBD' || p.device === 'TBD');

      if (filtered.length === 0) {
        container.innerHTML = '<div style="grid-column:1/-1;text-align:center;padding:2rem;color:#6b7280;">No players found.</div>';
        return;
      }

      container.innerHTML = filtered.map(p => {
        const complete = p.uid !== 'TBD' && p.device !== 'TBD';
        return `<div class="player-card ${complete?'complete':'incomplete'}" onclick="showEditPlayerModal('${p.id}')" style="cursor:pointer;">
          <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:0.8rem;">
            <div style="font-size:1.8rem;">👤</div>
            <div style="font-size:0.75rem;font-weight:700;padding:3px 9px;border-radius:20px;background:${complete?'#d1fae5':'#fef3c7'};color:${complete?'#065f46':'#92400e'};">${complete?'✅ READY':'⚠️ PENDING'}</div>
          </div>
          <div class="pcard-name">${p.name}</div>
          <div class="pcard-field">
            <span class="icon">📱</span>
            <span class="${p.uid==='TBD'?'val-missing':'val-complete'}">${p.uid==='TBD'?'No UID':p.uid}</span>
          </div>
          <div class="pcard-field">
            <span class="icon">📲</span>
            <span class="${p.device==='TBD'?'val-missing':'val-complete'}">${p.device==='TBD'?'No Device':p.device}</span>
          </div>
          <div class="pcard-actions">
            <button class="btn-primary" onclick="event.stopPropagation();showEditPlayerModal('${p.id}')">✏️ Edit</button>
          </div>
        </div>`;
      }).join('');
    }

    function updateSquadStats() {
      const total = allPlayers.length;
      const complete = allPlayers.filter(p => p.uid !== 'TBD' && p.device !== 'TBD').length;
      const incomplete = total - complete;
      const pct = total > 0 ? Math.round((complete / total) * 100) : 0;

      document.getElementById('stat-total').textContent = total;
      document.getElementById('stat-complete').textContent = complete;
      document.getElementById('stat-incomplete').textContent = incomplete;

      const bar = document.getElementById('completion-bar');
      const pctEl = document.getElementById('completion-pct');
      if (bar) bar.style.width = pct + '%';
      if (pctEl) pctEl.textContent = pct + '%';
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
      let text = '📋 PLAYER ROSTER EXPORT\n' + '='.repeat(50) + '\n\n';
      allPlayers.forEach((p, i) => {
        text += `${i+1}. ${p.name}\n   UID: ${p.uid}\n   Device: ${p.device}\n   Status: ${p.uid !== 'TBD' && p.device !== 'TBD' ? '✅ Complete' : '⚠️ Incomplete'}\n\n`;
      });
      text += '='.repeat(50) + `\nTotal: ${allPlayers.length} | Complete: ${allPlayers.filter(p=>p.uid!=='TBD'&&p.device!=='TBD').length}\n`;
      const blob = new Blob([text], { type: 'text/plain' });
      const a = document.createElement('a');
      a.href = URL.createObjectURL(blob);
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

    // ====== SLOT CATEGORIZATION ======
    function getPlayersByCategory(squad, category) {
      return squad.filter(p => {
        if (!p.slot) return false;
        const slot = p.slot.toLowerCase();
        if (category === 'firstday_key')   return (slot.includes('first day') && p.isKey);
        if (category === 'firstday_normal') return (slot.includes('first day') && !p.isKey);
        if (category === 'star_key')       return (slot.includes('star') && p.isKey && !slot.includes('last'));
        if (category === 'star_normal')    return (slot.includes('star') && !p.isKey && !slot.includes('last') && !slot.includes('🔑'));
        if (category === 'lastday_vpn')    return (slot.includes('vpn') && p.isKey) || (slot.includes('last day') && p.isKey && !slot.includes('non'));
        if (category === 'lastday_nonvpn') return (slot.includes('non-vpn') || (slot.includes('last day') && !p.isKey));
        if (category === 'captain')        return slot.includes('captain');
        return false;
      });
    }

    // ====== NAV ======
    function showSection(section) {
      document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
      document.getElementById(section).classList.add('active');
      document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
      document.getElementById('nav-' + section).classList.add('active');
      if (section === 'viewer')   { loadViewerData(); loadViewerFixtures(); calculateStandings(); }
      if (section === 'admin')    loadAdminTeams();
      if (section === 'scorecard'){ loadScArchives(); loadTeamLogosForSc(); }
      if (section === 'captain')  loadTeamsForCaptainLogin();
    }

    function showAdminTab(tab) {
      for (let i = 1; i <= 5; i++) {
        document.getElementById('admin-tab' + i).style.display = tab === i ? 'block' : 'none';
        const btn = document.getElementById('atab-' + i);
        if (btn) btn.classList.toggle('active', tab === i);
      }
      if (tab === 2) { loadTeamsForFixtures(); loadAdminFixtures(); }
      if (tab === 3) loadAdminResults();
      if (tab === 4) { renderScConfigDisplay(); loadFixtureTimings(); }
      if (tab === 5) loadFixtureGeneratorData();
    }

    function showCaptainTab(tab) {
      for (let i = 1; i <= 4; i++) {
        document.getElementById('captain-tab' + i).style.display = tab === i ? 'block' : 'none';
        const btn = document.getElementById('ctab-' + i);
        if (btn) btn.classList.toggle('active', tab === i);
      }
      if (tab === 1) loadPlayersForEdit();
      if (tab === 2) { loadPlayersForEdit(); }
      if (tab === 3) { loadPlayersForEdit(); updateSquadStats(); initializeSquadForms(); }
    }

    function selectSquadFormat(format) {
      document.querySelectorAll('.squad-form').forEach(f => f.classList.remove('active'));
      document.getElementById('squad-' + format).classList.add('active');
      document.querySelectorAll('.format-pill').forEach(b => b.classList.remove('active'));
      document.getElementById('btn-' + format).classList.add('active');
    }

    // ====== UPLOAD HELPERS ======
    function uploadTeamLogo() {
      if (!storage) return showToast('Firebase loading…', 'error');
      const file = document.getElementById('teamLogoInput').files[0];
      if (!file) return showToast('Select logo first', 'error');
      const ref = storage.ref('team-logos/' + Date.now() + '_' + file.name);
      ref.put(file).then(s => s.ref.getDownloadURL()).then(url => {
        teamLogoUrl = url;
        document.getElementById('teamLogoUrlDisplay').innerHTML = `✅ URL Ready:<br><strong>${url}</strong>`;
        document.getElementById('teamLogoUrl').value = url;
        showToast('Logo uploaded!');
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
      const name     = document.getElementById('teamName').value.trim();
      const captain  = document.getElementById('captainName').value.trim();
      const password = document.getElementById('teamPassword').value.trim();
      if (!name || !captain || !password) return showToast('Fill all required fields', 'error');
      if (!teamLogoUrl) return showToast('Upload and set a team logo', 'error');

      const teamId = 'team_' + Date.now();
      db.ref('teams/' + teamId).set({ name, captain, password, logo: teamLogoUrl, createdAt: Date.now() })
        .then(() => {
          showToast('✅ Team registered!');
          ['teamName','captainName','teamPassword','teamLogoUrl'].forEach(id => document.getElementById(id).value = '');
          teamLogoUrl = '';
          document.getElementById('teamLogoPreview').style.display = 'none';
          document.getElementById('teamLogoUrlDisplay').textContent = 'URL will appear after upload…';
          loadAdminTeams();
        });
    }

    function loadAdminTeams() {
      if (!checkFirebase()) return;
      db.ref('teams').once('value', snapshot => {
        const teams = snapshot.val() || {};
        const entries = Object.entries(teams);
        if (entries.length === 0) {
          document.getElementById('admin-teams-list').innerHTML = '<div style="text-align:center;padding:2rem;color:#6b7280;">No teams registered yet.</div>';
          return;
        }
        let html = `<div style="overflow-x:auto;"><table>
          <thead><tr><th>Logo</th><th>Team</th><th>Captain</th><th>Password</th><th>Actions</th></tr></thead>
          <tbody>`;
        entries.forEach(([id, team]) => {
          html += `<tr>
            <td><img src="${team.logo}" style="width:44px;height:44px;border-radius:50%;object-fit:cover;border:2px solid var(--border);"></td>
            <td><strong>${team.name}</strong></td>
            <td style="color:var(--text-light);">${team.captain}</td>
            <td>
              <span class="pass-badge">${team.password || '—'}</span>
              <button class="btn-warning" style="padding:6px 10px;font-size:0.8rem;margin-left:6px;" onclick="adminResetPassword('${id}','${team.name}')">Reset</button>
            </td>
            <td>
              <button class="btn-danger" style="padding:8px 14px;" onclick="deleteTeam('${id}')">🗑️ Delete</button>
            </td>
          </tr>`;
        });
        html += '</tbody></table></div>';
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
      if (confirm('Delete this team? This cannot be undone.')) {
        db.ref('teams/' + teamId).remove().then(() => { showToast('Team deleted'); loadAdminTeams(); });
      }
    }

    function loadTeamsForFixtures() {
      if (!checkFirebase()) return;
      db.ref('teams').once('value', snapshot => {
        const teams = snapshot.val() || {};
        let options = '<option value="">Select Team</option>';
        Object.entries(teams).forEach(([id, team]) => { options += `<option value="${id}">${team.name}</option>`; });
        document.getElementById('fixtureTeam1').innerHTML = options;
        document.getElementById('fixtureTeam2').innerHTML = options;
      });
    }

    function createFixture() {
      if (!checkFirebase()) return;
      const t1     = document.getElementById('fixtureTeam1').value;
      const t2     = document.getElementById('fixtureTeam2').value;
      const date   = document.getElementById('fixtureDate').value;
      const format = document.getElementById('fixtureFormat').value;
      const venue  = document.getElementById('fixtureVenue').value.trim();
      // Time is REMOVED — date, format, and teams are required
      if (!t1 || !t2 || !date || !format) return showToast('Fill all required fields', 'error');
      if (t1 === t2) return showToast('Select different teams', 'error');

      db.ref('fixtures/fixture_' + Date.now()).set({ team1: t1, team2: t2, date, format, venue, createdAt: Date.now() })
        .then(() => {
          showToast('✅ Fixture created!');
          ['fixtureTeam1','fixtureTeam2','fixtureDate','fixtureFormat','fixtureVenue'].forEach(id => document.getElementById(id).value = '');
          loadAdminFixtures();
        });
    }

    function loadAdminFixtures() {
      if (!checkFirebase()) return;
      Promise.all([db.ref('fixtures').once('value'), db.ref('teams').once('value')]).then(([fs, ts]) => {
        const fixtures = fs.val() || {}, teams = ts.val() || {};
        const entries = Object.entries(fixtures);
        if (entries.length === 0) {
          document.getElementById('admin-fixtures-list').innerHTML = '<div style="text-align:center;padding:2rem;color:#6b7280;">No fixtures yet.</div>';
          return;
        }
        let html = '';
        entries.forEach(([id, f]) => {
          const t1 = teams[f.team1]?.name || 'Unknown';
          const t2 = teams[f.team2]?.name || 'Unknown';
          html += `<div class="fixture-item">
            <div>
              <div class="fixture-teams">${t1} vs ${t2}</div>
              <div class="fixture-date">📅 ${f.date} • ${f.format}</div>
              ${f.venue ? `<div class="fixture-date">📍 ${f.venue}</div>` : ''}
            </div>
            <button class="btn-danger" style="padding:8px 16px;" onclick="deleteFixture('${id}')">🗑️ Delete</button>
          </div>`;
        });
        document.getElementById('admin-fixtures-list').innerHTML = html;
      });
    }

    function deleteFixture(fixtureId) {
      if (!checkFirebase()) return;
      if (confirm('Delete this fixture?')) {
        db.ref('fixtures/' + fixtureId).remove().then(() => { showToast('Fixture deleted'); loadAdminFixtures(); });
      }
    }

    function saveScConfig() {
      if (!checkFirebase()) return;
      const name  = document.getElementById('scConfigName').value.trim();
      const stage = document.getElementById('scConfigStage').value.trim();
      const logo  = document.getElementById('scConfigLogoUrl').value.trim();
      if (!name || !stage) return showToast('Enter name and stage', 'error');
      const config = { name, stage, logo, updatedAt: Date.now() };
      db.ref('scConfig').set(config).then(() => {
        scConfig = config;
        showToast('Config saved!');
        renderScConfigDisplay();
        ['scConfigName','scConfigStage','scConfigLogoUrl'].forEach(id => document.getElementById(id).value = '');
      });
    }

    function loadScConfig() {
      if (!checkFirebase()) return;
      db.ref('scConfig').once('value', snap => { const d = snap.val(); if (d) scConfig = d; });
    }

    function renderScConfigDisplay() {
      const d = document.getElementById('scConfigDisplay');
      if (!d) return;
      d.innerHTML = `<div style="display:flex;align-items:center;gap:1rem;">
        ${scConfig.logo ? `<img src="${scConfig.logo}" style="width:50px;height:50px;border-radius:50%;object-fit:cover;">` : '<div style="width:50px;height:50px;border-radius:50%;background:var(--card);border:2px solid var(--border);display:flex;align-items:center;justify-content:center;font-size:1.5rem;">🏆</div>'}
        <div>
          <div style="font-weight:800;font-size:1.1rem;color:var(--text);">${scConfig.name}</div>
          <div style="color:var(--text-light);font-size:0.9rem;">${scConfig.stage}</div>
        </div>
      </div>`;
    }

    function renderTimingsDisplay() {
      const d = document.getElementById('timingsDisplay');
      if (!d) return;
      d.innerHTML = `<div>
        <div style="font-weight:800;font-size:1rem;color:var(--primary);margin-bottom:0.8rem;">⏰ Current Match Deadlines</div>
        <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:1rem;">
          <div style="background:linear-gradient(135deg,rgba(245,158,11,0.1),rgba(251,191,36,0.1));border-radius:8px;padding:1rem;border-left:4px solid #f59e0b;">
            <div style="color:#f59e0b;font-weight:700;font-size:0.85rem;">🚨 First Day</div>
            <div style="font-size:1.5rem;font-weight:800;color:var(--text);margin-top:0.5rem;">${fixtureTimings.firstDayTime || '00:00'}</div>
          </div>
          <div style="background:linear-gradient(135deg,rgba(251,191,36,0.1),rgba(253,230,138,0.1));border-radius:8px;padding:1rem;border-left:4px solid #fbbf24;">
            <div style="color:#fbbf24;font-weight:700;font-size:0.85rem;">⭐ Star</div>
            <div style="font-size:1.5rem;font-weight:800;color:var(--text);margin-top:0.5rem;">${fixtureTimings.starTime || '23:00'}</div>
          </div>
          <div style="background:linear-gradient(135deg,rgba(239,68,68,0.1),rgba(252,165,165,0.1));border-radius:8px;padding:1rem;border-left:4px solid #ef4444;">
            <div style="color:#ef4444;font-weight:700;font-size:0.85rem;">⛔ Last Day</div>
            <div style="font-size:1.5rem;font-weight:800;color:var(--text);margin-top:0.5rem;">${fixtureTimings.lastDayTime || '01:20'}</div>
          </div>
        </div>
      </div>`;
    }

    function saveFixtureTimings() {
      if (!checkFirebase()) return;
      const firstDay = document.getElementById('timingFirstDay').value;
      const star = document.getElementById('timingStar').value;
      const lastDay = document.getElementById('timingLastDay').value;
      
      if (!firstDay || !star || !lastDay) return showToast('Fill all timing fields', 'error');
      
      fixtureTimings = { firstDayTime: firstDay, starTime: star, lastDayTime: lastDay };
      
      db.ref('fixtureTimings').set(fixtureTimings).then(() => {
        showToast('✅ Timings saved!');
        renderTimingsDisplay();
      });
    }

    function loadFixtureTimings() {
      if (!checkFirebase()) return;
      db.ref('fixtureTimings').once('value', snap => {
        const data = snap.val();
        if (data) {
          fixtureTimings = data;
          document.getElementById('timingFirstDay').value = fixtureTimings.firstDayTime || '00:00';
          document.getElementById('timingStar').value = fixtureTimings.starTime || '23:00';
          document.getElementById('timingLastDay').value = fixtureTimings.lastDayTime || '01:20';
          renderTimingsDisplay();
        }
      });
    }

    // ====== FIXTURE GENERATOR ======
    function loadFixtureGeneratorData() {
      if (!checkFirebase()) return;
      const now = new Date();

      Promise.all([db.ref('fixtures').once('value'), db.ref('teams').once('value')]).then(([fs, ts]) => {
        const fixtures = fs.val() || {}, teams = ts.val() || {};
        let html = '';
        let visibleCount = 0;

        Object.entries(fixtures).forEach(([fixtureId, fixture]) => {
          // Fixtures disappear 24 hours after their match date
          const fixtureDay  = new Date(fixture.date);
          const expireTime  = new Date(fixtureDay);
          expireTime.setDate(expireTime.getDate() + 1); // +24h

          if (now > expireTime) return; // expired — skip

          const t1 = teams[fixture.team1], t2 = teams[fixture.team2];
          if (!t1 || !t2) return;

          visibleCount++;

          // How much time left before expiry
          const hoursLeft = Math.max(0, Math.round((expireTime - now) / 3600000));
          const expiryNote = hoursLeft < 6 ? `⚠️ Expires in ~${hoursLeft}h` : `🕒 Expires on ${expireTime.toLocaleDateString('en-GB',{day:'2-digit',month:'short'})}`;

          html += `<div class="fixture-generator-box">
            <div style="display:flex;justify-content:space-between;align-items:flex-start;flex-wrap:wrap;gap:0.5rem;margin-bottom:1rem;">
              <div>
                <div class="fixture-teams">${t1.name} vs ${t2.name}</div>
                <div class="fixture-date">📅 ${formatDateForDisplay(fixture.date)} • ${fixture.format}</div>
                ${fixture.venue ? `<div class="fixture-date">📍 ${fixture.venue}</div>` : ''}
              </div>
              <span style="font-size:0.8rem;background:${hoursLeft<6?'#fee2e2':'#d1fae5'};color:${hoursLeft<6?'#991b1b':'#065f46'};padding:5px 10px;border-radius:20px;font-weight:700;">${expiryNote}</span>
            </div>
            <div class="fixture-generator-btn-group">
              <button class="btn-primary" onclick="generateFixtureFormat('${fixtureId}','${fixture.team1}','${fixture.team2}','${fixture.date}','${fixture.format}')">🚀 Generate Fixture</button>
            </div>
            <div id="fg-output-${fixtureId}"></div>
          </div>`;
        });

        if (visibleCount === 0) {
          html = `<div style="text-align:center;padding:3rem;color:#6b7280;">
            <div style="font-size:3rem;margin-bottom:1rem;">📭</div>
            <div style="font-weight:700;margin-bottom:0.5rem;">No active fixtures</div>
            <div style="font-size:0.9rem;">Fixtures appear here until 24 hours after their match date.</div>
          </div>`;
        }

        document.getElementById('fg-fixtures-list').innerHTML = html;
      });
    }

    function generateFixtureFormat(fixtureId, team1Id, team2Id, fixtureDate, format) {
      if (!checkFirebase()) return;
      Promise.all([
        db.ref('teams/' + team1Id).once('value'),
        db.ref('teams/' + team2Id).once('value')
      ]).then(([t1Snap, t2Snap]) => {
        const team1 = t1Snap.val(), team2 = t2Snap.val();
        if (!team1 || !team2) return showToast('Error: Cannot fetch team data', 'error');

        Promise.all([
          db.ref('teams/' + team1Id + '/squads/' + format).once('value'),
          db.ref('teams/' + team2Id + '/squads/' + format).once('value')
        ]).then(([squad1Snap, squad2Snap]) => {
          const squad1Data = squad1Snap.val(), squad2Data = squad2Snap.val();
          if (!squad1Data || !squad2Data) return showToast('Squad submission not found for this format', 'error');

          const squad1 = squad1Data.squad, squad2 = squad2Data.squad;
          const fixtureText = buildFixtureText(team1.name, team2.name, squad1, squad2, fixtureDate, format);

          window['fgData_' + fixtureId] = { team1, team2, team1Id, team2Id, squad1, squad2, fixtureDate, format };

          document.getElementById('fg-output-' + fixtureId).innerHTML = `
            <div style="margin-top:1.5rem;">
              <div class="fixture-generator-output">${escapeHtml(fixtureText)}</div>
              <div style="display:flex;gap:1rem;flex-wrap:wrap;margin-top:1rem;">
                <button class="btn-success" onclick="copyFixtureText('${fixtureId}')">📋 Copy Text</button>
                <button class="btn-primary" onclick="downloadTeamInfoJPG('${fixtureId}', 1)">⬇ Home JPG</button>
                <button class="btn-primary" onclick="downloadTeamInfoJPG('${fixtureId}', 2)">⬇ Away JPG</button>
              </div>
            </div>`;

          window['fixture_text_' + fixtureId] = fixtureText;
          showToast('✅ Fixture generated!');
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

      const fdDate = new Date(date); fdDate.setDate(fdDate.getDate()+1); 
      const [fdH, fdM] = (fixtureTimings.firstDayTime || '00:00').split(':');
      fdDate.setHours(parseInt(fdH), parseInt(fdM), 0, 0);
      
      const starDate = new Date(date); starDate.setDate(starDate.getDate()+1); 
      const [stH, stM] = (fixtureTimings.starTime || '23:00').split(':');
      starDate.setHours(parseInt(stH), parseInt(stM), 0, 0);
      
      const ldDate = new Date(date); ldDate.setDate(ldDate.getDate()+2); 
      const [ldH, ldM] = (fixtureTimings.lastDayTime || '01:20').split(':');
      ldDate.setHours(parseInt(ldH), parseInt(ldM), 0, 0);

      const firstDayMatches = buildMatchups(t1_fdKey, t1_fdNorm, t2_fdKey, t2_fdNorm);
      const starMatches     = buildMatchups(t1_starKey, t1_starNorm, t2_starKey, t2_starNorm);
      const lastDayMatches  = buildLastDayMatchups(t1_ldVPN, t1_ldNon, t2_ldVPN, t2_ldNon);

      return `🔥 𝗧𝗵𝗿𝗶𝗹𝗹𝗲𝗿 𝗟𝗼𝗮𝗱𝗶𝗻𝗴…

🏆 𝗟𝗩𝗟 𝗜𝗡𝗧𝗥𝗔 𝗕𝗜𝗗 2026

𝗚𝗥𝗢𝗨𝗣 𝗦𝗧𝗔𝗚𝗘

${team1Name} ⚒️ ${team2Name}

🚨 𝗙𝗶𝗿𝘀𝘁 𝗗𝗮𝘆: ${formatDateTime(fdDate)}

${firstDayMatches}

⭐ 𝗦𝘁𝗮𝗿: ${formatDateTime(starDate)}

${starMatches}

⛔ 𝗟𝗮𝘀𝘁 𝗗𝗮𝘆: ${formatDateTime(ldDate)}

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
        if (p1 && p2) matches.push(`${p1.playerName} 🔑 🆚 ${p2.playerName}`);
      }
      const len2 = Math.max(nonVpn1.length, vpn2.length);
      for (let i = 0; i < len2; i++) {
        const p1 = nonVpn1[i], p2 = vpn2[i];
        if (p1 && p2) matches.push(`${p1.playerName} 🆚 🔑 ${p2.playerName}`);
      }
      return matches.join('\n') || '(No matches)';
    }

    function formatDateForDisplay(dateStr) {
      const months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
      const date = new Date(dateStr);
      const day = date.getDate();
      const month = months[date.getMonth()];
      return `${day} ${month}`;
    }

    function formatDateTime(date) {
      const months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
      const day = date.getDate();
      const month = months[date.getMonth()];
      
      let hours = date.getHours();
      const minutes = date.getMinutes().toString().padStart(2, '0');
      const ampm = hours >= 12 ? 'PM' : 'AM';
      hours = hours % 12;
      hours = hours ? hours : 12; // 0 should be 12
      const timeStr = `${hours}:${minutes} ${ampm}`;
      
      return `${day} ${month} | ${timeStr}`;
    }

    function escapeHtml(text) {
      const map = {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#039;'};
      return text.replace(/[&<>"']/g, m => map[m]);
    }

    function copyFixtureText(fixtureId) {
      const text = window['fixture_text_' + fixtureId];
      if (!text) return showToast('No text to copy', 'error');
      
      // Try modern clipboard API first
      if (navigator.clipboard && navigator.clipboard.writeText) {
        navigator.clipboard.writeText(text)
          .then(() => showToast('✅ Copied to clipboard!'))
          .catch(() => fallbackCopy(text));
      } else {
        fallbackCopy(text);
      }
    }
    
    function fallbackCopy(text) {
      const textArea = document.createElement('textarea');
      textArea.value = text;
      document.body.appendChild(textArea);
      textArea.select();
      try {
        document.execCommand('copy');
        showToast('✅ Copied to clipboard!');
      } catch (err) {
        showToast('❌ Copy failed', 'error');
      }
      document.body.removeChild(textArea);
    }

    // ====== TEAM INFO JPG ======
    async function downloadTeamInfoJPG(fixtureId, teamNum) {
      const data = window['fgData_' + fixtureId];
      if (!data) return showToast('Generate fixture first', 'error');
      const isHome = teamNum === 1;
      const myTeam = isHome ? data.team1 : data.team2;
      const teamId = isHome ? data.team1Id : data.team2Id;
      const opponent = isHome ? data.team2 : data.team1;
      const mySquad  = isHome ? data.squad1 : data.squad2;
      const side = isHome ? 'HOME' : 'AWAY';
      showToast(`Generating ${side} info card…`);
      
      // Fetch full team roster
      if (!checkFirebase()) return;
      db.ref('teams/' + teamId + '/players').once('value', snap => {
        const fullRoster = snap.val() || {};
        const zone = document.getElementById('team-info-render-zone');
        zone.innerHTML = buildTeamInfoCardHTML(myTeam.name, myTeam.logo, mySquad, opponent.name, side, data.fixtureDate, data.format, fullRoster);
        const el = zone.firstElementChild;
        if (!el) return showToast('Render error', 'error');
        document.fonts.ready.then(async () => {
          const imgs = el.querySelectorAll('img');
          await Promise.all(Array.from(imgs).map(img => new Promise(res => { if (img.complete) return res(); img.onload = res; img.onerror = res; })));
          await new Promise(r => setTimeout(r, 300));
          try {
            const canvas = await html2canvas(el, { scale: 2.5, useCORS: true, backgroundColor: '#0a0514', logging: false });
            const link = document.createElement('a');
            link.download = `${myTeam.name.replace(/\s+/g,'_')}_${side}_squad.jpg`;
            link.href = canvas.toDataURL('image/jpeg', 0.97);
            link.click();
            showToast(`✅ ${side} info downloaded!`);
          } catch(e) { showToast('Download failed: ' + e.message, 'error'); }
        });
      });
    }

    function buildTeamInfoCardHTML(teamName, teamLogo, squadData, opponentName, side, fixtureDate, format, fullRoster = {}) {
      const fdKey   = getPlayersByCategory(squadData, 'firstday_key');
      const fdNorm  = getPlayersByCategory(squadData, 'firstday_normal');
      const stKey   = getPlayersByCategory(squadData, 'star_key');
      const stNorm  = getPlayersByCategory(squadData, 'star_normal');
      const ldVPN   = getPlayersByCategory(squadData, 'lastday_vpn');
      const ldNon   = getPlayersByCategory(squadData, 'lastday_nonvpn');
      const captain = getPlayersByCategory(squadData, 'captain')[0];
      const formDate = fixtureDate ? new Date(fixtureDate).toLocaleDateString('en-GB',{day:'2-digit',month:'short',year:'numeric'}) : '';
      const sideColor = side === 'HOME' ? '#22c55e' : '#f97316';

      // Convert fullRoster to array of player objects
      const allTeamPlayers = Object.entries(fullRoster).map(([id, p]) => ({ 
        playerName: p.name || 'Unknown',
        uid: p.uid || 'N/A',
        device: p.device || 'N/A'
      }));

      const playerBoxWithInfo = (p, accent, bg) => {
        const uid = p.uid && p.uid !== 'TBD' && p.uid !== 'N/A' ? `<div style="font-size:10px;color:#7c7ca0;margin-top:2px;">🔑 ${p.uid}</div>` : '';
        const device = p.device && p.device !== 'TBD' && p.device !== 'N/A' ? `<div style="font-size:9px;color:#6b6b8d;">📱 ${p.device}</div>` : '';
        return `<div style="background:${bg};border:2px solid ${accent};border-radius:8px;padding:10px 12px;margin-bottom:8px;display:flex;flex-direction:column;align-items:flex-start;gap:2px;">
          <span style="color:${accent};font-family:'Orbitron',sans-serif;font-size:13px;font-weight:700;word-break:break-word;">${p.playerName}</span>
          ${uid}
          ${device}
        </div>`;
      };

      function hexToRgb(hex) {
        return `${parseInt(hex.slice(1,3),16)},${parseInt(hex.slice(3,5),16)},${parseInt(hex.slice(5,7),16)}`;
      }

      const keyBox  = (p, acc) => playerBoxWithInfo(p, acc, `rgba(${hexToRgb(acc)},0.15)`);
      const normBox = (p, acc) => playerBoxWithInfo(p, `${acc}99`, `rgba(${hexToRgb(acc)},0.07)`);

      // Build roster table HTML with compact styling
      const rosterTableHTML = allTeamPlayers.map((p, idx) => `
        <div style="display:flex;justify-content:space-between;align-items:center;padding:10px 12px;border-bottom:1px solid rgba(167,139,250,0.15);${idx%2===0?'background:rgba(139,92,246,0.05)':''}">
          <div style="flex:1;">
            <div style="color:#ffffff;font-weight:700;font-size:13px;margin-bottom:3px;font-family:'Orbitron',sans-serif;">${p.playerName}</div>
            <div style="display:flex;gap:16px;font-size:11px;">
              <div style="color:#a78bfa;"><span style="color:#c4b5fd;">🔑</span> ${p.uid}</div>
              <div style="color:#7c7ca0;"><span style="color:#a78bfa;">📱</span> ${p.device}</div>
            </div>
          </div>
        </div>
      `).join('');

      return `<div style="width:700px;background:linear-gradient(145deg,#0a0514 0%,#150a2e 40%,#0c0a1e 80%,#080415 100%);padding:28px;border-radius:20px;border:3px solid #8b5cf6;font-family:'Montserrat',sans-serif;box-sizing:border-box;box-shadow:0 0 60px rgba(139,92,246,0.4);">
        <div style="height:4px;background:linear-gradient(90deg,#8b5cf6,${sideColor},#8b5cf6);border-radius:4px;margin-bottom:20px;"></div>
        <div style="display:flex;align-items:center;gap:18px;margin-bottom:20px;padding-bottom:18px;border-bottom:1px solid rgba(139,92,246,0.35);">
          ${teamLogo ? `<img src="${teamLogo}" style="width:88px;height:88px;border-radius:50%;border:3px solid #a78bfa;object-fit:cover;box-shadow:0 0 25px rgba(139,92,246,0.7);">` : `<div style="width:88px;height:88px;border-radius:50%;border:3px solid #a78bfa;background:linear-gradient(135deg,#2d1b69,#1e1b4b);display:flex;align-items:center;justify-content:center;font-size:36px;">⚽</div>`}
          <div style="flex:1;min-width:0;">
            <div style="font-family:'Orbitron',sans-serif;font-size:26px;color:#ffffff;font-weight:800;letter-spacing:0.5px;margin-bottom:6px;">${teamName}</div>
            <div style="display:inline-block;background:${sideColor}22;border:2px solid ${sideColor};color:${sideColor};padding:4px 14px;border-radius:20px;font-size:12px;font-weight:800;letter-spacing:1.5px;margin-bottom:8px;font-family:'Orbitron',sans-serif;">${side}</div>
            <div style="color:#7c7ca0;font-size:13px;">vs <span style="color:#a78bfa;font-weight:700;">${opponentName}</span>${formDate ? ` • ${formDate}` : ''}${format ? ` • ${format}` : ''}</div>
          </div>
          <div style="text-align:center;flex-shrink:0;"><div style="font-size:28px;">🏆</div><div style="color:rgba(167,139,250,0.5);font-size:9px;font-weight:700;letter-spacing:2px;margin-top:4px;">LA VIOLA</div></div>
        </div>
        <div style="margin-bottom:16px;">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:14px;padding:0 4px;">
            <div style="width:4px;height:20px;background:linear-gradient(180deg,#8b5cf6,${sideColor});border-radius:2px;"></div>
            <span style="color:#a78bfa;font-size:14px;font-weight:800;letter-spacing:2px;font-family:'Orbitron',sans-serif;">📋 FULL ROSTER (${allTeamPlayers.length})</span>
          </div>
          <div style="border:2px solid rgba(139,92,246,0.3);border-radius:12px;overflow:hidden;background:rgba(0,0,0,0.3);">
            ${rosterTableHTML}
          </div>
        </div>
        <div style="border-top:1px solid rgba(139,92,246,0.25);padding-top:14px;margin-top:4px;display:flex;justify-content:space-between;align-items:center;">
          ${captain ? `<span style="color:#a78bfa;font-size:13px;font-weight:700;font-family:'Orbitron',sans-serif;">👑 <span style="color:#fbbf24;">${captain.playerName}</span></span>` : '<span></span>'}
          <span style="color:rgba(167,139,250,0.4);font-size:10px;letter-spacing:2px;font-weight:700;font-family:'Orbitron',sans-serif;">LA VIOLA FLEUR DE LIS</span>
        </div>
        <div style="height:2px;background:linear-gradient(90deg,transparent,#8b5cf6,transparent);border-radius:4px;margin-top:12px;"></div>
      </div>`;
    }

    // ====== CAPTAIN ======
    function loadTeamsForCaptainLogin() {
      if (!checkFirebase()) return;
      db.ref('teams').once('value', snap => {
        const teams = snap.val() || {};
        let opts = '<option value="">— Choose Team —</option>';
        Object.entries(teams).forEach(([id, t]) => { opts += `<option value="${id}">${t.name}</option>`; });
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
          document.getElementById('currentTeamName').textContent = '🎖️ ' + team.name;
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
          showToast('✅ Password changed!');
          ['currentPassInput','newPassInput','confirmPassInput'].forEach(id => document.getElementById(id).value = '');
        });
      });
    }

    function addPlayer() {
      if (!checkFirebase() || !currentTeamId) return showToast('Login first', 'error');
      const name = document.getElementById('playerName').value.trim();
      if (!name) return showToast('Player Name is required', 'error');
      const uid    = document.getElementById('playerUid').value.trim() || 'TBD';
      const device = document.getElementById('playerDevice').value.trim() || 'TBD';

      db.ref('teams/' + currentTeamId + '/players/player_' + Date.now()).set({ name, uid, device, addedAt: Date.now() })
        .then(() => {
          showToast('✅ Player added!');
          ['playerName','playerUid','playerDevice'].forEach(id => document.getElementById(id).value = '');
          loadPlayersForEdit();
        });
    }

    function loadPlayersForEdit() {
      if (!checkFirebase() || !currentTeamId) return;
      db.ref('teams/' + currentTeamId + '/players').once('value', snapshot => {
        const players = snapshot.val() || {};
        allPlayers = Object.entries(players).map(([id, p]) => ({ id, ...p }));
        updateSquadStats();

        // Update hidden table for JPG export
        let tbodyHtml = '';
        allPlayers.forEach(p => {
          const complete = p.uid !== 'TBD' && p.device !== 'TBD';
          tbodyHtml += `<tr>
            <td><strong>${p.name}</strong></td>
            <td>${p.uid}</td>
            <td>${p.device}</td>
            <td>${complete ? '✅ Complete' : '⚠️ Incomplete'}</td>
          </tr>`;
        });
        const tbody = document.querySelector('#playerTable tbody');
        if (tbody) tbody.innerHTML = tbodyHtml;

        // Render the quick-edit cards in Transfer tab
        const editContainer = document.getElementById('captain-edit-players-list');
        if (editContainer) {
          if (allPlayers.length === 0) {
            editContainer.innerHTML = '<div style="text-align:center;padding:2rem;color:#6b7280;">No players yet. Add players above.</div>';
          } else {
            editContainer.innerHTML = `<div class="roster-grid">` + allPlayers.map(p => {
              const complete = p.uid !== 'TBD' && p.device !== 'TBD';
              return `<div class="player-card ${complete?'complete':'incomplete'}" onclick="showEditPlayerModal('${p.id}')" style="cursor:pointer;">
                <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:0.8rem;">
                  <div style="font-size:1.8rem;">👤</div>
                  <div style="font-size:0.75rem;font-weight:700;padding:3px 9px;border-radius:20px;background:${complete?'#d1fae5':'#fef3c7'};color:${complete?'#065f46':'#92400e'};">${complete?'✅':'⚠️'}</div>
                </div>
                <div class="pcard-name">${p.name}</div>
                <div class="pcard-field"><span class="icon">📱</span><span class="${p.uid==='TBD'?'val-missing':'val-complete'}">${p.uid==='TBD'?'No UID':p.uid}</span></div>
                <div class="pcard-field"><span class="icon">📲</span><span class="${p.device==='TBD'?'val-missing':'val-complete'}">${p.device==='TBD'?'No Device':p.device}</span></div>
                <div class="pcard-actions">
                  <button class="btn-primary" onclick="event.stopPropagation();showEditPlayerModal('${p.id}')">✏️ Edit</button>
                </div>
              </div>`;
            }).join('') + `</div>`;
          }
        }

        // Re-render roster grid if on Squad Info tab
        renderRosterGrid();
      });
    }

    function initializeSquadForms() {
      loadPlayersForEdit();
      setTimeout(() => {
        Object.keys(squadConfigs).forEach(format => createSquadSlots(format));
        loadSavedSquads();
        selectSquadFormat('10v10');
      }, 300);
    }

    function createSquadSlots(format) {
      const container = document.getElementById('squad-' + format + '-slots');
      const config = squadConfigs[format];
      let prevCategory = '';
      let html = '';

      config.forEach((slot, index) => {
        if (slot.category !== prevCategory) {
          const catLabels = {firstday:'🚨 First Day', star:'⭐ Star', lastday:'⛔ Last Day', captain:'👑 Captain'};
          const catColors = {firstday:'#f59e0b', star:'#fbbf24', lastday:'#ef4444', captain:'#8b5cf6'};
          html += `<div style="background:${catColors[slot.category]}15;border-left:4px solid ${catColors[slot.category]};padding:10px 12px;border-radius:6px;margin:16px 0 8px;font-weight:800;color:${catColors[slot.category]};font-size:0.95rem;">${catLabels[slot.category]||slot.category}</div>`;
          prevCategory = slot.category;
        }
        const slotClass = slot.vpn === true ? 'player-slot vpn-player' : slot.vpn === false ? 'player-slot nonvpn-player' : slot.key ? 'player-slot key-player' : 'player-slot';
        const vpnLabel = slot.vpn === true ? ' 🔐 VPN' : slot.vpn === false ? ' 🌐 Non-VPN' : '';
        html += `<div class="${slotClass}" style="margin:8px 0;padding:12px;border-radius:10px;display:flex;align-items:center;gap:10px;flex-wrap:wrap;">
          <label style="min-width:180px;font-weight:700;color:var(--primary);font-size:0.9rem;">${slot.label}${vpnLabel}</label>
          <select id="${format}-slot-${index}" style="flex:1;min-width:180px;margin:0;">
            <option value="">👈 Select Player</option>
            ${allPlayers.map(p => `<option value="${p.id}">${p.name} ${p.uid==='TBD'||p.device==='TBD'?'⚠️':'✅'}</option>`).join('')}
          </select>
        </div>`;
      });
      container.innerHTML = html || '<p style="text-align:center;color:#6b7280;padding:1rem;">No players yet. Add from Transfer Window.</p>';
    }

    function saveSquad(format) {
      if (!currentTeamId) return showToast('Login first', 'error');
      const config = squadConfigs[format];
      const squad = [], missing = [];

      for (let i = 0; i < config.length; i++) {
        const playerId = document.getElementById(format + '-slot-' + i)?.value;
        if (!playerId) { missing.push(config[i].label); continue; }
        const player = allPlayers.find(p => p.id === playerId);
        if (!player) continue;
        squad.push({ slot: config[i].label, playerId, playerName: player.name, isKey: config[i].key, category: config[i].category, vpn: config[i].vpn });
      }

      if (missing.length > 0) {
        const statusDiv = document.getElementById('squad-' + format + '-status');
        statusDiv.innerHTML = `<div style="background:#fee2e2;border:2px solid #ef4444;color:#991b1b;padding:14px;border-radius:8px;font-weight:600;">❌ Missing ${missing.length} slot(s): ${missing.join(', ')}</div>`;
        statusDiv.style.display = 'block';
        showToast('Fill all slots first', 'error');
        return;
      }

      db.ref('teams/' + currentTeamId + '/squads/' + format).set({ squad, updatedAt: Date.now(), submittedAt: new Date().toISOString() })
        .then(() => {
          const statusDiv = document.getElementById('squad-' + format + '-status');
          statusDiv.innerHTML = `<div style="background:#dcfce7;border:2px solid #16a34a;color:#166534;padding:14px;border-radius:8px;font-weight:600;">✅ ${format} Squad Submitted!</div>`;
          statusDiv.style.display = 'block';
          showToast('✅ ' + format + ' squad submitted!');
          setTimeout(() => { statusDiv.style.display = 'none'; }, 5000);
        })
        .catch(e => showToast('Error: ' + e.message, 'error'));
    }

    function loadSavedSquads() {
      if (!currentTeamId) return;
      db.ref('teams/' + currentTeamId + '/squads').once('value', snapshot => {
        const squads = snapshot.val() || {};
        Object.entries(squads).forEach(([format, data]) => {
          data.squad.forEach((slot, index) => {
            const el = document.getElementById(format + '-slot-' + index);
            if (el) el.value = slot.playerId;
          });
        });
      });
    }

    // ====== VIEWER ======
    function loadViewerData() {
      if (!checkFirebase()) {
        document.getElementById('viewer-teams').innerHTML = '<div class="card" style="text-align:center;color:#6b7280;"><p>📡 Connecting to database...</p></div>';
        return;
      }
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
                <p style="color:var(--text-light);">👑 Captain: ${team.captain}</p>
                <p style="color:var(--text-light);">👥 Players: ${playerCount}</p>
              </div>
            </div>
          </div>`;
        });
        document.getElementById('viewer-teams').innerHTML = html || '<p style="text-align:center;color:#6b7280;">No teams yet</p>';
      });
    }

    function loadViewerFixtures() {
      if (!checkFirebase()) {
        document.getElementById('viewer-fixtures').innerHTML = '<div style="text-align:center;padding:2rem;color:#6b7280;">📡 Connecting to database...</div>';
        return;
      }
      Promise.all([db.ref('fixtures').once('value'), db.ref('teams').once('value')]).then(([fs, ts]) => {
        const fixtures = fs.val() || {}, teams = ts.val() || {};
        let html = '';
        
        const withResults = [];
        Object.entries(fixtures).forEach(([id, f]) => {
          const t1 = teams[f.team1], t2 = teams[f.team2];
          if (!t1 || !t2) return;
          if (f.result) {
            withResults.push({id, ...f, t1, t2});
          }
        });

        // Show most recent matches first
        withResults.sort((a, b) => new Date(b.date) - new Date(a.date));

        if (withResults.length === 0) {
          html = '<div style="text-align:center;padding:2rem;color:#6b7280;">No completed matches yet.</div>';
        } else {
          withResults.slice(0, 5).forEach(f => {
            const r = f.result;
            const winner = r.score1 > r.score2 ? f.t1.name : r.score2 > r.score1 ? f.t2.name : 'Draw';
            const winnerEmoji = r.score1 > r.score2 ? '🥇' : r.score2 > r.score1 ? '🥈' : '🤝';
            html += `<div class="fixture-item">
              <div style="display:flex;align-items:center;gap:1rem;flex:1;min-width:200px;">
                <img src="${f.t1.logo}" style="width:50px;height:50px;border-radius:50%;object-fit:cover;">
                <div>
                  <div class="fixture-teams">${f.t1.name} ${r.score1}</div>
                  <div class="fixture-date">📅 ${f.date} • ${f.format}</div>
                </div>
              </div>
              <div style="text-align:center;font-size:1.3rem;font-weight:800;color:var(--primary);">
                ${r.score1} - ${r.score2}
              </div>
              <div style="display:flex;align-items:center;gap:1rem;flex:1;justify-content:flex-end;min-width:200px;">
                <div style="text-align:right;">
                  <div class="fixture-teams">${r.score2} ${f.t2.name}</div>
                  <div class="fixture-date" style="text-align:right;">${winnerEmoji} ${winner}</div>
                </div>
                <img src="${f.t2.logo}" style="width:50px;height:50px;border-radius:50%;object-fit:cover;">
              </div>
            </div>`;
          });
        }
        document.getElementById('viewer-fixtures').innerHTML = html;
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

      if (!team1 || !team2) return showToast('❌ Cannot find team names. Use: Team1 ⚒️ Team2', 'error');

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

      if (matches.length === 0) return showToast('❌ No matches found.', 'error');
      if (!motmPlayer) return showToast('❌ No Man of the Match (👑) found', 'error');

      let t1pts = 0, t2pts = 0;
      matches.forEach(([, s1, s2]) => {
        if (parseInt(s1) > parseInt(s2)) t1pts += 3;
        else if (parseInt(s2) > parseInt(s1)) t2pts += 3;
        else { t1pts++; t2pts++; }
      });

      const stage = document.getElementById('scStageInput').value.trim() || scConfig.stage || 'Group Stage';
      const logo  = document.getElementById('scLogoInput').value.trim()  || scConfig.logo  || '';
      const tName = document.getElementById('scNameInput').value.trim()  || scConfig.name  || 'La Viola Cup';
      const t1logo = teamLogoMap[team1] || '';
      const t2logo = teamLogoMap[team2] || '';

      ['dark','light'].forEach(theme => {
        const t1Panel = document.getElementById('sc-' + theme + '-t1');
        const t2Panel = document.getElementById('sc-' + theme + '-t2');
        t1Panel.innerHTML = `${t1logo ? `<img src="${t1logo}" class="sc-team-logo">` : ''}${team1}`;
        t2Panel.innerHTML = `${t2logo ? `<img src="${t2logo}" class="sc-team-logo">` : ''}${team2}`;
        document.getElementById('sc-' + theme + '-s1').textContent = t1pts;
        document.getElementById('sc-' + theme + '-s2').textContent = t2pts;
        document.getElementById('sc-' + theme + '-name').textContent = tName;
        document.getElementById('sc-' + theme + '-stage').textContent = stage;
        const logoEl = document.getElementById('sc-' + theme + '-logo');
        if (logo) { logoEl.src = logo; logoEl.style.display = 'block'; } else logoEl.style.display = 'none';

        const matchesDiv = document.getElementById('sc-' + theme + '-matches');
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

        const winText = t1pts > t2pts ? team1 + ' wins! 🎉' : t2pts > t1pts ? team2 + ' wins! 🎉' : 'Draw! 🤝';
        document.getElementById('sc-' + theme + '-winner').textContent = 'Winner: ' + winText;
        document.getElementById('sc-' + theme + '-motm').textContent   = 'Man of the Match: 👑 ' + motmPlayer;
      });

      saveScArchive(team1, team2, t1pts, t2pts, matches, motmPlayer, stage, tName, logo);
      showToast('✅ Scorecards generated!');
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
      const countEl = document.getElementById('archive-count');
      if (countEl) countEl.innerHTML = scArchives.length + ' Saved';
      if (scArchives.length === 0) {
        list.innerHTML = '<p style="text-align:center;color:#6b7280;padding:2rem;">No archives yet.</p>';
        return;
      }
      list.innerHTML = scArchives.map((a, i) => {
        const timeStr = new Date(a.timestamp).toLocaleString('en-GB',{day:'2-digit',month:'short',year:'numeric',hour:'2-digit',minute:'2-digit'});
        const winner = a.t1pts > a.t2pts ? a.team1 : a.t2pts > a.t1pts ? a.team2 : 'Draw';
        return `<div style="background:white;border:2px solid var(--border);border-radius:12px;padding:1.2rem;margin:1rem 0;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:1rem;">
          <div style="flex:1;min-width:220px;">
            <div style="display:flex;align-items:center;gap:0.8rem;margin-bottom:0.5rem;">
              <div style="font-weight:800;color:var(--primary);">${a.team1} <span style="color:var(--text-light);font-weight:400;">vs</span> ${a.team2}</div>
              <div style="font-size:1.4rem;">${winner === 'Draw' ? '🤝' : '🏆'}</div>
            </div>
            <div style="font-size:0.85rem;color:var(--text-light);">📊 ${a.t1pts} - ${a.t2pts} • 🏆 ${a.tName} • ${a.stage}</div>
            <div style="font-size:0.85rem;color:var(--text-light);">👑 MOTM: <strong>${a.motm}</strong> • 📅 ${timeStr}</div>
          </div>
          <div style="display:flex;gap:0.5rem;">
            <button class="btn-primary" style="padding:8px 14px;font-size:0.85rem;" onclick="loadScArchiveItem(${i})">📂 Load</button>
            <button class="btn-danger"  style="padding:8px 14px;font-size:0.85rem;" onclick="deleteScArchive('${a.id}')">🗑️</button>
          </div>
        </div>`;
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
        await Promise.all(Array.from(imgs).map(img => new Promise(res => { if (img.complete) { res(); return; } img.onload = res; img.onerror = res; })));
        const canvas = await html2canvas(el, { backgroundColor: id === 'sc-dark' ? '#2c2c2c' : '#ffffff', scale: 2, useCORS: true });
        const link = document.createElement('a');
        link.download = 'scorecard_' + id + '_' + Date.now() + '.png';
        link.href = canvas.toDataURL('image/png');
        link.click();
        showToast('Downloaded!');
      } catch(e) { showToast('Download failed: ' + e.message, 'error'); }
    }

    // ====== POINT TABLE SYSTEM ======
    function calculateStandings() {
      if (!checkFirebase()) {
        const container = document.getElementById('points-table-view');
        if (container) container.innerHTML = '<div class="card" style="text-align:center;color:#6b7280;"><p>📡 Connecting to database...</p></div>';
        return;
      }
      db.ref('teams').once('value', teamsSnap => {
        const teams = teamsSnap.val() || {};
        standings = {};

        Object.entries(teams).forEach(([teamId, team]) => {
          standings[teamId] = {
            teamId, name: team.name, logo: team.logo,
            played: 0, wins: 0, draws: 0, losses: 0, gf: 0, ga: 0, points: 0
          };
        });

        db.ref('fixtures').once('value', fixturesSnap => {
          const fixtures = fixturesSnap.val() || {};
          Object.entries(fixtures).forEach(([id, fixture]) => {
            if (!fixture.result) return;
            const r = fixture.result;
            const t1 = standings[fixture.team1];
            const t2 = standings[fixture.team2];
            if (!t1 || !t2) return;

            t1.played++; t2.played++;
            t1.gf += r.score1; t1.ga += r.score2;
            t2.gf += r.score2; t2.ga += r.score1;

            if (r.score1 > r.score2) {
              t1.wins++; t1.points += 3;
              t2.losses++;
            } else if (r.score2 > r.score1) {
              t2.wins++; t2.points += 3;
              t1.losses++;
            } else {
              t1.draws++; t1.points++;
              t2.draws++; t2.points++;
            }
          });

          Object.values(standings).forEach(t => { t.gd = t.gf - t.ga; });
          const sorted = Object.values(standings).sort((a, b) => {
            if (b.points !== a.points) return b.points - a.points;
            if (b.gd !== a.gd) return b.gd - a.gd;
            return b.gf - a.gf;
          });

          renderPointsTable(sorted);
        });
      });
    }

    function renderPointsTable(sorted) {
      const container = document.getElementById('points-table-view');
      if (!container) return;

      let html = `<div class="pt-container">
        <div class="pt-header">🏆 LA VIOLA FLEUR DE LIS TOURNAMENT STANDINGS 🏆</div>
        <table class="pt-table">
          <thead class="pt-thead">
            <tr>
              <th style="width:60px;">Rank</th>
              <th style="flex:1;">Team</th>
              <th class="pt-stats">M</th>
              <th class="pt-stats">W</th>
              <th class="pt-stats">D</th>
              <th class="pt-stats">L</th>
              <th class="pt-stats">GF</th>
              <th class="pt-stats">GA</th>
              <th class="pt-stats">GD</th>
              <th style="width:70px;" class="pt-points">Pts</th>
            </tr>
          </thead>
          <tbody class="pt-tbody">`;

      sorted.forEach((team, idx) => {
        const rank = idx + 1;
        const medal = rank === 1 ? '🥇' : rank === 2 ? '🥈' : rank === 3 ? '🥉' : '';
        html += `<tr>
          <td class="pt-rank">${rank}</td>
          <td>
            <div class="pt-team">
              <img src="${team.logo}" class="pt-team-logo">
              <span>${team.name}</span>
            </div>
          </td>
          <td class="pt-stats">${team.played}</td>
          <td class="pt-stats">${team.wins}</td>
          <td class="pt-stats">${team.draws}</td>
          <td class="pt-stats">${team.losses}</td>
          <td class="pt-stats">${team.gf}</td>
          <td class="pt-stats">${team.ga}</td>
          <td class="pt-stats">${team.gd > 0 ? '+' : ''}${team.gd}</td>
          <td class="pt-points">${team.points}</td>
        </tr>`;
      });

      html += `</tbody></table></div>`;
      container.innerHTML = html;
    }

    async function downloadPointTableJPG() {
      if (Object.keys(standings).length === 0) return showToast('No standings data', 'error');
      const sorted = Object.values(standings).sort((a, b) => {
        if (b.points !== a.points) return b.points - a.points;
        if (b.gd !== a.gd) return b.gd - a.gd;
        return b.gf - a.gf;
      });

      showToast('Generating point table…');
      const zone = document.getElementById('pt-render-zone');
      zone.innerHTML = buildPointTableHTML(sorted);
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
        const canvas = await html2canvas(el, { scale: 2.5, useCORS: true, backgroundColor: '#0f172a', logging: false });
        const link = document.createElement('a');
        link.download = `LA_VIOLA_STANDINGS_${new Date().toISOString().split('T')[0]}.jpg`;
        link.href = canvas.toDataURL('image/jpeg', 0.95);
        link.click();
        showToast('✅ Point table downloaded!');
      } catch(e) {
        showToast('Download failed: ' + e.message, 'error');
      }
    }

    function buildPointTableHTML(sorted) {
      const now = new Date();
      const dateStr = now.toLocaleDateString('en-GB', { day: '2-digit', month: 'short', year: 'numeric' });

      let tableHTML = `<tr>
        <th>Rank</th>
        <th>Team</th>
        <th>M</th>
        <th>W</th>
        <th>D</th>
        <th>L</th>
        <th>GF</th>
        <th>GA</th>
        <th>GD</th>
        <th>Points</th>
      </tr>`;

      sorted.forEach((team, idx) => {
        const rank = idx + 1;
        const medalClass = rank === 1 ? 'gold' : rank === 2 ? 'silver' : rank === 3 ? 'bronze' : '';
        const medal = rank === 1 ? '🥇' : rank === 2 ? '🥈' : rank === 3 ? '🥉' : '';
        const medalHTML = medalClass ? `<div class="pt-render-medal ${medalClass}">${medal}</div>` : `<div class="pt-render-rank">${rank}</div>`;

        tableHTML += `<tr>
          <td>${medalHTML}</td>
          <td>
            <div class="pt-render-team">
              <img src="${team.logo}" class="pt-render-logo">
              <span>${team.name}</span>
            </div>
          </td>
          <td class="pt-render-stat">${team.played}</td>
          <td class="pt-render-stat">${team.wins}</td>
          <td class="pt-render-stat">${team.draws}</td>
          <td class="pt-render-stat">${team.losses}</td>
          <td class="pt-render-stat">${team.gf}</td>
          <td class="pt-render-stat">${team.ga}</td>
          <td class="pt-render-stat">${team.gd > 0 ? '+' : ''}${team.gd}</td>
          <td class="pt-render-points">${team.points}</td>
        </tr>`;
      });

      return `<div class="pt-render-wrapper">
        <div class="pt-render-title">🏆 LA VIOLA FLEUR DE LIS 🏆</div>
        <div class="pt-render-subtitle">INTRA BID TOURNAMENT STANDINGS</div>
        <table class="pt-render-table">
          ${tableHTML}
        </table>
        <div class="pt-render-footer">
          📊 STANDINGS AS OF ${dateStr} | FOLLOW US 🔥
        </div>
      </div>`;
    }

    // ====== ADMIN RESULTS SUBMISSION ======
    function loadAdminResults() {
      if (!checkFirebase()) return;
      Promise.all([db.ref('fixtures').once('value'), db.ref('teams').once('value')]).then(([fs, ts]) => {
        const fixtures = fs.val() || {}, teams = ts.val() || {};
        let html = '';

        Object.entries(fixtures).forEach(([fixtureId, f]) => {
          const t1 = teams[f.team1], t2 = teams[f.team2];
          if (!t1 || !t2) return;

          const result = f.result;
          const hasResult = !!result;
          const status = hasResult ? '✅ Completed' : '⏳ Pending';
          const statusColor = hasResult ? '#10b981' : '#f59e0b';

          html += `<div class="score-submit-box">
            <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:1rem;flex-wrap:wrap;gap:1rem;">
              <div>
                <div style="display:flex;align-items:center;gap:10px;margin-bottom:0.5rem;">
                  <img src="${t1.logo}" style="width:40px;height:40px;border-radius:50%;object-fit:cover;">
                  <span style="font-weight:800;font-size:1rem;">${t1.name}</span>
                </div>
                <div style="font-size:0.9rem;color:var(--text-light);">📅 ${f.date} • ${f.format}</div>
              </div>
              <span style="background:${statusColor}20;color:${statusColor};padding:6px 12px;border-radius:20px;font-weight:700;font-size:0.85rem;">
                ${hasResult ? '✅' : '⏳'} ${status}
              </span>
            </div>

            <div class="match-vs">
              <div class="team">
                <img src="${t1.logo}" class="team-logo">
                <div class="team-name">${t1.name}</div>
              </div>
              <div class="vs-separator">⚽</div>
              <div class="team">
                <img src="${t2.logo}" class="team-logo">
                <div class="team-name">${t2.name}</div>
              </div>
            </div>

            <div class="score-input-group" style="justify-content:center;gap:1.5rem;">
              <input type="number" id="score1-${fixtureId}" min="0" placeholder="0" value="${result ? result.score1 : ''}" style="width:80px;font-size:1.5rem;">
              <span style="font-weight:900;font-size:1.5rem;color:#ef4444;">-</span>
              <input type="number" id="score2-${fixtureId}" min="0" placeholder="0" value="${result ? result.score2 : ''}" style="width:80px;font-size:1.5rem;">
            </div>

            <div style="display:flex;gap:1rem;flex-wrap:wrap;margin-top:1.2rem;">
              <button class="btn-success" style="flex:1;padding:12px;" onclick="submitFixtureResult('${fixtureId}')">
                ${hasResult ? '📝 Update' : '✅ Submit'} Result
              </button>
              ${hasResult ? `<button class="btn-danger" style="flex:1;padding:12px;" onclick="clearFixtureResult('${fixtureId}')">🗑️ Clear</button>` : ''}
            </div>
          </div>`;
        });

        if (Object.keys(fixtures).length === 0) {
          html = '<div style="text-align:center;padding:3rem;color:#6b7280;"><div style="font-size:2rem;margin-bottom:1rem;">📭</div>No fixtures created yet.</div>';
        }

        document.getElementById('admin-results-list').innerHTML = html;
      });
    }

    function submitFixtureResult(fixtureId) {
      if (!checkFirebase()) return;
      const s1 = parseInt(document.getElementById('score1-' + fixtureId).value);
      const s2 = parseInt(document.getElementById('score2-' + fixtureId).value);

      if (isNaN(s1) || isNaN(s2)) return showToast('Enter valid scores', 'error');

      db.ref('fixtures/' + fixtureId + '/result').set({ score1: s1, score2: s2, submittedAt: new Date().toISOString() })
        .then(() => {
          showToast('✅ Result submitted! Point table updated.');
          calculateStandings();
          loadAdminResults();
        });
    }

    function clearFixtureResult(fixtureId) {
      if (!checkFirebase()) return;
      if (!confirm('Clear this result?')) return;
      db.ref('fixtures/' + fixtureId + '/result').remove()
        .then(() => {
          showToast('Result cleared');
          calculateStandings();
          loadAdminResults();
        });
    }

    // ====== FIREBASE INITIALIZATION (After all functions are defined) ======
    window.addEventListener('load', function () {
      console.log('Page load event fired');
      
      // Show content even if Firebase fails
      showSection('viewer');
      
      if (typeof firebase === 'undefined') {
        console.log('Firebase not loaded, showing offline mode');
        showToast('⚠️ Firebase not connected. Offline mode.', 'error');
        document.body.style.opacity = '1';
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
        console.log('Firebase initialized successfully');
        loadScConfig();
        loadScArchives();
        loadFixtureTimings();
        document.body.style.opacity = '1';
      } catch (e) {
        console.error('Firebase error:', e);
        showToast('Firebase connection error', 'error');
        document.body.style.opacity = '1';
      }
    });

    // Fallback: Show page after 3 seconds if load event doesn't trigger
    setTimeout(() => {
      console.log('Fallback timeout triggered');
      if (document.body.style.opacity !== '1') {
        document.body.style.opacity = '1';
        showSection('viewer');
      }
    }, 3000);
  </script>
</body>
</html>
