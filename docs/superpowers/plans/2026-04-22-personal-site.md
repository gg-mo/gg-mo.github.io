# Personal Site Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Transform the existing `index.html` launch-page template into Gao Mo's personal site per the spec at `docs/superpowers/specs/2026-04-22-personal-site-design.md`.

**Architecture:** Single static `index.html` with inline `<style>` and inline `<script>`. No build step. Served by GitHub Pages as-is. All edits happen to this one file. Changes are grouped into bite-sized tasks that each produce an independently working site — so at every commit the page still loads and renders.

**Tech Stack:** HTML5, CSS3 (custom properties), vanilla JS, Google Fonts (Inter + JetBrains Mono), inline SVG favicon.

**Spec reference:** `docs/superpowers/specs/2026-04-22-personal-site-design.md` — re-read it alongside this plan. When this plan says "per spec §N", go look.

**Testing approach:** This is a single static HTML file with no framework and no test runner. "Tests" in this plan are **manual browser verification**: after each task, the engineer opens `index.html` in a browser and walks the checklist for that task. Verification expectations are spelled out for each task. At the end (Task 16) there is a full golden-path verification pass.

**Line numbers:** The original `index.html` has 524 lines. Line numbers in this plan refer to the **original** file only. After any edit, line numbers shift — when locating code in later tasks, **search by landmark string** (e.g., "find the `<nav class=\"top\">` block"), not by line number.

**Execution order (updated after Phase A):** Tasks in this plan are grouped into execution phases. The phase order is:

- **Phase A** (structural/content): Tasks 1 → 2 → 3 → 4 → 5 → 6 → 7. *(Already complete.)*
- **Phase B** (ambient + hero hierarchy + scroll cue): Task 17 → 18 → 8 → 9 → 19 → 10.
- **Phase C** (cards): Task 11.
- **Phase D** (polish — hover, halo, scanline, reveal motion): Task 12 → 13 → 14 → 15 → 20.
- **Final verification:** Task 16.

Tasks 17–20 were added after Phase A per user direction (hero dominance, viewport border glow, scroll cue, stronger scroll-reveal). Find them at the bottom of this file.

**File layout (unchanged):**
```
index.html   ← all edits happen here
README.md    ← untouched
```

---

## Task 1: Add CSS tokens, JetBrains Mono font, and new palette

**Files:**
- Modify: `index.html` — `<link>` tags in `<head>` and `:root { ... }` CSS custom properties block

**Why:** Later tasks depend on these tokens. Adding them up front means the rest of the work is purely structural. No visible change expected in this task — tokens are defined but unused.

- [ ] **Step 1: Add JetBrains Mono to the Google Fonts `<link>`**

Find in `<head>`:

```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&display=swap" rel="stylesheet" />
```

Replace with:

```html
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet" />
```

- [ ] **Step 2: Update `--sc-accent` and add new tokens**

Find the `:root { ... }` block (starts around line 12 of the original file).

Replace the single line:

```css
  --sc-accent: #eef0f5;
```

with:

```css
  --sc-accent: #c8e9ff;
  --sc-accent-deep: #4fc3ff;
  --sc-accent-glow: rgba(79, 195, 255, 0.35);
  --sc-grid: rgba(255, 255, 255, 0.05);
```

Then, immediately after the existing `--sc-sans` definition:

```css
  --sc-sans: -apple-system, "SF Pro Text", Inter, ui-sans-serif, system-ui, sans-serif;
```

add:

```css
  --sc-mono: "JetBrains Mono", ui-monospace, SFMono-Regular, Menlo, monospace;
```

- [ ] **Step 3: Verify the page still loads with no console errors**

Open `index.html` in a browser (or reload). Open DevTools → Console.

