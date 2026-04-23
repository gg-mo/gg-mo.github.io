# Personal Site — Design Spec

**Date:** 2026-04-22
**Owner:** Gao Mo (`gg-mo`)
**Target:** `gg-mo.github.io` (served by GitHub Pages)
**Status:** Approved — ready for implementation planning

---

## 1. Purpose

Turn the existing `index.html` template into Gao Mo's personal site: a classy, minimal portfolio + links hub with a restrained "futuristic" aesthetic (cool cyan glow, subtle grid backdrop). Voice is terse and confident — the hero is literally two words.

Site combines:
- **Portfolio** — four real GitHub projects as the centerpiece.
- **Links hub** — LinkedIn, GitHub, email as the primary calls-to-action for reaching out.

## 2. Non-goals

- No framework, no build step, no bundler. Single static `index.html` deployable as-is to GitHub Pages.
- No blog or writing section (for now).
- No contact form, no analytics, no telemetry.
- No cyberpunk-loud neon. Glow is accent, not substance.
- No job title, role, or employer text surfaced on the page (explicit user choice — see §4.1).

## 3. Success criteria

- Loads as a single `index.html` with no external build. GitHub Pages serves it directly.
- Hero reads `I build.` with a one-time cyan shimmer on load, then settles.
- Four project cards render with real repo data (title, description, topic chips, language, stars, updated date) and open the repo in a new tab on click.
- Hover on project cards raises a cyan outer glow and a cursor-follow spotlight.
- `prefers-reduced-motion` disables all shimmer, parallax, and card spotlight; fades still respect the existing path.
- Lighthouse accessibility score ≥ 95 (baseline sanity).
- Visually verified in a browser: golden path (hero → scroll → hover cards → click through to GitHub) + reduced-motion emulation.

## 4. Content

### 4.1 Hero

- **Eyebrow:** *omitted*. No role, no employer, no year kicker. Naked hero.
- **Headline:** `I build.` — single line, unchanged `h1` size (clamp 40–72px), weight 500, letter-spacing -0.025em.
- **Lede:** *omitted*. The two-word headline is the whole statement.
- **CTA row:** one button only — `See the work →` (primary). The ghost `Get in touch` button is removed; contact lives in the Links section and nav.

### 4.2 Selected work

Four project cards, pulled from:

All four cards are the **same size** — no featured/wide card.

| # | Repo | Title shown | Description | Chips (from GitHub topics) |
|---|---|---|---|---|
| 1 | `gg-mo/second-thought-agent-skill` | `second-thought-agent-skill` | A critique-first agent skill that challenges every decision before execution — catching flaws, surfacing risks, and improving outcomes in real time. | `agent-skill` · `ai-safety` · `claude-code` |
| 2 | `gg-mo/stagecraft-agent-skill` | `stagecraft-agent-skill` | Claude Code plugin for premium, keynote-style UI — dark, minimal, cinematic reveals with disciplined motion. | `claude-code-plugin` · `motion-design` · `design-system` |
| 3 | `gg-mo/agent-tea` | `agent-tea` | A personality test where AI agents and chatbots spill the tea on their humans. | `ai-personality-test` · `nextjs` · `supabase` |
| 4 | `NoTestsNoFear/Flexing` | `Flexing` | Flex. Roast. Build. — the feed-first community where humans and AI agents show their work, get roasted, build reputation, and get hired. | `ai-agents` · `community-platform` · `mcp` |

Card content is **hardcoded in HTML** (not fetched at runtime) — values copied from GitHub repo metadata at build/edit time. Acceptable because the repo set is small and changes rarely; avoids an API dependency and keeps the page fully static.

Section heading: `Selected work` (replaces `Selected projects`). Kicker: `2025 — 2026`.

### 4.3 About

One compact paragraph, understated. Reference draft:

> I build tools for thinking — agent skills, ML systems, and the quiet interfaces that make models feel inevitable.

No explicit job title on the page. (User choice; revisit later if desired.)

Section heading unchanged. Kicker unchanged.

### 4.4 Links (replaces Contact)

Three links, plain text, bullet-separated:
- `LinkedIn` → `https://www.linkedin.com/in/gao-mo/`
- `GitHub` → `https://github.com/gg-mo`
- `Email` → `mailto:happyhhour123@gmail.com`

Section heading: `Links`. Kicker: `Open inbox`. The existing two-column "label / body" layout is retained.

### 4.5 Nav

`Work` · `About` · `Links` (Links replaces Contact).

### 4.6 Footer

- Left: `© 2026 Gao Mo` + small monospace touch: `// end of line`.
- Right: `Back to top` · `GitHub`.

### 4.7 Page `<title>`

`Gao Mo` (replaces `gg-mo — Work & Projects`).

## 5. Visual system — "restrained futuristic"

### 5.1 Color tokens (additions / changes)

Existing palette is kept; the following are added or changed.

