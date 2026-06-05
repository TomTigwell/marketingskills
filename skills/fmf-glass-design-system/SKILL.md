---
name: fmf-glass-design-system
description: >
  The Fill My Funnel "glass" design system, extracted verbatim from complete-linkedinads.html. Use this skill any time you build a standalone HTML deliverable for a Fill My Funnel use case — client strategies, proposals, one-pagers, pitch decks, landing pages, internal reports — and want the page to look like an FMF artefact rather than a generic template. Triggers include "build a strategy doc / pitch / one-pager / proposal / deck / landing page in the FMF style", "use the glass design system", "make it look like the Complete deck / Andy Webb proposal", or whenever the user asks for output to feel premium, calm, and on-brand without prescribing exact components. This skill defines the colour palette, typography (Arizona Flare display + Inter body, italic-electric callouts), the signature glassmorphic mesh background with funnel watermark, the centred floating glass nav, glass cards, pills, hero, section heads, grids, waterfall, step lists, and the connector/opportunity callout patterns. Apply it as the default look-and-feel for any FMF HTML output where another skill (fmf-proposal-builder, fmf-eom-report, etc.) does not already prescribe its own layout. Do NOT trigger for: code reviews, non-HTML deliverables, or generic non-FMF web design tasks.
---

# FMF Glass Design System

The visual language for every Fill My Funnel HTML artefact. Premium, calm, blue-family, glassmorphic. Cornflower for callouts, navy/prussian for gravitas, italic-electric on the punchword in every heading.

This skill is reference material — it does not produce a specific document on its own. Other skills (proposal-builder, EOM report, strategy doc) call into this system. When the user asks for "a strategy / one-pager / pitch / landing page in the FMF style", apply the patterns below to build a single self-contained HTML file.

---

## When to apply

Use this design system as the default skin whenever you produce HTML for FMF that is **not already governed by another skill's own layout**. Specifically:

- Client strategy docs and one-pagers
- Pitch decks and proposals (unless `fmf-proposal-builder` is the better fit)
- Landing pages, microsites, sales collateral
- Internal HTML reports and dashboards that need to feel branded
- Any "make it look like the Complete deck" request

Do not override another skill's design choices — if `fmf-eom-report` says use red/green status colours, use those. This skill is the **fallback house style**.

---

## Hard rules

1. **Stay in the blue family.** Cornflower, electric, pale-sky, prussian, dusk. No orange, no warm yellow except as a status colour (yellow = critical alignment / needs sign-off, used sparingly).
2. **Italic-electric on the punchword in every H1 and H2.** `<em>` styles to `font-style: italic; color: var(--electric); font-weight: 400;` — purely a visual highlight, not a copy edit.
3. **Glass cards over flat fills.** Translucent white with 1px white-translucent border, blur(20–28px), soft cornflower-tinted shadow.
4. **Centred floating glass nav, never a top bar.** Pill-shaped, fixed `top: 20px`, horizontally centred via `left:50%; transform: translateX(-50%)`. Backdrop blur. Active tab is prussian-filled with white text.
5. **Eyebrows are cornflower, uppercase, 12px, letter-spacing 0.14em.** They sit above every section H2.
6. **Background is the signature mesh** (radial gradients + diagonal linear gradient + faint funnel watermark). Never a flat colour.
7. **Display type is Arizona Flare** (or Fraunces fallback). Body is Inter. No other fonts.
8. **Numbers in display type** (the metric, the price, the stat) — large, light weight, tight letter-spacing, prussian or electric colour.

---

## Drop-in CSS bundle

