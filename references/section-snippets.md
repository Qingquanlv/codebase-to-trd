# Section HTML Snippets

Reference file for HTML templates used in specific report sections.
The agent reads this file when it needs the exact markup for a section.

---

## §5.1.1 目录树引导语

One-line intro that opens the repo browser panel on click:

```html
<p>先从<a href="#" onclick="openRepoBrowser();return false;"
   style="color:#2563eb;font-weight:700;text-decoration:underline dotted;cursor:pointer"
   >目录结构</a>了解项目的整体轮廓。</p>
```

---

## §3.2 Schema Table

### HTML structure

```html
<p class="schema-obj-title">
  <span class="file-link" data-file="path/to/source.ts" data-label="source.ts">ObjectRow</span>
  — 对象描述
</p>
<table class="schema-table">
  <thead><tr><th>字段</th><th>类型</th><th>含义</th></tr></thead>
  <tbody>
    <tr><td>field_name</td><td>TEXT PK</td><td>含义解释，如 <code>"M01"</code></td></tr>
  </tbody>
</table>
```

### Required CSS (add to `<style>` block)

```css
.schema-obj-title {
  font-size: 13px; font-weight: 700; color: var(--text);
  margin: 24px 0 6px; border-left: 3px solid var(--accent);
  padding-left: 10px; line-height: 1.5;
}
.schema-table { width: 100%; border-collapse: collapse; margin: 0 0 20px; font-size: 12.5px; }
.schema-table th {
  background: var(--bg-alt); font-weight: 700; padding: 6px 10px;
  border: 1px solid var(--border); text-align: left;
  font-size: 11px; text-transform: uppercase; letter-spacing: 0.3px; color: #374151;
}
.schema-table td { padding: 6px 10px; border: 1px solid var(--border); vertical-align: top; line-height: 1.55; }
.schema-table td:first-child {
  font-family: 'JetBrains Mono', Consolas, monospace; font-size: 12px;
  color: var(--accent); white-space: nowrap; width: 170px; font-weight: 600;
}
.schema-table td:nth-child(2) {
  font-family: 'JetBrains Mono', Consolas, monospace; font-size: 11px;
  color: #6d28d9; white-space: nowrap; width: 160px;
}
.schema-table tr:nth-child(even) td { background: var(--bg-alt); }
```

---

## §4.1 Timeline

### Required CSS (add to `<style>` block)

```css
.section-lead { font-size: 15px; font-weight: 600; color: var(--text); margin: 0 0 16px; line-height: 1.6; }
.timeline { margin: 20px 0; }
.timeline-step { display: flex; gap: 16px; padding: 16px 0; border-bottom: 1px solid var(--border); }
.timeline-step:last-child { border-bottom: none; }
.step-title { font-weight: 600; font-size: 14px; min-width: 220px; color: var(--accent); }
.step-desc { font-size: 14px; color: var(--text-secondary); }
```

### HTML structure

```html
<p class="section-lead">以「具体功能描述」为例，走一遍完整链路。</p>
<div class="timeline">
  <div class="timeline-step">
    <div class="step-title">① 阶段标题</div>
    <div class="step-desc">描述，含 <code>命令</code> 和
      <strong style="color:#6d28d9;font-weight:600">关键概念</strong>。</div>
  </div>
  <!-- repeat for each step -->
</div>
```

---

## §5.3 Iron Law Callout

For highlighting Iron Laws inside module descriptions:

```html
<div class="iron-law">
  <strong>Iron Law</strong>：规则内容。
</div>
```

### Required CSS

```css
.iron-law {
  margin: 14px 0; padding: 12px 16px;
  background: #fff1f2; border: 1px solid #fecdd3;
  border-left: 4px solid #dc2626; border-radius: 0 8px 8px 0;
  font-size: 14px;
}
.iron-law strong { color: #dc2626; }
```