| Token | Value | Use |
|---|---|---|
| `--sc-accent` (changed) | `#c8e9ff` | Near-white with a cyan bias. Still used for primary button + focus rings. |
| `--sc-accent-glow` (new) | `rgba(79, 195, 255, 0.35)` | Outer glow on card hover, CTA halo, divider scanline peak. |
| `--sc-accent-deep` (new) | `#4fc3ff` | Used *only* for small highlights (card chevrons, mono path prefix). Never as a large fill. |
| `--sc-grid` (new) | `rgba(255, 255, 255, 0.05)` | Faint grid line color. |
| `--sc-mono` (new) | `"JetBrains Mono", ui-monospace, SFMono-Regular, Menlo, monospace` | Terminal-style lines and chips. |

### 5.2 Ambient backdrop

Augment the existing radial gradient with a **faint fixed grid**:
- 1px lines, 48px cell, `var(--sc-grid)`.
- Rendered as an SVG/CSS background on the existing `.backdrop` layer. Does not scroll.
- Mouse parallax: the grid drifts ≤4px in each axis based on cursor position, smoothed. Disabled under `prefers-reduced-motion`.

### 5.3 Hero shimmer

One-time reveal only:
- On load, after the existing `sc-fade-up` settles, sweep a cyan sheen across the `h1` letters over ~1.2s using a `background-clip: text` gradient animation.
- After the sweep completes, the headline remains solid `var(--sc-text)`. No looping.
- Disabled under `prefers-reduced-motion` — headline fades in as plain white.

### 5.4 Project cards — terminal preview

Each card's new visual structure:

```
┌──────────────────────────────────────────┐
│ ● ● ●                        [language]  │  ← faux window chrome + language chip
│                                          │
│ ~/gg-mo/repo-name                        │  ← monospace path, --sc-accent-deep prefix
│                                          │
│ Project Title                            │  ← existing h3 style
│ One-line description from repo.          │  ← existing p style
│                                          │
│ [topic] [topic] [topic]                  │  ← chip row, monospace, 11px
│                                          │
│ ─────────────────────────────────────── │
│  Updated Apr 2026              View →   │  ← stat footer
└──────────────────────────────────────────┘
```

Card behavior:
- Hover: existing translateY(-2px) lift is kept. **Add** a cyan outer `box-shadow` using `--sc-accent-glow`, fading in over 400ms.
- Hover: **cursor-follow spotlight** — a radial gradient mask inside the card at the pointer position, ~18% opacity cyan. Implemented with a `::before` layer using CSS variables updated from a `mousemove` handler (throttled via `requestAnimationFrame`). Disabled under `prefers-reduced-motion`.
- Click: opens the repo URL in a new tab (`target="_blank"`, `rel="noopener"`).

### 5.5 CTA button — demoted for hero dominance

"I build." must be unambiguously the loudest element on the page. To enforce this, the primary CTA under the hero is **demoted to a ghost-style outline button**:

- Background: transparent (not `var(--sc-accent)` filled).
- Border: `1px solid rgba(200, 233, 255, 0.25)` (thin cyan hairline).
- Text color: `var(--sc-text-secondary)` at rest.
- Padding: reduced to `10px 16px` (from `12px 20px`).
- Font size: `13px` (from `14px`).
- On hover: border and text brighten to `var(--sc-text)`; a **soft cyan halo** fades in via `box-shadow: 0 0 20px var(--sc-accent-glow)` over 300ms. Existing `translateY(-1px)` lift is kept.
- The `.btn.primary` class keeps its name (no CSS rename needed) — its filled-background rule is overridden inside the hero context by `.hero .btn.primary { ... }` scoping.

Nav links are dimmed one step further: from `--sc-text-secondary` (60%) to `--sc-text-tertiary` (35%) at rest, lifting to `--sc-text-secondary` on hover. This maintains a clear visual hierarchy: `I build.` > everything else.

### 5.5a Scroll cue

A small animated chevron sits ~48px below the CTA row:
- Glyph: `↓` rendered at 16px, `var(--sc-text-tertiary)` color.
- Animation: gentle 2s `sc-bounce` — `translateY(0)` ↔ `translateY(6px)`, infinite, easing out/in.
- Fades to opacity 0 over 300ms once the user has scrolled >120px. Re-shows if the user scrolls back to the top.
- Disabled (static, no bounce) under `prefers-reduced-motion`.

### 5.5b Viewport border glow

A fixed overlay renders a very subtle cyan glow along the inner edge of the viewport, suggesting a contained screen frame:
- `.viewport-frame` element, `position: fixed; inset: 0; pointer-events: none; z-index: 3` (above backdrop and content, but since it has no pointer events and no solid fill, it never blocks interactions).
- Primary effect: `box-shadow: inset 0 0 80px rgba(79, 195, 255, 0.08)`.
- Secondary effect: 1px inset border at `rgba(200, 233, 255, 0.04)` to sharpen the edge.
- Slow breath animation: `sc-viewport-breath` 8s infinite, opacity varying between 0.06 and 0.10 on the inset glow.
- Under `prefers-reduced-motion`, the breath animation is removed; the frame stays static at the midpoint (opacity 0.08).