Paste this into the `<head>` of any HTML deliverable. It is self-contained and works on a single `*.html` file with no build step. Font fallbacks (Inter from Google, Fraunces as serif fallback) load via the Google Fonts link.

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,wght@0,400;0,500;0,700;1,400&family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
<style>
:root {
  /* Colour palette — blue family, cool light */
  --cornflower: #6A88FD;
  --pale-sky: #C0D5F0;
  --electric: #5068FF;      /* italic-em callout colour, primary CTA */
  --dusk: #5F4C78;
  --prussian: #111C33;      /* primary text, dark cards, active tab */
  --blue-black: #0C1010;
  --alice: #E7ECF3;
  --platinum: #F2F5F8;
  --white: #FFFFFF;
  --blue-300: #1D6BFE;
  --green: #05C168;         /* semantic only — success / fit / done */

  /* Text */
  --text-strong: #111C33;
  --text-body: rgba(17,28,51,0.75);
  --text-muted: rgba(17,28,51,0.55);
  --text-faint: rgba(17,28,51,0.35);

  /* Spacing */
  --xs: 8px; --sm: 16px; --md: 32px; --lg: 48px; --xl: 80px; --xxl: 120px;

  /* Radii */
  --r-sm: 8px; --r-md: 16px; --r-lg: 28px; --r-xl: 48px; --r-pill: 100px;

  /* Shadows */
  --shadow-soft: 0 2px 16px rgba(106,136,253,0.07);
  --shadow-card: 0 1px 3px rgba(17,28,51,0.04), 0 8px 24px rgba(17,28,51,0.06);
  --shadow-pop:  0 12px 48px rgba(17,28,51,0.12);
  --shadow-cta:  0 1px 0 rgba(255,255,255,0.12) inset, 0 8px 20px rgba(17,28,51,0.18);

  /* Type — Arizona Flare is the canonical display face; Fraunces is the closest free fallback */
  --font-display: 'ABC Arizona Flare', 'Fraunces', Georgia, 'Times New Roman', serif;
  --font-body:    'Inter', 'Helvetica Neue', Arial, sans-serif;

  /* The signature glassy mesh background — never a flat colour */
  --axis-bg:
    radial-gradient(at 12% 8%,  rgba(106,136,253,0.22) 0%, transparent 45%),
    radial-gradient(at 88% 12%, rgba(192,213,240,0.55) 0%, transparent 50%),
    radial-gradient(at 78% 78%, rgba(95,76,120,0.10)   0%, transparent 55%),
    radial-gradient(at 18% 92%, rgba(106,136,253,0.18) 0%, transparent 50%),
    linear-gradient(135deg, #F2F5F8 0%, #E7ECF3 60%, #DDE5F2 100%);
}

* { box-sizing: border-box; margin: 0; padding: 0; }

html, body {
  font-family: var(--font-body);
  color: var(--text-strong);
  font-size: 17px;
  line-height: 1.55;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background: var(--axis-bg);
  background-attachment: fixed;
  min-height: 100vh;
}

/* Giant funnel watermark — signature decorative element, fixed centre, very low opacity */
body::before {
  content: "";
  position: fixed;
  top: 50%; left: 50%;
  width: 720px; height: 720px;
  transform: translate(-50%, -50%);
  background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><path d='M15 22 L85 22 L58 58 L58 82 Q58 88 50 88 Q42 88 42 82 L42 58 Z' fill='%23111C33'/></svg>");
  background-repeat: no-repeat;
  background-size: contain;
  opacity: 0.018;
  z-index: 0;
  pointer-events: none;
}

/* ─── Floating centred glass nav ─── */
.nav {
  position: fixed;
  top: 20px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 100;
  background: rgba(255,255,255,0.62);
  border: 1px solid rgba(255,255,255,0.85);
  backdrop-filter: blur(24px);
  -webkit-backdrop-filter: blur(24px);
  border-radius: var(--r-pill);
  padding: 8px;
  display: flex;
  align-items: center;
  gap: 4px;
  box-shadow: 0 4px 32px rgba(106,136,253,0.12), inset 0 1px 0 rgba(255,255,255,0.85);
  max-width: calc(100vw - 40px);
  overflow-x: auto;
  scrollbar-width: none;
}
.nav::-webkit-scrollbar { display: none; }

.nav-brand {
  display: flex; align-items: center; gap: 8px;
  padding: 6px 12px 6px 14px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 16px;
  color: var(--prussian);
  letter-spacing: -0.01em;
  white-space: nowrap;
}
.nav-brand span.x { opacity: 0.4; padding: 0 2px; }

.nav-tabs { display: flex; gap: 2px; }

.tab-btn {
  font-family: var(--font-body);
  font-size: 13px;
  font-weight: 500;
  color: var(--text-body);
  background: transparent;
  border: none;
  padding: 9px 14px;
  border-radius: var(--r-pill);
  cursor: pointer;
  display: flex; align-items: center; gap: 6px;
  white-space: nowrap;
  transition: background 200ms ease-out, color 200ms ease-out;
}
.tab-btn:hover { background: rgba(106,136,253,0.10); color: var(--prussian); }
.tab-btn .num {
  display: inline-flex; align-items: center; justify-content: center;
  width: 18px; height: 18px;
  background: rgba(17,28,51,0.08);
  color: var(--prussian);
  font-size: 10px;
  font-weight: 700;
  border-radius: 50%;
}
.tab-btn.active {
  background: var(--prussian);
  color: var(--white);
  box-shadow: var(--shadow-cta);
}
.tab-btn.active .num { background: rgba(255,255,255,0.18); color: var(--white); }

/* ─── Layout shell ─── */
.shell {
  position: relative;
  z-index: 1;
  max-width: 1180px;
  margin: 0 auto;
  padding: 120px 32px 80px;
}
.panel { display: none; }
.panel.active { display: block; animation: panelIn 400ms ease-out; }
@keyframes panelIn {
  from { opacity: 0; transform: translateY(12px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* ─── Typography ─── */
.eyebrow {
  font-family: var(--font-body);
  font-size: 12px;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.14em;
  color: var(--cornflower);
  margin-bottom: 18px;
}

h1.display {
  font-family: var(--font-display);
  font-weight: 400;
  font-size: clamp(48px, 6.4vw, 92px);
  line-height: 1.04;
  letter-spacing: -0.03em;
  color: var(--prussian);
  margin-bottom: 32px;
}
h1.display em { font-style: italic; color: var(--electric); font-weight: 400; }

h2.section {
  font-family: var(--font-display);
  font-weight: 400;
  font-size: clamp(36px, 4.4vw, 60px);
  line-height: 1.08;
  letter-spacing: -0.025em;
  color: var(--prussian);
  margin-bottom: 24px;
  max-width: 900px;
}
h2.section em { font-style: italic; color: var(--electric); font-weight: 400; }

h3 {
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 26px;
  line-height: 1.15;
  letter-spacing: -0.02em;
  color: var(--prussian);
  margin-bottom: 12px;
}
h4 {
  font-family: var(--font-body);
  font-weight: 700;
  font-size: 15px;
  color: var(--prussian);
  margin-bottom: 8px;
  text-transform: none;
}
p { color: var(--text-body); margin-bottom: 16px; font-size: 16px; line-height: 1.6; }
p:last-child { margin-bottom: 0; }
.lead { font-size: 19px; line-height: 1.55; color: var(--text-body); max-width: 760px; }
strong { color: var(--prussian); font-weight: 700; }

/* ─── Glass cards ─── */
.glass {
  background: rgba(255,255,255,0.42);
  border: 1px solid rgba(255,255,255,0.78);
  border-radius: var(--r-lg);
  backdrop-filter: blur(24px);
  -webkit-backdrop-filter: blur(24px);
  box-shadow: 0 4px 32px rgba(106,136,253,0.10), inset 0 1px 0 rgba(255,255,255,0.85);
  padding: 36px;
}
.glass-xl {
  background: rgba(255,255,255,0.46);
  border: 1px solid rgba(255,255,255,0.82);
  border-radius: var(--r-xl);
  backdrop-filter: blur(28px);
  -webkit-backdrop-filter: blur(28px);
  box-shadow: 0 8px 48px rgba(106,136,253,0.14), inset 0 1px 0 rgba(255,255,255,0.90);
  padding: 56px;
}

/* ─── Pills & badges ─── */
.pill {
  display: inline-flex; align-items: center; gap: 6px;
  padding: 7px 14px;
  border-radius: var(--r-pill);
  font-family: var(--font-body);
  font-size: 11px;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.14em;
}
.pill-eyebrow {
  background: rgba(106,136,253,0.12);
  color: var(--electric);
  border: 1px solid rgba(106,136,253,0.22);
}
.pill-dark { background: var(--prussian); color: var(--white); }
.pill-soft { background: rgba(17,28,51,0.06); color: var(--prussian); }
.pill-green {
  background: rgba(5,193,104,0.12);
  color: #058a4a;
}
.pill-green .dot {
  width: 6px; height: 6px; border-radius: 50%;
  background: var(--green);
  box-shadow: 0 0 0 3px rgba(5,193,104,0.22);
  animation: pulse 2s ease-in-out infinite;
}
@keyframes pulse { 0%,100% { opacity: 1; } 50% { opacity: 0.6; } }

/* ─── Hero (cover panel) ─── */
.hero { padding-top: 16px; }
.hero .pill-row { display: flex; gap: 8px; flex-wrap: wrap; margin-bottom: 32px; }
.hero-stats {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
  margin-top: 56px;
}
.stat {
  padding: 24px 24px 22px;
  background: rgba(255,255,255,0.38);
  border: 1px solid rgba(255,255,255,0.72);
  border-radius: var(--r-md);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
}
.stat .num {
  font-family: var(--font-display);
  font-size: 44px;
  font-weight: 400;
  line-height: 1;
  color: var(--prussian);
  letter-spacing: -0.03em;
  margin-bottom: 6px;
}
.stat .label { font-size: 13px; color: var(--text-muted); line-height: 1.4; }
.hero-meta {
  display: flex; gap: 24px; align-items: center;
  margin-top: 40px;
  font-size: 13px;
  color: var(--text-muted);
}
.hero-meta .dot-sep { width: 3px; height: 3px; background: var(--text-faint); border-radius: 50%; }

/* ─── Section heads ─── */
.section-head { margin-bottom: 48px; max-width: 920px; }

/* ─── Grids ─── */
.grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 24px; margin-bottom: 24px; }
.grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 24px; margin-bottom: 24px; }
.grid-4 { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; }

/* ─── Feature cards ─── */
.feature {
  background: rgba(255,255,255,0.50);
  border: 1px solid rgba(255,255,255,0.80);
  border-radius: var(--r-lg);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  box-shadow: var(--shadow-soft);
  padding: 32px;
  transition: transform 200ms ease-out, box-shadow 200ms ease-out;
}
.feature:hover {
  transform: translateY(-2px);
  box-shadow: var(--shadow-card);
}
.feature .num-lg {
  font-family: var(--font-display);
  font-size: 14px;
  font-weight: 500;
  color: var(--cornflower);
  letter-spacing: 0.1em;
  margin-bottom: 14px;
  display: block;
}

/* ─── Step list (numbered cards) ─── */
.steps { display: grid; grid-template-columns: repeat(2, 1fr); gap: 16px; }
.step {
  background: rgba(255,255,255,0.48);
  border: 1px solid rgba(255,255,255,0.78);
  border-radius: var(--r-lg);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  padding: 28px;
  display: flex; gap: 18px; align-items: flex-start;
}
.step .step-num {
  flex: 0 0 auto;
  width: 40px; height: 40px;
  background: var(--prussian);
  color: var(--white);
  border-radius: 50%;
  display: flex; align-items: center; justify-content: center;
  font-family: var(--font-display);
  font-size: 14px;
  font-weight: 500;
}
.step .step-body h4 { font-size: 17px; font-weight: 700; margin-bottom: 6px; }
.step .step-body p  { font-size: 14px; line-height: 1.55; margin: 0; }

/* ─── Waterfall (funnel stages) ─── */
.waterfall { display: flex; flex-direction: column; gap: 12px; max-width: 720px; margin: 0 auto; }
.wf-row {
  display: grid; grid-template-columns: 1fr auto; align-items: center;
  padding: 22px 28px;
  background: rgba(255,255,255,0.56);
  border: 1px solid rgba(255,255,255,0.82);
  border-radius: var(--r-md);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
}
.wf-row .wf-label { font-family: var(--font-display); font-size: 18px; letter-spacing: -0.01em; color: var(--prussian); }
.wf-row .wf-num   { font-family: var(--font-display); font-size: 32px; font-weight: 400; color: var(--prussian); letter-spacing: -0.02em; }
.wf-row.tier-1 { background: rgba(255,255,255,0.40); }
.wf-row.tier-2 { background: rgba(192,213,240,0.45); }
.wf-row.tier-3 { background: rgba(106,136,253,0.22); }
.wf-row.tier-4 { background: rgba(80,104,255,0.20); }
.wf-row.tier-5 { background: var(--prussian); }
.wf-row.tier-5 .wf-label, .wf-row.tier-5 .wf-num { color: var(--white); }
.wf-arrow { align-self: center; font-size: 20px; color: var(--text-faint); text-align: center; margin: -2px 0; }

/* ─── Evidence stats (big metric blocks) ─── */
.evidence-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 24px; }
.evidence {
  background: rgba(255,255,255,0.52);
  border: 1px solid rgba(255,255,255,0.82);
  border-radius: var(--r-lg);
  backdrop-filter: blur(22px);
  -webkit-backdrop-filter: blur(22px);
  box-shadow: var(--shadow-soft);
  padding: 36px 36px 32px;
}
.evidence .ev-stat {
  font-family: var(--font-display);
  font-size: 88px;
  font-weight: 400;
  line-height: 0.95;
  letter-spacing: -0.04em;
  color: var(--electric);
  margin-bottom: 18px;
}
.evidence .ev-stat .ev-unit { font-size: 38px; color: var(--prussian); letter-spacing: -0.02em; }
.evidence h4 { font-family: var(--font-display); font-weight: 400; font-size: 22px; letter-spacing: -0.015em; text-transform: none; color: var(--prussian); margin-bottom: 8px; line-height: 1.2; }
.evidence p  { font-size: 14px; line-height: 1.55; margin: 0; color: var(--text-body); }

/* ─── Opportunity callout (dark prussian card with funnel watermark) ─── */
.opp-card {
  background: var(--prussian);
  color: var(--white);
  border-radius: var(--r-lg);
  padding: 36px 40px;
  margin-bottom: 32px;
  position: relative;
  overflow: hidden;
}
.opp-card::before {
  content: "";
  position: absolute;
  top: -40%; right: -10%;
  width: 380px; height: 380px;
  background-image: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><path d='M15 22 L85 22 L58 58 L58 82 Q58 88 50 88 Q42 88 42 82 L42 58 Z' fill='%23FFFFFF'/></svg>");
  background-repeat: no-repeat; background-size: contain;
  opacity: 0.04;
  pointer-events: none;
}
.opp-card .opp-eyebrow { font-family: var(--font-body); font-size: 11px; font-weight: 500; text-transform: uppercase; letter-spacing: 0.14em; color: var(--pale-sky); margin-bottom: 14px; }
.opp-card h3.opp-head { font-family: var(--font-display); font-weight: 400; font-size: 32px; line-height: 1.15; letter-spacing: -0.02em; color: var(--white); margin-bottom: 18px; max-width: 720px; position: relative; z-index: 1; }
.opp-card h3.opp-head em { font-style: italic; color: var(--pale-sky); font-weight: 400; }
.opp-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 24px; margin-top: 4px; position: relative; z-index: 1; }
.opp-col h4 { font-family: var(--font-body); font-size: 12px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.12em; color: var(--cornflower); margin-bottom: 8px; }
.opp-col p  { color: rgba(255,255,255,0.78); font-size: 15px; line-height: 1.55; margin: 0; }
.opp-col p strong { color: var(--white); font-weight: 700; }

