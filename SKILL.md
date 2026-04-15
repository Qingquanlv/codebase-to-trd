---
name: codebase-to-trd
description: >-
  Deep-dive into a code repository to understand its architecture, core
  implementation logic, and design decisions; produce a self-contained HTML
  report with Mermaid diagrams, interactive file links (github1s.com iframe),
  and auto-tagged term explanations. Use when the user wants to learn a
  codebase, understand how code works, trace implementation reasoning, or asks
  "help me understand this repo / this code / how this works." Also use when
  the user says: 学习仓库, 源码分析, 代码解读, 实现思路, understand codebase,
  code walkthrough, deep dive code.
---

# Codebase to TRD

Systematically learn a code repository and produce a **self-contained HTML report** with Mermaid diagrams, clickable file links (github1s.com), and interactive term explanations.

Match user's language: respond in Chinese if user writes Chinese, English if English.

## Workflow

```
Phase 1: Reconnaissance        → project profile & tech stack
Phase 2: Architecture Mapping  → actors, layers, data flow, diagrams
Phase 3: Core Logic Deep-Dive  → implementation reasoning for key modules
Phase 4: HTML Report           → assemble into final HTML with interactive panel
```

For the HTML template and interactive panel JS, see [references/html-template.md](references/html-template.md).

---

## Phase 1: Reconnaissance

Fast scan — do NOT read every file.

**Locate the repo:** GitHub URL → clone to `/tmp/deep-repo-learner-<name>` · "this repo" → cwd · local path → use directly

**Read in parallel:**
```
README.md, package.json / pyproject.toml / Cargo.toml / go.mod
ARCHITECTURE.md, docs/, .github/workflows/*
```

**Extract:**
- 项目名称 / 一句话定义 / 技术栈 / 目标用户
- Directory tree top-3 levels (skip node_modules, .git, dist, build, target, .next)

---

## Phase 2: Architecture Mapping

### 2.1 Cast of Characters

Extract **5–10 actors** — the main modules. Collapse minor utilities into their parent.

| Actor | 文件/目录 | 职责一句话 | 与谁交互 |

Rules:
- "职责一句话" = behavior perspective, not implementation: "记住整个项目当前阶段" not "reads state.json"
- "与谁交互" = direct collaborators only (1–3 items)

### 2.2 Architecture Diagram

Use Cast of Characters to build a `graph TD` Mermaid diagram:
- **Mandatory `subgraph` clusters** grouped by layer (启动层/执行层/存储层/扩展层…)
- First line must be `%% 主链路: A → B → C → D`
- Every node label must be two-line: `["name\nrole-hint"]`
- 5–12 nodes, 2–5 subgraphs

### 2.3 Directory Role Mapping

Group top-level dirs and key files by **system role** (not alphabetical). For each group:
- Role label (启动与引导 / 工作流引擎 / 状态存储 / 原生性能层 / 能力扩展)
- Entry point marked with ★
- One "why this group exists" sentence

### 2.4 Competitive Landscape

Search 2–3 similar tools. Comparison table: `| 工具 | 定位 | 优势 | 劣势 |`. Conclude: where does this project win/lose?

---

## Phase 3: Core Objects, Usage & Module Logic

### 3.1 Core Objects & Terms

Before writing the HTML, identify the project's core domain objects. For each object:
1. Find the **TypeScript interface / SQL DDL** that defines it (fetch from raw GitHub if needed)
2. Map to a source file (`FILE_MAP` key)
3. List all fields with Chinese explanations

This becomes §3 in the report.

### 3.2 Usage Pattern Analysis

- 适合/不适合任务类型 → §2.4 fit-grid
- 一次完整链路: concrete end-to-end example with ≥5 steps → §4.1 timeline
- 最小可运行路径: install → init → first task → check results → §4.2 cmd-flow
- 2–3 核心使用模式 → §4.3 usage-grid

### 3.3 Core Module Deep-Dive

Select **3–5 modules** central to the project's value proposition with non-trivial logic.

For each module, use the **six-part structure** (this is the exact §5.3 template):