### 5.6 Section dividers

The existing `border-bottom` under `.section-head` gains a companion **scanline**:
- A 1px gradient line overlaid on the border, near-transparent by default.
- When the section scrolls into view, the scanline brightens to `--sc-accent-glow` over 600ms, then dims to 15% over the next 1.2s and stays there.
- Triggered via the existing `IntersectionObserver` pipeline (piggyback on `.in-view`).

### 5.6a Stronger scroll-reveal motion

The existing `.in-view` → `.in-view.shown` transition (`translateY(16px) → 0`, opacity `0 → 1`, duration 0.6s) is replaced by a more cinematic pop-in:

- **Start state:** `opacity: 0; transform: translateY(32px) scale(0.98);`
- **End state:** `opacity: 1; transform: translateY(0) scale(1);`
- **Duration:** 0.8s, easing `var(--sc-ease-out-soft)`.
- **Flash:** project cards additionally run a one-time `sc-card-flash` keyframe when they enter view — a brief cyan outer box-shadow pulse (0 → `--sc-accent-glow` → low-opacity baseline), 1.2s, then leaves the baseline shadow in place. Non-card `.in-view` elements do not flash.
- Staggered delays (existing `el.style.transitionDelay = (i % 6) * 80 + 'ms'`) are preserved.
- Under `prefers-reduced-motion`, all of the above are disabled: `.in-view` renders at rest-state immediately (opacity 1, no transform, no flash).

### 5.7 Grid layout for cards

12-col grid. **All four cards span 6 cols** — forming a 2×2 grid on desktop. The `.card.wide` modifier from the current template is removed (all cards use the default size). Gap between cards stays at `var(--sc-space-4)` (16px). Breakpoint: below 760px, all cards stack full-width.

## 6. Typography

- **Sans (unchanged):** Inter 400/500/600 via Google Fonts.
- **Mono (new):** JetBrains Mono 400/500 via Google Fonts. Used in: card window path line, card topic chips, card stat footer, footer `// end of line`.

## 7. Favicon

Inline SVG favicon, embedded via `<link rel="icon" href="data:image/svg+xml,...">`:
- 32×32 viewport.
- A single filled circle, radius ~12, centered.
- Fill: `--sc-accent-deep` (`#4fc3ff`) with a soft outer blur.
- Matches the `nav .mark .dot` visual.

No separate file; lives inline in `<head>` to avoid an extra request.

## 8. Accessibility

- All color pairs maintain ≥4.5:1 contrast for body text, ≥3:1 for large text.
- `prefers-reduced-motion` disables: hero shimmer, grid parallax, card spotlight, scanline brighten. Fades are kept as existing `@media (prefers-reduced-motion: reduce)` rules already permit them.
- Interactive elements keep visible `:focus-visible` outlines (existing rule).
- Project card tiles are `<a>` elements with `aria-label` describing the project (e.g., `second-thought-agent-skill — critique-first agent skill`).
- Topic chips are decorative text; no separate interactive targets inside the card.

## 9. File layout

Single file, unchanged in structure:

```
index.html   ← all HTML + inline <style> + inline <script>
README.md    ← optional, leave as-is
```

No build step, no package.json, no dependencies beyond Google Fonts (already present).

## 10. Testing plan

Manual, in a browser:

1. **Load test** — open `index.html` locally (or via `python3 -m http.server`). Confirm hero shimmer plays once, grid is visible, no console errors.
2. **Hover test** — hover each of the 4 cards. Confirm lift + cyan glow + cursor-follow spotlight.
3. **Click test** — click each card. Confirm correct repo URL opens in new tab.
4. **Nav test** — click `Work`, `About`, `Links`. Confirm smooth scroll to each section.
5. **Reduced motion test** — in DevTools, emulate `prefers-reduced-motion: reduce`. Reload. Confirm no shimmer, no parallax, no spotlight, but fades still work and content is visible.
6. **Responsive test** — resize to 375px, 760px, 1200px. Confirm cards stack correctly and nav remains readable.
7. **Email link test** — click email link. Confirm `mailto:` opens.
8. **Focus test** — tab through the page. Confirm all interactive elements show focus outlines.

## 11. Out of scope (explicit non-features)

- Dark/light theme toggle — site is dark-only.
- Per-project detail pages / case studies — cards link straight to GitHub.
- CMS / markdown-driven project list — content is hardcoded in `index.html`.
- Analytics.
- RSS feed / writing section.
- Social card preview images (`og:image`) — can be added later if shared.

## 12. Open questions (none — all resolved)

- ✅ Hero copy: `I build.`
- ✅ Role/title on page: omit entirely
- ✅ Email: `happyhhour123@gmail.com`
- ✅ Mono font: JetBrains Mono
- ✅ Favicon: generated inline SVG (cyan filled circle)
