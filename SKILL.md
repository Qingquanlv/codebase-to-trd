---

## name: codebase-to-trd
description: >-
  Deep-dive into a code repository to understand its architecture, core
  implementation logic, and design decisions; produce a self-contained HTML
  report with Mermaid diagrams, interactive file links (github1s.com iframe),
  and auto-tagged term explanations. Use when the user wants to learn a
  codebase, understand how code works, trace implementation reasoning, or asks
  "help me understand this repo / this code / how this works." Also use when
  the user says: 学习仓库, 源码分析, 代码解读, 实现思路, understand codebase,
  code walkthrough, deep dive code.

# Codebase to TRD

Systematically learn a code repository and produce a **self-contained HTML report** with Mermaid diagrams, clickable file links (github1s.com), and interactive term explanations.

# Language

Language
Match user's language: Respond in the same language the user uses. If user writes in Chinese, respond in Chinese. If user writes in English, respond in English.

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

## Phase 3: Usage Pattern & Core Logic

### 3.1 Usage Pattern Analysis

- 适合的任务类型 / 不适合的任务类型 (user perspective, not project description)
- 最小可运行路径: install → init → first task → check results
- 2–3 核心使用模式: when / input / output
- 一次完整链路: concrete example end-to-end

### 3.2 Core Module Deep-Dive

Select **3–5 modules** central to the project's value proposition with non-trivial logic.

For each, use the **问题 → 方案 → 代价** framework:
1. 要解决的问题 (why this module exists)
2. 选择了什么、为什么 (core design decision)
3. 代价与局限 (what was sacrificed)

Include: core code snippet ≤25 lines + Mermaid sequence diagram.

---

## Phase 4: HTML Report

### 4.1 Report Structure

Strictly follow this section order:

```
主标题 / 副标题 / 导语 / 核心结论
1 摘要 → 1.1 项目背景 / 1.2 项目预期
2 设计理念 (2–5 个，含 2.3 竞品对比)
3 使用方式与执行链路
  3.1 适合/不适合 fit-grid
  3.2 最小路径 cmd-flow + mermaid LR
  3.3 核心使用模式 usage-grid
  3.4 完整链路 timeline
4 系统详细设计
  4.1.1 dir-tree → cast-table → 架构图 → 如何读这张图
  4.1.2 目录与核心文件 (grouped by role, dir-rationale per group)
  4.2.x 关键模块 module-card (3–5 个)
5 方案评价 eval-grid (优势 / 局限)
```

Full CSS + HTML components are in [references/html-template.md](references/html-template.md).

### 4.2 Diagrams

| Diagram | Type | Key rules |
|---------|------|-----------|
| 架构图 (4.1.1) | `graph TD` | `subgraph` required; `%% 主链路:` comment; two-line node labels |
| 最小路径 (3.2) | `graph LR` | 4–8 nodes; user journey only |
| 模块时序 (4.2.x) | `sequenceDiagram` | One per module; key interactions only |

All diagrams: wrapped in `<div class="diagram-container"><div class="mermaid">…</div></div>`.

### 4.3 Interactive Panel System

Every report **must** include the panel system from [references/html-template.md](references/html-template.md). Agent fills in two project-specific objects:

**`FILE_MAP`** — maps every `<code>` text variant in the document → full GitHub blob path:
- Include short keys used in dir-tables (e.g., `'auto-dispatch.ts'`)
- Include `★ filename` star-prefixed variants
- Map `gsd/*` shorthands to real `src/resources/extensions/…` paths

**`PROJECT_TERMS`** — 8–12 architectural terms specific to this project:
```
'Term Name': {
  tags, def (HTML), mechanics (HTML array), related (term keys), files (FILE_MAP keys), patterns (text to auto-tag)
}
```
- Choose terms specific to this project's architecture — not generic programming concepts
- `patterns` = text substrings that appear in the document body, longest first
- `files` values must exist as keys in `FILE_MAP`

**Behavior:**
- `<code>` text matching FILE_MAP → blue dotted **file link** → github1s.com iframe (10s timeout fallback)
- Text matching term patterns (outside `code/pre/a/h1-4`) → purple dotted **term link** → explanation panel
- No directory links — only files are interactive
- Panel is resizable by dragging the left-edge grip

### 4.4 Output & Delivery

```bash
# Write to:
<project-name>-deep-dive.html

# Open:
open <project-name>-deep-dive.html   # macOS
```

Present:
1. HTML file path
2. "这个项目的核心是 [X]，最值得学习的是 [Y]"
3. Suggested next step

### 4.5 Quality Checklist

**Content:**
- [ ] 导语 and 核心结论 are substantive, not generic
- [ ] 设计理念 extracted from real code patterns (not platitudes)
- [ ] Section 2.3 has a competitive comparison table (2–3 alternatives)
- [ ] Section 3.1 covers both 适合 and 不适合 task types
- [ ] Section 3.4 traces a concrete end-to-end example
- [ ] 优势分析 and 局限分析 are honest and specific
- [ ] Total: 4000–10000 Chinese characters

**Diagrams:**
- [ ] Architecture diagram: `subgraph` clusters (flat graph = failure)
- [ ] Architecture diagram: `%% 主链路:` comment + two-line node labels
- [ ] "如何读这张图" paragraph traces the main user journey through named nodes
- [ ] Section 3.2: one minimal flow diagram present

**Section 4.1:**
- [ ] Order is: dir-tree → cast-table → Mermaid diagram → "如何读这张图"
- [ ] Files grouped by role/layer, NOT alphabetical
- [ ] Entry point files marked ★
- [ ] Each role group has `dir-rationale` explaining why it exists

**Interactive Panel:**
- [ ] FILE_MAP covers all `<code>` filenames in cast-table and dir-table cells
- [ ] PROJECT_TERMS has 8–12 entries with `patterns` that match text in the report
- [ ] `GH_REPO` is set correctly (no placeholder left)
- [ ] Panel opens/closes; resize grip works both directions

---

## Adapting to User Preferences

| User says | Adjustment |
|-----------|-----------|
| "快速了解" | Sections 1 + 2 + 4.1.1 + 5 only |
| "我是新手" | Expand section 3; add more concept explanations |
| "关注性能" | Add performance analysis in section 5 |
| "关注安全" | Add security audit perspective in section 5 |
| Specifies a module | Phase 3.2 focuses on that module with doubled depth |
| Wants WeChat article | After analysis, invoke `repo-to-wechat-article` skill |

## Additional Resources

- HTML template + panel JS: [references/html-template.md](references/html-template.md)
- Analysis patterns: [references/analysis-patterns.md](references/analysis-patterns.md)