Expected:
- Page renders identically to before (the headline, cards, etc. look the same — we haven't changed structure yet).
- Primary button color shifts very slightly from pure white toward a cool white (because `--sc-accent` changed from `#eef0f5` to `#c8e9ff`). This is expected.
- No console errors.
- Network tab: `JetBrains+Mono` font file downloaded.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add JetBrains Mono font and cool-cyan accent palette tokens"
```

---

## Task 2: Update page title, nav link, and nav mark

**Files:**
- Modify: `index.html` — `<title>`, `<nav class="top">` block

**Why:** Quick textual updates. Independent of layout/visual work.

- [ ] **Step 1: Update `<title>`**

Find:

```html
<title>gg-mo — Work & Projects</title>
```

Replace with:

```html
<title>Gao Mo</title>
```

- [ ] **Step 2: Update `<meta name="description">`**

Find:

```html
<meta name="description" content="Personal site and project index." />
```

Replace with:

```html
<meta name="description" content="Gao Mo — personal site." />
```

- [ ] **Step 3: Update nav mark and links**

Find the `<nav class="top">` block:

```html
<nav class="top">
  <div class="mark"><span class="dot"></span>gg-mo</div>
  <div class="links">
    <a href="#work">Work</a>
    <a href="#about">About</a>
    <a href="#contact">Contact</a>
  </div>
</nav>
```

Replace with:

```html
<nav class="top">
  <div class="mark"><span class="dot"></span>Gao Mo</div>
  <div class="links">
    <a href="#work">Work</a>
    <a href="#about">About</a>
    <a href="#links">Links</a>
  </div>
</nav>
```

(Note: the `Contact` section `id="contact"` still exists; we'll rename its id in a later task.)

- [ ] **Step 4: Verify**

Reload the browser.

Expected:
- Browser tab shows "Gao Mo".
- Nav top-left shows "Gao Mo" (with dot).
- Nav top-right shows "Work · About · Links".
- Clicking `Links` scrolls to nowhere yet (404-hash), which is fine for now.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "Update page title and nav to Gao Mo / Links"
```

---

## Task 3: Replace hero section with `I build.`

**Files:**
- Modify: `index.html` — `<section class="hero" id="top">` block

**Why:** Core identity statement. Strips eyebrow and lede per spec §4.1.

- [ ] **Step 1: Replace the hero block**

Find:

```html
<section class="hero" id="top">
  <div class="eyebrow reveal" style="--d:0">Personal index — 2026</div>
  <h1 class="reveal" style="--d:1">
    Building quiet tools<br/>
    <span class="muted">for loud problems.</span>
  </h1>
  <p class="lede reveal" style="--d:2">
    I design and ship software at the edge of craft and systems — interfaces, infrastructure, and everything between.
  </p>
  <div class="cta-row reveal" style="--d:3">
    <a class="btn primary" href="#work">
      See the work
      <span class="arrow" aria-hidden="true">→</span>
    </a>
    <a class="btn ghost" href="#contact">Get in touch</a>
  </div>
</section>
```

Replace with:

```html
<section class="hero" id="top">
  <h1 class="reveal" style="--d:0">I build.</h1>
  <div class="cta-row reveal" style="--d:1">
    <a class="btn primary" href="#work">
      See the work
      <span class="arrow" aria-hidden="true">→</span>
    </a>
  </div>
</section>
```

- [ ] **Step 2: Verify**

Reload.

Expected:
- Hero shows only "I build." (giant type, centered).
- One button below: "See the work →".
- No eyebrow, no lede, no ghost button.
- Clicking "See the work" smooth-scrolls to the Work section.
- Layout still has the large top/bottom padding (`.hero` styles unchanged).

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m 'Replace hero with "I build." and single CTA'
```

---

## Task 4: Replace About copy

**Files:**
- Modify: `index.html` — `<section id="about">` block body

**Why:** Understated line per spec §4.3.

- [ ] **Step 1: Replace the About body**

Find inside `<section id="about">`:

```html
<div class="about in-view">
  <div class="label">Notes</div>
  <div>
    <p>I care about tools that feel inevitable — the kind where the right thing is also the obvious one.</p>
    <p>Currently exploring interfaces for AI systems, developer tooling, and the quieter parts of design engineering. Based on the internet.</p>
  </div>
</div>
```

Replace with:

```html
<div class="about in-view">
  <div class="label">Notes</div>
  <div>
    <p>I build tools for thinking — agent skills, ML systems, and the quiet interfaces that make models feel inevitable.</p>
  </div>
</div>
```

- [ ] **Step 2: Verify**

Reload. Scroll to About.

Expected:
- About section shows a single paragraph ending in "…feel inevitable."
- The `Notes` label on the left still shows.
- Fade-up animation still plays.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Replace About copy with terse one-liner"
```

---

## Task 5: Replace Contact section with Links section

**Files:**
- Modify: `index.html` — `<section id="contact">` block

**Why:** Per spec §4.4 / §4.5, `Contact` becomes `Links` with only LinkedIn, GitHub, Email.

- [ ] **Step 1: Replace the Contact section**

Find:

```html
<section id="contact">
  <div class="section-head in-view">
    <h2>Contact</h2>
    <span class="kicker">Open inbox</span>
  </div>
  <div class="about in-view">
    <div class="label">Reach out</div>
    <div>
      <p>The fastest way is email.</p>
      <p>
        <a href="mailto:hello@example.com">hello@example.com</a> ·
        <a href="https://github.com/gg-mo" target="_blank" rel="noopener">GitHub</a> ·
        <a href="#" target="_blank" rel="noopener">Twitter</a>
      </p>
    </div>
  </div>
</section>
```

Replace with:

```html
<section id="links">
  <div class="section-head in-view">
    <h2>Links</h2>
    <span class="kicker">Open inbox</span>
  </div>
  <div class="about in-view">
    <div class="label">Reach out</div>
    <div>
      <p>
        <a href="https://www.linkedin.com/in/gao-mo/" target="_blank" rel="noopener">LinkedIn</a> ·
        <a href="https://github.com/gg-mo" target="_blank" rel="noopener">GitHub</a> ·
        <a href="mailto:happyhhour123@gmail.com">happyhhour123@gmail.com</a>
      </p>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Verify**

Reload.

Expected:
- Section heading shows "Links".
- Body shows three links: LinkedIn · GitHub · email address.
- Clicking nav "Links" now scrolls to this section (fixes the broken hash from Task 2).
- Email link opens mail client; LinkedIn and GitHub open in new tabs.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Replace Contact with Links section (LinkedIn, GitHub, email)"
```

---

## Task 6: Update footer

**Files:**
- Modify: `index.html` — `<footer>` block

**Why:** Spec §4.6 — name update + subtle monospace touch.

- [ ] **Step 1: Replace the footer block**

Find:

```html
<footer>
  <div>© 2026 gg-mo. Built with restraint.</div>
  <div class="links">
    <a href="#top">Back to top</a>
    <a href="https://github.com/gg-mo" target="_blank" rel="noopener">GitHub</a>
  </div>
</footer>
```

Replace with:

```html
<footer>
  <div>© 2026 Gao Mo. <span class="end-of-line">// end of line</span></div>
  <div class="links">
    <a href="#top">Back to top</a>
    <a href="https://github.com/gg-mo" target="_blank" rel="noopener">GitHub</a>
  </div>
</footer>
```

- [ ] **Step 2: Add the `.end-of-line` monospace style**

Find the footer CSS block (starts with `/* Footer */` comment). After the existing `footer a:hover` rule, add:

```css
footer .end-of-line {
  font-family: var(--sc-mono);
  color: var(--sc-text-tertiary);
  margin-left: 8px;
}
```

- [ ] **Step 3: Verify**

Reload. Scroll to bottom.

Expected:
- Footer shows: `© 2026 Gao Mo. // end of line`.
- `// end of line` is rendered in JetBrains Mono, dimmed color.
- `Back to top` and `GitHub` links still on the right.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Update footer with Gao Mo name and monospace end-of-line touch"
```

---

## Task 7: Add inline SVG favicon

**Files:**
- Modify: `index.html` — `<head>`

**Why:** Spec §7 — favicon matching the nav mark dot.

- [ ] **Step 1: Insert the favicon `<link>`**

In `<head>`, immediately after the `<meta name="description">` line, add:

```html
<link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32'%3E%3Cdefs%3E%3Cfilter id='g' x='-50%25' y='-50%25' width='200%25' height='200%25'%3E%3CfeGaussianBlur stdDeviation='2'/%3E%3C/filter%3E%3C/defs%3E%3Ccircle cx='16' cy='16' r='10' fill='%234fc3ff' filter='url(%23g)' opacity='0.5'/%3E%3Ccircle cx='16' cy='16' r='7' fill='%234fc3ff'/%3E%3C/svg%3E" />
```

- [ ] **Step 2: Verify**

Hard-reload (Cmd+Shift+R) to bust cached favicon.

Expected:
- Browser tab shows a small cyan filled circle with a soft outer glow.
- No 404 in the Network tab for favicon.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Add inline SVG favicon (cyan circle with soft glow)"
```

---

## Task 8: Add faint grid backdrop

**Files:**
- Modify: `index.html` — `.backdrop` CSS

**Why:** Spec §5.2 — adds the "ML-lab console" ambient layer under the existing radial gradient.

- [ ] **Step 1: Update the `.backdrop` style**

Find:

```css
.backdrop {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 0;
  background:
    radial-gradient(ellipse 60% 40% at 50% 0%, rgba(122, 162, 247, 0.06), transparent 70%),
    radial-gradient(ellipse 40% 30% at 50% 100%, rgba(255, 255, 255, 0.02), transparent 70%);
}
```

Replace with:

```css
.backdrop {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 0;
  background:
    radial-gradient(ellipse 60% 40% at 50% 0%, rgba(122, 162, 247, 0.06), transparent 70%),
    radial-gradient(ellipse 40% 30% at 50% 100%, rgba(255, 255, 255, 0.02), transparent 70%);
}
.backdrop::before {
  content: "";
  position: absolute;
  inset: -2px;
  background-image:
    linear-gradient(to right, var(--sc-grid) 1px, transparent 1px),
    linear-gradient(to bottom, var(--sc-grid) 1px, transparent 1px);
  background-size: 48px 48px;
  mask-image: radial-gradient(ellipse 80% 60% at 50% 40%, black 30%, transparent 85%);
  -webkit-mask-image: radial-gradient(ellipse 80% 60% at 50% 40%, black 30%, transparent 85%);
  transform: translate3d(0, 0, 0);
  will-change: transform;
}
```

- [ ] **Step 2: Verify**

Reload.

Expected:
- A faint grid is visible behind content, densest near the hero, softening toward the edges.
- Grid does not scroll with page content (because `.backdrop` is `position: fixed`).
- No performance jank — page scroll is still smooth.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Add faint fixed grid backdrop under radial gradient"
```

---

## Task 9: Add mouse parallax to grid backdrop

**Files:**
- Modify: `index.html` — `<script>` block

**Why:** Spec §5.2 — ≤4px cursor-driven drift, disabled under `prefers-reduced-motion`.

- [ ] **Step 1: Append parallax script**

Find the existing `<script>` block near the end of `<body>`:

```html
<script>
  // Scroll reveal — single observer, single pattern.
  const io = new IntersectionObserver((entries) => {
    for (const e of entries) {
      if (e.isIntersecting) {
        e.target.classList.add('shown');
        io.unobserve(e.target);
      }
    }
  }, { threshold: 0.18, rootMargin: '0px 0px -40px 0px' });

  document.querySelectorAll('.in-view').forEach((el, i) => {
    el.style.transitionDelay = (i % 6) * 80 + 'ms';
    io.observe(el);
  });
</script>
```

Replace with:

```html
<script>
  // Scroll reveal — single observer, single pattern.
  const io = new IntersectionObserver((entries) => {
    for (const e of entries) {
      if (e.isIntersecting) {
        e.target.classList.add('shown');
        io.unobserve(e.target);
      }
    }
  }, { threshold: 0.18, rootMargin: '0px 0px -40px 0px' });

  document.querySelectorAll('.in-view').forEach((el, i) => {
    el.style.transitionDelay = (i % 6) * 80 + 'ms';
    io.observe(el);
  });

  // Grid backdrop mouse parallax (skipped under reduced motion)
  const reduceMotion = matchMedia('(prefers-reduced-motion: reduce)').matches;
  const backdrop = document.querySelector('.backdrop');
  if (!reduceMotion && backdrop) {
    let tx = 0, ty = 0, rx = 0, ry = 0, raf = 0;
    const MAX = 4;
    const tick = () => {
      rx += (tx - rx) * 0.08;
      ry += (ty - ry) * 0.08;
      backdrop.style.setProperty('--grid-x', rx.toFixed(2) + 'px');
      backdrop.style.setProperty('--grid-y', ry.toFixed(2) + 'px');
      if (Math.abs(tx - rx) > 0.05 || Math.abs(ty - ry) > 0.05) {
        raf = requestAnimationFrame(tick);
      } else {
        raf = 0;
      }
    };
    window.addEventListener('mousemove', (e) => {
      const cx = e.clientX / window.innerWidth - 0.5;
      const cy = e.clientY / window.innerHeight - 0.5;
      tx = -cx * MAX * 2;
      ty = -cy * MAX * 2;
      if (!raf) raf = requestAnimationFrame(tick);
    }, { passive: true });
  }
</script>
```

- [ ] **Step 2: Update `.backdrop::before` to read the CSS variables**

Find the `.backdrop::before` rule added in Task 8 and change its `transform` line from:

```css
  transform: translate3d(0, 0, 0);
```

to:

```css
  transform: translate3d(var(--grid-x, 0), var(--grid-y, 0), 0);
```

- [ ] **Step 3: Verify**

Reload. Move the mouse around the viewport.

Expected:
- Grid backdrop drifts ≤4px opposite to cursor direction, smoothly.
- Effect is subtle — you may need to move the mouse edge-to-edge to notice.
- In DevTools → Rendering → "Emulate CSS media feature prefers-reduced-motion: reduce", reload: parallax is disabled (no drift on mouse move).
- No console errors.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add mouse parallax drift to grid backdrop (reduced-motion aware)"
```

---

## Task 10: Add hero shimmer (one-time on load)

**Files:**
- Modify: `index.html` — CSS (new keyframes + class) and hero `<h1>` markup

**Why:** Spec §5.3 — cyan sheen sweeps across "I build." once on load, then settles.

- [ ] **Step 1: Add shimmer CSS**

Find the existing motion section (starts with `/* Motion */` comment). After the last `@keyframes` rule but before `body { animation: ... }`, add:

```css
@keyframes sc-shimmer {
  0%   { background-position: -150% 0; }
  100% { background-position: 250% 0; }
}
.hero h1.shimmer {
  background: linear-gradient(
    90deg,
    var(--sc-text) 0%,
    var(--sc-text) 40%,
    var(--sc-accent-deep) 50%,
    var(--sc-text) 60%,
    var(--sc-text) 100%
  );
  background-size: 200% 100%;
  background-position: -150% 0;
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  animation: sc-shimmer 1.2s var(--sc-ease-out-soft) 0.6s 1 forwards;
}
@media (prefers-reduced-motion: reduce) {
  .hero h1.shimmer {
    background: none;
    color: var(--sc-text);
    animation: none;
  }
}
```

- [ ] **Step 2: Add the `shimmer` class to the hero `<h1>`**

Find:

```html
<h1 class="reveal" style="--d:0">I build.</h1>
```

Replace with:

```html
<h1 class="reveal shimmer" style="--d:0">I build.</h1>
```

- [ ] **Step 3: Reset the headline to plain text after the shimmer completes**

Append to the existing `<script>` block (inside the same `<script>` element, after the parallax block):

```javascript
  // Hero shimmer one-shot: after animation, drop shimmer class so text stays solid
  const heroH1 = document.querySelector('.hero h1.shimmer');
  if (heroH1) {
    heroH1.addEventListener('animationend', (e) => {
      if (e.animationName === 'sc-shimmer') {
        heroH1.classList.remove('shimmer');
      }
    });
  }
```

- [ ] **Step 4: Verify**

Reload.

Expected:
- "I build." fades up (existing fade), then a cyan sheen sweeps across the letters once (~1.2s).
- After the sweep, the letters stay solid `var(--sc-text)` (near-white) — no second sweep on further interaction.
- With `prefers-reduced-motion: reduce`: headline fades in plain white, no sweep.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m 'Add one-time cyan shimmer sweep on "I build." hero'
```

---

## Task 11: Rebuild project cards (structure + content + 2×2 grid)

**Files:**
- Modify: `index.html` — `<section id="work">` body + `.grid` / `.card` CSS

**Why:** Core visual change. Replaces placeholder cards with terminal-style preview tiles containing real repo data. Per spec §4.2 and §5.4 and §5.7.

- [ ] **Step 1: Remove the `.card.wide` rule**

Find:

```css
.card.wide { grid-column: span 12; min-height: 340px; }
```

Delete this line. (All cards are equal size now.)

- [ ] **Step 2: Add card terminal-style sub-element CSS**

Find the `.card .meta .view .arrow` rules (end of the card CSS). After the last card-related rule, add:

```css
.card .window-chrome {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: var(--sc-space-4);
}
.card .dots {
  display: inline-flex;
  gap: 6px;
}
.card .dots span {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: var(--sc-surface-3);
}
.card .lang {
  font-family: var(--sc-mono);
  font-size: 11px;
  letter-spacing: 0.04em;
  color: var(--sc-text-tertiary);
}
.card .path {
  font-family: var(--sc-mono);
  font-size: 13px;
  color: var(--sc-text-secondary);
  margin-bottom: var(--sc-space-4);
}
.card .path .prefix {
  color: var(--sc-accent-deep);
}
.card .chips {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-top: var(--sc-space-4);
}
.card .chip {
  font-family: var(--sc-mono);
  font-size: 11px;
  letter-spacing: 0.02em;
  color: var(--sc-text-secondary);
  padding: 3px 8px;
  border: 1px solid var(--sc-border);
  border-radius: 999px;
  background: var(--sc-surface-2);
}
.card .meta .updated {
  font-family: var(--sc-mono);
  font-size: 12px;
  color: var(--sc-text-tertiary);
}
```

- [ ] **Step 3: Replace the Work section body**

Find the entire `<section id="work">` block and replace with:

```html
<section id="work">
  <div class="section-head in-view">
    <h2>Selected work</h2>
    <span class="kicker">2025 — 2026</span>
  </div>

  <div class="grid">
    <a class="card in-view" href="https://github.com/gg-mo/second-thought-agent-skill" target="_blank" rel="noopener" aria-label="second-thought-agent-skill — critique-first agent skill">
      <div class="window-chrome">
        <span class="dots"><span></span><span></span><span></span></span>
        <span class="lang">Python</span>
      </div>
      <div class="path"><span class="prefix">~/</span>gg-mo/second-thought-agent-skill</div>
      <h3>second-thought-agent-skill</h3>
      <p>A critique-first agent skill that challenges every decision before execution — catching flaws, surfacing risks, and improving outcomes in real time.</p>
      <div class="chips">
        <span class="chip">agent-skill</span>
        <span class="chip">ai-safety</span>
        <span class="chip">claude-code</span>
      </div>
      <div class="meta">
        <span class="updated">Updated Apr 2026</span>
        <span class="view">View <span class="arrow" aria-hidden="true">→</span></span>
      </div>
    </a>

    <a class="card in-view" href="https://github.com/gg-mo/stagecraft-agent-skill" target="_blank" rel="noopener" aria-label="stagecraft-agent-skill — keynote-style UI plugin">
      <div class="window-chrome">
        <span class="dots"><span></span><span></span><span></span></span>
        <span class="lang">Shell</span>
      </div>
      <div class="path"><span class="prefix">~/</span>gg-mo/stagecraft-agent-skill</div>
      <h3>stagecraft-agent-skill</h3>
      <p>Claude Code plugin for premium, keynote-style UI — dark, minimal, cinematic reveals with disciplined motion.</p>
      <div class="chips">
        <span class="chip">claude-code-plugin</span>
        <span class="chip">motion-design</span>
        <span class="chip">design-system</span>
      </div>
      <div class="meta">
        <span class="updated">Updated Apr 2026</span>
        <span class="view">View <span class="arrow" aria-hidden="true">→</span></span>
      </div>
    </a>

    <a class="card in-view" href="https://github.com/gg-mo/agent-tea" target="_blank" rel="noopener" aria-label="agent-tea — AI personality test">
      <div class="window-chrome">
        <span class="dots"><span></span><span></span><span></span></span>
        <span class="lang">TypeScript</span>
      </div>
      <div class="path"><span class="prefix">~/</span>gg-mo/agent-tea</div>
      <h3>agent-tea</h3>
      <p>A personality test where AI agents and chatbots spill the tea on their humans.</p>
      <div class="chips">
        <span class="chip">ai-personality-test</span>
        <span class="chip">nextjs</span>
        <span class="chip">supabase</span>
      </div>
      <div class="meta">
        <span class="updated">Updated Apr 2026</span>
        <span class="view">View <span class="arrow" aria-hidden="true">→</span></span>
      </div>
    </a>

    <a class="card in-view" href="https://github.com/NoTestsNoFear/Flexing" target="_blank" rel="noopener" aria-label="Flexing — feed-first community platform">
      <div class="window-chrome">
        <span class="dots"><span></span><span></span><span></span></span>
        <span class="lang">TypeScript</span>
      </div>
      <div class="path"><span class="prefix">~/</span>NoTestsNoFear/Flexing</div>
      <h3>Flexing</h3>
      <p>Flex. Roast. Build. — the feed-first community where humans and AI agents show their work, get roasted, build reputation, and get hired.</p>
      <div class="chips">
        <span class="chip">ai-agents</span>
        <span class="chip">community-platform</span>
        <span class="chip">mcp</span>
      </div>
      <div class="meta">
        <span class="updated">Updated Apr 2026</span>
        <span class="view">View <span class="arrow" aria-hidden="true">→</span></span>
      </div>
    </a>
  </div>
</section>
```

- [ ] **Step 4: Verify**

Reload. Scroll to Work.

Expected:
- Section heading reads "Selected work" with kicker "2025 — 2026".
- 4 cards in a 2×2 grid on desktop.
- Each card shows: 3 dots (top-left), language chip (top-right), mono path like `~/gg-mo/second-thought-agent-skill`, title, description, 3 topic chips, and footer with `Updated Apr 2026` on the left and `View →` on the right.
- Clicking any card opens the correct repo in a new tab.
- At viewport <760px, cards stack full-width.
- Existing fade-up animation still plays on scroll.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "Rebuild project cards as terminal-style 2×2 grid with real repo data"
```

---

## Task 12: Add card hover glow

**Files:**
- Modify: `index.html` — `.card:hover` CSS

**Why:** Spec §5.4 — soft cyan outer glow fading in over 400ms on hover.

- [ ] **Step 1: Update the `.card` transition list and `.card:hover` rule**

Find:

```css
.card {
  position: relative;
  background: var(--sc-surface-1);
  border: 1px solid var(--sc-border);
  border-radius: var(--sc-radius-lg);
  padding: var(--sc-space-8);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  min-height: 280px;
  transition:
    background 0.4s var(--sc-ease-in-out-soft),
    border-color 0.4s var(--sc-ease-in-out-soft),
    transform 0.5s var(--sc-ease-out-soft);
  grid-column: span 6;
}
```

Replace the `transition` block inside this rule with:

```css
  transition:
    background 0.4s var(--sc-ease-in-out-soft),
    border-color 0.4s var(--sc-ease-in-out-soft),
    box-shadow 0.4s var(--sc-ease-in-out-soft),
    transform 0.5s var(--sc-ease-out-soft);
```

Then find:

```css
.card:hover {
  background: var(--sc-surface-2);
  border-color: rgba(255, 255, 255, 0.10);
  transform: translateY(-2px);
}
```

Replace with:

```css
.card:hover {
  background: var(--sc-surface-2);
  border-color: rgba(200, 233, 255, 0.18);
  transform: translateY(-2px);
  box-shadow: 0 0 0 1px rgba(200, 233, 255, 0.08), 0 12px 40px var(--sc-accent-glow);
}
```

- [ ] **Step 2: Verify**

Reload. Hover each card.

Expected:
- Card lifts slightly (translateY -2px).
- A soft cyan glow fades in around the card's outer edge over ~400ms.
- Border color picks up a subtle cyan tint.
- No layout shift or jank.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Add soft cyan hover glow to project cards"
```

---

## Task 13: Add card cursor-follow spotlight

**Files:**
- Modify: `index.html` — `.card::before` CSS + `<script>` block

**Why:** Spec §5.4 — radial spotlight inside the card follows the mouse on hover. Disabled under `prefers-reduced-motion`.

- [ ] **Step 1: Add `.card::before` CSS**

Find the `.card` rule and, immediately after it (before `.card.wide` was — which is now removed, so before the `@media (max-width: 760px) { .card { grid-column: span 12; } }` block), add:

```css
.card::before {
  content: "";
  position: absolute;
  inset: 0;
  pointer-events: none;
  background: radial-gradient(
    220px circle at var(--mx, 50%) var(--my, 50%),
    rgba(200, 233, 255, 0.12),
    transparent 60%
  );
  opacity: 0;
  transition: opacity 0.35s var(--sc-ease-in-out-soft);
}
.card:hover::before { opacity: 1; }
@media (prefers-reduced-motion: reduce) {
  .card::before { display: none; }
}
```

Note: the `.card` rule needs `position: relative` — it already has that (check it in the existing rule; if not, add it).

- [ ] **Step 2: Add mousemove handler for the spotlight**

Append to the existing `<script>` block (after the hero shimmer block):

```javascript
  // Card cursor-follow spotlight (skipped under reduced motion)
  if (!reduceMotion) {
    document.querySelectorAll('.card').forEach((card) => {
      let pending = false;
      card.addEventListener('mousemove', (e) => {
        if (pending) return;
        pending = true;
        requestAnimationFrame(() => {
          const rect = card.getBoundingClientRect();
          const x = ((e.clientX - rect.left) / rect.width) * 100;
          const y = ((e.clientY - rect.top) / rect.height) * 100;
          card.style.setProperty('--mx', x + '%');
          card.style.setProperty('--my', y + '%');
          pending = false;
        });
      }, { passive: true });
    });
  }
```

- [ ] **Step 3: Verify**

Reload. Hover a card and move the cursor inside it.

Expected:
- A soft cyan radial spotlight follows the cursor within the card.
- Spotlight fades out when cursor leaves the card.
- Outer hover glow from Task 12 still visible — both effects co-exist.
- With `prefers-reduced-motion: reduce` emulated: spotlight is disabled (the `::before` layer doesn't render).
- No visible performance degradation during mouse movement.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add cursor-follow spotlight to project cards"
```

---

## Task 14: Add CTA button halo

**Files:**
- Modify: `index.html` — `.btn.primary:hover` CSS

**Why:** Spec §5.5 — primary button gets a soft cyan halo on hover.

- [ ] **Step 1: Update `.btn.primary:hover`**

Find:

```css
.btn.primary:hover { transform: translateY(-1px); }
```

Replace with:

```css
.btn.primary:hover {
  transform: translateY(-1px);
  box-shadow: 0 0 24px var(--sc-accent-glow);
}
```

- [ ] **Step 2: Update `.btn` transition list**

Find:

```css
.btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 12px 20px;
  border-radius: var(--sc-radius-sm);
  font-size: 14px;
  font-weight: 500;
  transition:
    background 0.3s var(--sc-ease-in-out-soft),
    border-color 0.3s var(--sc-ease-in-out-soft),
    color 0.3s var(--sc-ease-in-out-soft),
    transform 0.3s var(--sc-ease-out-soft);
```

Replace the `transition` block inside it with:

```css
  transition:
    background 0.3s var(--sc-ease-in-out-soft),
    border-color 0.3s var(--sc-ease-in-out-soft),
    color 0.3s var(--sc-ease-in-out-soft),
    box-shadow 0.3s var(--sc-ease-in-out-soft),
    transform 0.3s var(--sc-ease-out-soft);
```

- [ ] **Step 3: Verify**

Reload. Hover the "See the work →" button.

Expected:
- Button lifts (translateY -1px) and a soft cyan halo fades in around it over ~300ms.
- On mouse-leave, halo fades out smoothly.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add cyan halo to primary CTA button on hover"
```

---

## Task 15: Add section divider scanline

**Files:**
- Modify: `index.html` — `.section-head` CSS + `<script>` IntersectionObserver

**Why:** Spec §5.6 — thin horizontal scanline brightens when section scrolls into view.

- [ ] **Step 1: Add scanline CSS**

Find:

```css
.section-head {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  padding-bottom: var(--sc-space-8);
  margin-bottom: var(--sc-space-8);
  border-bottom: 1px solid var(--sc-border);
}
```

Replace with:

```css
.section-head {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  padding-bottom: var(--sc-space-8);
  margin-bottom: var(--sc-space-8);
  border-bottom: 1px solid var(--sc-border);
  position: relative;
}
.section-head::after {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  bottom: -1px;
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--sc-accent-glow), transparent);
  opacity: 0;
  transition: opacity 0.6s var(--sc-ease-in-out-soft);
}
.section-head.shown::after {
  opacity: 1;
  animation: sc-scanline-fade 1.8s var(--sc-ease-in-out-soft) forwards;
}
@keyframes sc-scanline-fade {
  0%   { opacity: 0; }
  33%  { opacity: 1; }
  100% { opacity: 0.15; }
}
@media (prefers-reduced-motion: reduce) {
  .section-head::after { animation: none; opacity: 0.15; }
}
```

- [ ] **Step 2: Verify**

Reload. Scroll down slowly.

Expected:
- As each section heading scrolls into the viewport, a cyan scanline brightens along its bottom border, then dims to ~15% and stays there.
- Works for "Selected work", "About", and "Links" section heads.
- Piggybacks on existing `.in-view` → `.shown` class toggle (`.section-head` already has `.in-view` in the HTML).
- With `prefers-reduced-motion`: scanline appears at 15% without the brighten animation.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Add section divider scanline that brightens on scroll-in"
```

