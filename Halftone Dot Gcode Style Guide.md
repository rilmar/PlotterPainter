# Plotting Tools — UI Style Guide

Use this document as a reference when building new single-page apps for the Plotting Tools suite. Paste the relevant sections into your prompt to ensure visual consistency.

---

## 1. Design Principles

- **Industrial terminal aesthetic.** Monospace-first, dark, dense. Everything looks like it belongs on a CNC machine interface.
- **Acid-green on near-black.** One strong accent, used sparingly and purposefully.
- **Flat layering.** Three surface levels (`--bg`, `--surface`, `--surface2`) create depth without shadows.
- **All labels are uppercase monospace.** No sentence-case UI labels. Section heads, tags, stat keys — always `letter-spacing: 0.1em+; text-transform: uppercase`.
- **Numbers in `--accent`.** Slider readout values, stat figures, and live counts always render in `--accent` to draw the eye.
- **No decorative flourishes.** No box-shadows, no gradients, no rounded corners beyond `--radius: 6px`.

---

## 2. Fonts

```
@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:ital,wght@0,300;0,400;0,500;0,600;1,300&family=IBM+Plex+Sans:wght@300;400&display=swap');
```

| Role | Font | Weight | Usage |
|---|---|---|---|
| Primary / UI | IBM Plex Mono | 400, 500, 600 | All labels, inputs, buttons, headers, code |
| Italic accent | IBM Plex Mono Italic | 300 | Hero titles, decorative italic text |
| Body prose | IBM Plex Sans | 300, 400 | Descriptions, drop-zone copy, card descriptions |

**Rules:**
- `font-family: var(--mono)` on `body` — monospace is the default.
- `font-family: var(--sans); font-weight: 300` for any multi-sentence descriptive prose.
- Never use system fonts, Inter, Roboto, or Arial.

---

## 3. Color Tokens

```css
:root {
  --bg:          #0f0f0f;   /* page background */
  --surface:     #1a1a1a;   /* sidebar, panels, cards */
  --surface2:    #252525;   /* section heads, elevated elements */
  --border:      #2e2e2e;   /* all borders (default) */
  --border-hi:   #444;      /* hovered border */
  --accent:      #c8f542;   /* primary action color, active states, live values */
  --accent-dim:  #8ab02e;   /* muted accent — coming-soon badges, secondary accent */
  --text:        #e8e8e0;   /* primary text */
  --text-muted:  #777;      /* labels, secondary text, placeholders */
  --text-dim:    #333;      /* disabled, decorative, line numbers */
  --mono:        'IBM Plex Mono', monospace;
  --sans:        'IBM Plex Sans', sans-serif;
  --radius:      6px;
}
```

**Usage rules:**
- `--bg` is the page canvas. `--surface` is any raised panel. `--surface2` is any header within a panel.
- `--accent` only on: active states, primary button fill, slider thumbs, active tab indicator, live numeric readouts, the app wordmark.
- `--accent-dim` only on: badges, secondary accent hints, saved/status indicators.
- `--text-dim` for anything intentionally invisible at rest — line numbers, decorative elements, disabled items.
- Hover on borders goes `--border` → `--border-hi`.
- Focus on inputs shows `border-color: var(--accent)`.

---

## 4. Layout

### App shell (sidebar + main)
```css
body {
  font-family: var(--mono);
  background: var(--bg);
  color: var(--text);
  display: flex;
  flex-direction: column;
  font-size: 14px;
  min-height: 100vh;
}

.app {
  display: grid;
  grid-template-columns: 340px 1fr;  /* sidebar width typically 340–380px */
  flex: 1;
  min-height: 0;
}
```

### Sticky header
```css
header {
  padding: 14px 24px;
  border-bottom: 1px solid var(--border);
  background: rgba(15,15,15,0.96);
  display: flex;
  align-items: baseline;
  gap: 16px;
  position: sticky;
  top: 0;
  z-index: 50;
}

header h1 {
  font-size: 15px;
  font-weight: 600;
  letter-spacing: 0.08em;
  color: var(--accent);
  text-transform: uppercase;
  font-family: var(--mono);
}

header span {                  /* subtitle / tagline */
  font-size: 11px;
  color: var(--text-muted);
  letter-spacing: 0.05em;
  font-family: var(--mono);
}
```
Pattern: `APP NAME` (accent, uppercase) · `verb → verb → output` (muted, lowercase)

### Sidebar
```css
.sidebar {
  background: var(--surface);
  border-right: 1px solid var(--border);
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 20px;
  overflow-y: auto;
}
```

### Main content area
```css
.main { display: flex; flex-direction: column; overflow: hidden; }
```

---

## 5. Sections & Panels