```
1. module-meta div         → 文件路径 + 层级标签 + 触发时机
2. 要解决的问题            → why this module exists (1–2 paragraphs)
3. 解决方案                → core design decision (1–2 paragraphs)
4. Mermaid sequenceDiagram → key interactions, labeled participants
5. seq-table               → numbered correspondence table aligned to diagram
   (columns: # | 发起方 | 发起方所在文件 | 接收方 | 消息/返回 | 本步说明)
6. 代价与局限              → honest tradeoffs (1 paragraph)
```

**seq-table rules:**
- Every row corresponds to one message arrow in the sequence diagram
- "发起方所在文件" must be a clickable file-link where a source file exists
- Self-calls and Note annotations: mark "(内部)" in 接收方 column
- Include alt/loop branches as separate rows with "alt 分支" notation

For projects without executable code (e.g., pure Markdown skill frameworks), omit code snippets; sequenceDiagram + seq-table are sufficient.

---

## Phase 4: HTML Report

### 4.1 Report Structure

Strictly follow this section order:

```
主标题 / 副标题 / 导语 / 核心结论
  导语写法（三句话结构）：
    句1 是什么 — 一句话定义工具类型与核心能力
    句2 怎么做 — 核心机制/工作方式（≤20字，不写数据流箭头链或文件引用）
    句3 用什么方式用 — 主要使用入口/模式（CLI / SDK / 插件等）
  禁止：数据流箭头链（A → B → C → …）、版本/文件引用（见 pyproject.toml 等）
1 摘要
  1.1 项目背景
  1.2 能解决的问题 (1.2.1–1.2.x 每个痛点独立 h4，标题格式：「痛点描述」——解决方案简述)
2 项目设计理念
  2.1–2.3  核心理念 + 竞品对比
  2.4  当前适合解决什么问题 (fit-grid: 适合 / 不适合两栏卡片)
3 核心对象与术语          ← 必须包含
  3.1 术语表              ← arch-read-table，术语列用 term-link / file-link
  3.2 对象之间的关系      ← 3列表：对象 | 定义 | 分属模块
                            + 每个对象的 schema 字段表 (schema-table)
  3.3 状态存储载体        ← 各存储层各自职责（见 §4.4 规则）
  3.4 常见误解            ← 列举 3–5 条针对本项目的错误认知
4 项目使用方式
  4.1 从需求到交付的一次完整过程  ← timeline 6步（含具体示例命令）
  4.2 最小可运行路径              ← cmd-flow + mermaid LR
  4.3 核心使用模式                ← usage-grid (Step / Auto / Headless)
5 系统详细设计
  5.1 系统架构设计
    5.1.1 架构图 (dir-tree → graph TD → arch-read-table "如何读这张图")
  5.2 关键执行链路 (sequenceDiagram + chain-table 编号对照表)
  5.3 关键模块实现细节 (3–5 个模块，每个：module-meta → 问题 → 解决方案
                        → sequenceDiagram → seq-table → 代价)
6 方案评价
  6.1 优势分析 / 6.2 局限分析  ← eval-grid 卡片
7 版本化维护区            ← 必须包含
  7.1 本文观察基线        ← 仓库 URL / 分支 / 日期
```

Full CSS + HTML components are in [references/html-template.md](references/html-template.md).
HTML snippets for individual sections are in [references/section-snippets.md](references/section-snippets.md).

### 4.2 Diagrams

| Diagram | Type | Location | Key rules |
|---------|------|----------|-----------|
| 架构图 (5.1.1) | `graph TD` | §5.1.1 | `subgraph` required; `%% 主链路:` comment; two-line node labels |
| 执行链路 (5.2) | `sequenceDiagram` | §5.2 | Full end-to-end flow; **must be followed by chain-table** |
| 模块时序 (5.3.x) | `sequenceDiagram` | §5.3 per module | One per module; **must be followed by seq-table** |
| 最小路径 (4.2) | `graph LR` | §4.2 | 4–8 nodes; user journey only |

All diagrams: wrapped in `<div class="diagram-container"><div class="mermaid">…</div></div>`.

