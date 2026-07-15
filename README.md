<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=yes">
  <title>全职颂歌 · 天赋抽取</title>
  <style>
    :root {
      --bg: #faf6f0;
      --bg-warm: #f5efe4;
      --card-bg: #ffffff;
      --card-shadow: rgba(80, 60, 30, 0.08);
      --gold: #c8960c;
      --gold-light: #e8b830;
      --gold-pale: #fdf3d0;
      --text: #3e3224;
      --text-secondary: #6b5e4e;
      --text-muted: #9b8e7c;
      --border: rgba(180, 150, 110, 0.35);
      --border-light: rgba(180, 150, 110, 0.2);
      --accent-blue: #4a80d4;
      --accent-red: #d4505e;
      --accent-purple: #8b5cb8;
      --accent-green: #3da86d;
      --accent-amber: #d4882a;
      --input-bg: #fdfaf5;
      --input-border: #d5c9b3;
      --input-focus: #c8960c;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      background: var(--bg);
      background-image:
        radial-gradient(ellipse at 50% 0%, rgba(220, 190, 130, 0.12) 0%, transparent 60%),
        radial-gradient(ellipse at 20% 80%, rgba(200, 160, 100, 0.06) 0%, transparent 50%),
        radial-gradient(ellipse at 80% 70%, rgba(210, 175, 120, 0.05) 0%, transparent 50%);
      font-family: 'PingFang SC', 'Microsoft YaHei', 'Noto Sans SC', system-ui, sans-serif;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow-x: hidden;
      position: relative;
      -webkit-tap-highlight-color: transparent;
      user-select: none;
      -webkit-user-select: none;
    }

    #particles-canvas {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      pointer-events: none;
      z-index: 0;
    }

    .main-container {
      position: relative;
      z-index: 1;
      width: 100%;
      max-width: 650px;
      padding: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 20px;
    }

    .header {
      text-align: center;
    }
    .header .icon-row {
      font-size: 42px;
      margin-bottom: 4px;
      animation: floatIcon 3s ease-in-out infinite;
      display: inline-block;
      filter: drop-shadow(0 2px 6px rgba(180, 140, 60, 0.3));
    }
    @keyframes floatIcon {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-8px); }
    }
    .header h1 {
      font-size: 2.2em;
      font-weight: 700;
      letter-spacing: 0.06em;
      background: linear-gradient(180deg, #c8960c 0%, #a07608 50%, #7a5606 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      filter: drop-shadow(0 1px 3px rgba(160, 120, 30, 0.3));
      margin: 0;
      line-height: 1.3;
    }
    .header .subtitle {
      font-size: 0.9em;
      color: var(--text-muted);
      letter-spacing: 0.1em;
      margin-top: 2px;
    }

    .draw-card {
      background: var(--card-bg);
      border: 1px solid var(--border-light);
      border-radius: 20px;
      padding: 28px 28px 24px;
      width: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 18px;
      box-shadow: 0 4px 24px var(--card-shadow), 0 1px 3px rgba(0,0,0,0.04), 0 0 0 1px rgba(255,255,255,0.8) inset;
      transition: all 0.4s ease;
      position: relative;
      overflow: visible;
    }
    .draw-card::before {
      content: '';
      position: absolute;
      top: -1px;
      left: 20%;
      right: 20%;
      height: 1px;
      background: linear-gradient(90deg, transparent, rgba(200,150,50,0.4), transparent);
      border-radius: 50%;
    }
    .draw-card.shaking {
      animation: cardShake 0.6s ease-in-out;
    }
    @keyframes cardShake {
      0%,100%{transform:translateX(0);}
      10%{transform:translateX(-5px) rotate(-0.4deg);}
      20%{transform:translateX(5px) rotate(0.4deg);}
      30%{transform:translateX(-4px) rotate(-0.3deg);}
      40%{transform:translateX(4px) rotate(0.3deg);}
      50%{transform:translateX(-2px) rotate(-0.15deg);}
      60%{transform:translateX(2px) rotate(0.15deg);}
      70%{transform:translateX(-1px);}
      80%{transform:translateX(1px);}
      90%{transform:translateX(-0.5px);}
    }

    .name-input-wrap {
      width: 100%;
      max-width: 280px;
      position: relative;
    }
    .name-input-wrap .name-icon {
      position: absolute;
      left: 14px;
      top: 50%;
      transform: translateY(-50%);
      font-size: 1.1em;
      pointer-events: none;
      z-index: 2;
    }
    .name-input-wrap input {
      width: 100%;
      padding: 12px 16px 12px 42px;
      font-size: 1em;
      font-family: inherit;
      letter-spacing: 0.04em;
      color: var(--text);
      background: var(--input-bg);
      border: 2px solid var(--input-border);
      border-radius: 30px;
      outline: none;
      transition: all 0.3s ease;
      text-align: center;
    }
    .name-input-wrap input::placeholder {
      color: #c4b8a2;
      letter-spacing: 0.04em;
    }
    .name-input-wrap input:focus {
      border-color: var(--input-focus);
      box-shadow: 0 0 0 4px rgba(200,150,12,0.08);
      background: #fffefb;
    }

    .draw-cylinder {
      position: relative;
      width: 85px;
      height: 105px;
      cursor: pointer;
      transition: transform 0.2s ease;
    }
    .draw-cylinder:hover { transform: scale(1.05); }
    .draw-cylinder:active { transform: scale(0.95); }
    .draw-cylinder .cylinder-body {
      width: 74px;
      height: 94px;
      background: linear-gradient(180deg, #7a5230 0%, #5c3618 30%, #6b4225 60%, #3e1e08 100%);
      border-radius: 14px 14px 18px 18px;
      margin: 0 auto;
      position: relative;
      border: 2px solid #8b6040;
      box-shadow: 0 4px 20px rgba(60,25,5,0.35), inset 0 2px 3px rgba(255,220,160,0.08), 0 0 16px rgba(160,110,40,0.15);
      overflow: hidden;
    }
    .draw-cylinder .cylinder-body::after {
      content: '';
      position: absolute;
      top: 6px;
      left: 50%;
      transform: translateX(-50%);
      width: 44px;
      height: 3px;
      background: rgba(240,200,140,0.45);
      border-radius: 50%;
      box-shadow: 0 0 10px rgba(240,200,130,0.4);
    }
    .draw-cylinder .sticks {
      position: absolute;
      top: -6px;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      gap: 5px;
      z-index: 2;
      pointer-events: none;
    }
    .draw-cylinder .stick {
      width: 5px;
      height: 50px;
      background: linear-gradient(180deg, #faf3e2 0%, #ede0c0 50%, #d4b880 100%);
      border-radius: 3px 3px 2px 2px;
      box-shadow: 0 1px 3px rgba(0,0,0,0.25);
      transition: transform 0.3s ease;
    }
    .stick:nth-child(1) { transform: rotate(-7deg); }
    .stick:nth-child(2) { transform: rotate(-3deg); }
    .stick:nth-child(3) { transform: rotate(2deg); }
    .stick:nth-child(4) { transform: rotate(6deg); }
    .stick.highlight {
      animation: stickPop 0.5s ease-out forwards;
    }
    @keyframes stickPop {
      0% { transform: translateY(0) rotate(0deg); }
      40% { transform: translateY(-28px) rotate(-3deg); }
      70% { transform: translateY(-42px) rotate(1deg); }
      100% { transform: translateY(-35px) rotate(0deg); opacity: 0.65; }
    }

    .rolling-display {
      min-height: 32px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.95em;
      color: var(--text-secondary);
      letter-spacing: 0.04em;
      text-align: center;
      padding: 0 10px;
      font-weight: 500;
    }
    .rolling-display.active {
      color: #b8860c;
      font-weight: 700;
      animation: textFlicker 0.08s infinite;
    }
    @keyframes textFlicker {
      0%,100%{opacity:1;}
      50%{opacity:0.65;}
    }
    .rolling-display.stopped {
      color: #8b6508;
      font-weight: 700;
      animation: none;
    }

    .btn-draw {
      display: inline-flex;
      align-items: center;
      gap: 10px;
      padding: 14px 40px;
      font-size: 1.15em;
      font-weight: 700;
      letter-spacing: 0.06em;
      color: #fffef9;
      background: linear-gradient(180deg, #e8b830 0%, #c8960c 40%, #a07808 100%);
      border: none;
      border-radius: 50px;
      cursor: pointer;
      box-shadow: 0 6px 24px rgba(180,130,30,0.35), 0 0 0 3px rgba(200,150,40,0.2), inset 0 1px 0 rgba(255,255,255,0.3);
      transition: all 0.25s ease;
      outline: none;
      -webkit-tap-highlight-color: transparent;
    }
    .btn-draw:hover {
      box-shadow: 0 8px 30px rgba(190,140,35,0.5), 0 0 0 5px rgba(200,150,40,0.3), inset 0 1px 0 rgba(255,255,255,0.35);
      transform: translateY(-2px);
    }
    .btn-draw:active {
      transform: scale(0.95);
      box-shadow: 0 3px 10px rgba(180,130,30,0.35), 0 0 0 2px rgba(200,150,40,0.25);
      transition: all 0.1s ease;
    }
    .btn-draw:disabled {
      opacity: 0.6;
      cursor: not-allowed;
      pointer-events: none;
      filter: brightness(0.85);
    }
    .btn-draw .btn-icon {
      font-size: 1.25em;
      transition: transform 0.3s ease;
    }
    .btn-draw:hover .btn-icon { transform: rotate(-15deg); }

    .result-card {
      width: 100%;
      background: #fffefb;
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 24px 22px 20px;
      display: none;
      flex-direction: column;
      gap: 14px;
      box-shadow: 0 4px 28px rgba(80,60,30,0.1), 0 1px 2px rgba(0,0,0,0.03);
      animation: resultReveal 0.5s ease-out;
      position: relative;
    }
    .result-card.visible { display: flex; }
    .result-card::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      height: 2px;
      background: linear-gradient(90deg, transparent, rgba(200,150,40,0.5), transparent);
    }
    @keyframes resultReveal {
      0% { opacity: 0; transform: translateY(18px) scale(0.96); }
      60% { opacity: 1; transform: translateY(-2px) scale(1.01); }
      100% { opacity: 1; transform: translateY(0) scale(1); }
    }
    .result-title {
      text-align: center;
      font-size: 1.05em;
      font-weight: 700;
      color: #8b6508;
    }
    .result-title .name-highlight { color: #b8860c; font-weight: 800; }
    .attr-list { display: flex; flex-direction: column; gap: 10px; }
    .attr-item { display: flex; align-items: center; gap: 10px; flex-wrap: wrap; }
    .attr-label {
      min-width: 68px;
      font-size: 0.88em;
      color: var(--text-secondary);
      text-align: right;
      flex-shrink: 0;
      font-weight: 500;
    }
    .attr-bar-wrap {
      flex: 1;
      min-width: 120px;
      height: 16px;
      background: #f0ebe0;
      border-radius: 10px;
      overflow: hidden;
    }
    .attr-bar {
      height: 100%;
      border-radius: 10px;
      transition: width 0.8s cubic-bezier(0.22, 0.61, 0.36, 1);
      width: 0;
      box-shadow: 0 0 10px currentColor;
    }
    .attr-val { font-weight: 700; font-size: 0.95em; min-width: 20px; text-align: center; }
    .attr-bar.blue { background: linear-gradient(90deg, #3b6db5, #5b8def); color: #4a80d4; }
    .attr-bar.red { background: linear-gradient(90deg, #b83a48, #e85d75); color: #d4505e; }
    .attr-bar.purple { background: linear-gradient(90deg, #6d3a8c, #a070d0); color: #8b5cb8; }
    .attr-bar.green { background: linear-gradient(90deg, #2d8c55, #4ecb8c); color: #3da86d; }
    .attr-bar.amber { background: linear-gradient(90deg, #b8701a, #f0a050); color: #d4882a; }
    .attr-val.blue { color: #4a80d4; } .attr-val.red { color: #d4505e; }
    .attr-val.purple { color: #8b5cb8; } .attr-val.green { color: #3da86d; }
    .attr-val.amber { color: #d4882a; }

    .total-display {
      text-align: center;
      margin-top: 2px;
      padding-top: 10px;
      border-top: 1px solid rgba(180,150,100,0.25);
      font-size: 1em;
      color: #6b5e3e;
      font-weight: 600;
    }
    .total-display .total-num {
      font-size: 1.5em;
      background: linear-gradient(180deg, #d4a020, #b8860c);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      margin: 0 4px;
      font-weight: 900;
    }

    /* 历史记录表格区域 */
    .history-section {
      width: 100%;
      background: #fffefb;
      border: 1px solid var(--border-light);
      border-radius: 14px;
      padding: 18px 16px;
      box-shadow: 0 2px 16px rgba(80,60,30,0.05);
      display: none;
      flex-direction: column;
      gap: 12px;
    }
    .history-section.visible { display: flex; }
    .history-title {
      font-size: 0.9em;
      color: var(--text-muted);
      letter-spacing: 0.05em;
      text-align: center;
      font-weight: 600;
    }
    .table-wrapper {
      overflow-x: auto;
      -webkit-overflow-scrolling: touch;
      border-radius: 10px;
      border: 1px solid #efe6d8;
    }
    .history-table {
      width: 100%;
      border-collapse: collapse;
      font-size: 0.88em;
      min-width: 500px;
      white-space: nowrap;
    }
    .history-table th {
      background: #faf6ee;
      color: #5e4e34;
      font-weight: 700;
      padding: 10px 12px;
      text-align: center;
      letter-spacing: 0.04em;
      border-bottom: 2px solid #e3d5be;
    }
    .history-table td {
      padding: 10px 12px;
      text-align: center;
      border-bottom: 1px solid #efe6d8;
      color: #4a3c2e;
    }
    .history-table tbody tr {
      transition: background 0.15s ease;
      cursor: pointer;
    }
    .history-table tbody tr:hover {
      background: #fef9f0;
    }
    .history-table .name-cell {
      font-weight: 700;
      color: #7a5e20;
    }
    .history-table .total-cell {
      font-weight: 700;
      color: #b8860c;
      font-size: 1.05em;
    }
    .history-table .action-btn {
      background: none;
      border: 1px solid #d5c09e;
      background: #fdfaf3;
      color: #6b5e3e;
      padding: 5px 14px;
      border-radius: 20px;
      font-size: 0.8em;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.2s;
      letter-spacing: 0.03em;
      white-space: nowrap;
    }
    .history-table .action-btn:hover {
      background: #fef7e8;
      border-color: #c8960c;
      color: #8b6508;
    }

    .footer {
      text-align: center;
      font-size: 0.72em;
      color: #b5a890;
      letter-spacing: 0.04em;
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 8px 16px;
    }
    .footer a { color: #a09070; text-decoration: none; }
    .footer a:hover { color: #c8960c; }

    .deploy-note {
      background: #fef9f0;
      border: 1px solid #e8dcc8;
      border-radius: 12px;
      padding: 14px 18px;
      font-size: 0.82em;
      color: #6b5e3e;
      text-align: left;
      line-height: 1.6;
    }
    .deploy-note strong { color: #8b6508; }

    @media (max-width: 480px) {
      .header h1 { font-size: 1.5em; }
      .draw-card { padding: 20px 14px 18px; border-radius: 16px; gap: 14px; }
      .btn-draw { padding: 12px 28px; font-size: 1em; }
      .result-card { padding: 16px 12px 14px; gap: 10px; }
      .attr-label { min-width: 54px; font-size: 0.76em; }
      .attr-bar-wrap { height: 13px; min-width: 70px; }
      .attr-val { font-size: 0.82em; min-width: 16px; }
      .draw-cylinder { width: 62px; height: 82px; }
      .cylinder-body { width: 54px; height: 72px; border-radius: 10px 10px 14px 14px; }
      .stick { width: 4px; height: 36px; }
      .name-input-wrap { max-width: 230px; }
      .name-input-wrap input { padding: 10px 14px 10px 36px; font-size: 0.9em; }
      .name-input-wrap .name-icon { left: 10px; font-size: 1em; }
    }
  </style>
</head>
<body>
  <canvas id="particles-canvas"></canvas>

  <div class="main-container">
    <div class="header">
      <div class="icon-row">⚔️</div>
      <h1>全职颂歌</h1>
      <div class="subtitle">天 赋 抽 取</div>
    </div>

    <div class="draw-card" id="drawCard">
      <div class="name-input-wrap">
        <span class="name-icon">👤</span>
        <input type="text" id="nameInput" placeholder="请输入您的姓名" maxlength="20" autocomplete="off">
      </div>

      <div class="draw-cylinder" id="cylinder" title="点击签筒抽签">
        <div class="sticks" id="sticksContainer">
          <div class="stick"></div>
          <div class="stick"></div>
          <div class="stick"></div>
          <div class="stick"></div>
        </div>
        <div class="cylinder-body"></div>
      </div>

      <div class="rolling-display" id="rollingDisplay">输入姓名后点击按钮或签筒抽取天赋</div>

      <button class="btn-draw" id="btnDraw">
        <span class="btn-icon">🎯</span> 抽取天赋
      </button>
    </div>

    <div class="result-card" id="resultCard">
      <div class="result-title" id="resultTitle">✨的天赋是✨</div>
      <div class="attr-list" id="attrList"></div>
      <div class="total-display" id="totalDisplay"></div>
    </div>

    <div class="history-section" id="historySection">
      <div class="history-title">📜 抽取记录 · 含时间</div>
      <div class="table-wrapper">
        <table class="history-table" id="historyTable">
          <thead>
            <tr>
              <th>序号</th>
              <th>姓名</th>
              <th>天赋总和</th>
              <th>抽取时间</th>
              <th>操作</th>
            </tr>
          </thead>
          <tbody id="historyTableBody"></tbody>
        </table>
      </div>
    </div>

    <div class="footer">
      <span>作者：方晏</span>
      <span>2024-11-09</span>
      <span>CC-BY-NC-SA 4.0</span>
    </div>
  </div>

  <script>
    (function() {
      const talentPool = [
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：4，心态抗压：4，比赛经验：5，总和：25",
        "您的天赋数值为：协调记忆:5，操作反应：7，战术意识：4，心态抗压：4，比赛经验：5，总和：25",
        "您的天赋数值为：协调记忆:6，操作反应：5，战术意识：5，心态抗压：4，比赛经验：5，总和：25",
        "您的天赋数值为：协调记忆:5，操作反应：5，战术意识：7，心态抗压：4，比赛经验：4，总和：25",
        "您的天赋数值为：协调记忆:5，操作反应：7，战术意识：4，心态抗压：4，比赛经验：5，总和：25",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：4，心态抗压：5，比赛经验：5，总和：26",
        "您的天赋数值为：协调记忆:7，操作反应：6，战术意识：4，心态抗压：5，比赛经验：4，总和：26",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：4，心态抗压：5，比赛经验：5，总和：26",
        "您的天赋数值为：协调记忆:6，操作反应：5，战术意识：5，心态抗压：5，比赛经验：5，总和：26",
        "您的天赋数值为：协调记忆:7，操作反应：6，战术意识：6，心态抗压：5，比赛经验：2，总和：26",
        "您的天赋数值为：协调记忆:5，操作反应：6，战术意识：6，心态抗压：4，比赛经验：5，总和：26",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：4，心态抗压：5，比赛经验：5，总和：26",
        "您的天赋数值为：协调记忆:7，操作反应：6，战术意识：4，心态抗压：5，比赛经验：4，总和：26",
        "您的天赋数值为：协调记忆:6，操作反应：5，战术意识：5，心态抗压：5，比赛经验：5，总和：26",
        "您的天赋数值为：协调记忆:7，操作反应：6，战术意识：6，心态抗压：5，比赛经验：2，总和：26",
        "您的天赋数值为：协调记忆:5，操作反应：6，战术意识：6，心态抗压：4，比赛经验：5，总和：26",
        "您的天赋数值为：协调记忆:5，操作反应：7，战术意识：4，心态抗压：6，比赛经验：5，总和：27",
        "您的天赋数值为：协调记忆:7，操作反应：7，战术意识：4，心态抗压：6，比赛经验：3，总和：27",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：5，心态抗压：5，比赛经验：5，总和：27",
        "您的天赋数值为：协调记忆:7，操作反应：7，战术意识：4，心态抗压：4，比赛经验：5，总和：27",
        "您的天赋数值为：协调记忆:7，操作反应：6，战术意识：5，心态抗压：4，比赛经验：5，总和：27",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：5，心态抗压：5，比赛经验：5，总和：27",
        "您的天赋数值为：协调记忆:5，操作反应：7，战术意识：4，心态抗压：6，比赛经验：5，总和：27",
        "您的天赋数值为：协调记忆:7，操作反应：7，战术意识：4，心态抗压：6，比赛经验：3，总和：27",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：5，心态抗压：5，比赛经验：5，总和：27",
        "您的天赋数值为：协调记忆:7，操作反应：7，战术意识：4，心态抗压：5，比赛经验：4，总和：27",
        "您的天赋数值为：协调记忆:7，操作反应：6，战术意识：5，心态抗压：4，比赛经验：5，总和：27",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：5，心态抗压：5，比赛经验：5，总和：27",
        "您的天赋数值为：协调记忆:6，操作反应：5，战术意识：5，心态抗压：6，比赛经验：5，总和：27",
        "您的天赋数值为：协调记忆:7，操作反应：6，战术意识：5，心态抗压：4，比赛经验：5，总和：27",
        "您的天赋数值为：协调记忆:7，操作反应：7，战术意识：5，心态抗压：4，比赛经验：5，总和：28",
        "您的天赋数值为：协调记忆:7，操作反应：5，战术意识：5，心态抗压：6，比赛经验：5，总和：28",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：6，心态抗压：6，比赛经验：4，总和：28",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：7，心态抗压：5，比赛经验：4，总和：28",
        "您的天赋数值为：协调记忆:7，操作反应：5，战术意识：5，心态抗压：6，比赛经验：5，总和：28",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：7，心态抗压：4，比赛经验：5，总和：28",
        "您的天赋数值为：协调记忆:8，操作反应：6，战术意识：5，心态抗压：5，比赛经验：4，总和：28",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：7，心态抗压：4，比赛经验：5，总和：28",
        "您的天赋数值为：协调记忆:7，操作反应：5，战术意识：5，心态抗压：6，比赛经验：5，总和：28",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：6，心态抗压：6，比赛经验：4，总和：28",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：7，心态抗压：5，比赛经验：4，总和：28",
        "您的天赋数值为：协调记忆:6，操作反应：5，战术意识：5，心态抗压：7，比赛经验：5，总和：28",
        "您的天赋数值为：协调记忆:7，操作反应：5，战术意识：5，心态抗压：6，比赛经验：5，总和：28",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：7，心态抗压：4，比赛经验：5，总和：28",
        "您的天赋数值为：协调记忆:8，操作反应：6，战术意识：5，心态抗压：5，比赛经验：4，总和：28",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：7，心态抗压：4，比赛经验：5，总和：28",
        "您的天赋数值为：协调记忆:6，操作反应：5，战术意识：7，心态抗压：4，比赛经验：7，总和：29",
        "您的天赋数值为：协调记忆:7，操作反应：5，战术意识：6，心态抗压：6，比赛经验：5，总和：29",
        "您的天赋数值为：协调记忆:8，操作反应：6，战术意识：4，心态抗压：6，比赛经验：5，总和：29",
        "您的天赋数值为：协调记忆:6，操作反应：5，战术意识：7，心态抗压：4，比赛经验：7，总和：29",
        "您的天赋数值为：协调记忆:7，操作反应：5，战术意识：7，心态抗压：6，比赛经验：4，总和：29",
        "您的天赋数值为：协调记忆:8，操作反应：6，战术意识：6，心态抗压：6，比赛经验：3，总和：29",
        "您的天赋数值为：协调记忆:6，操作反应：7，战术意识：8，心态抗压：6，比赛经验：2，总和：29",
        "您的天赋数值为：协调记忆:7，操作反应：6，战术意识：6，心态抗压：6，比赛经验：4，总和：29",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：6，心态抗压：6，比赛经验：5，总和：29",
        "您的天赋数值为：协调记忆:6，操作反应：7，战术意识：7，心态抗压：4，比赛经验：5，总和：29",
        "您的天赋数值为：协调记忆:6，操作反应：5，战术意识：7，心态抗压：4，比赛经验：7，总和：29",
        "您的天赋数值为：协调记忆:7，操作反应：5，战术意识：6，心态抗压：6，比赛经验：5，总和：29",
        "您的天赋数值为：协调记忆:8，操作反应：6，战术意识：4，心态抗压：6，比赛经验：5，总和：29",
        "您的天赋数值为：协调记忆:6，操作反应：5，战术意识：7，心态抗压：4，比赛经验：7，总和：29",
        "您的天赋数值为：协调记忆:7，操作反应：5，战术意识：7，心态抗压：6，比赛经验：4，总和：29",
        "您的天赋数值为：协调记忆:8，操作反应：6，战术意识：6，心态抗压：6，比赛经验：3，总和：29",
        "您的天赋数值为：协调记忆:6，操作反应：7，战术意识：8，心态抗压：6，比赛经验：2，总和：29",
        "您的天赋数值为：协调记忆:7，操作反应：6，战术意识：6，心态抗压：6，比赛经验：4，总和：29",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：6，心态抗压：6，比赛经验：5，总和：29",
        "您的天赋数值为：协调记忆:6，操作反应：7，战术意识：7，心态抗压：5，比赛经验：5，总和：30",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：6，心态抗压：6，比赛经验：6，总和：30",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：6，心态抗压：7，比赛经验：5，总和：30",
        "您的天赋数值为：协调记忆:7，操作反应：8，战术意识：5，心态抗压：6，比赛经验：4，总和：30",
        "您的天赋数值为：协调记忆:8，操作反应：7，战术意识：6，心态抗压：4，比赛经验：6，总和：31",
        "您的天赋数值为：协调记忆:6，操作反应：7，战术意识：7，心态抗压：4，比赛经验：7，总和：31",
        "您的天赋数值为：协调记忆:7，操作反应：8，战术意识：7，心态抗压：5，比赛经验：4，总和：31",
        "您的天赋数值为：协调记忆:6，操作反应：7，战术意识：7，心态抗压：5，比赛经验：7，总和：32",
        "您的天赋数值为：协调记忆:7，操作反应：7，战术意识：7，心态抗压：5，比赛经验：6，总和：32",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：8，心态抗压：6，比赛经验：7，总和：33",
        "您的天赋数值为：协调记忆:7，操作反应：8，战术意识：7，心态抗压：5，比赛经验：7，总和：34",
        "您的天赋数值为：协调记忆:8，操作反应：6，战术意识：7，心态抗压：6，比赛经验：7，总和：34",
        "您的天赋数值为：协调记忆:5，操作反应：8，战术意识：7，心态抗压：7，比赛经验：7，总和：34",
        "您的天赋数值为：协调记忆:8，操作反应：6，战术意识：8，心态抗压：6，比赛经验：7，总和：35",
        "您的天赋数值为：协调记忆:6，操作反应：6，战术意识：8，心态抗压：6，比赛经验：9，总和：35",
        "您的天赋数值为：协调记忆:7，操作反应：8，战术意识：6，心态抗压：8，比赛经验：7，总和：36"
      ];

      const drawCard = document.getElementById('drawCard');
      const cylinder = document.getElementById('cylinder');
      const sticksContainer = document.getElementById('sticksContainer');
      const rollingDisplay = document.getElementById('rollingDisplay');
      const btnDraw = document.getElementById('btnDraw');
      const nameInput = document.getElementById('nameInput');
      const resultCard = document.getElementById('resultCard');
      const resultTitle = document.getElementById('resultTitle');
      const attrList = document.getElementById('attrList');
      const totalDisplay = document.getElementById('totalDisplay');
      const historySection = document.getElementById('historySection');
      const historyTableBody = document.getElementById('historyTableBody');
      const canvas = document.getElementById('particles-canvas');
      const ctx = canvas.getContext('2d');

      let isDrawing = false;
      let drawHistory = [];
      const MAX_HISTORY = 20;
      const STORAGE_KEY = 'fulltimeOde_history_v2';
      const STORAGE_NAME = 'fulltimeOde_lastName';
      const DEFAULT_NAME = '匿名冒险者';

      const attrColorMap = {
        '协调记忆': { barClass: 'blue', valClass: 'blue' },
        '操作反应': { barClass: 'red', valClass: 'red' },
        '战术意识': { barClass: 'purple', valClass: 'purple' },
        '心态抗压': { barClass: 'green', valClass: 'green' },
        '比赛经验': { barClass: 'amber', valClass: 'amber' },
      };

      // 粒子背景
      let particles = [];
      function resizeCanvas() {
        canvas.width = window.innerWidth;
        canvas.height = window.innerHeight;
      }
      window.addEventListener('resize', () => { resizeCanvas(); initParticles(); });
      function initParticles() {
        particles = [];
        for (let i = 0; i < 55; i++) {
          particles.push({
            x: Math.random() * canvas.width,
            y: Math.random() * canvas.height,
            r: Math.random() * 1.4 + 0.3,
            vx: (Math.random() - 0.5) * 0.25,
            vy: (Math.random() - 0.5) * 0.25,
            alpha: Math.random() * 0.35 + 0.12,
            twinkleSpeed: Math.random() * 0.012 + 0.004,
            twinkleOffset: Math.random() * Math.PI * 2,
          });
        }
      }
      function drawParticles(ts) {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        for (const p of particles) {
          const alpha = p.alpha + Math.sin(ts * p.twinkleSpeed + p.twinkleOffset) * 0.15;
          const a = Math.max(0.05, Math.min(0.5, alpha));
          ctx.beginPath();
          ctx.arc(p.x, p.y, p.r, 0, Math.PI*2);
          ctx.fillStyle = `rgba(180,140,70,${a})`;
          ctx.fill();
          ctx.beginPath();
          ctx.arc(p.x, p.y, p.r*2.2, 0, Math.PI*2);
          ctx.fillStyle = `rgba(190,150,80,${a*0.2})`;
          ctx.fill();
          p.x += p.vx; p.y += p.vy;
          if (p.x < -10) p.x = canvas.width+10;
          if (p.x > canvas.width+10) p.x = -10;
          if (p.y < -10) p.y = canvas.height+10;
          if (p.y > canvas.height+10) p.y = -10;
        }
        requestAnimationFrame(drawParticles);
      }
      resizeCanvas();
      initParticles();
      requestAnimationFrame(drawParticles);

      // 本地存储
      function loadHistory() {
        try {
          const raw = localStorage.getItem(STORAGE_KEY);
          if (raw) drawHistory = JSON.parse(raw).slice(0, MAX_HISTORY);
        } catch (e) { drawHistory = []; }
      }
      function saveHistory() {
        try { localStorage.setItem(STORAGE_KEY, JSON.stringify(drawHistory)); } catch (e) {}
      }
      function loadLastName() {
        try {
          const n = localStorage.getItem(STORAGE_NAME);
          if (n && n.trim()) nameInput.value = n.trim();
        } catch (e) {}
      }
      function saveLastName(name) {
        try { localStorage.setItem(STORAGE_NAME, name.trim()); } catch (e) {}
      }
      function getCurrentName() {
        const raw = nameInput.value.trim();
        return raw.length > 0 ? raw : DEFAULT_NAME;
      }

      function parseTalent(str) {
        const content = str.replace(/^您的天赋数值为[：:]\s*/, '');
        const parts = content.split(/[，,]\s*/);
        const attrs = [];
        let total = 0;
        for (const p of parts) {
          const m = p.match(/^(.+?)[：:](\d+)$/);
          if (m) {
            if (m[1].trim() === '总和') total = parseInt(m[2]);
            else attrs.push({ name: m[1].trim(), value: parseInt(m[2]) });
          }
        }
        return { attrs, total };
      }

      function escapeHTML(str) {
        const div = document.createElement('div');
        div.textContent = str;
        return div.innerHTML;
      }

      function renderResult(talentStr, personName, animate = true) {
        const { attrs, total } = parseTalent(talentStr);
        resultTitle.innerHTML = `✨ <span class="name-highlight">【${escapeHTML(personName)}】</span> 的天赋觉醒 ✨`;
        let html = '';
        for (const attr of attrs) {
          const colors = attrColorMap[attr.name] || { barClass: 'blue', valClass: 'blue' };
          const pct = Math.min(100, Math.max(0, (attr.value / 10) * 100));
          html += `<div class="attr-item">
            <span class="attr-label">${attr.name}</span>
            <div class="attr-bar-wrap"><div class="attr-bar ${colors.barClass}" style="width:${animate?'0':pct+'%'}" data-width="${pct}%"></div></div>
            <span class="attr-val ${colors.valClass}">${attr.value}</span>
          </div>`;
        }
        attrList.innerHTML = html;
        totalDisplay.innerHTML = `天赋总和：<span class="total-num">${total}</span> 点`;
        resultCard.classList.add('visible');
        resultCard.style.animation = 'none';
        resultCard.offsetHeight;
        resultCard.style.animation = 'resultReveal 0.5s ease-out';
        if (animate) {
          requestAnimationFrame(() => {
            requestAnimationFrame(() => {
              attrList.querySelectorAll('.attr-bar').forEach(b => b.style.width = b.dataset.width);
            });
          });
        } else {
          attrList.querySelectorAll('.attr-bar').forEach(b => b.style.width = b.dataset.width);
        }
      }

      function addToHistory(talentStr, personName) {
        const { total } = parseTalent(talentStr);
        const entry = {
          name: personName || DEFAULT_NAME,
          str: talentStr,
          total,
          time: Date.now()
        };
        drawHistory.unshift(entry);
        if (drawHistory.length > MAX_HISTORY) drawHistory.pop();
        saveHistory();
        renderHistoryTable();
      }

      function formatTime(ts) {
        const d = new Date(ts);
        const pad = n => n.toString().padStart(2, '0');
        return `${d.getFullYear()}-${pad(d.getMonth()+1)}-${pad(d.getDate())} ${pad(d.getHours())}:${pad(d.getMinutes())}:${pad(d.getSeconds())}`;
      }

      function renderHistoryTable() {
        if (drawHistory.length === 0) {
          historySection.classList.remove('visible');
          return;
        }
        historySection.classList.add('visible');
        let rows = '';
        drawHistory.forEach((item, idx) => {
          rows += `<tr data-index="${idx}">
            <td>${idx + 1}</td>
            <td class="name-cell">${escapeHTML(item.name)}</td>
            <td class="total-cell">${item.total}</td>
            <td>${formatTime(item.time)}</td>
            <td><button class="action-btn">查看详情</button></td>
          </tr>`;
        });
        historyTableBody.innerHTML = rows;
        // 绑定点击行或按钮查看详情
        historyTableBody.querySelectorAll('tr').forEach(row => {
          row.addEventListener('click', (e) => {
            const idx = parseInt(row.dataset.index, 10);
            if (!isNaN(idx) && drawHistory[idx]) {
              const rec = drawHistory[idx];
              renderResult(rec.str, rec.name, false);
              rollingDisplay.textContent = '📋 查看历史记录';
              rollingDisplay.className = 'rolling-display stopped';
              resultCard.scrollIntoView({ behavior: 'smooth', block: 'center' });
            }
          });
        });
        // 按钮单独阻止冒泡以免触发两次（但这里用行点击即可，按钮无需额外事件）
        historyTableBody.querySelectorAll('.action-btn').forEach(btn => {
          btn.addEventListener('click', (e) => {
            e.stopPropagation();
            const row = btn.closest('tr');
            if (row) row.click();
          });
        });
      }

      function getRandomTalent() {
        return talentPool[Math.floor(Math.random() * talentPool.length)];
      }

      function performDraw() {
        if (isDrawing) return;
        isDrawing = true;
        btnDraw.disabled = true;
        const currentName = getCurrentName();
        if (currentName !== DEFAULT_NAME || nameInput.value.trim().length > 0) {
          saveLastName(currentName);
        }
        const finalResult = getRandomTalent();
        resultCard.classList.remove('visible');
        drawCard.classList.add('shaking');
        const sticks = sticksContainer.querySelectorAll('.stick');
        sticks.forEach(s => s.classList.remove('highlight'));
        const randomStick = sticks[Math.floor(Math.random() * sticks.length)];
        randomStick.classList.add('highlight');
        rollingDisplay.classList.add('active');
        rollingDisplay.textContent = '抽取中...';

        const scrollIntervals = [
          { duration: 400, interval: 60 },
          { duration: 400, interval: 100 },
          { duration: 350, interval: 160 },
          { duration: 250, interval: 240 },
          { duration: 100, interval: 350 },
        ];
        let elapsed = 0, phaseIdx = 0, lastSwitch = 0, timer = null;

        function step(ts) {
          if (!lastSwitch) lastSwitch = ts;
          const dt = ts - lastSwitch;
          if (phaseIdx < scrollIntervals.length) {
            const phase = scrollIntervals[phaseIdx];
            if (elapsed >= phase.duration) {
              phaseIdx++;
              if (phaseIdx >= scrollIntervals.length) {
                rollingDisplay.classList.remove('active');
                rollingDisplay.classList.add('stopped');
                const { total } = parseTalent(finalResult);
                rollingDisplay.textContent = `🎉 ${currentName} 的天赋总和：${total} 点`;
                setTimeout(() => {
                  renderResult(finalResult, currentName, true);
                  addToHistory(finalResult, currentName);
                  drawCard.classList.remove('shaking');
                  sticks.forEach(s => s.classList.remove('highlight'));
                  isDrawing = false;
                  btnDraw.disabled = false;
                  resultCard.scrollIntoView({ behavior: 'smooth', block: 'center' });
                }, 200);
                return;
              }
              lastSwitch = ts;
            }
            if (dt >= phase.interval) {
              const preview = getRandomTalent();
              rollingDisplay.textContent = `🌟 天赋总和：${parseTalent(preview).total} 点`;
              lastSwitch = ts;
              elapsed += dt;
            }
          }
          if (phaseIdx < scrollIntervals.length) timer = requestAnimationFrame(step);
        }
        requestAnimationFrame(step);

        setTimeout(() => {
          if (isDrawing) {
            if (timer) cancelAnimationFrame(timer);
            rollingDisplay.classList.remove('active');
            rollingDisplay.classList.add('stopped');
            const { total } = parseTalent(finalResult);
            rollingDisplay.textContent = `🎉 ${currentName} 的天赋总和：${total} 点`;
            renderResult(finalResult, currentName, true);
            addToHistory(finalResult, currentName);
            drawCard.classList.remove('shaking');
            sticks.forEach(s => s.classList.remove('highlight'));
            isDrawing = false;
            btnDraw.disabled = false;
            resultCard.scrollIntoView({ behavior: 'smooth', block: 'center' });
          }
        }, 2200);
      }

      btnDraw.addEventListener('click', performDraw);
      cylinder.addEventListener('click', (e) => { e.stopPropagation(); performDraw(); });
      document.addEventListener('keydown', (e) => {
        if (e.code === 'Space' && !isDrawing && document.activeElement !== nameInput) {
          e.preventDefault();
          performDraw();
        }
      });
      nameInput.addEventListener('keydown', (e) => {
        if (e.code === 'Enter' && !isDrawing) { e.preventDefault(); performDraw(); }
      });

      function init() {
        loadHistory();
        loadLastName();
        renderHistoryTable();
        rollingDisplay.textContent = '输入姓名后点击按钮或签筒抽取天赋';
        rollingDisplay.className = 'rolling-display';
      }
      init();
    })();
  </script>
</body>
</html>