---

## Task 16: Final golden-path verification

**Files:** none modified — this is the end-to-end check.

**Why:** The sum of changes must be experienced together. Previous tasks only verified their own change.

- [ ] **Step 1: Load fresh and watch the intro sequence**

1. Hard-reload `index.html` (Cmd+Shift+R) with DevTools → Console open.
2. Watch the load:
   - Page fades in.
   - Nav fades in.
   - "I build." fades up, then shimmer sweeps once, then stays solid near-white.
   - Background grid is visible, faint.
3. Confirm: no console errors, no 404s (except acceptable ones if offline).

- [ ] **Step 2: Scroll and hover path**

1. Move the mouse across the viewport — grid drifts very subtly.
2. Click "See the work →" — smooth-scrolls to Work.
3. Observe: section header's scanline brightens then dims.
4. Hover each project card — outer cyan glow + lift + cursor-follow spotlight inside.
5. Click one card (e.g., `second-thought-agent-skill`) — opens correct GitHub repo in a new tab.
6. Scroll to About — paragraph fades up, scanline on header brightens.
7. Scroll to Links — same.
8. Click LinkedIn, GitHub — each opens in new tab. Click email — mail client opens (or handler prompt).
9. Click "Back to top" — smooth-scrolls to hero.

- [ ] **Step 3: Nav path**