**Figure numbering:** §5.1.1 = "图 5-1", §5.2 = "图 5-2", §5.3 modules = "图 5-3", "图 5-4", etc.

**chain-table** (§5.2):
- Columns: `# | 阶段 | 技能/函数/动作 | 所在文件 | 作用简述 | 关键约束`
- One row per diagram message/action; CSS class `.chain-table`
- "所在文件" must be clickable file-links

**seq-table** (§5.3 per module):
- Columns: `# | 发起方 | 发起方所在文件 | 接收方 | 消息/返回 | 本步说明`
- Maps 1:1 to diagram arrows; CSS class `.seq-table`
- Introduce with `<p class="seq-table-caption"><strong>时序对照表</strong>（…）</p>`

### 4.3 Interactive Panel System

Every report **must** include the panel system from [references/html-template.md](references/html-template.md). Agent fills in two project-specific objects:

**`FILE_MAP`** — maps every `<code>` text variant in the document → full GitHub blob path:
- Include short keys used in dir-tables (e.g., `'auto-dispatch.ts'`)
- Include `★ filename` star-prefixed variants

**`PROJECT_TERMS`** — 8–12 architectural terms specific to this project:
```
'Term Name': {
  tags, def (HTML), mechanics (HTML array), related (term keys), files (FILE_MAP keys), patterns (text to auto-tag)
}
```
- Choose terms specific to this project's architecture — not generic programming concepts
- `patterns` = text substrings that appear in the document body, longest first
- `files` values must exist as keys in `FILE_MAP`

### 4.4 Section Rules

**§3.1 术语表**
- 术语列：若有对应源文件 → `<span class="file-link" data-file="…">Term</span>`；若有 PROJECT_TERMS 条目 → `<span class="term-link" data-term="…">Term</span>`。不得使用纯文本 `<strong>`
- 覆盖：项目核心对象 + 关键配置项 + 运行时概念

**§3.2 对象之间的关系**
- 上方：3 列表 `对象 | 定义 | 分属模块`（分属模块列用 file-link）
- 下方：每个核心对象一张 schema-table（字段 | 类型 | 含义）
  - 字段数据**必须**来自真实源码（TypeScript interface / SQL DDL），非推断
  - 拉取路径：`https://raw.githubusercontent.com/<org>/<repo>/main/<path>`
- HTML snippet → see [references/section-snippets.md](references/section-snippets.md)

**§3.3 状态存储载体**
- 有持久化数据库的项目：分 Markdown / SQLite / runtime 三类
- 无运行时 DB 的项目（如纯 Markdown 框架）：按实际层次（Git / 文档文件 / 会话临时状态），表格对比各层持久性与职责
- 关键：明确"谁是状态真相来源"

**§3.4 常见误解**
- 3–5 条，每条针对本项目的具体特点（非通用编程误区）

**§4.1 End-to-End Timeline**
- Lead sentence (`.section-lead`): concrete realistic feature request as example
- 5–7 steps with `①②③…` in `.step-title`; inline `<code>` for commands, `<strong style="color:#6d28d9">` for key concepts

**§5.1.1 目录树引导语**
- One-line linked intro triggering `openRepoBrowser()` → see [references/section-snippets.md](references/section-snippets.md)

**§7.1 本文观察基线**
- Short: repo URL + branch + date only. No commit SHA needed.
- Pattern: `<p>本文…以开源仓库 <a href="…">…</a> 的 <strong>main</strong> 分支为参照；基线日期 <strong>YYYY-MM-DD</strong>。</p>`

### 4.5 Output & Delivery

Write the final HTML to **`example/<project-name>-deep-dive.html`** (same directory for splice fragments: `example/_<project>_maps.js`, `example/_<project>_nav_main.html`, `example/_splice_<project>.py` when used). The repo root keeps **`SKILL.md`** and **`references/`** only; generated reports and rebuild scripts live under **`example/`**.

Present:
1. HTML file path (e.g. `example/foo-deep-dive.html`)
2. "这个项目的核心是 [X]，最值得学习的是 [Y]"
3. Suggested next step