### Section label (sidebar groups)
```css
.section-label {
  font-size: 10px;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--text-muted);
  margin-bottom: 10px;
  display: block;
  font-family: var(--mono);
}
```

### Section card (for settings and structured panels)
```html
<div class="section">
  <div class="section-head">Label Text</div>
  <div class="section-body"> ... </div>
</div>
```

```css
.section {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  overflow: hidden;
}

.section-head {
  padding: 10px 14px;
  background: var(--surface2);
  border-bottom: 1px solid var(--border);
  font-family: var(--mono);
  font-size: 10px;
  font-weight: 500;
  letter-spacing: 0.15em;
  color: var(--text-muted);
  text-transform: uppercase;
  display: flex;
  align-items: center;
  justify-content: space-between;  /* for right-side badges/counts */
}

.section-body { padding: 14px; }
```

---

## 6. Form Controls

### Slider (range input)
```css
input[type=range] {
  width: 100%;
  -webkit-appearance: none;
  height: 4px;
  border-radius: 2px;
  background: var(--border);
  outline: none;
  cursor: pointer;
  border: none;
  padding: 0;
}
input[type=range]::-webkit-slider-thumb {
  -webkit-appearance: none;
  width: 14px; height: 14px;
  border-radius: 50%;
  background: var(--accent);
  border: none;
  cursor: pointer;
}
input[type=range]::-moz-range-thumb {
  width: 14px; height: 14px;
  border-radius: 50%;
  background: var(--accent);
  border: none;
  cursor: pointer;
}
```

Slider rows use a label with a live readout value on the right in `--accent`:
```html
<div class="control-row">
  <label>Density <span id="densityVal">70</span>%</label>
  <input type="range" id="density" min="0" max="100" value="70">
</div>
```
```css
.control-row { display: flex; flex-direction: column; gap: 6px; }
.control-row label {
  font-size: 11px;
  color: var(--text-muted);
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-family: var(--mono);
}
.control-row label span { font-size: 11px; color: var(--accent); font-weight: 500; }
```

### Number input
```css
input[type=number] {
  color: var(--text);
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 6px 8px;
  font-family: var(--mono);
  font-size: 12px;
  outline: none;
  transition: border-color 0.15s;
  -moz-appearance: textfield;
}
input[type=number]:focus { border-color: var(--accent); }
/* Hide spinners */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }
```

### Select
```css
select {
  width: 100%;
  padding: 6px 8px;
  font-family: var(--mono);
  font-size: 12px;
  border: 1px solid var(--border);
  border-radius: var(--radius);
  background: var(--bg);
  color: var(--text);
  cursor: pointer;
  outline: none;
  transition: border-color 0.15s;
}
select:focus { border-color: var(--accent); }
```

### Textarea
```css
textarea {
  width: 100%;
  font-family: var(--mono);
  font-size: 11px;
  line-height: 1.6;
  color: var(--text);
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 8px 10px;
  resize: vertical;
  outline: none;
  transition: border-color 0.15s;
}
textarea:focus { border-color: var(--accent); }
```

---

## 7. Buttons

Two variants only. Always monospace, always uppercase.

```css
/* Primary */
.btn {
  padding: 8px 16px;
  font-family: var(--mono);
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 0.06em;
  border: 1px solid var(--accent);
  border-radius: var(--radius);
  background: var(--accent);
  color: #0f0f0f;
  cursor: pointer;
  transition: all 0.12s;
  text-transform: uppercase;
}
.btn:hover            { background: #d4ff55; border-color: #d4ff55; }
.btn:active           { transform: scale(0.98); }
.btn:disabled         { opacity: 0.35; cursor: not-allowed; transform: none; }

/* Ghost / secondary */
.btn.secondary {
  background: transparent;
  color: var(--text-muted);
  border-color: var(--border);
}
.btn.secondary:hover  { color: var(--text); border-color: var(--border-hi); background: var(--surface2); }
```

**Toggle button** — add `.active` class to flip to filled accent:
```css
.btn.toggle.active { background: var(--accent); color: #0f0f0f; border-color: var(--accent); }
```

---

## 8. Drop Zone

```html
<div class="drop-zone" id="dropZone">
  <input type="file" id="fileInput" accept="image/*">
  <div class="drop-zone-icon">⬡</div>
  <p><strong>Drop image here</strong><br>or click to browse</p>
</div>
```

```css
.drop-zone {
  border: 1.5px dashed var(--border);
  border-radius: var(--radius);
  padding: 28px 16px;
  text-align: center;
  cursor: pointer;
  transition: border-color 0.15s, background 0.15s;
  background: var(--bg);
  position: relative;
}
.drop-zone:hover, .drop-zone.dragover {
  border-color: var(--accent);
  background: rgba(200,245,66,0.04);
}
.drop-zone .icon  { font-size: 28px; margin-bottom: 8px; display: block; opacity: 0.4; }
.drop-zone p      { font-size: 12px; color: var(--text-muted); line-height: 1.55;
                    font-family: var(--sans); font-weight: 300; }
.drop-zone strong { color: var(--text); }
/* If using a full-area invisible file input: */
.drop-zone input[type=file] { position: absolute; inset: 0; opacity: 0; cursor: pointer; width: 100%; height: 100%; }
```