1. Click nav "Work" — scrolls to Work section.
2. Click nav "About" — scrolls to About section.
3. Click nav "Links" — scrolls to Links section.

- [ ] **Step 4: Reduced-motion path**

1. DevTools → Rendering → "Emulate CSS media feature prefers-reduced-motion: reduce".
2. Hard-reload.
3. Verify:
   - "I build." fades in plain white, no shimmer.
   - Mouse movement causes no grid drift.
   - Card hover: outer glow still shows (box-shadow, not motion), but cursor-follow spotlight is disabled.
   - Scanlines visible at 15% opacity, no brighten animation.

- [ ] **Step 5: Responsive path**

1. DevTools → device toolbar.
2. Test at 375px (iPhone SE), 760px (breakpoint edge), 1200px (desktop).
3. Verify:
   - At 375px and below 760px: cards stack full-width, nav still readable, hero type scales down.
   - At 760px and above: cards in 2×2 grid, hero type scales up.

- [ ] **Step 6: Focus/keyboard path**

1. Click the page, then `Tab` through the whole document.
2. Verify: every link and the CTA button show a visible cyan focus outline (existing `a:focus-visible, button:focus-visible` rule).
3. `Enter` on a project card while focused → opens the repo.

- [ ] **Step 7: Fix any issues, then commit a final tag**

