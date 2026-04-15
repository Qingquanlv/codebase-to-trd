# HTML Template Reference

Agent 生成最终报告时使用此模板。将所有分析内容填入对应位置，生成单个自包含 HTML 文件。

## Complete HTML Template

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{{PROJECT_NAME}} 深度解读</title>
<script src="https://cdn.jsdelivr.net/npm/mermaid@11/dist/mermaid.min.js"></script>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/highlightjs/cdn-release@11/build/styles/github-dark.min.css">
<script src="https://cdn.jsdelivr.net/gh/highlightjs/cdn-release@11/build/highlight.min.js"></script>
<style>
  :root {
    --bg: #ffffff;
    --bg-alt: #f8f9fa;
    --text: #1a1a2e;
    --text-secondary: #555;
    --accent: #2563eb;
    --accent-light: #dbeafe;
    --border: #e5e7eb;
    --code-bg: #1e1e2e;
    --success: #059669;
    --warning: #d97706;
    --danger: #dc2626;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'PingFang SC',
                 'Hiragino Sans GB', 'Microsoft YaHei', sans-serif;
    color: var(--text);
    background: var(--bg);
    line-height: 1.8;
    display: flex;
  }

  /* --- Sidebar TOC --- */
  .toc {
    position: fixed;
    top: 0; left: 0;
    width: 260px; height: 100vh;
    overflow-y: auto;
    padding: 24px 16px;
    background: var(--bg-alt);
    border-right: 1px solid var(--border);
    font-size: 13px; z-index: 100;
    transition: width 0.25s ease, padding 0.25s ease;
  }
  .toc.collapsed { width: 42px; padding: 16px 0; overflow: hidden; }
  .toc.collapsed .toc-links { display: none; }
  .toc-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 12px; }
  .toc h3 { font-size: 14px; color: var(--accent); margin: 0; text-transform: uppercase; letter-spacing: 0.5px; white-space: nowrap; overflow: hidden; }
  .toc-toggle {
    width: 26px; height: 26px; border: none; background: transparent;
    cursor: pointer; color: var(--text-secondary); font-size: 16px;
    display: flex; align-items: center; justify-content: center;
    border-radius: 4px; flex-shrink: 0; transition: all 0.15s;
  }
  .toc-toggle:hover { background: var(--border); color: var(--accent); }
  .toc.collapsed .toc-header { justify-content: center; }
  .toc.collapsed h3 { display: none; }
  .toc a { display: block; padding: 4px 8px; color: var(--text-secondary); text-decoration: none; border-radius: 4px; transition: all 0.2s; }
  .toc a:hover, .toc a.active { color: var(--accent); background: var(--accent-light); }
  .toc .toc-h2 { padding-left: 8px; font-weight: 600; margin-top: 8px; }
  .toc .toc-h3 { padding-left: 24px; font-size: 12px; }
  .toc .toc-h4 { padding-left: 40px; font-size: 12px; color: #888; }

  /* --- Main Content --- */
  .content {
    margin-left: 260px;
    max-width: 860px;
    padding: 48px 40px 120px;
    transition: margin-left 0.25s ease;
  }
  .content.toc-collapsed { margin-left: 42px; }

  /* --- Header --- */
  .header { margin-bottom: 48px; }
  .header h1 {
    font-size: 32px;
    font-weight: 700;
    color: var(--text);
    margin-bottom: 8px;
  }
  .header .subtitle {
    font-size: 18px;
    color: var(--text-secondary);
    margin-bottom: 16px;
  }
  .header .lead {
    font-size: 16px;
    color: var(--text-secondary);
    line-height: 1.8;
    padding: 16px 20px;
    background: var(--bg-alt);
    border-left: 4px solid var(--accent);
    border-radius: 0 8px 8px 0;
  }

  /* --- Core Conclusions Box --- */
  .conclusions {
    margin: 32px 0;
    padding: 20px 24px;
    background: linear-gradient(135deg, #eff6ff 0%, #f0fdf4 100%);
    border: 1px solid #bfdbfe;
    border-radius: 12px;
  }
  .conclusions h3 {
    font-size: 16px;
    color: var(--accent);
    margin-bottom: 12px;
  }
  .conclusions ul {
    list-style: none;
    padding: 0;
  }
  .conclusions li {
    padding: 6px 0 6px 24px;
    position: relative;
    font-size: 15px;
  }
  .conclusions li::before {
    content: "✦";
    position: absolute;
    left: 0;
    color: var(--accent);
  }

  /* --- Section Headings --- */
  h2 {
    font-size: 24px;
    font-weight: 700;
    color: var(--text);
    margin: 48px 0 16px;
    padding-bottom: 8px;
    border-bottom: 2px solid var(--accent);
  }
  h3 {
    font-size: 20px;
    font-weight: 600;
    color: var(--text);
    margin: 32px 0 12px;
  }
  h4 {
    font-size: 17px;
    font-weight: 600;
    color: var(--text-secondary);
    margin: 24px 0 8px;
  }

  /* --- Body Text --- */
  p {
    font-size: 15px;
    margin-bottom: 16px;
    color: var(--text);
  }
  ul, ol {
    margin: 8px 0 16px 24px;
    font-size: 15px;
  }
  li { margin-bottom: 6px; }

  /* --- Mermaid Diagrams --- */
  .diagram-container {
    margin: 24px 0;
    padding: 24px;
    background: var(--bg-alt);
    border: 1px solid var(--border);
    border-radius: 12px;
    overflow-x: auto;
    position: relative;
  }
  .diagram-container .caption {
    text-align: center;
    font-size: 13px;
    color: var(--text-secondary);
    margin-top: 12px;
  }
  .diagram-zoom-btn {
    position: absolute; top: 8px; right: 8px;
    width: 28px; height: 28px; border: 1px solid var(--border);
    background: white; border-radius: 6px; cursor: pointer;
    font-size: 14px; display: flex; align-items: center; justify-content: center;
    color: var(--text-secondary); opacity: 0; transition: opacity 0.15s;
    z-index: 5;
  }
  .diagram-container:hover .diagram-zoom-btn { opacity: 1; }
  /* Lightbox */
  .diagram-lightbox {
    position: fixed; inset: 0; z-index: 500;
    background: rgba(0,0,0,.75);
    display: flex; flex-direction: column; align-items: center; justify-content: center;
  }
  .diagram-lightbox-inner {
    background: white; border-radius: 12px;
    overflow: hidden; position: relative;
    box-shadow: 0 24px 64px rgba(0,0,0,.4);
    max-width: 92vw; max-height: 90vh;
  }

  /* --- Code Blocks --- */
  pre {
    margin: 16px 0;
    border-radius: 8px;
    overflow-x: auto;
  }
  pre code {
    font-family: 'JetBrains Mono', 'Fira Code', Consolas, Monaco, monospace;
    font-size: 13px;
    line-height: 1.6;
    padding: 16px !important;
  }
  code:not(pre code) {
    font-family: 'JetBrains Mono', Consolas, monospace;
    font-size: 13px;
    padding: 2px 6px;
    background: #f1f5f9;
    color: #c7254e;
    border-radius: 4px;
  }

  /* --- Command Flow Block --- */
  .cmd-flow {
    margin: 16px 0;
    padding: 16px 20px;
    background: #0f172a;
    color: #e2e8f0;
    border-radius: 8px;
    font-family: 'JetBrains Mono', Consolas, monospace;
    font-size: 13px;
    line-height: 2;
  }
  .cmd-flow .cmd-comment {
    color: #64748b;
  }
  .cmd-flow .cmd-line {
    color: #7dd3fc;
  }

  /* --- Usage Mode Cards --- */
  .usage-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
    gap: 16px;
    margin: 20px 0;
  }
  .usage-card {
    padding: 20px;
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 12px;
    box-shadow: 0 1px 3px rgba(0,0,0,0.06);
  }
  .usage-card .usage-title {
    font-size: 15px;
    font-weight: 600;
    color: var(--accent);
    margin-bottom: 12px;
    padding-bottom: 8px;
    border-bottom: 1px solid var(--border);
  }
  .usage-card .usage-row {
    display: flex;
    gap: 8px;
    margin-bottom: 8px;
    font-size: 13px;
  }
  .usage-card .usage-label {
    color: var(--text-secondary);
    min-width: 52px;
    font-weight: 500;
  }
  .usage-card .usage-value {
    color: var(--text);
  }

  /* --- Fit/Unfit Section --- */
  .fit-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
    margin: 16px 0;
  }
  .fit-card {
    padding: 16px 20px;
    border-radius: 10px;
    border: 1px solid var(--border);
  }
  .fit-card.fit {
    background: #f0fdf4;
    border-color: #bbf7d0;
  }
  .fit-card.unfit {
    background: #fef9f0;
    border-color: #fed7aa;
  }
  .fit-card h4 {
    margin: 0 0 10px;
    font-size: 14px;
  }
  .fit-card.fit h4 { color: var(--success); }
  .fit-card.unfit h4 { color: var(--warning); }
  .fit-card ul {
    margin: 0;
    padding-left: 18px;
    font-size: 14px;
  }

  /* --- Walkthrough Timeline --- */
  .timeline {
    margin: 20px 0;
    position: relative;
    padding-left: 28px;
  }
  .timeline::before {
    content: '';
    position: absolute;
    left: 10px;
    top: 8px;
    bottom: 8px;
    width: 2px;
    background: var(--border);
  }
  .timeline-step {
    position: relative;
    margin-bottom: 20px;
  }
  .timeline-step::before {
    content: '';
    position: absolute;
    left: -22px;
    top: 7px;
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background: var(--accent);
    border: 2px solid white;
    box-shadow: 0 0 0 2px var(--accent);
  }
  .timeline-step .step-title {
    font-size: 14px;
    font-weight: 600;
    color: var(--text);
    margin-bottom: 4px;
  }
  .timeline-step .step-desc {
    font-size: 13px;
    color: var(--text-secondary);
  }

  /* --- Module Card --- */
  .module-card {
    margin: 24px 0;
    padding: 24px;
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 12px;
    box-shadow: 0 1px 3px rgba(0,0,0,0.06);
  }
  .module-card .module-meta {
    display: flex;
    gap: 16px;
    margin-bottom: 12px;
    font-size: 13px;
    color: var(--text-secondary);
    flex-wrap: wrap;
  }
  .module-card .module-meta span {
    padding: 2px 10px;
    background: var(--bg-alt);
    border-radius: 20px;
  }

  /* --- Directory Table --- */
  .dir-table {
    width: 100%;
    border-collapse: collapse;
    margin: 8px 0 20px;
    font-size: 14px;
  }
  .dir-table th {
    background: var(--bg-alt);
    font-weight: 600;
    text-align: left;
    padding: 8px 12px;
    border: 1px solid var(--border);
  }
  .dir-table td {
    padding: 8px 12px;
    border: 1px solid var(--border);
    vertical-align: top;
  }
  .dir-table td:first-child {
    font-family: 'JetBrains Mono', Consolas, monospace;
    font-size: 12px;
    color: var(--accent);
    white-space: nowrap;
    width: 220px;
  }

  /* --- Directory Group Rationale --- */
  .dir-rationale {
    font-size: 13px !important;
    color: var(--text-secondary) !important;
    font-style: italic;
    margin: 4px 0 8px !important;
    padding: 8px 12px;
    background: var(--bg-alt);
    border-left: 3px solid var(--border);
    border-radius: 0 4px 4px 0;
  }

  /* --- Directory Tree --- */
  .dir-tree {
    margin: 16px 0 24px;
    padding: 18px 22px;
    background: #f8fafc;
    border: 1px solid var(--border);
    border-radius: 8px;
    overflow-x: auto;
  }
  .dir-tree pre {
    margin: 0;
    background: transparent;
    font-family: 'JetBrains Mono', 'Fira Code', Consolas, monospace;
    font-size: 13px;
    line-height: 1.9;
    white-space: pre;
  }
  .dir-tree .tree-root  { color: #1a1a2e; font-weight: 700; font-size: 14px; }
  .dir-tree .tree-branch { color: #9ca3af; user-select: none; }
  .dir-tree .tree-dir   { color: var(--accent); font-weight: 600; }
  .dir-tree .tree-entry { color: #c7254e; font-weight: 600; } /* ★ entry point */
  .dir-tree .tree-file  { color: #374151; }
  .dir-tree .tree-comment { color: #9ca3af; font-style: italic; }

  /* --- Cast of Characters Table --- */
  .cast-table {
    width: 100%;
    border-collapse: collapse;
    margin: 16px 0 24px;
    font-size: 14px;
  }
  .cast-table th {
    background: var(--accent);
    color: #ffffff;
    font-weight: 600;
    text-align: left;
    padding: 10px 14px;
    border: 1px solid var(--accent);
    font-size: 12px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }
  .cast-table td {
    padding: 10px 14px;
    border: 1px solid var(--border);
    vertical-align: top;
  }
  .cast-table tr:nth-child(even) td { background: var(--bg-alt); }
  .cast-table td:first-child { font-weight: 600; color: var(--accent); }
  .cast-table td:nth-child(2) code {
    font-size: 12px; color: #c7254e; background: #f1f5f9;
    padding: 1px 5px; border-radius: 3px;
  }
  .cast-table td:last-child { font-size: 13px; color: var(--text-secondary); }

  /* --- Evaluation Section --- */
  .eval-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 24px;
    margin: 16px 0;
  }
  .eval-card {
    padding: 20px;
    border-radius: 12px;
    border: 1px solid var(--border);
  }
  .eval-card.strength {
    background: #f0fdf4;
    border-color: #bbf7d0;
  }
  .eval-card.weakness {
    background: #fffbeb;
    border-color: #fde68a;
  }
  .eval-card h4 {
    margin: 0 0 12px;
    font-size: 16px;
  }
  .eval-card.strength h4 { color: var(--success); }
  .eval-card.weakness h4 { color: var(--warning); }

  /* --- Table (generic) --- */
  table {
    width: 100%;
    border-collapse: collapse;
    margin: 16px 0;
    font-size: 14px;
  }
  th {
    background: var(--bg-alt);
    font-weight: 600;
    text-align: left;
    padding: 10px 12px;
    border: 1px solid var(--border);
  }
  td {
    padding: 10px 12px;
    border: 1px solid var(--border);
  }

  /* --- Mobile --- */
  @media (max-width: 900px) {
    .toc { display: none; }
    .content { margin-left: 0; padding: 24px 16px 80px; }
    .eval-grid { grid-template-columns: 1fr; }
    .fit-grid { grid-template-columns: 1fr; }
    .usage-grid { grid-template-columns: 1fr; }
  }

  /* ─── Interactive Panel System ───────────────────────────────────────── */
  .code-backdrop { position:fixed; inset:0; background:rgba(0,0,0,.38); z-index:200; opacity:0; pointer-events:none; transition:opacity .25s; }
  .code-backdrop.open { opacity:1; pointer-events:auto; }
  .code-panel { position:fixed; top:0; right:0; height:100vh; width:min(820px,65vw); min-width:300px; max-width:96vw; background:#1e1e2e; color:#cdd6f4; transform:translateX(100%); transition:transform .26s cubic-bezier(.4,0,.2,1); z-index:201; display:flex; flex-direction:column; box-shadow:-4px 0 32px rgba(0,0,0,.3); }
  .code-panel.open { transform:translateX(0); }
  .panel-resize-grip { position:absolute; left:0; top:0; width:6px; height:100%; cursor:ew-resize; z-index:10; background:transparent; border-radius:3px 0 0 3px; }
  .panel-resize-grip:hover,.panel-resize-grip.dragging { background:rgba(89,174,250,.35); }
  .code-panel-header { display:flex; align-items:center; gap:8px; padding:0 14px 0 18px; height:48px; border-bottom:1px solid #313244; flex-shrink:0; background:#181825; overflow:hidden; }
  .code-panel-path { font-size:13px; font-family:'JetBrains Mono',Consolas,monospace; color:#cdd6f4; overflow:hidden; text-overflow:ellipsis; white-space:nowrap; flex:1; min-width:0; }
  .code-panel-badge { font-size:11px; padding:2px 8px; border-radius:4px; background:#313244; color:#89b4fa; white-space:nowrap; flex-shrink:0; }
  .panel-gh-link { font-size:12px; color:#89b4fa; text-decoration:none; white-space:nowrap; border:1px solid #89b4fa44; padding:2px 9px; border-radius:4px; flex-shrink:0; transition:background .15s; }
  .panel-gh-link:hover { background:#89b4fa22; }
  .code-panel-close { width:28px; height:28px; border:none; background:transparent; color:#9399b2; cursor:pointer; font-size:22px; line-height:1; display:flex; align-items:center; justify-content:center; border-radius:4px; transition:all .15s; flex-shrink:0; }
  .code-panel-close:hover { background:#313244; color:#cdd6f4; }
  #iframePanelBody { display:none; flex:1; flex-direction:column; overflow:hidden; }
  #iframePanelBody iframe { width:100%; flex:1; border:none; display:none; }
  .iframe-loading { display:flex; flex-direction:column; align-items:center; justify-content:center; flex:1; color:#6c7086; gap:12px; font-size:14px; }
  .iframe-spin { width:32px; height:32px; border:3px solid #313244; border-top-color:#89b4fa; border-radius:50%; animation:panel-spin .8s linear infinite; }
  @keyframes panel-spin { to { transform:rotate(360deg); } }
  #termPanelBody { display:none; flex:1; overflow-y:auto; padding:20px 22px 32px; }
  .term-tags { display:flex; gap:6px; flex-wrap:wrap; margin-bottom:12px; }
  .term-tag { font-size:11px; padding:2px 8px; border-radius:12px; background:#313244; color:#89b4fa; }
  .term-title-h { font-size:18px; font-weight:700; color:#cdd6f4; margin-bottom:12px; }
  .term-def { font-size:14px; line-height:1.75; color:#bac2de; margin-bottom:16px; padding:12px 14px; background:#181825; border-left:3px solid #89b4fa; border-radius:0 6px 6px 0; }
  .term-def code,.term-mechanics code { background:#313244; color:#f38ba8; padding:1px 5px; border-radius:3px; font-size:12px; }
  .term-sec { font-size:11px; font-weight:700; color:#89b4fa; text-transform:uppercase; letter-spacing:.5px; margin:14px 0 8px; }
  .term-mechanics { list-style:none; padding:0; margin:0; }
  .term-mechanics li { font-size:13px; color:#bac2de; padding:6px 0 6px 18px; position:relative; border-bottom:1px solid #313244; line-height:1.6; }
  .term-mechanics li::before { content:"·"; position:absolute; left:4px; color:#6c7086; font-size:18px; line-height:1.4; }
  .term-mechanics li:last-child { border-bottom:none; }
  .term-related-list { display:flex; gap:8px; flex-wrap:wrap; }
  .term-rel-btn { font-size:12px; padding:3px 10px; border-radius:4px; background:#313244; color:#a6e3a1; border:1px solid #45475a; cursor:pointer; transition:all .15s; }
  .term-rel-btn:hover { background:#45475a; }
  .term-files-list { display:flex; flex-direction:column; gap:4px; margin-top:4px; }
  .term-file-btn { font-size:12px; font-family:'JetBrains Mono',Consolas,monospace; padding:5px 10px; background:#181825; color:#89b4fa; border:1px solid #313244; border-radius:4px; cursor:pointer; text-align:left; transition:all .15s; }
  .term-file-btn:hover { background:#313244; }
  .file-link { color:#2563eb !important; text-decoration:underline; text-decoration-style:dotted; cursor:pointer; }
  .file-link:hover { color:#1d4ed8 !important; }
  .term-link { color:#7c3aed; text-decoration:underline; text-decoration-style:dotted; cursor:pointer; }
  .term-link:hover { color:#6d28d9; }
  /* ─── Panel nav / pin buttons ─────────────────────────────────────────── */
  .panel-nav-btn { width:26px;height:26px;border:none;background:transparent;color:#9399b2;cursor:pointer;font-size:20px;line-height:1;display:flex;align-items:center;justify-content:center;border-radius:4px;transition:all .15s;flex-shrink:0; }
  .panel-nav-btn:hover:not(:disabled) { background:#313244;color:#cdd6f4; }
  .panel-nav-btn:disabled { opacity:.28;cursor:default; }
  .panel-pin-btn { width:26px;height:26px;border:none;background:transparent;color:#9399b2;cursor:pointer;font-size:13px;display:flex;align-items:center;justify-content:center;border-radius:4px;transition:all .15s;flex-shrink:0; }
  .panel-pin-btn:hover { background:#313244; }
  .panel-pin-btn.active { color:#89b4fa;background:#313244; }
  .code-panel.pinned { box-shadow:-2px 0 20px rgba(0,0,0,.2); }
</style>
</head>
<body>

<!-- Sidebar TOC -->
<nav class="toc" id="tocNav">
  <div class="toc-header">
    <h3>目录</h3>
    <button class="toc-toggle" id="tocToggle" title="收起/展开目录">◀</button>
  </div>
  <div class="toc-links">
  <a href="#section-1" class="toc-h2">1 摘要</a>
  <a href="#section-1-1" class="toc-h3">1.1 项目背景</a>
  <a href="#section-1-2" class="toc-h3">1.2 项目预期</a>
  <!-- Agent: add toc-h4 entries for each 1.2.x 预期 -->
  <a href="#section-2" class="toc-h2">2 项目设计理念</a>
  <!-- Agent: add toc-h3 entries for each 理念 -->
  <a href="#section-3" class="toc-h2">3 使用方式与执行链路</a>
  <a href="#section-3-1" class="toc-h3">3.1 适合解决什么问题</a>
  <a href="#section-3-2" class="toc-h3">3.2 最小可运行路径</a>
  <a href="#section-3-3" class="toc-h3">3.3 核心使用模式</a>
  <a href="#section-3-4" class="toc-h3">3.4 完整链路示例</a>
  <a href="#section-4" class="toc-h2">4 系统详细设计</a>
  <a href="#section-4-1" class="toc-h3">4.1 系统架构设计</a>
  <a href="#section-4-1-1" class="toc-h4">4.1.1 架构图</a>
  <a href="#section-4-1-2" class="toc-h4">4.1.2 目录与核心文件</a>
  <a href="#section-4-2" class="toc-h3">4.2 关键模块实现细节</a>
  <!-- Agent: add toc-h4 entries for each 4.2.x module -->
  <a href="#section-5" class="toc-h2">5 方案评价</a>
  <a href="#section-5-1" class="toc-h3">5.1 优势分析</a>
  <a href="#section-5-2" class="toc-h3">5.2 局限分析</a>
  </div><!-- .toc-links -->
</nav>

<!-- Main Content -->
<main class="content">

  <!-- Header -->
  <header class="header">
    <h1>{{主标题}}</h1>
    <div class="subtitle">{{副标题}}</div>
    <div class="lead">{{导语: 2-3 句话}}</div>
  </header>

  <!-- Core Conclusions -->
  <div class="conclusions">
    <h3>核心结论</h3>
    <ul>
      <li>{{结论 1}}</li>
      <li>{{结论 2}}</li>
      <li>{{结论 3}}</li>
    </ul>
  </div>

  <!-- Section 1: 摘要 -->
  <h2 id="section-1">1 摘要</h2>

  <h3 id="section-1-1">1.1 项目背景</h3>
  <p>{{项目产生的背景、要解决的行业/技术痛点}}</p>

  <h3 id="section-1-2">1.2 项目预期</h3>

  <h4 id="section-1-2-1">1.2.1 预期一：{{预期标题}}</h4>
  <p>{{具体说明}}</p>

  <h4 id="section-1-2-2">1.2.2 预期二：{{预期标题}}</h4>
  <p>{{具体说明}}</p>

  <!-- Section 2: 设计理念 -->
  <h2 id="section-2">2 项目设计理念</h2>

  <h3 id="section-2-1">2.1 {{理念一标题}}</h3>
  <p>{{解释这个设计理念在项目中如何体现}}</p>

  <h3 id="section-2-2">2.2 {{理念二标题}}</h3>
  <p>{{解释}}</p>

  <h3 id="section-2-3">2.3 {{理念三标题}}（竞品横向对比）</h3>
  <p>{{解释这个理念}}</p>
  <p>与同类工具的对比：</p>
  <table>
    <thead>
      <tr><th>工具</th><th>定位</th><th>优势</th><th>劣势</th></tr>
    </thead>
    <tbody>
      <tr>
        <td>{{本项目名}}</td>
        <td>{{定位}}</td>
        <td>{{优势}}</td>
        <td>{{劣势}}</td>
      </tr>
      <tr>
        <td>{{竞品 1}}</td>
        <td>{{定位}}</td>
        <td>{{优势}}</td>
        <td>{{劣势}}</td>
      </tr>
      <tr>
        <td>{{竞品 2}}</td>
        <td>{{定位}}</td>
        <td>{{优势}}</td>
        <td>{{劣势}}</td>
      </tr>
    </tbody>
  </table>
  <p>{{结论：本项目在哪里赢，在哪里输，适合谁用}}</p>

  <!-- Section 3: 使用方式与执行链路 -->
  <h2 id="section-3">3 项目使用方式与执行链路</h2>

  <h3 id="section-3-1">3.1 当前适合解决什么问题</h3>

  <div class="fit-grid">
    <div class="fit-card fit">
      <h4>✅ 适合的任务类型</h4>
      <ul>
        <li>{{适合场景 1，如"新功能开发"}}</li>
        <li>{{适合场景 2，如"长任务拆解执行"}}</li>
        <li>{{适合场景 3，如"无人值守推进"}}</li>
        <li>{{适合场景 4，如"需要干净 git 历史的开发流程"}}</li>
      </ul>
    </div>
    <div class="fit-card unfit">
      <h4>⚠️ 不太适合的任务类型</h4>
      <ul>
        <li>{{不适合场景 1，如"十分钟内搞定的小改动"}}</li>
        <li>{{不适合场景 2，如"高度探索性的开放式研究"}}</li>
        <li>{{不适合场景 3，如"完全没有明确目标边界的任务"}}</li>
      </ul>
    </div>
  </div>

  <h3 id="section-3-2">3.2 最小可运行路径</h3>
  <p>{{安装完成后第一步做什么、如何初始化、如何启动最小任务、去哪看结果的简要说明}}</p>

  <div class="cmd-flow">
    <span class="cmd-comment"># 第一步：安装</span><br>
    <span class="cmd-line">{{安装命令}}</span><br>
    <br>
    <span class="cmd-comment"># 第二步：进入项目目录</span><br>
    <span class="cmd-line">{{cd 命令}}</span><br>
    <br>
    <span class="cmd-comment"># 第三步：启动工具</span><br>
    <span class="cmd-line">{{启动命令}}</span><br>
    <br>
    <span class="cmd-comment"># 第四步：查看结果</span><br>
    <span class="cmd-line">{{查看命令或路径}}</span>
  </div>

  <div class="diagram-container">
    <div class="mermaid">
graph LR
    A[安装工具] --> B[初始化项目]
    B --> C[描述目标]
    C --> D[启动执行]
    D --> E[查看产出]
    </div>
    <div class="caption">图 3-1: 从输入到产出的最小路径</div>
  </div>

  <h3 id="section-3-3">3.3 核心使用模式</h3>

  <div class="usage-grid">
    <!-- Agent: add usage-card for each mode, minimum 2 -->
    <div class="usage-card">
      <div class="usage-title">方式一：{{模式名称}}</div>
      <div class="usage-row">
        <span class="usage-label">何时用</span>
        <span class="usage-value">{{何时使用的说明}}</span>
      </div>
      <div class="usage-row">
        <span class="usage-label">输入</span>
        <span class="usage-value">{{输入是什么}}</span>
      </div>
      <div class="usage-row">
        <span class="usage-label">输出</span>
        <span class="usage-value">{{预期输出是什么}}</span>
      </div>
    </div>

    <div class="usage-card">
      <div class="usage-title">方式二：{{模式名称}}</div>
      <div class="usage-row">
        <span class="usage-label">何时用</span>
        <span class="usage-value">{{何时使用的说明}}</span>
      </div>
      <div class="usage-row">
        <span class="usage-label">输入</span>
        <span class="usage-value">{{输入是什么}}</span>
      </div>
      <div class="usage-row">
        <span class="usage-label">输出</span>
        <span class="usage-value">{{预期输出是什么}}</span>
      </div>
    </div>

    <div class="usage-card">
      <div class="usage-title">方式三：{{模式名称}}</div>
      <div class="usage-row">
        <span class="usage-label">何时用</span>
        <span class="usage-value">{{何时使用的说明}}</span>
      </div>
      <div class="usage-row">
        <span class="usage-label">输入</span>
        <span class="usage-value">{{输入是什么}}</span>
      </div>
      <div class="usage-row">
        <span class="usage-label">输出</span>
        <span class="usage-value">{{预期输出是什么}}</span>
      </div>
    </div>
  </div>

  <h3 id="section-3-4">3.4 从需求到交付的一次完整链路</h3>
  <p>以一个具体需求为例：<strong>{{具体需求描述，如"为现有系统新增用户登录失败次数限制功能"}}</strong></p>

  <div class="timeline">
    <div class="timeline-step">
      <div class="step-title">① 用户提出目标</div>
      <div class="step-desc">{{描述用户输入什么，如何触发工具}}</div>
    </div>
    <div class="timeline-step">
      <div class="step-title">② {{阶段名，如"discuss 明确边界"}}</div>
      <div class="step-desc">{{这个阶段发生了什么，产出了什么}}</div>
    </div>
    <div class="timeline-step">
      <div class="step-title">③ {{阶段名，如"生成 Milestone / Slice / Task"}}</div>
      <div class="step-desc">{{这个阶段发生了什么，产出了什么}}</div>
    </div>
    <div class="timeline-step">
      <div class="step-title">④ {{阶段名，如"auto 执行 Task"}}</div>
      <div class="step-desc">{{这个阶段发生了什么，产出了什么}}</div>
    </div>
    <div class="timeline-step">
      <div class="step-title">⑤ {{阶段名，如"写入 SUMMARY / UAT / commit"}}</div>
      <div class="step-desc">{{这个阶段发生了什么，产出了什么}}</div>
    </div>
    <div class="timeline-step">
      <div class="step-title">⑥ {{阶段名，如"继续下一轮调度直至完成"}}</div>
      <div class="step-desc">{{最终产出是什么，用户看到了什么}}</div>
    </div>
  </div>

  <!-- Section 4: 系统详细设计 -->
  <h2 id="section-4">4 系统详细设计</h2>

  <h3 id="section-4-1">4.1 系统架构设计</h3>

  <h4 id="section-4-1-1">4.1.1 系统架构图</h4>

  <!-- Directory Tree: 先看目录全貌，再认识演员 -->
  <p>先从目录结构了解项目的整体轮廓（<code>★</code> 为关键入口文件，蓝色为目录）——</p>
  <div class="dir-tree">
    <pre><span class="tree-root">{{项目名}}/</span>
<span class="tree-branch">├── </span><span class="tree-dir">src/</span>                          <span class="tree-comment">← {{核心应用层一句话说明，如"启动引导、CLI 分发、扩展注册"}}</span>
<span class="tree-branch">│   ├── </span><span class="tree-entry">{{入口文件}} ★</span>            <span class="tree-comment">← {{该文件职责一句话}}</span>
<span class="tree-branch">│   ├── </span><span class="tree-entry">{{入口文件2}} ★</span>           <span class="tree-comment">← {{该文件职责一句话}}</span>
<span class="tree-branch">│   ├── </span><span class="tree-dir">{{子目录1}}/</span>               <span class="tree-comment">← {{子目录职责}}</span>
<span class="tree-branch">│   └── </span><span class="tree-dir">{{子目录2}}/</span>               <span class="tree-comment">← {{子目录职责}}</span>
<span class="tree-branch">├── </span><span class="tree-dir">{{顶层目录1}}/</span>                 <span class="tree-comment">← {{该目录职责}}</span>
<span class="tree-branch">│   ├── </span><span class="tree-dir">{{子包1}}/</span>                 <span class="tree-comment">← {{子包职责}}</span>
<span class="tree-branch">│   └── </span><span class="tree-dir">{{子包2}}/</span>                 <span class="tree-comment">← {{子包职责}}</span>
<span class="tree-branch">├── </span><span class="tree-dir">{{顶层目录2}}/</span>                 <span class="tree-comment">← {{该目录职责}}</span>
<span class="tree-branch">├── </span><span class="tree-dir">{{顶层目录3}}/</span>                 <span class="tree-comment">← {{该目录职责}}</span>
<span class="tree-branch">└── </span><span class="tree-dir">{{运行时目录}}/</span>                <span class="tree-comment">← {{运行时生成的状态目录说明}}</span></pre>
  </div>

  <!-- Cast of Characters: 再认识系统演员 -->
  <p>在看架构图之前，先认识一下系统里的主要"演员"——</p>
  <table class="cast-table">
    <thead>
      <tr><th>角色</th><th>代表文件</th><th>职责一句话</th><th>与谁交互</th></tr>
    </thead>
    <tbody>
      <!-- Agent: fill in all actors from Cast of Characters extraction (Phase 2.2), 5-10 rows -->
      <tr>
        <td>{{角色名 1}}</td>
        <td><code>{{文件路径}}</code></td>
        <td>{{从用户/行为视角描述：这个模块负责"做到什么"，而不是"读取什么文件"。Bad: "parses YAML". Good: "记住整个项目进行到哪阶段，是整个系统的唯一状态真相来源"}}</td>
        <td>{{直接协作者 1}}, {{直接协作者 2}}</td>
      </tr>
      <tr>
        <td>{{角色名 2}}</td>
        <td><code>{{文件路径}}</code></td>
        <td>{{职责一句话}}</td>
        <td>{{直接协作者}}</td>
      </tr>
      <tr>
        <td>{{角色名 3}}</td>
        <td><code>{{文件路径}}</code></td>
        <td>{{职责一句话}}</td>
        <td>{{直接协作者}}</td>
      </tr>
      <!-- Agent: 5-10 actors total -->
    </tbody>
  </table>

  <p>{{1-2 句简要架构介绍，说明整体分几层、核心流转是什么}}</p>

  <div class="diagram-container">
    <div class="mermaid">
graph TD
    %% 主链路: {{A}} → {{B}} → {{C}} → {{D}}
    subgraph 启动层
      A["{{actor}}\n{{role-hint}}"] --> B["{{actor}}\n{{role-hint}}"]
    end
    subgraph 执行层
      B --> C["{{actor}}\n{{role-hint}}"]
      C --> D["{{actor}}\n{{role-hint}}"]
    end
    subgraph 存储层
      C --> E[("{{actor}}\n{{role-hint}}")]
      D --> E
    end
    subgraph 扩展层
      B --> F["{{actor}}\n{{role-hint}}"]
    end
    </div>
    <div class="caption">图 4-1: 系统架构图（按职责层分组）</div>
  </div>

  <!-- How to read: 追踪主用户旅程 -->
  <p><strong>如何读这张图：</strong>{{从主链路注释中的第一个节点出发，用一段话沿箭头追踪一次完整的用户动作——例如"当用户运行 X 命令，首先经过 A（启动层），A 设置好环境后调用 B（引擎层），B 读取磁盘状态 E 决定下一步……"。要提到每个 subgraph 的名字和代表文件。}}</p>

  <h4 id="section-4-1-2">4.1.2 目录与核心文件说明</h4>

  <!-- Agent: group files by role, NOT alphabetical. Each group gets a rationale. -->

  <!-- 角色组 1 -->
  <p><strong>【{{角色层名，如"启动与引导层"}}】</strong>
    <code>★ {{入口文件}}</code>&ensp;·&ensp;<code>{{文件 2}}</code>
  </p>
  <p class="dir-rationale">{{为什么这组文件存在：从系统整体的角度解释这层的存在意义，而不是描述文件内容。例如："负责在 SDK 代码求值前设置所有环境变量，使得品牌名称和状态目录在整个生命周期内保持一致。没有这层，SDK 的配置读取时序会错乱。"}}</p>
  <table class="dir-table">
    <thead><tr><th>文件</th><th>职责</th></tr></thead>
    <tbody>
      <tr><td>★ {{文件路径}}</td><td>{{职责说明}}</td></tr>
      <tr><td>{{文件路径}}</td><td>{{职责说明}}</td></tr>
    </tbody>
  </table>

  <!-- 角色组 2 -->
  <p><strong>【{{角色层名，如"工作流引擎层"}}】</strong>
    <code>★ {{入口文件}}</code>&ensp;·&ensp;<code>{{文件 2}}</code>&ensp;·&ensp;<code>{{文件 3}}</code>
  </p>
  <p class="dir-rationale">{{为什么这组文件存在}}</p>
  <table class="dir-table">
    <thead><tr><th>文件</th><th>职责</th></tr></thead>
    <tbody>
      <tr><td>★ {{文件路径}}</td><td>{{职责说明}}</td></tr>
      <tr><td>{{文件路径}}</td><td>{{职责说明}}</td></tr>
      <tr><td>{{文件路径}}</td><td>{{职责说明}}</td></tr>
    </tbody>
  </table>

  <!-- 角色组 3 -->
  <p><strong>【{{角色层名，如"状态存储层"}}】</strong>
    <code>{{文件/目录}}</code>
  </p>
  <p class="dir-rationale">{{为什么这组文件存在}}</p>
  <table class="dir-table">
    <thead><tr><th>文件/目录</th><th>职责</th></tr></thead>
    <tbody>
      <tr><td>{{路径}}</td><td>{{职责说明}}</td></tr>
    </tbody>
  </table>

  <!-- Agent: add more 角色组 as needed. Typical groups: 启动与引导/工作流引擎/状态存储/原生性能层/能力扩展/构建与工具 -->

  <h3 id="section-4-2">4.2 关键模块实现细节</h3>

  <!-- Module 1 -->
  <h4 id="section-4-2-1">4.2.1 关键模块一：{{模块名}}</h4>
  <div class="module-card">
    <div class="module-meta">
      <span>📁 {{file-path}}</span>
      <span>🏷️ {{设计模式}}</span>
    </div>
    <p>{{简要介绍：职责、实现思路、为什么这样设计}}</p>

    <div class="diagram-container">
      <div class="mermaid">
sequenceDiagram
    participant A as 组件A
    participant B as 组件B
    participant C as 组件C
    A->>B: 请求
    B->>C: 处理
    C-->>B: 结果
    B-->>A: 响应
      </div>
      <div class="caption">图 4-2: {{模块名}} 时序图</div>
    </div>

    <pre><code class="language-typescript">// 文件: path/to/file.ts (line X-Y)
// {{核心代码 + 中文注释}}
</code></pre>
  </div>

  <!-- Module 2 — same structure -->
  <!-- Module 3 — same structure -->

  <!-- Section 5: 方案评价 -->
  <h2 id="section-5">5 方案评价</h2>

  <div class="eval-grid">
    <div class="eval-card strength">
      <h4>5.1 优势分析</h4>
      <ul>
        <li>{{优势 1}}</li>
        <li>{{优势 2}}</li>
        <li>{{优势 3}}</li>
      </ul>
    </div>
    <div class="eval-card weakness">
      <h4>5.2 局限分析</h4>
      <ul>
        <li>{{局限 1}}</li>
        <li>{{局限 2}}</li>
        <li>{{局限 3}}</li>
      </ul>
    </div>
  </div>

</main>

<!-- ─── Interactive Panel System ─────────────────────────────────────────── -->
<!-- Agent: inject this block as-is; fill FILE_MAP and PROJECT_TERMS below   -->
<div class="code-backdrop" id="codeBackdrop"></div>
<div class="code-panel" id="codePanel">
  <div class="panel-resize-grip" id="panelResizeGrip"></div>
  <div class="code-panel-header">
    <button class="panel-nav-btn" id="panelNavBack" title="后退" disabled>‹</button>
    <button class="panel-nav-btn" id="panelNavFwd"  title="前进" disabled>›</button>
    <span class="code-panel-path" id="codePanelPath"></span>
    <span class="code-panel-badge" id="codePanelBadge"></span>
    <a id="panelGhLink" class="panel-gh-link" href="#" target="_blank" rel="noopener" style="display:none">⤢ 在浏览器中打开</a>
    <button class="panel-pin-btn" id="panelPin" title="固定面板">📌</button>
    <button class="code-panel-close" id="codePanelClose" title="关闭 (Esc)">×</button>
  </div>
  <div id="iframePanelBody">
    <div class="iframe-loading" id="iframeLoader">
      <div class="iframe-spin"></div>
      <span>正在加载 GitHub 页面…</span>
    </div>
    <iframe id="githubIframe" src="" title="GitHub"
            sandbox="allow-scripts allow-same-origin allow-forms allow-popups"></iframe>
  </div>
  <div id="termPanelBody"></div>
</div>

<script>
  mermaid.initialize({
    startOnLoad: true,
    theme: 'base',
    themeVariables: {
      primaryColor: '#dbeafe',
      primaryBorderColor: '#2563eb',
      primaryTextColor: '#1a1a2e',
      lineColor: '#6b7280',
      secondaryColor: '#f0fdf4',
      tertiaryColor: '#f8f9fa',
      fontFamily: '-apple-system, BlinkMacSystemFont, PingFang SC, sans-serif',
      fontSize: '14px'
    },
    sequence: {
      diagramMarginX: 20,
      diagramMarginY: 20,
      actorMargin: 60,
      mirrorActors: false,
      messageAlign: 'center'
    },
    flowchart: {
      curve: 'basis',
      padding: 20
    }
  });

  hljs.highlightAll();

  // TOC active highlight on scroll
  const tocLinks = document.querySelectorAll('.toc a');
  const sections = document.querySelectorAll('h2[id], h3[id], h4[id]');
  window.addEventListener('scroll', () => {
    let current = '';
    sections.forEach(s => {
      if (window.scrollY >= s.offsetTop - 100) current = s.id;
    });
    tocLinks.forEach(a => {
      a.classList.toggle('active', a.getAttribute('href') === '#' + current);
    });
  });
</script>

<script>
/* ─── Interactive Panel JS ────────────────────────────────────────────────
   Agent: replace FILE_MAP and PROJECT_TERMS with project-specific data.
   GH_REPO must be the full GitHub repo path, e.g. "owner/repo-name".
   GH_BRANCH is usually "main".
──────────────────────────────────────────────────────────────────────────── */
(function () {
  'use strict';

  // ── FILE_MAP: Agent fills this in ────────────────────────────────────────
  // Keys = all text variants that appear inside <code> tags in the document
  // Values = full blob path relative to repo root (no leading slash)
  // Examples:
  //   'src/loader.ts'        → 'src/loader.ts'
  //   '★ src/loader.ts'     → 'src/loader.ts'   (handle star prefix)
  //   'auto-dispatch.ts'     → 'src/resources/extensions/gsd/auto-dispatch.ts'
  const FILE_MAP = {
    // {{Agent: fill in all <code> filename mentions found in the document}}
    // 'short-or-full-name': 'full/path/in/repo.ts',
  };

  // ── PROJECT_TERMS: Agent fills this in ───────────────────────────────────
  // 8-12 key technical terms introduced in the report.
  // 'patterns' = text substrings to auto-tag in the document body.
  // Sort patterns longest-first within each entry for correct matching.
  const PROJECT_TERMS = {
    // '{{Term Name}}': {
    //   tags: ['tag1', 'tag2'],
    //   def: '{{HTML string, 2-3 sentences, may use <code> and <b>}}',
    //   mechanics: ['{{detail 1}}', '{{detail 2}}', '{{detail 3}}'],
    //   related: ['{{Other Term in this object}}'],
    //   files: ['{{short key that exists in FILE_MAP}}'],
    //   patterns: ['{{longest pattern first}}', '{{shorter alias}}'],
    // },
  };

  // ── GitHub repo config ───────────────────────────────────────────────────
  const GH_REPO   = '{{owner}}/{{repo}}';  // Agent: fill in
  const GH_BRANCH = 'main';
  const GH1S_BASE = 'https://github1s.com/' + GH_REPO;
  const GH_BASE   = 'https://github.com/'   + GH_REPO;

  // ── DOM refs ─────────────────────────────────────────────────────────────
  const backdrop     = document.getElementById('codeBackdrop');
  const panel        = document.getElementById('codePanel');
  const closeBtn     = document.getElementById('codePanelClose');
  const pathEl       = document.getElementById('codePanelPath');
  const badgeEl      = document.getElementById('codePanelBadge');
  const ghLink       = document.getElementById('panelGhLink');
  const iframeBody   = document.getElementById('iframePanelBody');
  const iframeLoader = document.getElementById('iframeLoader');
  const iframeEl     = document.getElementById('githubIframe');
  const termBody     = document.getElementById('termPanelBody');
  const resizeGrip   = document.getElementById('panelResizeGrip');
  const navBack      = document.getElementById('panelNavBack');
  const navFwd       = document.getElementById('panelNavFwd');
  const pinBtn       = document.getElementById('panelPin');

  // ── Navigation history ──────────────────────────────────────────────────
  const navHistory = [];
  let navIndex = -1;
  function updateNavBtns() {
    navBack.disabled = navIndex <= 0;
    navFwd.disabled  = navIndex >= navHistory.length - 1;
  }
  function pushNav(entry) {
    navHistory.splice(navIndex + 1);
    navHistory.push(entry);
    navIndex = navHistory.length - 1;
    updateNavBtns();
  }
  navBack.addEventListener('click', () => {
    if (navIndex > 0) { navIndex--; _loadEntry(navHistory[navIndex]); updateNavBtns(); }
  });
  navFwd.addEventListener('click', () => {
    if (navIndex < navHistory.length - 1) { navIndex++; _loadEntry(navHistory[navIndex]); updateNavBtns(); }
  });
  function _loadEntry(e) {
    if (e.type === 'github') _openGitHubPanel(e.path, e.label);
    else if (e.type === 'term') _openTermPanel(e.key);
  }

  // ── Pin state ───────────────────────────────────────────────────────────
  let pinned = false;
  pinBtn.addEventListener('click', () => {
    pinned = !pinned;
    pinBtn.classList.toggle('active', pinned);
    pinBtn.title = pinned ? '取消固定' : '固定面板';
    panel.classList.toggle('pinned', pinned);
    if (pinned) {
      backdrop.classList.remove('open');
      document.body.style.overflow = '';
    } else if (panel.classList.contains('open')) {
      backdrop.classList.add('open');
      document.body.style.overflow = 'hidden';
    }
  });

  // ── Panel open / close ───────────────────────────────────────────────────
  function openPanel() {
    panel.classList.add('open');
    if (!pinned) { backdrop.classList.add('open'); document.body.style.overflow = 'hidden'; }
  }
  function _doClose() {
    panel.classList.remove('open');
    backdrop.classList.remove('open');
    document.body.style.overflow = '';
    if (iframeEl) { iframeEl.src = ''; iframeEl.style.display = 'none'; }
    iframeBody.style.display = 'none';
    termBody.style.display   = 'none';
    navHistory.length = 0; navIndex = -1; updateNavBtns();
  }
  function closePanel() { if (!pinned) _doClose(); }
  function forceClose()  {
    pinned = false; pinBtn.classList.remove('active'); pinBtn.title = '固定面板';
    panel.classList.remove('pinned'); _doClose();
  }
  closeBtn.addEventListener('click', forceClose);
  backdrop.addEventListener('click', closePanel);
  document.addEventListener('keydown', e => { if (e.key === 'Escape') forceClose(); });

  // ── Resize (drag left edge) ───────────────────────────────────────────────
  // Overlay prevents the iframe from swallowing mousemove/mouseup during drag
  let resizing = false, rsX = 0, rsW = 0, rsOverlay = null;
  resizeGrip.addEventListener('mousedown', e => {
    resizing = true; rsX = e.clientX; rsW = panel.offsetWidth;
    resizeGrip.classList.add('dragging');
    document.body.style.userSelect = 'none';
    document.body.style.cursor = 'ew-resize';
    rsOverlay = document.createElement('div');
    rsOverlay.style.cssText = 'position:absolute;inset:0;z-index:9999;cursor:ew-resize;';
    panel.appendChild(rsOverlay);
    e.preventDefault();
  });
  document.addEventListener('mousemove', e => {
    if (!resizing) return;
    const newW = Math.max(300, Math.min(window.innerWidth * 0.96, rsW + (rsX - e.clientX)));
    panel.style.width = newW + 'px';
    panel.style.transition = 'none';
  });
  document.addEventListener('mouseup', () => {
    if (!resizing) return;
    resizing = false;
    resizeGrip.classList.remove('dragging');
    document.body.style.userSelect = '';
    document.body.style.cursor = '';
    panel.style.transition = '';
    if (rsOverlay) { rsOverlay.remove(); rsOverlay = null; }
  });

  // ── Open github1s iframe ─────────────────────────────────────────────────
  function _openGitHubPanel(ghPath, label) {
    panel.dataset.type   = 'github';
    pathEl.textContent   = label || ghPath;
    badgeEl.textContent  = 'github1s';
    ghLink.href          = GH_BASE + '/blob/' + GH_BRANCH + '/' + ghPath;
    ghLink.style.display = '';
    iframeBody.style.cssText = 'display:flex;flex-direction:column;flex:1;overflow:hidden;';
    termBody.style.display   = 'none';
    iframeLoader.style.display = 'flex';
    iframeLoader.innerHTML = '<div class="iframe-spin"></div><span>正在加载 GitHub 页面…</span>';
    iframeEl.style.display = 'none';
    iframeEl.src = '';
    openPanel();
    const tid = setTimeout(() => {
      if (iframeEl.style.display === 'none') {
        iframeLoader.innerHTML =
          '<div style="text-align:center;padding:24px">' +
          '<p style="color:#f38ba8;margin-bottom:14px">⚠ github1s.com 加载超时</p>' +
          '<a href="' + ghLink.href + '" target="_blank" rel="noopener" ' +
          'style="color:#89b4fa;font-size:13px;text-decoration:none;border:1px solid #89b4fa44;padding:4px 12px;border-radius:4px">' +
          '在新标签页中打开 →</a></div>';
      }
    }, 10000);
    setTimeout(() => {
      iframeEl.onload = () => {
        clearTimeout(tid);
        iframeLoader.style.display = 'none';
        iframeEl.style.flex = '1';
        iframeEl.style.display = 'block';
      };
      iframeEl.src = GH1S_BASE + '/blob/' + GH_BRANCH + '/' + ghPath;
    }, 60);
  }
  function openGitHubPanel(ghPath, label) {
    pushNav({ type: 'github', path: ghPath, label: label || ghPath });
    _openGitHubPanel(ghPath, label);
  }

  // ── Term panel ───────────────────────────────────────────────────────────
  function renderTermHTML(key) {
    const t = PROJECT_TERMS[key]; if (!t) return '';
    const tags  = (t.tags  || []).map(g => '<span class="term-tag">'  + g + '</span>').join('');
    const mech  = (t.mechanics || []).map(m => '<li>' + m + '</li>').join('');
    const rels  = (t.related || []).map(r => '<button class="term-rel-btn" data-term="' + r + '">' + r + '</button>').join('');
    const files = (t.files  || []).filter(f => FILE_MAP[f]).map(f =>
      '<button class="term-file-btn" data-file="' + FILE_MAP[f] + '">' + f + '</button>').join('');
    return '<div class="term-tags">' + tags + '</div>' +
      '<div class="term-title-h">' + key + '</div>' +
      '<div class="term-def">'  + t.def + '</div>' +
      (mech  ? '<div class="term-sec">实现机制</div><ul class="term-mechanics">' + mech + '</ul>' : '') +
      (rels  ? '<div class="term-sec">相关概念</div><div class="term-related-list">' + rels + '</div>' : '') +
      (files ? '<div class="term-sec" style="margin-top:14px">关键文件</div><div class="term-files-list">' + files + '</div>' : '');
  }
  function _openTermPanel(key) {
    panel.dataset.type   = 'term';
    pathEl.textContent   = key;
    badgeEl.textContent  = '术语解释';
    ghLink.style.display = 'none';
    iframeBody.style.display = 'none';
    termBody.style.cssText = 'display:flex;flex-direction:column;flex:1;overflow-y:auto;padding:20px 22px 32px;';
    termBody.innerHTML = renderTermHTML(key);
    termBody.querySelectorAll('.term-rel-btn[data-term]').forEach(btn =>
      btn.addEventListener('click', () => openTermPanel(btn.dataset.term)));
    termBody.querySelectorAll('.term-file-btn[data-file]').forEach(btn =>
      btn.addEventListener('click', () => openGitHubPanel(btn.dataset.file, btn.textContent.trim())));
    openPanel();
  }
  function openTermPanel(key) {
    if (!PROJECT_TERMS[key]) return;
    pushNav({ type: 'term', key });
    _openTermPanel(key);
  }

  // ── Unified click handler ────────────────────────────────────────────────
  document.addEventListener('click', function (e) {
    const fileEl = e.target.closest('[data-file]:not(.term-file-btn)');
    const termEl = e.target.closest('.term-link[data-term]');
    if (fileEl && fileEl.dataset.file) {
      e.preventDefault();
      openGitHubPanel(fileEl.dataset.file, fileEl.dataset.label || fileEl.dataset.file);
    } else if (termEl && PROJECT_TERMS[termEl.dataset.term]) {
      e.preventDefault();
      openTermPanel(termEl.dataset.term);
    }
  });

  // ── Auto-tag file links (inline <code>) ─────────────────────────────────
  document.querySelectorAll('code:not(pre code)').forEach(el => {
    if (el.dataset.file || el.classList.contains('file-link')) return;
    const raw = el.textContent.trim();
    const ghPath = FILE_MAP[raw];
    if (ghPath) {
      el.classList.add('file-link');
      el.dataset.file  = ghPath;
      el.dataset.label = raw.replace(/^★\s*/, '');
      el.title = '在 GitHub 中查看';
    }
  });

  // ── Auto-tag file links (table first-cell) ───────────────────────────────
  document.querySelectorAll('.cast-table td:first-child').forEach(td => {
    if (td.querySelector('.file-link')) return;
    const raw    = td.textContent.trim();
    const ghPath = FILE_MAP[raw];
    if (!ghPath) return;
    const label  = raw.replace(/^★\s*/, '');
    const btn    = document.createElement('span');
    btn.className    = 'file-link';
    btn.dataset.file  = ghPath;
    btn.dataset.label = label;
    btn.title         = '在 GitHub 中查看';
    btn.textContent   = raw;
    btn.style.cursor  = 'pointer';
    td.innerHTML = '';
    td.appendChild(btn);
  });

  // ── Auto-tag term links (TreeWalker) ─────────────────────────────────────
  const termPatterns = [];
  for (const [key, t] of Object.entries(PROJECT_TERMS)) {
    for (const pat of (t.patterns || [])) termPatterns.push({ pat, key });
  }
  termPatterns.sort((a, b) => b.pat.length - a.pat.length);
  if (termPatterns.length > 0) {
    const TERM_RE = new RegExp(
      termPatterns.map(({ pat }) => pat.replace(/[.*+?^${}()|[\]\\]/g, '\\$&')).join('|'), 'g');
    const PAT_KEY = Object.fromEntries(termPatterns.map(({ pat, key }) => [pat, key]));
    const SKIP_TAGS = new Set(['CODE','PRE','A','SCRIPT','STYLE','BUTTON',
      'H1','H2','H3','H4','TEXTAREA','INPUT','SELECT','TITLE']);
    const content = document.querySelector('.content');
    if (content) {
      const walker = document.createTreeWalker(content, NodeFilter.SHOW_TEXT, {
        acceptNode(node) {
          let p = node.parentNode;
          while (p && p !== content) {
            if (SKIP_TAGS.has(p.tagName) ||
                p.classList.contains('term-link') ||
                p.classList.contains('file-link') ||
                p.classList.contains('mermaid') ||
                p.classList.contains('diagram-container')) return NodeFilter.FILTER_REJECT;
            p = p.parentNode;
          }
          return NodeFilter.FILTER_ACCEPT;
        }
      });
      const nodes = []; let n;
      while ((n = walker.nextNode())) nodes.push(n);
      nodes.forEach(textNode => {
        const text = textNode.textContent;
        TERM_RE.lastIndex = 0;
        if (!TERM_RE.test(text)) return;
        TERM_RE.lastIndex = 0;
        const frag = document.createDocumentFragment();
        let last = 0, m;
        while ((m = TERM_RE.exec(text)) !== null) {
          if (m.index > last) frag.appendChild(document.createTextNode(text.slice(last, m.index)));
          const key = PAT_KEY[m[0]];
          if (key) {
            const sp = document.createElement('span');
            sp.className = 'term-link'; sp.dataset.term = key; sp.textContent = m[0];
            frag.appendChild(sp);
          } else {
            frag.appendChild(document.createTextNode(m[0]));
          }
          last = m.index + m[0].length;
        }
        if (last < text.length) frag.appendChild(document.createTextNode(text.slice(last)));
        textNode.parentNode.replaceChild(frag, textNode);
      });
    }
  }
})();

  // ── TOC collapse / expand ────────────────────────────────────────────────
  (function () {
    const toc     = document.getElementById('tocNav');
    const btn     = document.getElementById('tocToggle');
    const content = document.querySelector('.content');
    if (!toc || !btn) return;
    let collapsed = false;
    btn.addEventListener('click', () => {
      collapsed = !collapsed;
      toc.classList.toggle('collapsed', collapsed);
      content.classList.toggle('toc-collapsed', collapsed);
      btn.textContent = collapsed ? '▶' : '◀';
      btn.title       = collapsed ? '展开目录' : '收起目录';
    });
  })();

  // ── Diagram zoom (lightbox) ──────────────────────────────────────────────
  (function () {
    document.querySelectorAll('.diagram-container').forEach(container => {
      const zoomBtn = document.createElement('button');
      zoomBtn.className   = 'diagram-zoom-btn';
      zoomBtn.title       = '放大查看';
      zoomBtn.textContent = '⤢';
      container.appendChild(zoomBtn);

      function openLightbox() {
        const svg = container.querySelector('.mermaid svg');
        if (!svg) return;

        const rect = svg.getBoundingClientRect();
        const w = rect.width  * 1.5;
        const h = rect.height * 1.5;

        const lightbox = document.createElement('div');
        lightbox.className = 'diagram-lightbox';

        const inner = document.createElement('div');
        inner.className   = 'diagram-lightbox-inner';
        inner.style.width    = Math.min(w + 48, window.innerWidth  * 0.92) + 'px';
        inner.style.maxHeight = (window.innerHeight * 0.9) + 'px';
        inner.style.overflow = 'auto';
        inner.style.padding  = '20px';

        const cloneSvg = svg.cloneNode(true);
        cloneSvg.setAttribute('width',  w);
        cloneSvg.setAttribute('height', h);
        cloneSvg.style.display = 'block';

        function close() { lightbox.remove(); }
        lightbox.addEventListener('click', e => { if (e.target === lightbox) close(); });
        document.addEventListener('keydown', function esc(e) {
          if (e.key === 'Escape') { close(); document.removeEventListener('keydown', esc); }
        });

        inner.appendChild(cloneSvg);
        lightbox.appendChild(inner);
        document.body.appendChild(lightbox);
      }

      zoomBtn.addEventListener('click', e => { e.stopPropagation(); openLightbox(); });
      container.addEventListener('click', openLightbox);
    });
  })();
</script>
</body>
</html>
```

## Interactive Panel System

The panel system is injected at the bottom of every report. It provides two interactive features:

### File Links (blue dotted underline)
Two auto-tagging passes convert file names to interactive links:
1. **Inline `<code>` elements** — any `<code>` whose trimmed text matches a `FILE_MAP` key is tagged.
2. **Cast-table first column** — every `td:first-child` in `.cast-table` whose text exactly matches a `FILE_MAP` key is wrapped in a clickable `<span class="file-link">`.

Clicking either type opens the file in a github1s.com iframe on the right side. If github1s.com doesn't load within 10 seconds, a fallback "在新标签页中打开" link appears.

### Term Links (purple dotted underline)
Text matching any pattern in `PROJECT_TERMS[*].patterns` is auto-tagged as a term link. Clicking opens the term explanation panel showing: tags, definition, implementation mechanics, related terms (clickable), and key files (open github1s.com).

### Panel Resize
Drag the thin grip on the **left edge** of the panel to resize it. An overlay is injected during drag to prevent the iframe from swallowing mouse events.

### TOC Collapse
Clicking the `◀` button in the TOC header collapses the sidebar to 42 px wide; clicking `▶` expands it back. `.content` margin animates accordingly via the `toc-collapsed` class.

### Diagram Zoom (Lightbox)
Hovering a `.diagram-container` reveals a `⤢` button. Clicking it (or the container itself) opens a modal lightbox that renders the SVG at **1.5× its original size** by directly setting `width`/`height` attributes on the cloned SVG. Close by clicking the backdrop or pressing `Esc`.

### Agent Instructions

When filling in `FILE_MAP`:
- Include every unique string that appears in `<code>` elements in the report (cast-table, dir-table, inline code, module-meta spans)
- Include `★ filename` variants for entry-point files marked with the star prefix
- Map `gsd/*` shorthands used in dir-tables to their real `src/resources/extensions/...` paths

When filling in `PROJECT_TERMS`:
- Choose 8–12 terms that are **specific to this project's architecture** (not generic terms like "TypeScript" or "async/await")
- Write `patterns` arrays with longest pattern first; keep patterns specific enough not to create false positives in common text
- The `files` array values must exactly match keys present in `FILE_MAP`

## Template Usage Rules

1. **Replace all `{{...}}` placeholders** with actual content from the analysis
2. **TOC entries** must exactly match the sections in the content — add/remove entries for modules as needed
3. **Mermaid diagrams** are placed inside `<div class="diagram-container"><div class="mermaid">...</div></div>`
4. **Code blocks** use `<pre><code class="language-xxx">...</code></pre>`
5. **Module cards** wrap each 4.2.x module for visual separation
6. **Evaluation grid** uses the two-column layout for 优势 vs 局限
7. **fit-grid** (section 3.1) uses two-column layout for 适合 vs 不适合
8. **usage-grid** (section 3.3) uses auto-fit grid for mode cards
9. **timeline** (section 3.4) renders the end-to-end walkthrough as a vertical timeline
10. **cmd-flow** renders shell command sequences with comment styling
11. **Section 4.1.1 rules (STRICT)**:
    - `dir-tree` block must appear FIRST, before cast-table and before diagram — directory overview → actors → picture
    - `dir-tree` must use colored `<span>` tags: `.tree-root` for project root, `.tree-branch` for `├──/└──/│` chars, `.tree-dir` for directories, `.tree-entry` for ★ entry points, `.tree-comment` for `← description`
    - `cast-table` must appear AFTER dir-tree and BEFORE the Mermaid diagram
    - Mermaid architecture diagram MUST use `subgraph` clusters grouped by layer
    - First line of Mermaid MUST be `%% 主链路: A → B → C → D` naming the execution path
    - Every node label MUST be two-line: `["name\nrole-hint"]`
    - After the diagram, add a "如何读这张图" paragraph that traces the main user journey through named nodes
12. **Section 4.1.2 rules (STRICT)**:
    - Files MUST be grouped by role/layer — NOT alphabetical, NOT by directory depth
    - Entry point files MUST be marked with `★` in the first column
    - Each role group MUST have a `<p class="dir-rationale">` explaining WHY this group exists (not WHAT it contains)
    - Use one `dir-table` per role group, not one giant table
    - Typical groups: 启动与引导层 / 工作流引擎层 / 状态存储层 / 原生性能层 / 能力扩展层 / 构建与工具层
13. Section IDs follow the pattern: `section-1`, `section-1-1`, `section-4-1-1`, etc.
14. Mermaid and Highlight.js are loaded from CDN — the HTML is fully self-contained otherwise
15. Agent should **add or remove** 预期/理念/模块/步骤/角色组 subsections based on what the analysis actually finds — the template shows the minimum structure, not a rigid count
16. **Interactive Panel**: The panel JS block at the bottom is **mandatory**. Agent must fill in `FILE_MAP` and `PROJECT_TERMS` and set `GH_REPO`. Do NOT leave placeholder comments in the final output — replace every `{{...}}` with real values.
17. **No directory links**: Only files get `data-file` attributes. Do NOT add `data-dir` attributes or make directory names clickable — directories have no interactive behavior.