---

## 9. Tabs

```html
<div class="tabs">
  <button class="tab active" data-tab="preview">Preview</button>
  <button class="tab" data-tab="gcode">G-code</button>
  <button class="tab" data-tab="settings">Settings</button>
</div>
```

```css
.tabs {
  display: flex;
  border-bottom: 1px solid var(--border);
  background: var(--surface);
  padding: 0 16px;
}
.tab {
  padding: 12px 16px;
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-muted);
  cursor: pointer;
  border-bottom: 2px solid transparent;
  border-top: none; border-left: none; border-right: none;
  background: none;
  font-family: var(--mono);
  transition: color 0.15s, border-color 0.15s;
}
.tab:hover  { color: var(--text); }
.tab.active { color: var(--accent); border-bottom-color: var(--accent); }
```

---

## 10. Progress Bar & Status Bar

```css
/* 2px accent line — sits directly above/below content */
.progress-bar  { height: 2px; background: var(--border); overflow: hidden; }
.progress-fill { height: 100%; background: var(--accent); width: 0%; transition: width 0.1s; }

/* Status bar — sticky at bottom of main column */
.status-bar {
  padding: 8px 16px;
  font-size: 11px;
  color: var(--text-muted);
  border-top: 1px solid var(--border);
  background: var(--surface);
  letter-spacing: 0.04em;
  font-family: var(--mono);
}
```

Key-value status pattern used in dotwork:
```js
// Renders: "key: VALUE · key: VALUE"
function setStatusKV(pairs) {
  statusBar.innerHTML = pairs
    .map(([k,v]) => `${k}: <span style="color:var(--accent)">${v}</span>`)
    .join(' &nbsp;·&nbsp; ');
}
```

---

## 11. Tag Chips

Small read-only metadata labels, used on cards and layer info:

```html
<div class="card-tags">
  <span class="tag">Image → G-code</span>
  <span class="tag">Stipple</span>
</div>
```

```css
.tag {
  font-size: 9px;
  font-weight: 500;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--text-dim);
  border: 1px solid var(--border);
  border-radius: 3px;
  padding: 3px 7px;
  font-family: var(--mono);
}
```

---

## 12. Homepage Cards (index.html)

```css
.tool-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 24px;
  text-decoration: none;
  color: inherit;
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
  transition: border-color 0.15s, background 0.15s, transform 0.15s;
}
.tool-card:hover { border-color: var(--border-hi); background: var(--surface2); transform: translateY(-2px); }

/* Accent glow on hover — top-left radial */
.card-glow {
  position: absolute; inset: 0;
  background: radial-gradient(ellipse at top left, rgba(200,245,66,0.06), transparent 70%);
  opacity: 0; transition: opacity 0.25s; pointer-events: none;
}
.tool-card:hover .card-glow { opacity: 1; }

/* Coming-soon variant */
.tool-card.soon { cursor: default; pointer-events: none; }
.tool-card.soon .card-title { color: var(--text-dim); }

.coming-soon-badge {
  font-size: 9px; font-weight: 500; letter-spacing: 0.12em; text-transform: uppercase;
  color: var(--accent-dim); border: 1px solid var(--accent-dim);
  border-radius: 3px; padding: 3px 8px; width: fit-content;
  font-family: var(--mono);
}
```

---

## 13. G-code Viewer

```html
<div class="gcode-viewer">
  <div class="gcode-viewer-inner">
    <div class="gcode-linenos" id="gcodeLinenos"></div>
    <div class="gcode-code"    id="gcodeDisplay"></div>
  </div>
  <div class="gcode-resize-hint">drag bottom edge to resize ↕</div>
</div>
```

```css
.gcode-viewer {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  font-family: var(--mono);
  font-size: 11px;
  line-height: 1.6;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  min-height: 500px;
  height: 500px;
  resize: vertical;
}
.gcode-viewer-inner { display: flex; flex: 1; overflow: hidden; }
.gcode-linenos {
  flex-shrink: 0; width: 60px;
  padding: 10px 8px 10px 12px;
  background: var(--bg); border-right: 1px solid var(--border);
  color: var(--text-dim); text-align: right;
  overflow: hidden; user-select: none; white-space: pre;
}
.gcode-code {
  flex: 1; padding: 10px 14px;
  overflow: auto; color: var(--text-muted);
  white-space: pre;
}
.gcode-resize-hint {
  font-size: 10px; color: var(--text-dim); text-align: right;
  padding: 3px 10px; border-top: 1px solid var(--border);
  background: var(--bg); flex-shrink: 0; letter-spacing: 0.06em;
}
```