If Steps 1–6 all pass, commit a dummy marker:

```bash
git commit --allow-empty -m "Verify full golden path, reduced-motion, responsive, and a11y"
```

If any step fails, fix the underlying issue in its originating task, verify locally, and commit the fix with a message starting `Fix:`.

---

---

## Task 17: Demote hero CTA and dim nav for hierarchy

**Files:**
- Modify: `index.html` — `.hero .btn.primary` CSS override (new) + `nav .links` CSS

**Why:** Per spec §5.5 — "I build." must be unambiguously the loudest element. The hero CTA is demoted to a ghost outline button; nav links are dimmed one step further.

- [ ] **Step 1: Add hero-scoped CTA override**

Find the `.btn.ghost` rule block:

```css
.btn.ghost {
  border-color: var(--sc-border);
  color: var(--sc-text-secondary);
}
.btn.ghost:hover {
  color: var(--sc-text);
  border-color: rgba(255, 255, 255, 0.14);
}
```

Immediately after `.btn.ghost:hover`, add:

```css
.hero .btn.primary {
  background: transparent;
  color: var(--sc-text-secondary);
  border-color: rgba(200, 233, 255, 0.25);
  padding: 10px 16px;
  font-size: 13px;
}
.hero .btn.primary:hover {
  background: transparent;
  color: var(--sc-text);
  border-color: rgba(200, 233, 255, 0.45);
  transform: translateY(-1px);
  box-shadow: 0 0 20px var(--sc-accent-glow);
}
```