### 4.6 Quality Checklist

**Content:**
- [ ] 导语遵循「是什么 → 怎么做 → 用什么方式用」三句话结构，无数据流箭头链（A → B → C）、无文件引用
- [ ] 导语 and 核心结论 are substantive, not generic
- [ ] 设计理念 extracted from real code patterns (not platitudes)
- [ ] §2.3 has a competitive comparison table (2–3 alternatives)
- [ ] §2.4 fit-grid covers both 适合 and 不适合 task types
- [ ] §4.1 timeline traces a concrete end-to-end example with ≥5 steps
- [ ] 优势分析 and 局限分析 are honest and specific

**Chapter 3 — Core Objects & Terms:**
- [ ] §3.1: all term-column cells are clickable (file-link or term-link)
- [ ] §3.2: 3-column relationship table present
- [ ] §3.2: schema-table for each core object, fields sourced from actual code
- [ ] §3.3: storage layers explained with "source of truth" identified
- [ ] §3.4: ≥3 project-specific misconceptions listed

**Chapter 5 — System Design:**
- [ ] §5.1.1 order: dir-tree → `graph TD` → `arch-read-table` (4 cols: 层级/核心文件/核心职责/主要交互)
- [ ] §5.2: sequenceDiagram followed by `chain-table` (6 cols)
- [ ] §5.3: each module has module-meta → 问题 → 解决方案 → sequenceDiagram → seq-table → 代价
- [ ] Figure numbers match section (5.1.1 = "图 5-1", 5.2 = "图 5-2", 5.3.x = "图 5-3/5-4/…")

**Chapter 7 — Versioned Maintenance:**
- [ ] §7.1 present with repo URL / branch / date

**Diagrams:**
- [ ] Architecture diagram: `subgraph` clusters (flat graph = failure)
- [ ] Architecture diagram: `%% 主链路:` comment + two-line node labels
- [ ] "如何读这张图" arch-read-table traces main user journey

**Interactive Panel:**
- [ ] FILE_MAP covers all `<code>` filenames in dir-table and schema-table cells
- [ ] PROJECT_TERMS has 8–12 entries with `patterns` matching text in the report
- [ ] `GH_REPO` is set correctly (no placeholder left)

---

## Step 5: Self-verify before output

Before writing the final HTML file, run every check below. **If any item fails, fix it first — do not output a broken report.**

### 5.1 文章骨架完整性

- [ ] 报告包含所有必要一级/二级节（参考 §4.1 Report Structure 中的完整列表）：`1 摘要 → 1.1 项目背景 → 1.2 能解决的问题` / `2 设计理念` / `3 核心对象与术语 → 3.1–3.4` / `4 使用方式 → 4.1–4.3` / `5 系统详细设计 → 5.1–5.3` / `6 方案评价` / `7 版本化维护区`
- [ ] `1.2` 的标题为「能解决的问题」（非「项目预期」），各子节标题以「痛点」——「解决方案」格式呈现，内容围绕项目解决了哪些具体问题展开，而非学习目标
- [ ] 没有任何占位文本（`TODO`、`[待补充]`、`…` 等）残留在正文中
- [ ] 所有 `<a href="#section-x-x">§x.x</a>` 内部锚点都有对应的 `id="section-x-x"` 目标

### 5.2 §5.1.1 三件套检查

- [ ] **目录结构**：`<div class="dir-tree"><pre>…</pre></div>` 存在，且关键文件标注了 `★` 并带 `data-file` 属性（鼠标悬停可点击跳转）
- [ ] **架构图**：`graph TD` Mermaid 图存在，包含 `subgraph` 分层、`%% 主链路:` 注释、双行节点标签
- [ ] **如何读这张图**：`arch-read-table`（4 列：层级 / 核心模块 / 核心职责 / 主要交互）紧跟架构图之后

### 5.3 §5.2 调用链图 + 表