/* ─── Connector arrow (vertical hand-off between sections) ─── */
.connector { display: flex; flex-direction: column; align-items: center; margin: 28px 0 24px; gap: 10px; }
.connector .ctr-label { display: inline-flex; align-items: center; gap: 8px; padding: 8px 18px; background: var(--electric); color: var(--white); border-radius: var(--r-pill); font-family: var(--font-body); font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.14em; box-shadow: var(--shadow-cta); }
.connector .ctr-line { width: 2px; height: 36px; background: linear-gradient(to bottom, var(--electric), var(--cornflower)); border-radius: 2px; position: relative; }
.connector .ctr-line::after { content: ""; position: absolute; bottom: -8px; left: 50%; transform: translateX(-50%) rotate(45deg); width: 10px; height: 10px; border-right: 2px solid var(--cornflower); border-bottom: 2px solid var(--cornflower); }
.connector .ctr-sub { font-size: 13px; color: var(--text-muted); text-align: center; max-width: 540px; margin-top: 4px; }
</style>
```

---

## Page skeleton

Every artefact follows the same scaffold: floating centred nav at top, then `.shell` container, then one or more `.panel` sections (use multiple panels + tab buttons for a deck; use one panel for a single-scroll page).

```html
<body>

  <!-- Floating centred glass nav -->
  <nav class="nav">
    <div class="nav-brand">
      <span>Fill My Funnel</span>
      <span class="x">×</span>
      <span>{Client Name}</span>
    </div>
    <div class="nav-tabs">
      <button class="tab-btn active" data-tab="why"><span class="num">1</span>Why</button>
      <button class="tab-btn" data-tab="how"><span class="num">2</span>How</button>
      <button class="tab-btn" data-tab="what"><span class="num">3</span>What we do</button>
      <button class="tab-btn" data-tab="invest"><span class="num">4</span>Investment</button>
    </div>
  </nav>

  <div class="shell">
    <section class="panel active" data-panel="why">
      <div class="hero">
        <div class="pill-row">
          <span class="pill pill-eyebrow">EMEA's only LinkedIn Ads official marketing partner</span>
        </div>
        <h1 class="display">LinkedIn Advertising<br>for <em>{Client}</em>.</h1>
        <p class="lead">{One-sentence positioning statement. Sets up the strategy in plain English.}</p>
        <div class="hero-meta">
          <span>{Month YYYY}</span>
          <span class="dot-sep"></span>
          <span>fillmyfunnel.co.uk</span>
          <span class="dot-sep"></span>
          <span>Prepared for {Contact Name}</span>
        </div>
      </div>
      <!-- sections, callouts, stats, waterfall here -->
    </section>
  </div>

  <script>
    // Panel tab switching
    document.querySelectorAll('.tab-btn').forEach(btn => {
      btn.addEventListener('click', () => {
        const tab = btn.dataset.tab;
        document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
        document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
        btn.classList.add('active');
        document.querySelector(`.panel[data-panel="${tab}"]`).classList.add('active');
        window.scrollTo({ top: 0, behavior: 'smooth' });
      });
    });
  </script>