Note: the generic `.btn.primary:hover` rule (Task 14 target) still applies to CTAs *outside* the hero — but there are none in this site. This scoping is purely to enforce hierarchy.

- [ ] **Step 2: Dim nav links**

Find:

```css
nav .links {
  display: flex;
  gap: var(--sc-space-6);
  font-size: 13px;
  color: var(--sc-text-secondary);
}
nav .links a { transition: color 0.3s var(--sc-ease-in-out-soft); }
nav .links a:hover { color: var(--sc-text); }
```

Replace with:

```css
nav .links {
  display: flex;
  gap: var(--sc-space-6);
  font-size: 13px;
  color: var(--sc-text-tertiary);
}
nav .links a { color: var(--sc-text-tertiary); transition: color 0.3s var(--sc-ease-in-out-soft); }
nav .links a:hover { color: var(--sc-text-secondary); }
```

- [ ] **Step 3: Verify**

Reload.

Expected:
- "I build." remains the loudest element.
- "See the work →" button now has no filled background — thin cyan outline, dimmer text.
- Nav links `Work / About / Links` are noticeably more muted than before, still readable.
- On CTA hover: border and text brighten, halo appears.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Demote hero CTA to ghost outline and dim nav for I-build dominance"
```

---

## Task 18: Add viewport border glow frame

**Files:**
- Modify: `index.html` — add `<div class="viewport-frame">` markup and `.viewport-frame` CSS + `sc-viewport-breath` keyframes

**Why:** Per spec §5.5b — subtle cyan glow around the inner edge of the viewport, suggesting a contained screen frame.

- [ ] **Step 1: Add the markup**

Find:

```html
<div class="backdrop" aria-hidden="true"></div>
```

Immediately after it, add:

```html
<div class="viewport-frame" aria-hidden="true"></div>
```

- [ ] **Step 2: Add the CSS**

After the `.backdrop` / `.backdrop::before` rules, add:

```css
.viewport-frame {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 3;
  box-shadow:
    inset 0 0 0 1px rgba(200, 233, 255, 0.04),
    inset 0 0 80px rgba(79, 195, 255, 0.08);
  animation: sc-viewport-breath 8s var(--sc-ease-in-out-soft) infinite;
}
@keyframes sc-viewport-breath {
  0%, 100% {
    box-shadow:
      inset 0 0 0 1px rgba(200, 233, 255, 0.04),
      inset 0 0 80px rgba(79, 195, 255, 0.06);
  }
  50% {
    box-shadow:
      inset 0 0 0 1px rgba(200, 233, 255, 0.05),
      inset 0 0 100px rgba(79, 195, 255, 0.10);
  }
}
@media (prefers-reduced-motion: reduce) {
  .viewport-frame {
    animation: none;
    box-shadow:
      inset 0 0 0 1px rgba(200, 233, 255, 0.04),
      inset 0 0 80px rgba(79, 195, 255, 0.08);
  }
}
```

- [ ] **Step 3: Verify**

Reload.

Expected:
- A very subtle cyan glow sits around the inner edge of the viewport.
- The glow breathes slowly (~8s cycle) — barely visible, must be looked for.
- Frame does not block any clicks (test by clicking nav links and CTAs — they still work).
- Under `prefers-reduced-motion`: frame is static at the midpoint, no breath.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "Add breathing cyan viewport border glow (inset screen frame)"
```