- [ ] 至少一张 `sequenceDiagram`（完整端到端链路，参与者有中文标注）
- [ ] 图后紧跟 `chain-table`（6 列：`# | 阶段 | 函数/类 | 所在模块 | 作用简述 | 关键约束`），行数与图中消息箭头 1:1 对应
- [ ] `chain-table` 的"所在模块"列中，凡有源文件的条目均为可点击的 `file-link`

### 5.4 §5.3 时序图 + 表

- [ ] 每个关键模块（3–5 个）各有一张 `sequenceDiagram`（局部机制，非重复整体链路）
- [ ] 每张图后紧跟 `seq-table`（6 列：`# | 发起方 | 发起方所在文件 | 接收方 | 消息/返回 | 本步说明`），以 `<p class="seq-table-caption">…</p>` 引导
- [ ] `seq-table` 的"发起方所在文件"列有 `file-link`，指向真实源文件路径

### 5.5 所有图表包含放大/缩小功能

每个 `<div class="diagram-container">` 必须满足：

- [ ] CSS 中 `.diagram-container` 设置了 `position: relative; cursor: zoom-in`
- [ ] CSS 中存在 `.diagram-zoom-btn`（绝对定位在右上角，默认 `opacity:0`，hover 时 `opacity:1`）和 `.diagram-lightbox` / `.diagram-lightbox-inner` 样式
- [ ] JS 中存在图表放大逻辑：遍历所有 `.diagram-container`，动态插入 `⤢` 按钮；点击按钮或图表本身触发 `openLightbox()`，克隆 SVG 并放大 1.5× 后渲染到 lightbox；点击遮罩层或按 Escape 关闭

验证模板（直接在输出的 HTML 中搜索以下字符串，必须全部存在）：
```
diagram-zoom-btn
diagram-lightbox
openLightbox
```

### 5.6 术语/代码文件弹出框 + 前进后退

交互面板 JS 必须通过以下所有检查：

**文件链接行为**
- [ ] `_openGitHubPanel(ghPath, label)` 函数体内，在设置 `iframeBody.style.display = 'flex'` 的同时，有 `termBody.style.display = 'none'`（否则从术语面板跳转到代码文件时术语内容不会消失）

**术语面板行为**
- [ ] 存在 `_openTermPanelInternal(termName)` 函数（纯渲染，不写历史）
- [ ] 存在 `openTermPanel(termName)` 函数（先调用 `pushNav({ type: 'term', name: termName })`，再调用 `_openTermPanelInternal`）

**前进/后退按钮**
- [ ] `openGitHubPanel` 调用 `pushNav({ type: 'github', path, label })`（必须有 `type` 字段）
- [ ] 存在 `_loadEntry(e)` 函数，按 `e.type` 分发：`'github'` → `_openGitHubPanel`，`'term'` → `_openTermPanelInternal`
- [ ] `panelNavBack` / `panelNavFwd` 的 click handler 调用 `_loadEntry(navHistory[navIndex])`，而非硬编码 `_openGitHubPanel`

验证模板（在输出的 HTML 中搜索以下字符串，必须全部存在）：
```
termBody.style.display = 'none'      ← 在 _openGitHubPanel 内
_openTermPanelInternal
pushNav({ type: 'term'
pushNav({ type: 'github'
_loadEntry
```

### 5.7 修复后重验

If any check above fails → fix → re-run the full checklist from 5.1.  
Do **not** output the HTML until all 5.1–5.6 pass.

---

## Adapting to User Preferences

| User says | Adjustment |
|-----------|-----------|
| "快速了解" | Sections 1 + 2 + 4.1 + 5 only |
| "我是新手" | Expand section 3; add more concept explanations |
| "关注性能" | Add performance analysis in section 5 |
| "关注安全" | Add security audit perspective in section 5 |
| Specifies a module | Phase 3.3 focuses on that module with doubled depth |
| Wants WeChat article | After analysis, invoke `repo-to-wechat-article` skill |

## Additional Resources

- HTML template + panel JS: [references/html-template.md](references/html-template.md)
- HTML snippets for sections: [references/section-snippets.md](references/section-snippets.md)
- Analysis patterns: [references/analysis-patterns.md](references/analysis-patterns.md)