</body>
```

---

## Component cheat-sheet

### Section header (use above every H2)

```html
<div class="section-head">
  <div class="eyebrow">Why LinkedIn</div>
  <h2 class="section">The only channel that targets the <em>exact companies</em> and the <em>exact people</em> that matter to {Client}.</h2>
  <p class="lead">{Optional 1–2 sentence lead-in.}</p>
</div>
```

### Stats strip (4-up under the hero)

```html
<div class="hero-stats">
  <div class="stat"><div class="num">£750m+</div><div class="label">LinkedIn spend managed</div></div>
  <div class="stat"><div class="num">150+</div><div class="label">B2B clients served</div></div>
  <div class="stat"><div class="num">7yr</div><div class="label">LinkedIn Ads specialism</div></div>
  <div class="stat"><div class="num">1 of 1</div><div class="label">Official LinkedIn Marketing Partner in EMEA</div></div>
</div>
```

### Opportunity callout (dark prussian card)

```html
<div class="opp-card">
  <div class="opp-eyebrow">The opportunity</div>
  <h3 class="opp-head">{The shift framing.} <em>{The italic-pale-sky punchline.}</em></h3>
  <div class="opp-grid">
    <div class="opp-col">
      <h4>The problem</h4>
      <p>{What's broken in the current motion.}</p>
    </div>
    <div class="opp-col">
      <h4>What's working</h4>
      <p>The brands pulling ahead <strong>{do X first}</strong> and prioritise <strong>{Y}</strong>.</p>
    </div>
  </div>
</div>
```

### Evidence grid (big-stat block)

```html
<div class="evidence-grid">
  <div class="evidence">
    <div class="ev-stat">95<span class="ev-unit">%</span></div>
    <h4>Of your buyers aren't in market today</h4>
    <p>Intent tools only surface the 5% already shopping. The 95% is your pipeline in 6–18 months — you have to build mental availability with them now.</p>
  </div>
  <div class="evidence">
    <div class="ev-stat">92<span class="ev-unit">%</span></div>
    <h4>Of B2B deals are won by vendors on the day-one shortlist</h4>
    <p>Buyers don't run open RFPs anymore. They run shortlists. If you're not on the list on day one, the deal already belongs to someone else.</p>
  </div>
</div>
```

### Waterfall (funnel stages — light → dark)

```html
<div class="waterfall">
  <div class="wf-row tier-1"><div class="wf-label">Total accounts</div><div class="wf-num">2,500</div></div>
  <div class="wf-arrow">↓</div>
  <div class="wf-row tier-2"><div class="wf-label">Accounts reached</div><div class="wf-num">1,800</div></div>
  <div class="wf-arrow">↓</div>
  <div class="wf-row tier-3"><div class="wf-label">Engaged accounts</div><div class="wf-num">800</div></div>
  <div class="wf-arrow">↓</div>
  <div class="wf-row tier-5"><div class="wf-label">Top engaged accounts</div><div class="wf-num">50</div></div>
</div>
```

### Connector (vertical hand-off between two sections)

```html
<div class="connector">
  <span class="ctr-label">Hand-off to sales</span>
  <span class="ctr-line"></span>
  <span class="ctr-sub">We enrich the top engaged accounts with the specific people inside them, and the exact post they engaged with.</span>
</div>
```

### Step list (2-col numbered cards)

```html
<div class="steps">
  <div class="step">
    <div class="step-num">1</div>
    <div class="step-body">
      <h4>Define the ICP</h4>
      <p>Agree the shortlist of verticals and named accounts before any media spend.</p>
    </div>
  </div>
  <!-- repeat -->
</div>
```

### Feature card (3-up content blocks)

```html
<div class="grid-3">
  <div class="feature">
    <span class="num-lg">01</span>
    <h3>Know you</h3>
    <p>Make the offer legible. Most of the buying committee doesn't know what you actually do.</p>
  </div>
  <!-- repeat -->
</div>
```

---

## Italic-electric callout rule

Every H1, H2, H3 should have exactly one `<em>` highlighting the punchword of the sentence. Render rule is already in the CSS:

```css
h1.display em, h2.section em {
  font-style: italic;
  color: var(--electric);
  font-weight: 400;
}
```

Examples (good):

- `LinkedIn Advertising<br>for <em>Complete.</em>`
- `From target list, to engaged accounts, <em>to named people.</em>`
- `One package. <em>Everything included.</em>`
- `Setup in month one. Live by month two. <em>Traction by day 90.</em>`

Anti-patterns:

- Don't italicise the whole sentence
- Don't italicise more than one phrase per heading
- Don't use italic for any purpose other than this callout (it's a system signal, not a typographic accent)

On a dark `opp-card`, swap the `em` colour to `--pale-sky` for contrast:

```css
.opp-card h3.opp-head em { font-style: italic; color: var(--pale-sky); font-weight: 400; }
```

---

## Status colour discipline

The blue family is decorative. Status colours are semantic — use them only when they literally mean what they encode.

| Colour | Hex | Semantic meaning | Use case |
|---|---|---|---|
| Success green | `#05C168` | Done, fit, handover complete | Last step in a workflow, "ready to ship", positive-fit row in a comparison |
| Cornflower | `#6A88FD` | In-progress, neutral attention | Default for any in-flight state, default chip colour |
| Warm yellow | `#FDBD1A` | Critical alignment, needs sign-off | Rare. Only when the user must make a decision before work can proceed |
| Hot red | `#FF5A65` | Not a fit, failure, blocker | Negative-fit row in a comparison, blocking issue |

Never use yellow or red decoratively. If the surface doesn't carry a semantic state, leave it in the blue family.

---

## Don'ts

- No drop-shadows that aren't already in the `--shadow-*` tokens
- No fonts other than Arizona Flare and Inter (the Fraunces fallback is automatic)
- No ALL-CAPS section headings (eyebrows are uppercase; H1/H2/H3 are mixed-case)
- No "buy now" colour CTA buttons in orange/red. Primary buttons are the prussian-fill pill (`pill-dark`) or the electric solid (used on `connector .ctr-label`)
- No background images other than the mesh and the SVG funnel watermark
- No flat white panels with hard 1px borders. Every panel is glass — translucent + blur + faint white border
- No text on a coloured background that isn't either prussian (white text) or white (prussian text). No mid-tone backgrounds carrying body copy

---

## Source of truth

This system was extracted verbatim from `/Users/tomt/Downloads/complete-linkedinads.html` (the FMF × Complete LinkedIn Advertising proposal, March 2026 build). When in doubt, open that file and copy the exact treatment used there. Newer FMF artefacts (Andy Webb / DPD, Pipeline Cohort landing, etc.) iterate on the same vocabulary — the rules above are the stable core.