---

## Task 19: Add scroll-cue chevron under hero

**Files:**
- Modify: `index.html` — add `<div class="scroll-cue">` inside hero + CSS + `sc-bounce` keyframes + script-driven fade on scroll

**Why:** Per spec §5.5a — nudges the user to scroll for more, fades away once they do.

- [ ] **Step 1: Add markup inside the hero**

Find the hero block (which currently ends after `</div>` of `.cta-row`):

```html
<section class="hero" id="top">
  <h1 class="reveal shimmer" style="--d:0">I build.</h1>
  <div class="cta-row reveal" style="--d:1">
    <a class="btn primary" href="#work">
      See the work
      <span class="arrow" aria-hidden="true">→</span>
    </a>
  </div>
</section>
```

Replace with:

```html
<section class="hero" id="top">
  <h1 class="reveal shimmer" style="--d:0">I build.</h1>
  <div class="cta-row reveal" style="--d:1">
    <a class="btn primary" href="#work">
      See the work
      <span class="arrow" aria-hidden="true">→</span>
    </a>
  </div>
  <div class="scroll-cue reveal" style="--d:2" aria-hidden="true">↓</div>
</section>
```

(If Task 10 has already added the `shimmer` class to the `<h1>`, keep it. If not, the above line reflects the post-Task-10 state — adjust as needed.)