---

## 14. Page Load Animations

Staggered fade-up on entry. Keep it subtle — 10–12px travel, 0.4–0.55s duration.

```css
@keyframes rise {
  from { opacity: 0; transform: translateY(10px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* Apply to any element that should animate in */
.hero    { opacity: 0; animation: rise 0.55s cubic-bezier(0.22,1,0.36,1) 0.05s forwards; }
.section { opacity: 0; animation: rise 0.4s ease 0.25s forwards; }

/* Stagger cards */
.tool-card:nth-child(1) { opacity: 0; animation: rise 0.4s ease 0.30s forwards; }
.tool-card:nth-child(2) { opacity: 0; animation: rise 0.4s ease 0.38s forwards; }
.tool-card:nth-child(3) { opacity: 0; animation: rise 0.4s ease 0.46s forwards; }
```

---

## 15. Naming Conventions

| Pattern | Example |
|---|---|
| Section heads (sidebar) | `01 · Image`, `02 · Quantize`, `03 · Color Layers` |
| App wordmark | `APPNAME / SUBTITLE` in caps, separated by ` / ` |
| Header subtitle | lowercase, `→` arrow between stages: `image → strokes → g-code` |
| Tab labels | Single uppercase word: `Preview`, `G-code`, `Settings` |
| Button labels | Uppercase verb: `Generate Strokes`, `Export G-code`, `Analyze Image` |
| Settings variables | `{{VARIABLE_NAME}}` double-brace, screaming snake case |
| G-code comments | Semicolon style: `; This is a comment` |
| File naming | `kebab-case.html` — e.g. `dotwork-gcode.html`, `paintwork.html` |

---

## 16. Quick-start CSS Snippet

Paste this at the top of any new tool to establish the full design foundation:

```css
@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:ital,wght@0,300;0,400;0,500;0,600;1,300&family=IBM+Plex+Sans:wght@300;400&display=swap');

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --bg:         #0f0f0f;
  --surface:    #1a1a1a;
  --surface2:   #252525;
  --border:     #2e2e2e;
  --border-hi:  #444;
  --accent:     #c8f542;
  --accent-dim: #8ab02e;
  --text:       #e8e8e0;
  --text-muted: #777;
  --text-dim:   #333;
  --mono:       'IBM Plex Mono', monospace;
  --sans:       'IBM Plex Sans', sans-serif;
  --radius:     6px;
}

html, body { min-height: 100vh; }
body { font-family: var(--mono); background: var(--bg); color: var(--text);
       display: flex; flex-direction: column; font-size: 14px; }
```

---

## 17. Prompt Template

When asking Claude to build a new tool for this suite, include this block:

> **Style:** Match the Plotting Tools design system exactly.
> - Font: IBM Plex Mono (UI/code) + IBM Plex Sans weight 300 (prose). Import from Google Fonts.
> - Colors: `--bg: #0f0f0f`, `--surface: #1a1a1a`, `--surface2: #252525`, `--border: #2e2e2e`, `--accent: #c8f542`, `--text: #e8e8e0`, `--text-muted: #777`, `--text-dim: #333`, `--radius: 6px`.
> - Header: sticky, `background: rgba(15,15,15,0.96)`, app title in `--accent` uppercase monospace, subtitle in `--text-muted`.
> - Sidebar: `340px`, `background: var(--surface)`, `border-right: 1px solid var(--border)`, `padding: 20px`, `gap: 20px`.
> - Section heads: `10px`, `letter-spacing: 0.15em`, `text-transform: uppercase`, `color: var(--text-muted)`, inside a `--surface2` bar with `border-bottom: 1px solid var(--border)`.
> - Buttons: primary = `--accent` fill, `color: #0f0f0f`, uppercase mono; ghost = transparent, `color: var(--text-muted)`, `border: 1px solid var(--border)`.
> - Range sliders: 4px track in `--border`, 14px round thumb in `--accent`, no browser defaults.
> - All inputs/selects/textareas: `background: var(--bg)`, `border: 1px solid var(--border)`, `border-radius: 6px`, focus → `border-color: var(--accent)`.
> - Progress bar: `2px` height, `--accent` fill.
> - Status bar: `8px 16px` padding, `11px` mono, `--text-muted`, `border-top: 1px solid var(--border)`, `background: var(--surface)`.
> - Tabs (if needed): `--surface` background, `11px` uppercase mono, active = `--accent` text + `2px` bottom border.
> - No shadows. No gradients. No fonts other than the two above.