- [ ] **Step 2: Add the CSS**

After the hero CTA styles (near `.btn .arrow` / `.btn:hover .arrow`), add:

```css
.scroll-cue {
  margin-top: var(--sc-space-12);
  font-size: 16px;
  color: var(--sc-text-tertiary);
  animation: sc-bounce 2s var(--sc-ease-in-out-soft) infinite;
  transition: opacity 0.3s var(--sc-ease-in-out-soft);
}
.scroll-cue.hidden { opacity: 0; }
@keyframes sc-bounce {
  0%, 100% { transform: translateY(0); }
  50%      { transform: translateY(6px); }
}
@media (prefers-reduced-motion: reduce) {
  .scroll-cue { animation: none; }
}
```

- [ ] **Step 3: Add scroll handler in script**

Append to the existing `<script>` block (after whatever other blocks exist — at this point Tasks 9 and 10 have added parallax and shimmer handlers; add this next):

```javascript
  // Scroll cue: fade the chevron once the user has scrolled past the hero
  const scrollCue = document.querySelector('.scroll-cue');
  if (scrollCue) {
    const THRESHOLD = 120;
    let ticking = false;
    const update = () => {
      scrollCue.classList.toggle('hidden', window.scrollY > THRESHOLD);
      ticking = false;
    };
    window.addEventListener('scroll', () => {
      if (!ticking) {
        ticking = true;
        requestAnimationFrame(update);
      }
    }, { passive: true });
  }
```

- [ ] **Step 4: Verify**

Reload.

Expected:
- A small `↓` sits below the CTA button, gently bouncing every 2 seconds.
- Scroll down ≥120px: chevron fades out over 300ms.
- Scroll back to top: chevron reappears.
- Under `prefers-reduced-motion`: chevron is static (no bounce), but still fades on scroll (opacity transition is acceptable per reduced-motion guidelines).

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "Add bouncing scroll-cue chevron that fades after user scrolls"
```

---

## Task 20: Strengthen scroll-reveal motion (pop-in + card flash)

**Files:**
- Modify: `index.html` — `.in-view` CSS + add `.in-view.card` flash keyframes

**Why:** Per spec §5.6a — existing fade-up is too subtle. Switch to a more cinematic pop-in and add a one-time cyan flash on project cards as they enter view.

- [ ] **Step 1: Update `.in-view` / `.in-view.shown`**

Find:

```css
.in-view {
  opacity: 0;
  transform: translateY(16px);
  transition:
    opacity 0.6s var(--sc-ease-out-soft),
    transform 0.6s var(--sc-ease-out-soft);
}
.in-view.shown {
  opacity: 1;
  transform: translateY(0);
}
```

Replace with:

```css
.in-view {
  opacity: 0;
  transform: translateY(32px) scale(0.98);
  transition:
    opacity 0.8s var(--sc-ease-out-soft),
    transform 0.8s var(--sc-ease-out-soft);
}
.in-view.shown {
  opacity: 1;
  transform: translateY(0) scale(1);
}
.card.in-view.shown {
  animation: sc-card-flash 1.2s var(--sc-ease-out-soft) 0s 1;
}
@keyframes sc-card-flash {
  0%   { box-shadow: 0 0 0 0 rgba(79, 195, 255, 0); }
  40%  { box-shadow: 0 0 28px 2px var(--sc-accent-glow); }
  100% { box-shadow: 0 0 0 0 rgba(79, 195, 255, 0); }
}
@media (prefers-reduced-motion: reduce) {
  .card.in-view.shown { animation: none; }
}
```

Note: the existing reduced-motion block (`.in-view { opacity: 1 !important; transform: none !important; transition: none !important; }`) already handles the base `.in-view` / `.in-view.shown` reduced-motion case — no further change needed there.

- [ ] **Step 2: Verify**

Reload.

Expected:
- Scrolling into any section/card produces a more pronounced pop-in: elements rise 32px (up from 16px) with a slight scale-up.
- Project cards additionally flash a brief cyan glow as they enter view — the flash peaks around 40% of the animation (~480ms in), then fades to nothing by 1200ms. The hover glow (added in Task 12) is unaffected and still works independently.
- Under `prefers-reduced-motion`: all reveals are instant, no flash.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "Strengthen scroll-reveal motion and add cyan flash on cards entering view"
```

---

## Out of plan / intentionally deferred

- Any server-side behavior (contact form, analytics) — site stays fully static.
- `og:image` social preview — can add later.
- Writing/blog index — not in this spec.
- Automated tests (Playwright, etc.) — no framework; golden-path in Task 16 is the verification.
