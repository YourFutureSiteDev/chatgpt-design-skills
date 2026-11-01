# steal-the-polish knowledge file

This file holds the whole `steal-the-polish` skill: SKILL.md first, then every reference, asset and script it points to. Each one starts with a `## File:` heading that gives its path inside the skill.

## File: SKILL.md

# Steal the polish


> **Note for ChatGPT.** This skill was first written for Claude Code. Where it names Claude, a subagent, a slash command or a tool you do not have (a terminal, a browser preview, the user's own files), do that step yourself in the conversation, or give the user the exact code or command to run and say which step you could not run. Where it says to copy a file from `assets/` into the project, show the user that file's contents so they can paste it.

From a @shai.hq Instagram carousel, 17 Sep 2026 (breakdown in
`references/source-post.md`). Its whole argument fits on one slide:

> Stop building from scratch. Steal the polish. Three libraries for the
> components. Two feeds for the taste. All free.

That is the split this skill keeps. **Three libraries** give you finished
code to port. **Two feeds** give you a bar to build to before you touch code.
**Five house kits** (dashboard lift, login card, pay ring, scroll sequence, liquid nav) are already ported and drop straight in.

## Behaviour first

Before lifting a look from any source below, settle how the module behaves (settings, table, form, pricing, modal, nav, dashboard): its states, empty states and copy. This skill then supplies the finish. For dashboards that means number formatting, honest charts, loading and empty states first, and the house dashboard-lift kit here dresses what those specify.

## The sources

| # | Source | What it is | Where the code lives |
|---|---|---|---|
| 1 | **SmoothUI** smoothui.dev | 130 animated components for shadcn: tabs, toggles, inputs, modals, toasts, drawers, number flow, text reveals, 20 AI-chat parts, shader page transitions. MIT. | Repo: github.com/educlopez/smoothui (`packages/smoothui/components/<name>/`). Live: public REST API, `scripts/smoothui.sh`. |
| 2 | **Bencho** bencho.dev | 29 interactive blocks you tune with sliders before you take them: tilt card, slide to confirm, radial menu, command bar, liquid toggle, pull to refresh, reorder list, magnifying dock. Live, not mocked. MIT. | No public repo. Site only, read via the browser: `references/bencho.md`. |
| 3 | **Amicro** amicro.vercel.app | Micro-transitions: 30 buttons with one interaction each, card spreads, 3D carousels, loaders, 29 mono charts, 23 dither charts, CSS-only "whimsical" pieces. MIT. | Repo: github.com/Subhan-code/Amicro--Micro-transitions- (`src/components/`, and `src/data/*.ts` for the catalogue). |
| 4 | **Inspora** inspora.design | Design inspiration feed, mostly video so you see the motion: Web, Branding, Product, Motion, Illustration, 3D, Print. Updated hourly. | Feed. Read with `curl -s https://r.jina.ai/https://inspora.design/?category=Web`. Each post has an mp4 at `media.inspora.design`. |
| 5 | **Best Designs on X** bestdesignsonx.com | Hand-picked design posts from X, plus tabs for Fonts, Dribbble, Behance, App Icons and OG Images. Updated hourly. | Feed. Client-rendered, so open it in the built-in browser and search, `references/feeds.md`. |
| 6 | **Dashboard lift** (house kit) | Four dashboard looks lifted from the juicelab.uiux "Leadly AI" concept, 10 Sep 2026: dot-matrix numbers (Doto), glow stat tiles, white KPI card with dot clusters, target-vs-actual "ghost bars", plus the Instagram showcase frame and a count-up. Already ported, plain CSS and JS. | Local: `assets/dashboard-lift.css`, `assets/dashboard-lift.js`, demo `assets/dashboard-lift-demo.html`, breakdown `references/juicelab-dashboard.md`. |
| 7 | **Login card** (house kit) | The @code.xr "Login form V5" look, 20 Sep 2026: black ground with glowing circuit lines, a glass card split by a slanted divider, icon inputs that take the accent on focus, a gradient pill button. Plain CSS, one accent token. | Local: `assets/login-card.css`, demo `assets/login-card-demo.html`, breakdown `references/code-xr-login.md`. |
| 8 | **Godly** godly.design | Curated gallery of the best designed live websites, with section tabs for Hero, CTA, Footer, OG Images, Logos and App Icons. Added 1 Oct 2026. | Feed. Readable with `curl -s https://r.jina.ai/https://godly.design/hero` (or /cta, /footer, /websites). Screenshot one site and copy its decisions (spacing, type size, one accent), never its brand. |
| 9 | **transitions.dev** | 30 production UI transitions (card resize, number pop in, modal, menu dropdown, toast, success check, tabs sliding, skeleton reveal) on one motion token scale. Added 1 Oct 2026. | transitions.dev, which ships its own free `transitions-dev` and `transitions-polish` agent skills (install them from there). Plain CSS, so it ports straight in. |
| 10 | **Pay ring** (house kit) | A pay button that collapses into a progress ring while the payment runs, then turns into a green tick with a short burst, or reopens as Try again with the reason underneath. Indeterminate spin unless the code reports a real fraction. From a saved Instagram post, 2 Oct 2026. | Local: `assets/pay-ring.css`, `assets/pay-ring.js`, demo `assets/pay-ring-demo.html`. |
| 11 | **Scroll sequence** (house kit) | The "latte builds itself as you scroll" section: a sticky canvas scrubbing a JPG or WebP frame sequence through a tall section, with side steps (name, option chips, price, CTA) that swap at set points. Plain IntersectionObserver and rAF. 2 Oct 2026. | Local: `assets/scroll-sequence.css`, `assets/scroll-sequence.js`, demo `assets/scroll-sequence-demo.html` with 60 placeholder frames in `assets/scroll-sequence-frames/`. |
| 12 | **Liquid nav** (house kit) | Floating pill nav whose active indicator glides on a real spring, optional true glass refraction on Chromium (SVG displacement map in `backdrop-filter`) with a frosted fallback, scroll spy, and a theme toggle that reveals the new theme as a growing circle. 2 Oct 2026. | Local: `assets/liquid-nav.css`, `assets/liquid-nav.js`, demo `assets/liquid-nav-demo.html`. |

## When to reach for which

These kits assume hand-written HTML, CSS and JS sites, usually for small
local businesses, usually read on a phone. Pick by what the page needs, not by what
looks coolest in the demo.

- **A control that has to feel physical** (toggle, slider, confirm, stepper,
  dock, drag): **Bencho** first. Its blocks are the best-tuned and the CSS
  comes with a paragraph explaining *why* each value is what it is. Read that
  paragraph, it is the actual lesson.
- **A standard component with better motion** (tabs, dropdown, modal, toast,
  input, OTP, progress, avatar group, accordion): **SmoothUI**. Ask the API
  in plain words and it ranks matches.
- **A plain CSS transition** (modal, dropdown, toast, number pop in, success
  check, tab slide): the **transitions-dev** skill, since it is already plain
  CSS on one token scale.
- **A button or a chart with personality**, a loader, a number that counts:
  **Amicro**. `src/data/buttons.tsx` names all 30 button interactions
  (morph, sparkle, shake, pulse, ring, rotate).
- **Before designing anything visual**, or when a page is technically done
  but flat: ten minutes in **Inspora** (Web or Product category), **Best
  Designs on X** or **Godly** (best for a whole hero, CTA or footer section), pick one reference that is close to the job, and name it as
  the bar. `gauntlet-loop` wants exactly that.
- **A dashboard, stat row, KPI number or target-vs-actual chart**, or an
  Instagram post showing off a finished build: the **dashboard lift** kit.
  It is the only source here that is already plain CSS and JS, so it is
  the first stop for anything with big numbers on it. Read
  `references/juicelab-dashboard.md` for what each block is and the
  "two or three glow tiles per screen" limit. **The slop tell for a
  dashboard** (a @code.xr reel, 20 Sep 2026): near-black navy, purple to
  cyan gradient stat cards, a glowing donut, neon line chart, "Upgrade to
  Pro" card in the sidebar. That is the default every AI tool spits out and
  it reads as fake software. The "instead make these" side was a white or
  cream surface, one accent colour, flat stat tiles with a tiny sparkline,
  plain numbers, real table rows. Business dashboards should go that
  way: light surface, one brand accent, glow tiles only where the kit's
  limit allows, never the neon set.
- **A login, sign-up or client-portal page on a dark site:** the **login
  card** kit. Set `--lc-accent` to the brand colour and drop the circuit
  lines if the page already has a hero. Not for light brochure sites.
- **A pay, deposit or checkout button:** the **pay ring** kit. It owns
  the wait and the ending, never the payment. On a hosted checkout
  link (Square, Stripe and the like) use link mode, which only spins
  until the browser leaves. **Not for** plain
  form submits that are not money (a contact form wants a normal loading
  button from `ui-signals`), and never call `progress()` with a timer: a
  card payment has no real fraction, so it spins.
- **A product or process that is better shown building up** (a coffee being
  made, a burger stacking, a deck being built, before and after on a job):
  the **scroll sequence** kit. Needs real frames: a 3 to 6 second clip
  filmed or generated, cut with the ffmpeg line in the JS header. **Not
  for** a section with no real footage (placeholder frames read as cheap),
  for more than one sequence per page, or for a small business home page whose
  visitors want the phone number, not a show. Keep total frames under 5 MB
  and put every word of copy in the steps, never in the frames.
- **A site nav for a one page or few page business site:** the **liquid
  nav** kit, with `data-spy` on one page sites. Refraction only shows on
  Chromium and only reads over busy imagery; over a flat colour leave
  `data-glass` off. Add the theme toggle only when the site really has a
  dark theme designed. **Not for** sites with more than five or six top
  level links (use a normal header and menu), and not as well as an
  existing header.
- **Never** reach here for hero motion or text effects: that is React Bits.
  Never for feedback states (drag over, upload, retry): that is `ui-signals`.

## Getting the code

**SmoothUI, from the API (no auth):**

```bash
bash ~/.claude/skills/steal-the-polish/scripts/smoothui.sh suggest "sliding tab indicator"
bash ~/.claude/skills/steal-the-polish/scripts/smoothui.sh source animated-tabs
```

`suggest` returns ranked names with a one-line description. `source` returns
the metadata (dependencies, reduced-motion support, composition hints) and the
full TSX. Or read the same file from the local clone.

**Amicro, from the local clone.** Find the entry in `src/data/` (buttons,
cards, loaders, monoCharts, ditherCharts, metrics, textAnimations, toggles,
transitions), then open the component it points to. Buttons live inline in
`src/data/buttons.tsx` as small React functions; each one is one interaction.

**Bencho, from the site.** Follow `references/bencho.md`. In short: open
`https://bencho.dev/?c=<slug>`, set the sliders to what the page needs, open
the code tab, capture the Usage and CSS copies with a small clipboard shim in
the page. The CSS tab is the real payload.

**Inspora and Best Designs on X.** `references/feeds.md`. Do not scrape them
wholesale; pull one or two references per job and describe what they do in
words (spacing, weight, motion, colour) so the description survives even if
the link dies.

**Dashboard lift, from this skill.** Copy `assets/dashboard-lift.css` into the
project's css folder and `assets/dashboard-lift.js` into its js folder, keep
only the blocks the page uses, and add the Google Fonts link from the top of
the CSS file (Doto needs the `ROND` axis in the URL or the dots render
square). Class names: `.dot-num`, `.glow-tile`, `.kpi-card`, `.ghost-bars`,
`.showcase-post`. Open `assets/dashboard-lift-demo.html` to see all of them
and copy the markup. `.showcase-post` is for Instagram, never a client site.

**Pay ring, from this skill.** Copy `assets/pay-ring.css` and
`assets/pay-ring.js` into the project's css and js folders. Add
`data-pay-ring` to the pay button (add class `pay-ring--block` for full
width), set `--pr-bg` to the brand button colour, and call
`PayRing.start(btn)` when the payment begins, then `PayRing.success(btn)` or
`PayRing.fail(btn, "what went wrong and what to do")`. `PayRing.progress(btn,
0.6)` only when the code knows a true fraction. For a link out to Square or
any hosted checkout, use `data-pay-ring="link"` and no JS calls at all.
Labels: `data-label-busy`, `data-label-done`, `data-label-retry`.

**Scroll sequence, from this skill.** Cut the frames (`ffmpeg -i clip.mp4 -vf
"fps=24,scale=1280:-2" -q:v 4 frames/name-%03d.jpg`, WebP line in the JS
header, ffmpeg is at `~/.local/bin/ffmpeg`), copy the CSS and JS in, and copy
the section markup from `assets/scroll-sequence-demo.html`. Set
`data-frames` (with `{n}`), `data-count`, `data-pad`, and point the
`.scroll-seq__still` `<img>` at frame 1 with its real width and height. Each
`.scroll-seq__step` gets a `data-at` from 0 to 1. Set `--seq-bg` to the
frames' background colour and `--seq-track` for how long it plays (400vh
default). Optional `data-frames-small` for a 720px phone set. Reduced
motion and no JS get the still plus every step stacked, automatically.
GSAP ScrollTrigger can drive it instead (`data-driver="external"` plus
`ScrollSequence.seek(section, progress)`), see the JS header.

**Liquid nav, from this skill.** Copy the CSS and JS in, copy the `<nav>`
from `assets/liquid-nav-demo.html`, and set `--ln-pill`, `--ln-tint` and
`--ln-ink` (plus their `[data-theme="dark"]` versions) from the site
palette. `data-glass="refract"` for Chromium refraction, `data-spy` for one
page sites, `.liquid-nav--bottom` for a thumb reach bar. For the theme
toggle, put a `<button data-theme-toggle>` in the nav, put the one line
`<script>` from the JS header in `<head>` so there is no flash of the wrong
theme, and give the site's own tokens a `[data-theme="dark"]` block (and a
`prefers-color-scheme` block, as the demo does). Give section targets
`scroll-margin-top` so the floating bar does not cover headings.

## Porting rule

All three libraries are React. **Never add React, Tailwind or shadcn to a
static site to use one component.** Read the component, understand the effect,
rewrite it in the site's own CSS and JS. Check what it leans on first:

- `motion` / `motion/react` (Framer Motion): rewrite as CSS transitions,
  `@keyframes`, or the Web Animations API. Springs become
  `cubic-bezier(0.34, 1.56, 0.64, 1)` or a short WAAPI keyframe list.
- `gsap`: port to CSS, or load GSAP from cdnjs if the site CSP allows it.
- Shader and canvas transitions (SmoothUI's `shader-reveal-*`, `sdf-*`):
  WebGL. Wrong answer for a small business site on a phone. Skip.
- Bencho ships CSS with a `@media (hover: hover)` split and reduced-motion
  rules already. Keep both when porting.
- Keep every `prefers-reduced-motion` guard. SmoothUI marks
  `hasReducedMotion` in its metadata; if it is false, add the guard yourself.

Put the CSS in the project's existing css folder and the JS in its existing js
folder. Never a new folder for one component.

Tell the user which source and which component you lifted, by name and slug
(`SmoothUI animated-tabs`, `Bencho slide-to-confirm`, `Amicro btn-9 shake`),
so they can look at the original.

## Done means

Before handing this back, check every line. If any fails, fix it and check
again. Do not report the work as finished until all of them pass.

- [ ] The source and component are named in the reply (library, slug, and for
      a feed the post URL), so the user can open the original.
- [ ] No React, Tailwind, shadcn or Motion was added to the site. The ported
      code is plain CSS and JS in the project's existing folders.
- [ ] The port keeps the reduced-motion guard, and on a touch phone it does
      not depend on hover.
- [ ] The component was chosen because the page needed that element, not
      because it looked good in the demo. Say which element on the page it
      serves.
- [ ] If an inspiration feed was used, the reference is described in words
      (what it does with space, weight, motion, colour), not just linked.
- [ ] Bencho values were read from the tuned sliders, not the defaults, and
      the CSS comment explaining the values was read before porting.
- [ ] No WebGL or shader component went onto a client site without the user
      saying yes to it.
- [ ] The result was opened in the browser and the interaction actually runs
      (screenshot or a one-line description of what happened on click/hover).

- [ ] If the dashboard lift kit was used: only the blocks the page needs
      were kept, no more than three `.glow-tile`s share one screen, Doto is
      on numbers only, the font URL carries the `ROND` axis, and
      `.showcase-post` is not on a client site.

- [ ] If the pay ring kit was used: no fake percentage anywhere (no
      `progress()` fed by a timer), the error copy says what to do next,
      link mode is used on hosted checkout links, and the button was
      clicked through all three endings in the browser.
- [ ] If the scroll sequence kit was used: frame 1 shows before JS (the
      still `<img>` has width and height), total frames are under 5 MB,
      all copy is real DOM text in the steps, and reduced motion was
      checked and shows the still with stacked steps.
- [ ] If the liquid nav kit was used: no more than six links, the bar was
      checked in Safari or iPhone (frosted fallback) as well as Chrome, the
      active link carries `aria-current`, every link is at least 44px tall,
      and if the theme toggle is on, the dark theme was actually designed.

If a line cannot be checked without the user, say which one and why, rather
than assuming it passes.


## File: assets/dashboard-lift-demo.html

```html
<!DOCTYPE html>
<html lang="en-AU">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Dashboard Lift Demo</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Doto:wght,ROND@400..900,0..100&family=Outfit:wght@300;400;500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="dashboard-lift.css">
<style>
  /* page chrome only; everything that matters is in dashboard-lift.css */
  *{box-sizing:border-box}
  body{margin:0;background:#eef0f3;color:#111318;font-family:"Outfit",system-ui,sans-serif;padding:0 16px 80px;-webkit-font-smoothing:antialiased}
  .wrap{max-width:1080px;margin:0 auto}
  .banner{margin:20px auto 40px;padding:14px 18px;border:1px solid #f0c36d;background:#fff7e6;border-radius:14px;font-size:14px;line-height:1.5}
  h1{font-weight:500;font-size:30px;margin:0 0 6px}
  .sub{color:#7a7f8a;margin:0 0 36px;font-size:15px}
  h2{font-weight:500;font-size:19px;margin:56px 0 6px}
  .note{color:#7a7f8a;font-size:14px;margin:0 0 18px;line-height:1.5;max-width:70ch}
  .tiles{display:grid;grid-template-columns:repeat(4,1fr);gap:14px}
  .row{display:grid;grid-template-columns:1fr 1.4fr;gap:14px}
  .legend{display:grid;grid-template-columns:repeat(2,1fr);gap:12px;margin-top:14px}
  .legend div{background:#fff;border:1px solid #e1e4e9;border-radius:14px;padding:14px 16px;font-size:14px;line-height:1.5}
  .legend b{font-weight:500;display:block;margin-bottom:4px}
  .screen-grid{display:grid;grid-template-columns:1.2fr 1fr 1fr;gap:10px}
  .screen-grid .glow-tile{min-height:120px;padding:12px;--glow-radius:16px}
  .screen-grid .glow-tile .dot-num{font-size:28px}
  .screen-grid .glow-tile__label{font-size:11px}
  .screen-grid .mini{background:#fafbfc;border-radius:16px;padding:12px;grid-column:span 3}
  .screen-grid .mini p{font-size:12px;margin:0 0 6px}
  .screen-grid .ghost-bars{height:60px;gap:8px}
  @media (max-width:820px){.tiles{grid-template-columns:repeat(2,1fr)}.row,.legend{grid-template-columns:1fr}.screen-grid{grid-template-columns:1fr 1fr}.screen-grid .mini{grid-column:span 2}}
</style>
</head>
<body>
<div class="wrap">
  <div class="banner"><strong>Demo, not live.</strong> Everything below comes from <code>dashboard-lift.css</code> and <code>dashboard-lift.js</code>. Lifted from the juicelab.uiux "Leadly AI" concept (Instagram, 10 Sep 2026). Placeholder tradie numbers.</div>
  <h1>Dashboard lift</h1>
  <p class="sub">Doto for numbers, Outfit for the rest. No frameworks. Count-up ported from Magic UI number-ticker.</p>

  <h2>1. Glow stat tiles</h2>
  <p class="note"><code>.glow-tile</code> with <code>--h</code> set per tile. Two or three per screen, no more.</p>
  <div class="tiles">
    <div class="glow-tile" style="--h:340 96% 56%">
      <div class="glow-tile__label">Jobs booked this week</div>
      <div class="dot-num" data-to="32">0</div>
      <svg class="glow-tile__spark" viewBox="0 0 160 34"><line class="dash" x1="0" y1="14" x2="160" y2="14"/><path class="line" d="M0 30 C 30 26, 50 10, 80 12 S 130 28, 160 30"/><circle class="ring" cx="22" cy="27" r="3"/><circle class="dot" cx="80" cy="12" r="4"/></svg>
    </div>
    <div class="glow-tile" style="--h:224 96% 60%">
      <div class="glow-tile__label">Quotes accepted</div>
      <div class="dot-num" data-to="94" data-suffix="%">0%</div>
      <svg class="glow-tile__spark" viewBox="0 0 160 34"><line class="dash" x1="0" y1="14" x2="160" y2="14"/><path class="line" d="M0 24 C 30 30, 40 8, 70 12 S 120 30, 160 22"/><circle class="ring" cx="20" cy="26" r="3"/><circle class="dot" cx="70" cy="12" r="4"/></svg>
    </div>
    <div class="glow-tile" style="--h:120 92% 48%">
      <div class="glow-tile__label">Invoices paid on time</div>
      <div class="dot-num" data-to="118">0</div>
      <svg class="glow-tile__spark" viewBox="0 0 160 34"><line class="dash" x1="0" y1="14" x2="160" y2="14"/><path class="line" d="M0 28 C 40 28, 60 20, 90 14 S 140 6, 160 4"/><circle class="ring" cx="30" cy="27" r="3"/><circle class="dot" cx="90" cy="14" r="4"/></svg>
    </div>
    <div class="glow-tile" style="--h:268 70% 62%">
      <div class="glow-tile__label">Stock lines running low</div>
      <div class="dot-num" data-to="7">0</div>
      <svg class="glow-tile__spark" viewBox="0 0 160 34"><line class="dash" x1="0" y1="14" x2="160" y2="14"/><path class="line" d="M0 8 C 30 6, 50 22, 80 20 S 130 12, 160 14"/><circle class="ring" cx="24" cy="8" r="3"/><circle class="dot" cx="80" cy="20" r="4"/></svg>
    </div>
  </div>

  <h2>2. KPI card and ghost bars</h2>
  <p class="note"><code>.kpi-card</code> left. <code>.ghost-bars</code> right, drawn from <code>data-groups="target:actual,..."</code>. Grey is target, black is actual.</p>
  <div class="row">
    <div class="kpi-card">
      <p class="kpi-card__title kpi-card__title--grey">Revenue this month</p>
      <div class="dot-num kpi-card__big" data-to="28460" data-prefix="$">$0</div>
      <div class="kpi-card__cap">Paid invoices, ex GST</div>
      <div class="kpi-card__dots">
        <span style="--h:340 96% 56%;--c:4"><i></i><i></i><i></i><i></i><i></i><i></i><i></i></span>
        <span style="--h:224 96% 60%;--c:5"><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i></span>
        <span style="--h:120 92% 48%;--c:3"><i></i><i></i><i></i></span>
        <span style="--h:268 70% 62%;--c:6"><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i></span>
      </div>
      <div class="kpi-card__split">
        <div><div class="dot-num" data-to="41">0</div><small>Jobs completed</small></div>
        <div><div class="dot-num" data-to="6">0</div><small>Awaiting payment</small></div>
      </div>
    </div>
    <div class="kpi-card">
      <p class="kpi-card__title">Jobs per day, target vs actual</p>
      <div class="dot-num ghost-bars__total" data-to="169">0</div>
      <div class="ghost-bars" data-groups="80:6,90:7,70:5,85:7,95:8,50:3"></div>
      <div class="ghost-bars__axis"><span>Mon</span><span>Tue</span><span>Wed</span><span>Thu</span><span>Fri</span><span>Sat</span></div>
    </div>
  </div>

  <h2>3. Showcase post frame</h2>
  <p class="note"><code>.showcase-post</code>. For Instagram posts of client builds, never on a client site. Swap the word, drop a screenshot into the screen.</p>
  <div class="showcase-post">
    <div class="showcase-post__corner showcase-post__corner--tl">Light <span>Shot</span></div>
    <div class="showcase-post__corner showcase-post__corner--tr">Dashboard <span>Design</span></div>
    <div class="showcase-post__corner showcase-post__corner--bl">Your Future <span>Site</span></div>
    <div class="showcase-post__word">Dashboard</div>
    <div class="showcase-post__stage">
      <div class="showcase-post__screen">
        <div class="showcase-post__inner screen-grid">
          <div class="glow-tile" style="--h:268 70% 62%"><div class="glow-tile__label">Pipeline</div><div class="dot-num">$42.6K</div></div>
          <div class="glow-tile" style="--h:340 96% 56%"><div class="glow-tile__label">Lead rate</div><div class="dot-num">32%</div></div>
          <div class="glow-tile" style="--h:224 96% 60%"><div class="glow-tile__label">On time</div><div class="dot-num">94%</div></div>
          <div class="mini"><p>Sales activity</p><div class="ghost-bars" data-groups="80:6,90:7,70:5,85:7,95:8,50:3"></div></div>
        </div>
      </div>
      <div class="showcase-post__stand"></div>
    </div>
  </div>

  <h2>4. Take and leave</h2>
  <div class="legend">
    <div><b>Take: Doto numbers</b> Free Google font. Numbers only, never body text.</div>
    <div><b>Take: glow tiles</b> Pure CSS. Two or three per screen or it turns into a lolly shop.</div>
    <div><b>Take: ghost bars</b> Anywhere you show target vs actual.</div>
    <div><b>Leave: the forecast candle chart</b> Trading desk styling. A small business owner will not know what it means.</div>
  </div>
</div>
<script src="dashboard-lift.js"></script>
</body>
</html>

```

## File: assets/dashboard-lift.css

```css
/* dashboard-lift.css
   Four dashboard looks lifted from the juicelab.uiux "Leadly AI" concept
   (Instagram, 10 Sep 2026). Plain CSS, no framework. Drop this file into the
   project's existing css folder and load Doto + Outfit from Google Fonts:

   <link href="https://fonts.googleapis.com/css2?family=Doto:wght,ROND@400..900,0..100&family=Outfit:wght@300;400;500;600&display=swap" rel="stylesheet">

   Blocks (take only what the page needs, delete the rest):
     .dot-num        dot-matrix numeral, numbers only, never body text
     .glow-tile      radial glow stat tile, set --h to an HSL triple
     .kpi-card       plain white KPI card with coloured dot clusters
     .ghost-bars     target (grey) vs actual (black) bars, built by dashboard-lift.js
     .showcase-post  Instagram frame: giant faded word behind a tilted screen

   Colours are HSL triples so one variable drives the tile, its glow and its
   dot clusters: --h: 340 96% 56%  (pink)  224 96% 60% (blue)
                  120 92% 48%  (green) 268 70% 62% (purple)
*/

/* ---------- dot-matrix numerals ---------- */
.dot-num{
  font-family:"Doto",ui-monospace,monospace;
  font-weight:800;
  font-variation-settings:"ROND" 100;   /* round dots, like the reference */
  letter-spacing:.02em;
  line-height:1;
  font-variant-numeric:tabular-nums;
}

/* ---------- glow stat tile ---------- */
.glow-tile{
  --h:268 70% 62%;
  --glow-radius:26px;
  position:relative;
  border-radius:var(--glow-radius);
  padding:20px 20px 16px;
  min-height:200px;
  color:#fff;
  display:flex;flex-direction:column;justify-content:space-between;
  overflow:hidden;
  background:
    radial-gradient(circle at 50% 48%,
      hsl(var(--h) / 1) 0%,
      hsl(var(--h) / .78) 32%,
      hsl(var(--h) / .42) 62%,
      hsl(var(--h) / .28) 100%);
  box-shadow:0 1px 0 rgba(255,255,255,.7) inset, 0 10px 30px -18px hsl(var(--h) / .6);
}
.glow-tile::after{ /* frosted rim */
  content:"";position:absolute;inset:0;border-radius:inherit;pointer-events:none;
  background:radial-gradient(circle at 50% 50%, transparent 55%, rgba(255,255,255,.35) 100%);
}
.glow-tile>*{position:relative;z-index:1}
.glow-tile__label{font-size:15px;line-height:1.15;font-weight:300;opacity:.95;max-width:8em}
.glow-tile .dot-num{font-size:44px;text-align:center;margin:6px 0}
.glow-tile__spark{width:100%;height:34px;display:block;overflow:visible}
.glow-tile__spark .dash{stroke:#fff;stroke-width:1.2;stroke-dasharray:4 4;opacity:.8}
.glow-tile__spark .line{fill:none;stroke:#fff;stroke-width:1.3;opacity:.9}
.glow-tile__spark .dot{fill:#fff}
.glow-tile__spark .ring{fill:none;stroke:#fff;stroke-width:1.2}

/* ---------- KPI card ---------- */
.kpi-card{
  --card:#fafbfc; --ink:#111318; --muted:#7a7f8a; --line:#e1e4e9;
  background:var(--card);
  color:var(--ink);
  border-radius:26px;
  padding:22px 22px 18px;
  border:1px solid rgba(255,255,255,.8);
  box-shadow:0 1px 2px rgba(17,19,24,.04), 0 12px 30px -24px rgba(17,19,24,.25);
}
.kpi-card__title{font-size:17px;font-weight:400;margin:0 0 14px}
.kpi-card__title--grey{color:var(--muted)}
.kpi-card__big{font-size:42px}
.kpi-card__cap{color:var(--muted);font-size:14px;margin:6px 0 16px}
.kpi-card__dots{display:flex;gap:18px;align-items:flex-end;height:26px;margin-bottom:22px}
.kpi-card__dots span{display:grid;grid-template-columns:repeat(var(--c,4),5px);gap:3px;align-items:end}
.kpi-card__dots i{width:5px;height:5px;border-radius:50%;background:hsl(var(--h));display:block}
.kpi-card__split{display:grid;grid-template-columns:1fr 1fr;border-top:1px solid var(--line);padding-top:16px;gap:12px}
.kpi-card__split .dot-num{font-size:30px}
.kpi-card__split small{display:block;color:var(--muted);font-size:13px;margin-top:6px}

/* ---------- ghost bars ---------- */
.ghost-bars{display:flex;align-items:flex-end;justify-content:space-between;gap:14px;height:120px;margin-top:8px}
.ghost-bars__grp{display:flex;align-items:flex-end;gap:2px;height:100%;flex:1}
.ghost-bars__grp b{width:2px;height:var(--t);background:#cfd3da;border-radius:1px}
.ghost-bars__grp b.act{background:#111318;height:var(--a)}
.ghost-bars__axis{display:flex;justify-content:space-between;color:#7a7f8a;font-size:12px;margin-top:10px}
.ghost-bars__total{text-align:center;font-size:30px;margin:-6px 0 6px}

/* ---------- showcase post frame (for Instagram, not client sites) ---------- */
.showcase-post{
  position:relative;
  background:linear-gradient(180deg,#e9ebef 0%,#dfe2e7 100%);
  border-radius:24px;
  padding:44px 28px 40px;
  overflow:hidden;
  isolation:isolate;
  font-family:"Outfit",system-ui,sans-serif;
  color:#111318;
}
.showcase-post__corner{position:absolute;top:22px;font-size:14px;font-weight:500}
.showcase-post__corner span{font-weight:300;color:#5b606b}
.showcase-post__corner--tl{left:26px}
.showcase-post__corner--tr{right:26px}
.showcase-post__corner--bl{top:auto;bottom:20px;left:26px}
.showcase-post__word{
  position:absolute;left:50%;top:44px;transform:translateX(-50%);
  font-size:clamp(70px,16vw,190px);font-weight:600;letter-spacing:-.04em;
  color:transparent;
  background:linear-gradient(180deg,rgba(17,19,24,.16),rgba(17,19,24,.04));
  -webkit-background-clip:text;background-clip:text;
  white-space:nowrap;line-height:1;pointer-events:none;user-select:none;
}
.showcase-post__stage{perspective:1600px;margin:110px auto 0;max-width:760px}
.showcase-post__screen{
  transform:rotateX(9deg) rotateY(-11deg) rotateZ(1.5deg);
  transform-origin:50% 60%;
  background:#0b0c0f;border-radius:14px;padding:10px;
  box-shadow:0 40px 70px -30px rgba(0,0,0,.55), 0 2px 0 #2a2d33 inset;
}
.showcase-post__inner{background:#f3f4f7;border-radius:8px;padding:16px}
.showcase-post__stand{width:170px;height:70px;margin:0 auto;background:linear-gradient(180deg,#2b2e34,#15171b);clip-path:polygon(38% 0,62% 0,100% 100%,0 100%)}

/* ---------- phones ---------- */
@media (max-width:480px){
  .glow-tile{min-height:170px;padding:16px}
  .glow-tile .dot-num{font-size:36px}
  .kpi-card__big{font-size:36px}
  .showcase-post{padding:40px 14px 36px}
  .showcase-post__stage{margin-top:80px}
  .showcase-post__screen{transform:none}
}

```

## File: assets/dashboard-lift.js

```js
/* dashboard-lift.js
   Two helpers for dashboard-lift.css. Plain JS, no dependencies. Drop into
   the project's existing js folder and load after the DOM.

   1. Ghost bars. Mark up:
        <div class="ghost-bars" data-groups="80:6,90:7,70:5,85:7,95:8,50:3"></div>
      Each pair is target:actual. Target sets the grey bar height (0-100),
      actual is how many of the 14 bars turn black. Real data goes in the
      attribute server-side or from your own fetch; this only draws it.

   2. Count-up on stats (ported from Magic UI number-ticker, spring swapped
      for ease-out cubic so it needs no library). Mark up:
        <div class="dot-num" data-to="28460" data-prefix="$">$0</div>
      Optional data-suffix="%". Runs once when 40% of the element is on
      screen. Respects prefers-reduced-motion: the final value is set
      straight away with no animation.
*/
(function(){
  // ---- ghost bars ----
  document.querySelectorAll('.ghost-bars[data-groups]').forEach(function(el){
    el.dataset.groups.split(',').forEach(function(pair){
      var p=pair.split(':'), target=+p[0]||0, actual=+p[1]||0;
      var g=document.createElement('div'); g.className='ghost-bars__grp';
      for(var i=0;i<14;i++){
        var b=document.createElement('b');
        b.style.setProperty('--t',(target*(0.85+Math.random()*0.15))+'%');
        if(i<actual){ b.classList.add('act'); b.style.setProperty('--a',(target*(0.55+Math.random()*0.5))+'%'); }
        g.appendChild(b);
      }
      el.appendChild(g);
    });
  });

  // ---- count-up ----
  var reduce=window.matchMedia && matchMedia('(prefers-reduced-motion: reduce)').matches;
  var fmt=function(n){ return n.toLocaleString('en-AU'); };
  var targets=document.querySelectorAll('[data-to]');
  if(!targets.length) return;
  var finish=function(el){
    el.textContent=(el.dataset.prefix||'')+fmt(+el.dataset.to)+(el.dataset.suffix||'');
  };
  if(reduce || !('IntersectionObserver' in window)){ targets.forEach(finish); return; }
  var io=new IntersectionObserver(function(entries){
    entries.forEach(function(e){
      if(!e.isIntersecting) return;
      var el=e.target, to=+el.dataset.to, pre=el.dataset.prefix||'', suf=el.dataset.suffix||'';
      io.unobserve(el);
      var t0=performance.now(), dur=1400;
      (function tick(now){
        var p=Math.min(1,(now-t0)/dur), ease=1-Math.pow(1-p,3);
        el.textContent=pre+fmt(Math.round(to*ease))+suf;
        if(p<1) requestAnimationFrame(tick);
      })(t0);
    });
  },{threshold:.4});
  targets.forEach(function(el){ io.observe(el); });
})();

```

## File: assets/liquid-nav-demo.html

```html
<!DOCTYPE html>
<html lang="en-AU">
<head>
<meta charset="utf-8">
<link rel="icon" href="data:,">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Liquid Nav Demo</title>
<meta name="theme-color" content="#f8fafc">
<script>try{var t=localStorage.getItem('theme');if(t){document.documentElement.dataset.theme=t;document.documentElement.style.colorScheme=t;}}catch(e){}</script>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;800&display=swap" rel="stylesheet">
<link rel="stylesheet" href="liquid-nav.css">
<style>
  /* page chrome only; the nav lives in liquid-nav.css and .js.
     Design system query: Glassmorphism, #2563EB primary with #F97316
     accent on #F8FAFC, Inter throughout, blur 10 to 20px, 1px light edge. */
  :root { --bg: #f8fafc; --fg: #1e293b; --muted: #475569; --card: #ffffff; --line: #e2e8f0; --primary: #2563eb; --accent: #f97316; --ln-font: "Inter", system-ui, sans-serif; color-scheme: light; }
  [data-theme="dark"] { --bg: #0b1220; --fg: #e2e8f0; --muted: #94a3b8; --card: #111a2e; --line: #1e293b; --primary: #60a5fa; --accent: #fb923c; color-scheme: dark; }
  @media (prefers-color-scheme: dark) {
    :root:not([data-theme="light"]) { --bg: #0b1220; --fg: #e2e8f0; --muted: #94a3b8; --card: #111a2e; --line: #1e293b; --primary: #60a5fa; --accent: #fb923c; color-scheme: dark;
      --ln-tint: rgba(15,23,42,.5); --ln-edge: rgba(255,255,255,.14); --ln-ink: #e2e8f0; --ln-ink-active: #0f172a; --ln-pill: #93c5fd; }
  }
  * { box-sizing: border-box; }
  html { scroll-behavior: smooth; }
  @media (prefers-reduced-motion: reduce) { html { scroll-behavior: auto; } }
  body { margin: 0; background: var(--bg); color: var(--fg); font: 400 17px/1.6 "Inter", system-ui, sans-serif; -webkit-font-smoothing: antialiased; }
  section { scroll-margin-top: 90px; padding: 120px 16px 80px; }
  .inner { max-width: 1040px; margin: 0 auto; }
  .banner { padding: 14px 18px; border: 1px solid #f0c36d; background: #fff7e6; color: #1e293b; border-radius: 14px; font-size: 14px; margin-bottom: 40px; }
  h1 { font-size: clamp(44px, 9vw, 104px); line-height: .95; letter-spacing: -.035em; font-weight: 800; margin: 0 0 20px; }
  h1 em { font-style: normal; color: var(--accent); }
  h2 { font-size: clamp(28px, 4vw, 40px); letter-spacing: -.02em; margin: 0 0 16px; }
  p.lead { max-width: 54ch; color: var(--muted); font-size: 19px; margin: 0; }
  /* stripes behind the bar, so the refraction has edges to bend */
  .stripes { height: 140px; margin-top: 48px; border-radius: 24px; background: repeating-linear-gradient(90deg, var(--primary) 0 22px, transparent 22px 44px), linear-gradient(90deg, var(--accent), var(--primary)); background-blend-mode: normal; opacity: .9; }
  .grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }
  .card { background: var(--card); border: 1px solid var(--line); border-radius: 18px; padding: 22px; }
  .card h3 { margin: 0 0 6px; font-size: 18px; }
  .card p { margin: 0; color: var(--muted); font-size: 15px; }
  .quote { font-size: 22px; line-height: 1.45; max-width: 40ch; margin: 0 0 8px; }
  .who { color: var(--muted); font-size: 15px; }
  .cta { display: inline-flex; align-items: center; min-height: 48px; padding: 0 24px; border-radius: 999px; background: var(--primary); color: #fff; font-weight: 600; text-decoration: none; margin-top: 12px; }
  [data-theme="dark"] .cta { color: #0b1220; }
  .cta:focus-visible { outline: 3px solid var(--accent); outline-offset: 2px; }
  @media (max-width: 760px) { .grid { grid-template-columns: 1fr; } section { padding-top: 100px; } }
</style>
</head>
<body>
<nav class="liquid-nav" data-liquid-nav data-glass="refract" data-spy aria-label="Main">
  <div class="liquid-nav__track">
    <a href="#home" aria-current="location">Home</a>
    <a href="#services">Services</a>
    <a href="#reviews">Reviews</a>
    <a href="#contact">Contact</a>
  </div>
  <button class="liquid-nav__theme" type="button" data-theme-toggle aria-label="Switch to dark theme"></button>
</nav>

<main>
  <section id="home">
    <div class="inner">
      <div class="banner"><strong>Demo, not live.</strong> A placeholder plumber. On Chrome and Edge the bar bends the stripes behind it; Safari and Firefox get a frosted blur. Tap the moon to see the theme reveal.</div>
      <h1>Hot water back <em>today</em>.</h1>
      <p class="lead">Placeholder Plumbing Co. Licensed, local, on time. The nav above follows you down the page.</p>
      <div class="stripes" aria-hidden="true"></div>
    </div>
  </section>

  <section id="services">
    <div class="inner">
      <h2>Services</h2>
      <div class="grid">
        <div class="card"><h3>Hot water</h3><p>Repairs and same day replacements, gas and electric.</p></div>
        <div class="card"><h3>Blocked drains</h3><p>Camera inspection, jetting and relining.</p></div>
        <div class="card"><h3>Leaks</h3><p>Found without ripping up the floor first.</p></div>
      </div>
      <div class="stripes" aria-hidden="true"></div>
    </div>
  </section>

  <section id="reviews">
    <div class="inner">
      <h2>Reviews</h2>
      <p class="quote">Placeholder review. On a client site this is a real Google review with the name and date it carries.</p>
      <p class="who">Customer name, suburb</p>
      <div class="stripes" aria-hidden="true"></div>
    </div>
  </section>

  <section id="contact" style="min-height: 90vh">
    <div class="inner">
      <h2>Contact</h2>
      <p class="lead">Call between 7am and 6pm, seven days.</p>
      <a class="cta" href="tel:0400000000">Call 0400 000 000</a>
    </div>
  </section>
</main>
<script src="liquid-nav.js"></script>
</body>
</html>

```

## File: assets/liquid-nav.css

```css
/* liquid-nav.css
   A floating pill nav whose active indicator glides between items on a
   spring, with optional real glass refraction on Chromium and a theme toggle
   that reveals the new theme as a circle growing from the button.
   Pairs with liquid-nav.js. Dependency free.

   Markup:
     <nav class="liquid-nav" data-liquid-nav data-glass="refract" data-spy aria-label="Main">
       <div class="liquid-nav__track">
         <a href="#home" aria-current="page">Home</a>
         <a href="#services">Services</a>
         <a href="#contact">Contact</a>
       </div>
       <button class="liquid-nav__theme" type="button" data-theme-toggle aria-label="Switch to dark theme"></button>
     </nav>

   data-glass="refract"  bends what is behind the bar on Chromium; Safari and
                         Firefox get the frosted blur below, automatically
   data-spy              moves the indicator as the page scrolls past each
                         #section the links point at (one page sites)
   .liquid-nav--bottom   sits at the foot of the screen instead (thumb reach)

   Tokens: --ln-tint is the glass colour, --ln-pill the indicator, --ln-ink
   the text. Dark values sit under [data-theme="dark"], which the toggle sets
   on <html>. Demo: liquid-nav-demo.html */

:root {
  --ln-tint: rgba(255, 255, 255, 0.55);
  --ln-edge: rgba(255, 255, 255, 0.7);
  --ln-shadow: 0 1px 1px rgba(15, 23, 42, 0.06), 0 12px 32px -12px rgba(15, 23, 42, 0.28);
  --ln-ink: #1e293b;
  --ln-ink-active: #ffffff;
  --ln-pill: #2563eb;
  --ln-font: inherit;
  --ln-h: 52px;
}
[data-theme="dark"] {
  --ln-tint: rgba(15, 23, 42, 0.5);
  --ln-edge: rgba(255, 255, 255, 0.14);
  --ln-shadow: 0 1px 1px rgba(0, 0, 0, 0.3), 0 12px 32px -12px rgba(0, 0, 0, 0.7);
  --ln-ink: #e2e8f0;
  --ln-ink-active: #0f172a;
  --ln-pill: #93c5fd;
}

.liquid-nav {
  position: fixed;
  z-index: 50;
  top: calc(14px + env(safe-area-inset-top));
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  gap: 4px;
  max-width: calc(100vw - 24px);
  height: var(--ln-h);
  padding: 4px;
  border-radius: 999px;
  background: var(--ln-tint);
  box-shadow: inset 0 0 0 1px var(--ln-edge), var(--ln-shadow);
  font: 600 15px/1 var(--ln-font);
  /* frosted fallback for every browser */
  -webkit-backdrop-filter: blur(16px) saturate(170%);
  backdrop-filter: blur(16px) saturate(170%);
}
.liquid-nav--bottom {
  top: auto;
  bottom: calc(14px + env(safe-area-inset-bottom));
}
/* no backdrop-filter at all: make the bar solid enough to read */
@supports not ((backdrop-filter: blur(1px)) or (-webkit-backdrop-filter: blur(1px))) {
  .liquid-nav { background: color-mix(in srgb, var(--ln-tint) 30%, Canvas); }
}
/* Chromium refraction, switched on by liquid-nav.js only after it has
   checked the browser and built the displacement map */
.liquid-nav.is-refracting {
  backdrop-filter: var(--ln-refract) blur(3px) saturate(170%);
}

.liquid-nav__track {
  position: relative;
  display: flex;
  align-items: center;
  height: 100%;
  overflow-x: auto;
  overscroll-behavior-x: contain;
  scrollbar-width: none;
  border-radius: 999px;
}
.liquid-nav__track::-webkit-scrollbar { display: none; }

.liquid-nav__track a {
  position: relative;
  z-index: 1;
  display: inline-flex;
  align-items: center;
  height: 100%;
  min-width: 44px;
  padding: 0 18px;
  border-radius: 999px;
  color: var(--ln-ink);
  text-decoration: none;
  white-space: nowrap;
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
  transition: color 220ms ease;
}
.liquid-nav__track a[aria-current] { color: var(--ln-ink-active); }
.liquid-nav__track a:focus-visible,
.liquid-nav__theme:focus-visible {
  outline: 2px solid var(--ln-pill);
  outline-offset: 2px;
}
@media (hover: hover) {
  .liquid-nav__track a:not([aria-current]):hover { background: color-mix(in srgb, var(--ln-ink) 8%, transparent); }
}

/* the gliding indicator, created by liquid-nav.js */
.liquid-nav__pill {
  position: absolute;
  z-index: 0;
  top: 0;
  left: 0;
  height: 100%;
  border-radius: 999px;
  background: var(--ln-pill);
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.35), 0 4px 12px -4px color-mix(in srgb, var(--ln-pill) 70%, transparent);
  pointer-events: none;
  will-change: transform, width;
}

/* ---- theme toggle ---- */
.liquid-nav__theme {
  flex: none;
  display: inline-grid;
  place-items: center;
  width: 44px;
  height: 44px;
  border: 0;
  border-radius: 50%;
  background: transparent;
  color: var(--ln-ink);
  cursor: pointer;
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
}
@media (hover: hover) {
  .liquid-nav__theme:hover { background: color-mix(in srgb, var(--ln-ink) 8%, transparent); }
}
.liquid-nav__theme svg { width: 20px; height: 20px; }

/* the circle reveal: the script animates clip-path on the new snapshot,
   so the browser's default cross fade is switched off here */
::view-transition-old(root),
::view-transition-new(root) {
  animation: none;
  mix-blend-mode: normal;
}
::view-transition-new(root) { z-index: 2; }

@media (prefers-reduced-motion: reduce) {
  .liquid-nav__pill { transition: none !important; }
}

```

## File: assets/liquid-nav.js

```js
/* liquid-nav.js
   Drives liquid-nav.css. Plain JS, no dependencies, works on iPhone Safari.

   1. The indicator. Finds the link with aria-current (or the first link),
      puts a pill behind it, and glides the pill to whichever link is
      clicked. The glide is a real damped spring baked into a CSS linear()
      easing; browsers without linear() get a cubic-bezier overshoot.
      Reduced motion: the pill jumps.

   2. Scroll spy (data-spy on the nav). For one page sites: the pill follows
      the #section currently in the middle of the screen. It holds still
      while a clicked link is still scrolling the page there.

   3. Glass refraction (data-glass="refract"). Chromium only, because only
      Chromium runs SVG filters inside backdrop-filter. Builds a displacement
      map for the pill's exact size on a canvas, feeds it to an SVG
      feImage plus feDisplacementMap, and applies it with
      backdrop-filter: url(#id) blur(). Safari and Firefox keep the frosted
      blur from the CSS, which is the right look there anyway.

   4. Theme toggle ([data-theme-toggle], anywhere on the page). Sets
      data-theme="light|dark" on <html> and remembers it. Where the browser
      has document.startViewTransition, the new theme is revealed as a
      circle growing from the button; elsewhere, and with reduced motion,
      it switches instantly. Put this in <head> to avoid a flash of the
      wrong theme on load:
        <script>try{var t=localStorage.getItem('theme');if(t)document.documentElement.dataset.theme=t;}catch(e){}</script>
*/
(function () {
  var reduce = window.matchMedia && matchMedia('(prefers-reduced-motion: reduce)').matches;

  // ---- a damped spring, sampled into a linear() easing ----
  function springEasing(stiffness, damping) {
    var x = 0, v = 0, dt = 1 / 240, t = 0, pts = [0], still = 0;
    while (t < 2) {
      var a = -stiffness * (x - 1) - damping * v;
      v += a * dt; x += v * dt; t += dt;
      pts.push(x);
      if (Math.abs(x - 1) < 0.001 && Math.abs(v) < 0.01) { if (++still > 10) break; } else still = 0;
    }
    var step = Math.max(1, Math.floor(pts.length / 48)), out = [];
    for (var i = 0; i < pts.length; i += step) out.push(+pts[i].toFixed(4));
    out.push(1);
    return { easing: 'linear(' + out.join(', ') + ')', ms: Math.round(t * 1000) };
  }
  var spring = springEasing(380, 26);   // a short settle with one small overshoot
  var hasLinear = window.CSS && CSS.supports && CSS.supports('transition-timing-function', 'linear(0, 1)');
  var glide = reduce ? 'none'
    : hasLinear ? 'transform ' + spring.ms + 'ms ' + spring.easing + ', width ' + spring.ms + 'ms ' + spring.easing
    : 'transform 460ms cubic-bezier(0.34, 1.56, 0.64, 1), width 460ms cubic-bezier(0.34, 1.56, 0.64, 1)';

  // ---- Chromium check for backdrop-filter: url() ----
  function canRefract() {
    if (!(window.CSS && CSS.supports && CSS.supports('backdrop-filter', 'url(#a) blur(1px)'))) return false;
    var ua = navigator.userAgent;
    if (navigator.userAgentData && navigator.userAgentData.brands) {
      return navigator.userAgentData.brands.some(function (b) { return /Chromium/.test(b.brand); });
    }
    return /Chrome\/\d+/.test(ua) && !/CriOS|FxiOS|EdgiOS|Firefox/.test(ua);
  }

  // displacement map for a pill of w by h: neutral grey in the middle, the
  // rim pushes sideways along its outward normal so the backdrop bends there
  function pillMap(w, h, band) {
    var c = document.createElement('canvas');
    c.width = w; c.height = h;
    var ctx = c.getContext('2d'), img = ctx.createImageData(w, h), d = img.data;
    var r = h / 2, x0 = r, x1 = w - r;
    for (var y = 0; y < h; y++) {
      for (var x = 0; x < w; x++) {
        var cx = Math.max(x0, Math.min(x1, x + 0.5)), dx = x + 0.5 - cx, dy = y + 0.5 - r;
        var len = Math.sqrt(dx * dx + dy * dy) || 1;
        var inside = r - len;                                // px in from the rim
        var k = Math.max(0, 1 - inside / band); k = k * k;   // eases off toward the middle
        var o = (y * w + x) * 4;
        d[o] = 128 + (dx / len) * k * 127;
        d[o + 1] = 128 + (dy / len) * k * 127;
        d[o + 2] = 128;
        d[o + 3] = 255;
      }
    }
    ctx.putImageData(img, 0, 0);
    return c.toDataURL('image/png');
  }

  var uid = 0;
  function setupRefraction(nav) {
    var id = 'ln-glass-' + (++uid), NS = 'http://www.w3.org/2000/svg';
    var holder = document.createElementNS(NS, 'svg');
    holder.setAttribute('aria-hidden', 'true');
    holder.setAttribute('width', '0');
    holder.setAttribute('height', '0');
    holder.style.cssText = 'position:absolute;width:0;height:0;overflow:hidden';
    holder.innerHTML =
      '<filter id="' + id + '" x="0" y="0" width="100%" height="100%" color-interpolation-filters="sRGB">' +
      '<feImage x="0" y="0" preserveAspectRatio="none" result="map"/>' +
      '<feDisplacementMap in="SourceGraphic" in2="map" xChannelSelector="R" yChannelSelector="G"/>' +
      '</filter>';
    document.body.appendChild(holder);
    var feImage = holder.querySelector('feImage'), disp = holder.querySelector('feDisplacementMap');
    var lastW = 0, lastH = 0;
    function build() {
      var w = Math.round(nav.offsetWidth), h = Math.round(nav.offsetHeight);
      if (!w || !h || (w === lastW && h === lastH)) return;
      lastW = w; lastH = h;
      feImage.setAttribute('width', w);
      feImage.setAttribute('height', h);
      feImage.setAttribute('href', pillMap(w, h, Math.min(h * 0.5, 26)));
      disp.setAttribute('scale', String(-Math.round(h * 0.85)));
      nav.style.setProperty('--ln-refract', 'url(#' + id + ')');
      nav.classList.add('is-refracting');
    }
    build();
    if ('ResizeObserver' in window) new ResizeObserver(build).observe(nav);
    else window.addEventListener('resize', build);
  }

  function setupNav(nav) {
    var track = nav.querySelector('.liquid-nav__track') || nav;
    var links = [].slice.call(track.querySelectorAll('a'));
    if (!links.length) return;
    var pill = document.createElement('span');
    pill.className = 'liquid-nav__pill';
    pill.setAttribute('aria-hidden', 'true');
    track.insertBefore(pill, track.firstChild);

    var active = links.filter(function (a) { return a.hasAttribute('aria-current'); })[0] || links[0];
    function place(a, animate) {
      pill.style.transition = animate ? glide : 'none';
      pill.style.width = a.offsetWidth + 'px';
      pill.style.transform = 'translateX(' + a.offsetLeft + 'px)';
    }
    function activate(a, animate) {
      if (!a) return;
      links.forEach(function (l) { if (l !== a) l.removeAttribute('aria-current'); });
      a.setAttribute('aria-current', a.getAttribute('aria-current') || (nav.hasAttribute('data-spy') ? 'location' : 'page'));
      active = a;
      place(a, animate);
      // keep the active item in view when the track scrolls on a phone
      var l = a.offsetLeft, r = l + a.offsetWidth;
      if (l < track.scrollLeft || r > track.scrollLeft + track.clientWidth) {
        track.scrollTo({ left: l - 16, behavior: reduce ? 'auto' : 'smooth' });
      }
    }
    place(active, false);
    active.setAttribute('aria-current', active.getAttribute('aria-current') || 'page');

    var lockUntil = 0;
    links.forEach(function (a) {
      a.addEventListener('click', function () { lockUntil = performance.now() + 1000; activate(a, true); });
    });
    window.addEventListener('scrollend', function () { lockUntil = 0; });

    // fonts loading or a resize change the widths; re-seat without animating
    var reseat = function () { place(active, false); };
    if ('ResizeObserver' in window) new ResizeObserver(reseat).observe(track);
    window.addEventListener('resize', reseat);
    if (document.fonts && document.fonts.ready) document.fonts.ready.then(reseat);

    if (nav.hasAttribute('data-spy') && 'IntersectionObserver' in window) {
      var map = new Map();
      links.forEach(function (a) {
        var hash = a.getAttribute('href');
        if (!hash || hash.charAt(0) !== '#' || hash.length < 2) return;
        var sec = document.getElementById(decodeURIComponent(hash.slice(1)));
        if (sec) map.set(sec, a);
      });
      var io = new IntersectionObserver(function (entries) {
        if (performance.now() < lockUntil) return;
        entries.forEach(function (e) { if (e.isIntersecting) activate(map.get(e.target), true); });
      }, { rootMargin: '-45% 0px -50% 0px' });
      map.forEach(function (_, sec) { io.observe(sec); });
    }

    if (nav.dataset.glass === 'refract' && canRefract()) setupRefraction(nav);
  }

  // ---- theme toggle ----
  var SUN = '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true"><circle cx="12" cy="12" r="4"/><path d="M12 2v2M12 20v2M4.9 4.9l1.4 1.4M17.7 17.7l1.4 1.4M2 12h2M20 12h2M4.9 19.1l1.4-1.4M17.7 6.3l1.4-1.4"/></svg>';
  var MOON = '<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M21 12.8A9 9 0 1 1 11.2 3a7 7 0 0 0 9.8 9.8z"/></svg>';

  function currentTheme() {
    var t = document.documentElement.dataset.theme;
    if (t) return t;
    return window.matchMedia && matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
  }
  function paintToggle(btn) {
    var dark = currentTheme() === 'dark';
    if (!btn.hasAttribute('data-keep-icon')) btn.innerHTML = dark ? SUN : MOON;
    btn.setAttribute('aria-label', dark ? 'Switch to light theme' : 'Switch to dark theme');
  }
  function applyTheme(t) {
    document.documentElement.dataset.theme = t;
    document.documentElement.style.colorScheme = t;
    try { localStorage.setItem('theme', t); } catch (e) {}
    document.querySelectorAll('[data-theme-toggle]').forEach(paintToggle);
    var meta = document.querySelector('meta[name="theme-color"]');
    if (meta) meta.setAttribute('content', getComputedStyle(document.body).backgroundColor);
  }
  function toggleTheme(btn) {
    var next = currentTheme() === 'dark' ? 'light' : 'dark';
    if (reduce || !document.startViewTransition) { applyTheme(next); return; }
    var r = btn.getBoundingClientRect();
    var x = r.left + r.width / 2, y = r.top + r.height / 2;
    var end = Math.hypot(Math.max(x, innerWidth - x), Math.max(y, innerHeight - y));
    var vt = document.startViewTransition(function () { applyTheme(next); });
    vt.ready.then(function () {
      document.documentElement.animate(
        { clipPath: ['circle(0px at ' + x + 'px ' + y + 'px)', 'circle(' + end + 'px at ' + x + 'px ' + y + 'px)'] },
        { duration: 560, easing: 'cubic-bezier(0.65, 0, 0.35, 1)', pseudoElement: '::view-transition-new(root)' }
      );
    }).catch(function () {});
  }

  function init() {
    document.querySelectorAll('[data-liquid-nav]').forEach(setupNav);
    document.querySelectorAll('[data-theme-toggle]').forEach(function (btn) {
      paintToggle(btn);
      btn.addEventListener('click', function () { toggleTheme(btn); });
    });
  }
  window.LiquidNav = { applyTheme: applyTheme, springEasing: springEasing };
  if (document.readyState === 'loading') document.addEventListener('DOMContentLoaded', init);
  else init();
})();

```

## File: assets/login-card-demo.html

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Login card demo</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700;800&display=swap">
<link rel="stylesheet" href="login-card.css">
<style>
  /* demo only: a swatch row to prove the accent token swaps cleanly */
  .demo-swatches { position: fixed; top: 12px; right: 12px; display: flex; gap: 6px; z-index: 2; }
  .demo-swatches button { width: 26px; height: 26px; border-radius: 50%; border: 2px solid rgba(255,255,255,0.5); cursor: pointer; }
</style>
</head>
<body>
<div class="demo-swatches" aria-label="Accent">
  <button style="background:#e5232b" data-accent="#e5232b" title="Reel red"></button>
  <button style="background:#f26b1d" data-accent="#f26b1d" title="Orange"></button>
  <button style="background:#2dd4bf" data-accent="#2dd4bf" title="Teal"></button>
</div>

<main class="lc-stage">
  <svg class="lc-lines" viewBox="0 0 1200 800" preserveAspectRatio="none" aria-hidden="true">
    <path d="M0 210 H160 V300 H60"/>
    <path d="M0 470 H40 V560 H0"/>
    <path d="M1200 190 H1040 V300 H1110 V420 H1200"/>
    <path d="M1200 640 H1130 V560"/>
    <path d="M330 800 V740 H420"/>
    <path d="M1010 800 V700 H1200"/>
    <circle cx="160" cy="210" r="4"/><circle cx="60" cy="300" r="3"/>
    <circle cx="40" cy="470" r="4"/><circle cx="40" cy="560" r="3"/>
    <circle cx="1040" cy="190" r="4"/><circle cx="1110" cy="300" r="3"/><circle cx="1110" cy="420" r="4"/>
    <circle cx="1130" cy="640" r="4"/><circle cx="1130" cy="560" r="3"/>
    <circle cx="330" cy="740" r="4"/><circle cx="420" cy="740" r="3"/>
    <circle cx="1010" cy="700" r="4"/>
  </svg>

  <section class="lc-card">
    <div class="lc-welcome">
      <h2>Hello, friend!</h2>
      <p>Enter your details to start your journey with us.</p>
    </div>

    <form class="lc-form" action="#" method="post" onsubmit="event.preventDefault()">
      <h1>Sign up</h1>

      <label class="lc-field">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><circle cx="12" cy="8" r="4"/><path d="M4 21c0-4 3.6-7 8-7s8 3 8 7"/></svg>
        <input type="text" name="username" id="lc-username" placeholder="Username" autocomplete="username">
      </label>

      <label class="lc-field">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><rect x="3" y="5" width="18" height="14" rx="2"/><path d="M3 7l9 6 9-6"/></svg>
        <input type="email" name="email" id="lc-email" placeholder="Email" autocomplete="email">
      </label>

      <label class="lc-field">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><rect x="4" y="11" width="16" height="10" rx="2"/><path d="M8 11V8a4 4 0 0 1 8 0v3"/></svg>
        <input type="password" name="password" id="lc-password" placeholder="Password" autocomplete="new-password">
      </label>

      <button class="lc-button" type="submit">Sign up</button>
      <p class="lc-alt">Already have an account? <a href="#">Log in</a></p>
    </form>
  </section>
</main>

<script>
  document.querySelectorAll('[data-accent]').forEach(function (b) {
    b.addEventListener('click', function () { document.documentElement.style.setProperty('--lc-accent', b.dataset.accent); });
  });
</script>
</body>
</html>

```

## File: assets/login-card.css

```css
/* login-card: the @code.xr "Login form V5" look, ported by hand for
   hand-written HTML sites. Dependency free. One accent colour drives the
   circuit lines, the focus ring, the button and the link: set --lc-accent
   from the project palette. The reel used
   #e5232b red. Font: Poppins on Google Fonts, or the project's own sans.
   Demo: login-card-demo.html. Source breakdown: references/code-xr-login.md */

:root {
  --lc-accent: #e5232b;
  --lc-ground: #050505;
  --lc-text: #ffffff;
  --lc-muted: rgba(255, 255, 255, 0.62);
  --lc-font: "Poppins", system-ui, -apple-system, "Segoe UI", sans-serif;

  --lc-accent-glow: color-mix(in srgb, var(--lc-accent) 55%, transparent);
  --lc-accent-soft: color-mix(in srgb, var(--lc-accent) 22%, transparent);
  --lc-accent-faint: color-mix(in srgb, var(--lc-accent) 9%, transparent);
  --lc-ease: cubic-bezier(0.23, 1, 0.32, 1);
}

/* ---- stage: the black ground with the circuit lines and a low glow ---- */
.lc-stage {
  position: relative;
  min-height: 100svh;
  display: grid;
  place-items: center;
  padding: 32px 16px;
  background: var(--lc-ground);
  color: var(--lc-text);
  font-family: var(--lc-font);
  overflow: hidden;
  isolation: isolate;
}
.lc-stage::before {
  /* the warm pool bottom right, and a cooler one top left */
  content: "";
  position: absolute;
  inset: 0;
  z-index: -2;
  background:
    radial-gradient(60% 55% at 85% 90%, var(--lc-accent-soft), transparent 70%),
    radial-gradient(40% 40% at 10% 15%, var(--lc-accent-faint), transparent 70%);
}
.lc-lines {
  /* the circuit lines: an SVG stretched behind the card, drawn in the accent */
  position: absolute;
  inset: 0;
  z-index: -1;
  width: 100%;
  height: 100%;
  color: var(--lc-accent);
  opacity: 0.85;
  pointer-events: none;
}
.lc-lines path { fill: none; stroke: currentColor; stroke-width: 1.5; vector-effect: non-scaling-stroke; filter: drop-shadow(0 0 4px var(--lc-accent-glow)); }
.lc-lines circle { fill: currentColor; filter: drop-shadow(0 0 6px var(--lc-accent-glow)); }

/* ---- the card ---- */
.lc-card {
  --lc-split: 46%;         /* where the slanted divider sits on desktop */
  position: relative;
  width: min(100%, 740px);
  display: grid;
  grid-template-columns: var(--lc-split) 1fr;
  border-radius: 18px;
  border: 1px solid rgba(255, 255, 255, 0.14);
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.06), rgba(255, 255, 255, 0.02));
  backdrop-filter: blur(14px);
  -webkit-backdrop-filter: blur(14px);
  box-shadow:
    0 30px 80px rgba(0, 0, 0, 0.6),
    0 0 0 1px rgba(0, 0, 0, 0.4) inset,
    0 0 60px -20px var(--lc-accent-glow);
  overflow: hidden;
}
.lc-card::before {
  /* the darker left panel, cut on a slant. Skewed box, unskewed text. */
  content: "";
  position: absolute;
  top: 0;
  bottom: 0;
  left: -12%;
  width: calc(var(--lc-split) + 12%);
  background: linear-gradient(160deg, rgba(0, 0, 0, 0.55), rgba(0, 0, 0, 0.35));
  border-right: 1px solid rgba(255, 255, 255, 0.10);
  transform: skewX(-8deg);
  transform-origin: top left;
  pointer-events: none;
}

.lc-welcome {
  position: relative;
  display: grid;
  align-content: center;
  gap: 10px;
  padding: 48px 36px 48px 40px;
  text-align: center;
}
.lc-welcome h2 {
  margin: 0;
  font-size: clamp(24px, 3.4vw, 32px);
  font-weight: 800;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  line-height: 1.1;
  text-wrap: balance;
}
.lc-welcome p { margin: 0; color: var(--lc-muted); font-size: 15px; line-height: 1.5; max-width: 30ch; justify-self: center; }

.lc-form {
  position: relative;
  display: grid;
  gap: 14px;
  padding: 36px 32px 28px;
}
.lc-form h1 {
  margin: 0 0 6px;
  font-size: 30px;
  font-weight: 700;
  letter-spacing: 0.04em;
  text-align: center;
}

/* ---- fields: icon left, dark fill, accent on focus ---- */
.lc-field {
  position: relative;
  display: grid;
  grid-template-columns: 22px 1fr;
  align-items: center;
  gap: 10px;
  padding: 0 16px;
  height: 52px;
  border-radius: 12px;
  border: 1px solid rgba(255, 255, 255, 0.14);
  background: rgba(0, 0, 0, 0.42);
  transition: border-color 150ms var(--lc-ease), box-shadow 150ms var(--lc-ease);
}
.lc-field > svg { width: 18px; height: 18px; color: var(--lc-muted); transition: color 150ms var(--lc-ease); }
.lc-field > input {
  min-width: 0;
  height: 100%;
  border: 0;
  background: transparent;
  color: var(--lc-text);
  font: inherit;
  font-size: 15px;
  outline: none;
}
.lc-field > input::placeholder { color: var(--lc-muted); }
.lc-field:focus-within {
  border-color: var(--lc-accent);
  box-shadow: 0 0 0 1px var(--lc-accent), 0 0 18px -4px var(--lc-accent-glow);
}
.lc-field:focus-within > svg { color: var(--lc-accent); }
.lc-field > input:-webkit-autofill { -webkit-text-fill-color: var(--lc-text); box-shadow: 0 0 0 100px rgba(0, 0, 0, 0.9) inset; }

/* ---- the button: accent gradient pill with a glow underneath ---- */
.lc-button {
  margin-top: 6px;
  height: 50px;
  border: 0;
  border-radius: 999px;
  background: linear-gradient(90deg, color-mix(in srgb, var(--lc-accent) 85%, #000), var(--lc-accent), color-mix(in srgb, var(--lc-accent) 85%, #000));
  color: #fff;
  font: inherit;
  font-size: 15px;
  font-weight: 600;
  letter-spacing: 0.02em;
  cursor: pointer;
  box-shadow: 0 10px 30px -8px var(--lc-accent-glow);
  transition: transform 120ms var(--lc-ease), box-shadow 150ms var(--lc-ease), filter 150ms var(--lc-ease);
}
.lc-button:hover { filter: brightness(1.08); box-shadow: 0 14px 36px -8px var(--lc-accent-glow); }
.lc-button:active { transform: scale(0.98); }
.lc-button:focus-visible { outline: 2px solid #fff; outline-offset: 3px; }

.lc-alt { margin: 4px 0 0; text-align: center; font-size: 13px; color: var(--lc-muted); }
.lc-alt a { color: var(--lc-accent); text-decoration: none; font-weight: 500; }
.lc-alt a:hover { text-decoration: underline; text-underline-offset: 2px; }

/* ---- phone: stack, drop the slant, keep the glow ---- */
@media (max-width: 640px) {
  .lc-card { grid-template-columns: 1fr; }
  .lc-card::before { display: none; }
  .lc-welcome { padding: 32px 24px 24px; background: linear-gradient(160deg, rgba(0, 0, 0, 0.55), rgba(0, 0, 0, 0.35)); border-bottom: 1px solid rgba(255, 255, 255, 0.10); }
  .lc-form { padding: 24px 20px 24px; }
  .lc-form h1 { font-size: 26px; }
}

@media (prefers-reduced-motion: reduce) {
  .lc-field, .lc-button, .lc-field > svg { transition-duration: 1ms; }
  .lc-button:active { transform: none; }
}

```

## File: assets/pay-ring-demo.html

```html
<!DOCTYPE html>
<html lang="en-AU">
<head>
<meta charset="utf-8">
<link rel="icon" href="data:,">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Pay Ring Demo</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;500;600&family=Poppins:wght@500;600&display=swap" rel="stylesheet">
<link rel="stylesheet" href="pay-ring.css">
<style>
  /* page chrome only; everything that matters is in pay-ring.css.
     Design system query: Minimalism and Swiss Style, navy #0F172A with
     #0369A1 accent on #F8FAFC, Poppins headings over Open Sans. */
  :root { --pr-font: "Poppins", system-ui, sans-serif; }
  * { box-sizing: border-box; }
  body { margin: 0; background: #f8fafc; color: #020617; font: 400 16px/1.55 "Open Sans", system-ui, sans-serif; padding: 0 16px 80px; -webkit-font-smoothing: antialiased; }
  .wrap { max-width: 980px; margin: 0 auto; }
  .banner { margin: 20px 0 36px; padding: 14px 18px; border: 1px solid #f0c36d; background: #fff7e6; border-radius: 14px; font-size: 14px; }
  h1, h2 { font-family: "Poppins", system-ui, sans-serif; font-weight: 600; }
  h1 { font-size: 30px; margin: 0 0 6px; }
  .sub { color: #475569; margin: 0 0 32px; max-width: 64ch; }
  .grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }
  .card { background: #fff; border: 1px solid #e2e8f0; border-radius: 18px; padding: 22px; box-shadow: 0 1px 2px rgba(15,23,42,.04), 0 8px 24px -16px rgba(15,23,42,.2); }
  .card h2 { font-size: 17px; margin: 0 0 4px; }
  .card p.note { color: #475569; font-size: 14px; margin: 0 0 18px; min-height: 4.6em; }
  .row { display: flex; justify-content: space-between; font-size: 14px; padding: 8px 0; border-top: 1px solid #e2e8f0; }
  .row b { font-variant-numeric: tabular-nums; }
  .card .pay-ring-wrap { margin-top: 18px; }
  .again { display: block; margin: 8px auto 0; background: none; border: 0; color: #0369a1; font: 600 14px "Open Sans", sans-serif; text-decoration: underline; cursor: pointer; min-height: 44px; padding: 0 12px; }
  .again:focus-visible { outline: 2px solid #0369a1; outline-offset: 2px; border-radius: 6px; }
  @media (max-width: 860px) { .grid { grid-template-columns: 1fr; } .card p.note { min-height: 0; } }
</style>
</head>
<body>
<main class="wrap">
  <div class="banner"><strong>Demo, not live.</strong> No money moves. Each button fakes the reply with a timer so you can see the three endings. Everything visual comes from <code>pay-ring.css</code> and <code>pay-ring.js</code>.</div>
  <h1>Pay ring</h1>
  <p class="sub">The button turns into the wait. It spins with no number until the payment code reports a real fraction, then ends as a green tick or reopens with the reason it failed.</p>

  <div class="grid">
    <section class="card">
      <h2>Deposit, indeterminate</h2>
      <p class="note">The usual case. A card payment gives no progress, so the ring just spins until the reply.</p>
      <div class="row"><span>Makeover Pro</span><b>$350.00</b></div>
      <div class="row"><span>Deposit today</span><b>$175.00</b></div>
      <form id="f1"><button class="pay-ring pay-ring--block" type="submit" data-pay-ring>Pay $175 deposit</button></form>
      <button class="again" type="button" data-reset="#f1 .pay-ring">Reset</button>
    </section>

    <section class="card">
      <h2>Upload with real progress</h2>
      <p class="note">Only when the code knows a true fraction, like an upload or a multi step job, does it call progress().</p>
      <div class="row"><span>Brand assets</span><b>3 files</b></div>
      <div class="row"><span>Total</span><b>$120.00</b></div>
      <form id="f2"><button class="pay-ring pay-ring--block" type="submit" data-pay-ring data-label-busy="Uploading and paying…">Upload and pay $120</button></form>
      <button class="again" type="button" data-reset="#f2 .pay-ring">Reset</button>
    </section>

    <section class="card">
      <h2>Declined, with retry</h2>
      <p class="note">The ring reopens to a Try again button and the reason sits right under it, read out by screen readers.</p>
      <div class="row"><span>Care plan</span><b>$49.00 a month</b></div>
      <div class="row"><span>First month</span><b>$49.00</b></div>
      <form id="f3"><button class="pay-ring pay-ring--block" type="submit" data-pay-ring>Start care plan</button></form>
      <button class="again" type="button" data-reset="#f3 .pay-ring">Reset</button>
    </section>
  </div>
</main>

<script src="pay-ring.js"></script>
<script>
  // Fake payment replies for the demo. On a real site this is your fetch.
  function later(ms) { return new Promise(function (r) { setTimeout(r, ms); }); }

  document.getElementById('f1').addEventListener('submit', async function (e) {
    e.preventDefault();
    var b = this.querySelector('.pay-ring');
    PayRing.start(b);
    await later(1800);
    PayRing.success(b);
  });

  document.getElementById('f2').addEventListener('submit', async function (e) {
    e.preventDefault();
    var b = this.querySelector('.pay-ring');
    PayRing.start(b);
    for (var i = 1; i <= 5; i++) { await later(420); PayRing.progress(b, i / 5); }
    PayRing.success(b, { label: 'Uploaded and paid' });
  });

  var tries = 0;
  document.getElementById('f3').addEventListener('submit', async function (e) {
    e.preventDefault();
    var b = this.querySelector('.pay-ring');
    PayRing.start(b);
    await later(1500);
    tries++;
    if (tries % 2) PayRing.fail(b, 'Card declined by your bank. Check the card details or try another card.');
    else PayRing.success(b);
  });

  document.querySelectorAll('[data-reset]').forEach(function (btn) {
    btn.addEventListener('click', function () { PayRing.reset(btn.dataset.reset); });
  });
</script>
</body>
</html>

```

## File: assets/pay-ring.css

```css
/* pay-ring.css
   A pay button that collapses into a progress ring while the payment is
   handled, then turns into a green tick (with a short burst) or reopens with
   an inline error and a retry label. Pairs with pay-ring.js. Dependency free.

   Markup (pay-ring.js adds the ring, the tick and the status lines):
     <button class="pay-ring" type="submit" data-pay-ring>Pay $175 deposit</button>
   Full width variant: add class "pay-ring--block".
   Link out variant (Square hosted checkout): put data-pay-ring="link" on the <a>.

   Theme it with the tokens below. Set --pr-bg to the brand button colour and
   leave --pr-success green: green means paid everywhere, do not rebrand it.
   Demo: pay-ring-demo.html */

:root {
  --pr-bg: #0f172a;           /* idle and busy button colour */
  --pr-fg: #ffffff;           /* label, ring and tick colour */
  --pr-success: #16a34a;      /* paid */
  --pr-error: #dc2626;        /* error text and the error outline */
  --pr-muted: #475569;        /* status line under the button */
  --pr-h: 52px;               /* button height, also the ring diameter */
  --pr-radius: 999px;
  --pr-font: inherit;
  --pr-ease: cubic-bezier(0.23, 1, 0.32, 1);
  --pr-spring: cubic-bezier(0.34, 1.56, 0.64, 1);
}

.pay-ring-wrap {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  position: relative;
}
.pay-ring-wrap--block { align-items: stretch; }
.pay-ring-wrap--block > .pay-ring { align-self: center; }

.pay-ring {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  box-sizing: border-box;
  height: var(--pr-h);
  min-width: var(--pr-h);
  padding: 0 28px;
  border: 0;
  border-radius: var(--pr-radius);
  background: var(--pr-bg);
  color: var(--pr-fg);
  font: 600 16px/1 var(--pr-font);   /* 16px stops iOS Safari zooming */
  letter-spacing: 0.01em;
  text-decoration: none;
  white-space: nowrap;
  overflow: hidden;
  cursor: pointer;
  touch-action: manipulation;
  -webkit-tap-highlight-color: transparent;
  transition:
    width 380ms var(--pr-ease),
    padding 380ms var(--pr-ease),
    background-color 260ms ease,
    transform 160ms var(--pr-ease),
    box-shadow 200ms ease;
  box-shadow: 0 1px 2px rgba(15, 23, 42, 0.12), 0 6px 16px -6px rgba(15, 23, 42, 0.35);
}
.pay-ring--block { width: 100%; }

@media (hover: hover) {
  .pay-ring[data-state="idle"]:hover,
  .pay-ring[data-state="error"]:hover {
    box-shadow: 0 1px 2px rgba(15, 23, 42, 0.14), 0 10px 22px -8px rgba(15, 23, 42, 0.45);
  }
}
.pay-ring[data-state="idle"]:active,
.pay-ring[data-state="error"]:active { transform: scale(0.97); }

.pay-ring:focus-visible {
  outline: 3px solid color-mix(in srgb, var(--pr-bg) 45%, white);
  outline-offset: 3px;
}

/* the label fades out while the ring is up, and stays in the DOM so the
   button keeps its accessible name */
.pay-ring__label {
  display: inline-block;
  transition: opacity 180ms ease, transform 260ms var(--pr-ease);
}
.pay-ring[data-state="busy"] .pay-ring__label,
.pay-ring[data-state="success"] .pay-ring__label {
  opacity: 0;
  transform: scale(0.9);
}

.pay-ring[data-state="busy"],
.pay-ring[data-state="success"] {
  padding: 0;
  cursor: progress;
}
.pay-ring[data-state="success"] {
  background: var(--pr-success);
  cursor: default;
}
.pay-ring[data-state="error"] {
  box-shadow: 0 0 0 2px var(--pr-error), 0 6px 16px -6px rgba(15, 23, 42, 0.35);
}

/* ---- the ring ---- */
.pay-ring__ring,
.pay-ring__tick {
  position: absolute;
  inset: 0;
  margin: auto;
  width: calc(var(--pr-h) - 12px);
  height: calc(var(--pr-h) - 12px);
  pointer-events: none;
  opacity: 0;
  transition: opacity 200ms ease;
}
.pay-ring__ring { transform: rotate(-90deg); }  /* start at twelve o'clock */
.pay-ring__track,
.pay-ring__arc {
  fill: none;
  stroke-width: 3;
}
.pay-ring__track { stroke: color-mix(in srgb, var(--pr-fg) 22%, transparent); }
.pay-ring__arc {
  stroke: var(--pr-fg);
  stroke-linecap: round;
  transition: stroke-dashoffset 300ms var(--pr-ease);
}

.pay-ring[data-state="busy"] .pay-ring__ring { opacity: 1; transition-delay: 160ms; }

/* indeterminate: a quarter arc spinning, because nothing is known yet */
.pay-ring[data-state="busy"]:not([data-progress]) .pay-ring__ring {
  animation: pr-spin 900ms linear infinite;
}
@keyframes pr-spin { to { transform: rotate(270deg); } }

/* ---- the tick ---- */
.pay-ring__tick path {
  fill: none;
  stroke: var(--pr-fg);
  stroke-width: 3.2;
  stroke-linecap: round;
  stroke-linejoin: round;
  stroke-dasharray: 30;
  stroke-dashoffset: 30;
}
.pay-ring[data-state="success"] .pay-ring__tick { opacity: 1; }
.pay-ring[data-state="success"] .pay-ring__tick path {
  transition: stroke-dashoffset 420ms var(--pr-ease) 120ms;
  stroke-dashoffset: 0;
}
.pay-ring[data-state="success"] { animation: pr-pop 420ms var(--pr-spring); }
@keyframes pr-pop { 0% { transform: scale(0.92); } 100% { transform: scale(1); } }

/* ---- status and error lines ---- */
.pay-ring__status,
.pay-ring__error {
  margin: 0;
  min-height: 1.4em;
  font: 500 14px/1.4 var(--pr-font);
  text-align: center;
}
.pay-ring__status { color: var(--pr-muted); }
.pay-ring__status[data-tone="success"] { color: var(--pr-success); font-weight: 600; }
.pay-ring__error { color: var(--pr-error); }
.pay-ring__error:empty { display: none; }
/* while an error shows, the empty status line gives up its space */
.pay-ring-wrap:has(.pay-ring__error:not(:empty)) .pay-ring__status:empty { display: none; }

.pay-ring.is-shaking { animation: pr-shake 380ms var(--pr-ease); }
@keyframes pr-shake {
  20% { transform: translateX(-6px); }
  45% { transform: translateX(5px); }
  70% { transform: translateX(-3px); }
  100% { transform: translateX(0); }
}

/* ---- burst particles (made by pay-ring.js on success) ---- */
.pay-ring__spark {
  position: absolute;
  width: 7px;
  height: 7px;
  margin: -3.5px 0 0 -3.5px;
  border-radius: 50%;
  pointer-events: none;
}

/* ---- reduced motion: no collapse slide, no spin, no shake, no burst.
   The ring pulses instead of spinning so a wait still reads as a wait. ---- */
@media (prefers-reduced-motion: reduce) {
  .pay-ring,
  .pay-ring__label,
  .pay-ring__arc,
  .pay-ring[data-state="success"] .pay-ring__tick path { transition-duration: 0ms !important; transition-delay: 0ms !important; }
  .pay-ring[data-state="busy"]:not([data-progress]) .pay-ring__ring {
    animation: pr-pulse 1600ms ease-in-out infinite;
  }
  .pay-ring[data-state="success"],
  .pay-ring.is-shaking { animation: none; }
  @keyframes pr-pulse { 50% { opacity: 0.45; } }
}

```

## File: assets/pay-ring.js

```js
/* pay-ring.js
   Drives pay-ring.css. Plain JS, no dependencies, works on iPhone Safari.

   The kit never runs a payment. Your own code (or the Square checkout link)
   does that, and tells the button what happened:

     PayRing.start(btn)              collapse to the ring, indeterminate spin
     PayRing.progress(btn, 0.6)      optional: switch to a real fill, 0 to 1
     PayRing.success(btn)            green tick, short burst, "Payment received"
     PayRing.fail(btn, "Card declined. Check the details or try another card.")
     PayRing.reset(btn)              back to idle

   btn can be the element or a selector. Each call also returns a promise
   that settles once the state is on screen.

   Honest by design: the ring spins with no number until progress() is called
   with a real fraction. Never feed it a timer pretending to be progress.

   Data attributes (all optional):
     data-pay-ring            enhance this button (or "link" for an <a> that
                              leaves for a hosted checkout: the ring starts on
                              click and the browser navigates as normal;
                              the Back button, or coming back from a new
                              tab, puts it back to idle. Link mode never
                              shows success: only the checkout knows that)
     data-label-busy          status while waiting   (default "Processing payment…")
     data-label-done          status after success   (default "Payment received")
     data-label-retry         button label after an error (default "Try again")
     data-burst="off"         no particle burst on success
     data-min-busy="700"      ms the ring stays up at least, so a fast reply
                              does not flash (Vercel: 300 to 500ms minimum)

   A form example:
     form.addEventListener('submit', async e => {
       e.preventDefault();
       PayRing.start(btn);
       try { const r = await pay(); r.ok ? PayRing.success(btn) : PayRing.fail(btn, r.message); }
       catch (err) { PayRing.fail(btn, "We could not reach the payment service. Check your connection and try again."); }
     });
   While the ring is up, clicks and Enter are swallowed so nobody pays twice.
*/
(function () {
  var SVGNS = 'http://www.w3.org/2000/svg';
  var R = 20, C = 2 * Math.PI * R;          // ring radius in a 48 box, and its length
  var reduce = window.matchMedia && matchMedia('(prefers-reduced-motion: reduce)').matches;
  var store = new WeakMap();

  function $(el) { return typeof el === 'string' ? document.querySelector(el) : el; }
  function wait(ms) { return new Promise(function (r) { setTimeout(r, ms); }); }
  function svg(tag, attrs) {
    var n = document.createElementNS(SVGNS, tag);
    for (var k in attrs) n.setAttribute(k, attrs[k]);
    return n;
  }

  function enhance(el) {
    el = $(el);
    if (!el) throw new Error('PayRing: element not found');
    if (store.has(el)) return store.get(el);

    // the label
    var label = el.querySelector('.pay-ring__label');
    if (!label) {
      label = document.createElement('span');
      label.className = 'pay-ring__label';
      while (el.firstChild) label.appendChild(el.firstChild);
      el.appendChild(label);
    }
    el.classList.add('pay-ring');

    // the ring and the tick, hidden from assistive tech: the status line speaks
    var ring = svg('svg', { 'class': 'pay-ring__ring', viewBox: '0 0 48 48', 'aria-hidden': 'true', focusable: 'false' });
    ring.appendChild(svg('circle', { 'class': 'pay-ring__track', cx: 24, cy: 24, r: R }));
    var arc = svg('circle', { 'class': 'pay-ring__arc', cx: 24, cy: 24, r: R });
    arc.style.strokeDasharray = C;
    arc.style.strokeDashoffset = C * 0.72;
    ring.appendChild(arc);
    var tick = svg('svg', { 'class': 'pay-ring__tick', viewBox: '0 0 48 48', 'aria-hidden': 'true', focusable: 'false' });
    tick.appendChild(svg('path', { d: 'M15 24.5l6.2 6.2L33.5 18' }));
    el.appendChild(ring);
    el.appendChild(tick);

    // wrapper, status and error lines
    var wrap = el.parentElement && el.parentElement.classList.contains('pay-ring-wrap') ? el.parentElement : null;
    if (!wrap) {
      wrap = document.createElement('div');
      wrap.className = 'pay-ring-wrap';
      el.parentNode.insertBefore(wrap, el);
      wrap.appendChild(el);
    }
    if (el.classList.contains('pay-ring--block')) wrap.classList.add('pay-ring-wrap--block');
    var status = document.createElement('p');
    status.className = 'pay-ring__status';
    status.setAttribute('role', 'status');
    status.setAttribute('aria-live', 'polite');
    var error = document.createElement('p');
    error.className = 'pay-ring__error';
    error.setAttribute('role', 'alert');
    error.id = 'pr-err-' + Math.random().toString(36).slice(2, 8);
    wrap.appendChild(status);
    wrap.appendChild(error);

    var s = {
      el: el, label: label, arc: arc, wrap: wrap, status: status, error: error,
      original: label.innerHTML, startedAt: 0, queue: Promise.resolve()
    };
    store.set(el, s);
    el.dataset.state = 'idle';

    // swallow repeat clicks while busy or paid, so nobody pays twice
    el.addEventListener('click', function (e) {
      var st = el.dataset.state;
      if (st === 'busy' || st === 'success') { e.preventDefault(); e.stopImmediatePropagation(); return; }
      if (el.dataset.payRing === 'link') start(el);   // hosted checkout: let it navigate
    }, true);
    var form = el.form || el.closest('form');
    if (form) form.addEventListener('submit', function (e) {
      var st = el.dataset.state;
      if (st === 'busy' || st === 'success') { e.preventDefault(); e.stopImmediatePropagation(); }
    }, true);

    // back button from the checkout restores this page from cache with the
    // ring still spinning; put it back to idle
    if (el.dataset.payRing === 'link') {
      window.addEventListener('pageshow', function (e) { if (e.persisted) reset(el); });
      // checkout opened in a new tab: reset when the visitor comes back
      document.addEventListener('visibilitychange', function () {
        if (document.visibilityState === 'visible' && el.dataset.state === 'busy') reset(el);
      });
    }
    return s;
  }

  function collapse(s) {
    var el = s.el;
    el.style.width = el.offsetWidth + 'px';
    void el.offsetWidth;                       // commit the start width
    el.dataset.state = 'busy';
    el.style.width = getComputedStyle(el).height;
  }

  function expand(s) {
    var el = s.el, from = el.offsetWidth;
    el.style.width = '';
    var to = el.offsetWidth;                   // natural width with the new label
    el.style.width = from + 'px';
    void el.offsetWidth;
    el.style.width = to + 'px';
    var done = function () { el.style.width = ''; el.removeEventListener('transitionend', onEnd); };
    var onEnd = function (e) { if (e.propertyName === 'width') done(); };
    el.addEventListener('transitionend', onEnd);
    setTimeout(done, reduce ? 0 : 500);        // in case transitionend never fires
  }

  function start(el, opts) {
    var s = enhance(el);
    if (s.el.dataset.state === 'busy') return s.queue;
    opts = opts || {};
    s.startedAt = performance.now();
    s.error.textContent = '';
    s.el.removeAttribute('aria-describedby');
    s.el.removeAttribute('data-progress');
    s.arc.style.strokeDashoffset = C * 0.72;
    s.el.setAttribute('aria-busy', 'true');
    s.el.setAttribute('aria-disabled', 'true');
    collapse(s);
    s.status.removeAttribute('data-tone');
    s.status.textContent = opts.label || s.el.dataset.labelBusy || 'Processing payment…';
    s.queue = Promise.resolve();
    return s.queue;
  }

  function progress(el, p) {
    var s = enhance(el);
    if (s.el.dataset.state !== 'busy') return;
    p = Math.max(0, Math.min(1, +p || 0));
    s.el.dataset.progress = Math.round(p * 100);
    s.arc.style.strokeDashoffset = C * (1 - p);
  }

  // hold the ring up for at least data-min-busy ms, then run fn
  function settle(s, fn) {
    var min = +(s.el.dataset.minBusy || 700);
    var left = Math.max(0, min - (performance.now() - s.startedAt));
    s.queue = s.queue.then(function () { return wait(left); }).then(fn);
    return s.queue;
  }

  function success(el, opts) {
    var s = enhance(el);
    opts = opts || {};
    if (s.el.dataset.state !== 'busy') start(el);
    return settle(s, function () {
      s.el.dataset.progress = 100;
      s.arc.style.strokeDashoffset = 0;
      s.el.dataset.state = 'success';
      s.el.removeAttribute('aria-busy');
      s.status.dataset.tone = 'success';
      s.status.textContent = opts.label || s.el.dataset.labelDone || 'Payment received';
      if (s.el.dataset.burst !== 'off' && !reduce) burst(s);
      return wait(reduce ? 0 : 560);
    });
  }

  function fail(el, msg) {
    var s = enhance(el);
    if (s.el.dataset.state !== 'busy') start(el);
    return settle(s, function () {
      s.el.removeAttribute('aria-busy');
      s.el.removeAttribute('aria-disabled');
      s.el.removeAttribute('data-progress');
      s.label.textContent = s.el.dataset.labelRetry || 'Try again';
      s.el.dataset.state = 'error';
      expand(s);
      s.status.textContent = '';
      s.error.textContent = msg || 'The payment did not go through. Check your details and try again.';
      s.el.setAttribute('aria-describedby', s.error.id);
      if (!reduce) {
        s.el.classList.remove('is-shaking');
        void s.el.offsetWidth;
        s.el.classList.add('is-shaking');
        setTimeout(function () { s.el.classList.remove('is-shaking'); }, 420);
      }
      return wait(reduce ? 0 : 420);
    });
  }

  function reset(el) {
    var s = enhance(el);
    s.queue = Promise.resolve();
    s.label.innerHTML = s.original;
    s.el.removeAttribute('aria-busy');
    s.el.removeAttribute('aria-disabled');
    s.el.removeAttribute('aria-describedby');
    s.el.removeAttribute('data-progress');
    s.status.textContent = '';
    s.status.removeAttribute('data-tone');
    s.error.textContent = '';
    var was = s.el.dataset.state;
    s.el.dataset.state = 'idle';
    if (was === 'busy' || was === 'success') expand(s);
    return s.queue;
  }

  // eight to ten dots thrown out from the button centre, gone in 650ms
  function burst(s) {
    var el = s.el, wrap = s.wrap;
    var cx = el.offsetLeft + el.offsetWidth / 2, cy = el.offsetTop + el.offsetHeight / 2;
    var colours = [getComputedStyle(el).getPropertyValue('--pr-success').trim() || '#16a34a', '#86efac', '#facc15'];
    var n = 10;
    for (var i = 0; i < n; i++) {
      var d = document.createElement('span');
      d.className = 'pay-ring__spark';
      d.setAttribute('aria-hidden', 'true');
      d.style.left = cx + 'px';
      d.style.top = cy + 'px';
      d.style.background = colours[i % colours.length];
      wrap.appendChild(d);
      var a = (i / n) * Math.PI * 2 + Math.random() * 0.4;
      var dist = el.offsetHeight * (0.75 + Math.random() * 0.45);
      var anim = d.animate([
        { transform: 'translate(0,0) scale(1)', opacity: 1 },
        { transform: 'translate(' + Math.cos(a) * dist + 'px,' + Math.sin(a) * dist + 'px) scale(0.2)', opacity: 0 }
      ], { duration: 560 + Math.random() * 120, easing: 'cubic-bezier(0.23, 1, 0.32, 1)', fill: 'forwards' });
      anim.onfinish = (function (node) { return function () { node.remove(); }; })(d);
    }
  }

  window.PayRing = { start: start, progress: progress, success: success, fail: fail, reset: reset, enhance: enhance };

  function init() { document.querySelectorAll('[data-pay-ring]').forEach(function (el) { enhance(el); }); }
  if (document.readyState === 'loading') document.addEventListener('DOMContentLoaded', init);
  else init();
})();

```

## File: assets/scroll-sequence-demo.html

```html
<!DOCTYPE html>
<html lang="en-AU">
<head>
<meta charset="utf-8">
<link rel="icon" href="data:,">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Scroll Sequence Demo</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Nunito+Sans:wght@400;600;700&family=Rubik:wght@500;600;700&display=swap" rel="stylesheet">
<link rel="preload" as="image" href="scroll-sequence-frames/latte-001.jpg">
<link rel="stylesheet" href="scroll-sequence.css">
<style>
  /* page chrome only; the effect lives in scroll-sequence.css and .js.
     Design system query: Motion-Driven storytelling, warm brown #92400E on
     cream #FEF3C7, Rubik headings over Nunito Sans. */
  * { box-sizing: border-box; }
  body { margin: 0; background: #fef3c7; color: #78350f; font: 400 17px/1.6 "Nunito Sans", system-ui, sans-serif; -webkit-font-smoothing: antialiased; }
  h1, h2, h3 { font-family: "Rubik", system-ui, sans-serif; color: #451a03; line-height: 1.15; }
  .intro, .after { max-width: 760px; margin: 0 auto; padding: 56px 16px; }
  .banner { padding: 14px 18px; border: 1px solid #f0c36d; background: #fff7e6; border-radius: 14px; font-size: 14px; margin-bottom: 32px; }
  .intro h1 { font-size: clamp(34px, 6vw, 56px); margin: 0 0 12px; }
  .intro p { max-width: 56ch; margin: 0; }
  .hint { margin-top: 20px !important; font-weight: 700; color: #92400e; }

  .scroll-seq { --seq-bg: #f3e7cf; color: #92400e; }
  .eyebrow { font: 600 13px/1 "Rubik", sans-serif; letter-spacing: .08em; text-transform: uppercase; color: #92400e; margin: 0 0 10px; }
  .scroll-seq__step h2 { font-size: clamp(26px, 3.4vw, 40px); margin: 0 0 10px; }
  .scroll-seq__step p { margin: 0 0 14px; max-width: 40ch; }
  .chips { display: flex; flex-wrap: wrap; gap: 8px; margin: 0 0 14px; padding: 0; }
  .chip { min-height: 44px; padding: 0 16px; border-radius: 999px; border: 1.5px solid #d6b48a; background: #fffaf0; color: #78350f; font: 600 15px "Nunito Sans", sans-serif; cursor: pointer; touch-action: manipulation; transition: background-color .2s, border-color .2s, color .2s; }
  .chip[aria-pressed="true"] { background: #92400e; border-color: #92400e; color: #fff; }
  .chip:focus-visible, .cta:focus-visible { outline: 3px solid #b45309; outline-offset: 2px; }
  .price { font: 700 30px/1 "Rubik", sans-serif; color: #451a03; margin: 6px 0 16px; font-variant-numeric: tabular-nums; }
  .cta { display: inline-flex; align-items: center; min-height: 48px; padding: 0 24px; border-radius: 999px; background: #92400e; color: #fff; font: 600 16px "Rubik", sans-serif; text-decoration: none; }
  @media (hover: hover) { .cta:hover { background: #7c2d12; } }
  @media (max-width: 760px) {
    .scroll-seq__step h2 { font-size: 24px; }
    .scroll-seq__step p { font-size: 15px; margin-bottom: 10px; }
    .price { font-size: 24px; margin-bottom: 12px; }
  }
</style>
</head>
<body>
<header class="intro">
  <div class="banner"><strong>Demo, not live.</strong> The glass is a placeholder sequence of 60 frames drawn with ffmpeg. On a client site this is a real pour filmed or generated, cut into frames with the one liner in <code>scroll-sequence.js</code>.</div>
  <h1>Built the way you order it</h1>
  <p>A placeholder cafe page. Scroll down and the latte builds itself, one layer per step, with the options and the price changing beside it.</p>
  <p class="hint">Scroll to pour.</p>
</header>

<section class="scroll-seq" data-scroll-seq
         data-frames="scroll-sequence-frames/latte-{n}.jpg" data-count="60" data-pad="3"
         data-fit="contain" aria-label="How your iced latte is made">
  <div class="scroll-seq__stage">
    <div class="scroll-seq__media">
      <canvas class="scroll-seq__canvas" aria-hidden="true"></canvas>
      <img class="scroll-seq__still" src="scroll-sequence-frames/latte-001.jpg" width="720" height="960"
           alt="A tall glass ready for an iced latte">
    </div>
    <div class="scroll-seq__side">
      <div class="scroll-seq__step" data-at="0">
        <p class="eyebrow">Step 1 of 4</p>
        <h2>A cold glass</h2>
        <p>Straight from the fridge, so the ice lasts and the layers hold.</p>
      </div>
      <div class="scroll-seq__step" data-at="0.22">
        <p class="eyebrow">Step 2 of 4</p>
        <h2>Espresso, pulled to order</h2>
        <div class="chips" role="group" aria-label="Shots">
          <button class="chip" type="button" aria-pressed="false" data-group="shot" data-add="0">Single</button>
          <button class="chip" type="button" aria-pressed="true" data-group="shot" data-add="0.5">Double</button>
        </div>
      </div>
      <div class="scroll-seq__step" data-at="0.5">
        <p class="eyebrow">Step 3 of 4</p>
        <h2>Your milk, poured slow</h2>
        <div class="chips" role="group" aria-label="Milk">
          <button class="chip" type="button" aria-pressed="true" data-group="milk" data-add="0.6">Oat</button>
          <button class="chip" type="button" aria-pressed="false" data-group="milk" data-add="0">Full cream</button>
          <button class="chip" type="button" aria-pressed="false" data-group="milk" data-add="0.6">Almond</button>
        </div>
      </div>
      <div class="scroll-seq__step" data-at="0.8">
        <p class="eyebrow">Step 4 of 4</p>
        <h2>Iced oat latte</h2>
        <div class="chips" role="group" aria-label="Size">
          <button class="chip" type="button" aria-pressed="true" data-group="size" data-add="0">Regular</button>
          <button class="chip" type="button" aria-pressed="false" data-group="size" data-add="1">Large</button>
        </div>
        <p class="price" aria-live="polite">$<span data-price>6.60</span></p>
        <a class="cta" href="#order">Order for pickup</a>
      </div>
    </div>
    <div class="scroll-seq__bar" aria-hidden="true"></div>
  </div>
</section>

<section class="after" id="order">
  <h2>Ready in about four minutes</h2>
  <p>Content after the sequence scrolls in as normal. The sequence section is only as tall as its scroll track.</p>
</section>

<script src="scroll-sequence.js"></script>
<script>
  // demo only: the chips pick one per group and add up the price
  var base = 5.5;
  function total() {
    var t = base;
    document.querySelectorAll('.chip[aria-pressed="true"]').forEach(function (c) { t += +c.dataset.add; });
    document.querySelector('[data-price]').textContent = t.toFixed(2);
  }
  document.querySelectorAll('.chip').forEach(function (c) {
    c.addEventListener('click', function () {
      document.querySelectorAll('.chip[data-group="' + c.dataset.group + '"]').forEach(function (o) { o.setAttribute('aria-pressed', o === c ? 'true' : 'false'); });
      total();
    });
  });
  total();
</script>
</body>
</html>

```

## File: assets/scroll-sequence.css

```css
/* scroll-sequence.css
   The "latte builds itself as you scroll" section: a tall section whose
   sticky canvas plays an image sequence scrubbed by scroll, with optional
   side content (name, option chips, price, CTA) that changes at set points.
   Pairs with scroll-sequence.js. Dependency free.

   Progressive by default: without JS, or with prefers-reduced-motion, the
   section is a normal block with the still image and every step stacked as
   plain content. The JS adds .scroll-seq--live, which turns on the tall
   scroll track and the sticky stage.

   Tokens: --seq-track sets how much scrolling one play takes (300vh to 500vh
   reads well; under 250vh it feels like a jump). --seq-bg fills behind the
   canvas and should match the frames' own background.
   Demo: scroll-sequence-demo.html */

.scroll-seq {
  --seq-track: 400vh;
  --seq-bg: #f3e7cf;
  --seq-side-bg: rgba(255, 255, 255, 0.86);
  --seq-ease: cubic-bezier(0.23, 1, 0.32, 1);
  position: relative;
  background: var(--seq-bg);
}

/* ---- fallback layout (no JS, reduced motion) ---- */
.scroll-seq__stage {
  display: grid;
  gap: 24px;
  padding: 32px 16px;
  max-width: 1100px;
  margin: 0 auto;
}
.scroll-seq__media { position: relative; }
.scroll-seq__still {
  display: block;
  width: 100%;
  max-width: 520px;
  height: auto;
  margin: 0 auto;
}
.scroll-seq__canvas { display: none; }
.scroll-seq__side { display: grid; gap: 28px; }
.scroll-seq__step { margin: 0; }

/* ---- live layout ---- */
.scroll-seq--live { height: var(--seq-track); }
.scroll-seq--live .scroll-seq__stage {
  position: sticky;
  top: 0;
  height: 100vh;
  height: 100svh;            /* iPhone: ignore the toolbar so the stage never jumps */
  max-width: none;
  margin: 0;
  padding: 0;
  display: grid;
  grid-template-columns: minmax(0, 1.25fr) minmax(300px, 0.75fr);
  align-items: center;
  overflow: hidden;
}
.scroll-seq--live .scroll-seq__media {
  height: 100%;
  min-height: 0;
}
.scroll-seq--live .scroll-seq__canvas {
  display: block;
  width: 100%;
  height: 100%;
}
/* the still stays on top as frame 0 until the canvas has drawn */
.scroll-seq--live .scroll-seq__still {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  max-width: none;
  object-fit: cover;
  transition: opacity 240ms ease;
}
.scroll-seq--live[data-fit="contain"] .scroll-seq__still { object-fit: contain; }
.scroll-seq--live.is-drawn .scroll-seq__still { opacity: 0; pointer-events: none; }

.scroll-seq--live .scroll-seq__side {
  display: grid;
  gap: 0;
  padding: 0 clamp(16px, 4vw, 56px) 0 0;
}
.scroll-seq--live .scroll-seq__step {
  grid-area: 1 / 1;          /* every step in one cell, only the active one shows */
  opacity: 0;
  transform: translateY(14px);
  visibility: hidden;
  transition:
    opacity 320ms var(--seq-ease),
    transform 420ms var(--seq-ease),
    visibility 0s linear 420ms;
}
.scroll-seq--live .scroll-seq__step.is-active {
  opacity: 1;
  transform: none;
  visibility: visible;
  transition:
    opacity 320ms var(--seq-ease) 60ms,
    transform 420ms var(--seq-ease) 60ms,
    visibility 0s;
}

/* progress hairline, optional: <div class="scroll-seq__bar"></div> in the stage */
.scroll-seq__bar { display: none; }
.scroll-seq--live .scroll-seq__bar {
  display: block;
  position: absolute;
  left: 0;
  bottom: 0;
  height: 3px;
  width: 100%;
  background: currentColor;
  opacity: 0.35;
  transform-origin: 0 50%;
  transform: scaleX(var(--seq-p, 0));
}

/* phones: the canvas takes the top of the screen and the steps sit in a
   card underneath it, so the card never covers the product */
@media (max-width: 760px) {
  .scroll-seq--live .scroll-seq__stage {
    grid-template-columns: 1fr;
    grid-template-rows: minmax(0, 1fr) auto;
    align-items: stretch;
  }
  .scroll-seq--live .scroll-seq__side {
    margin: 0 12px calc(12px + env(safe-area-inset-bottom));
    padding: 18px;
    border-radius: 20px;
    background: var(--seq-side-bg);
    box-shadow: 0 10px 30px -12px rgba(0, 0, 0, 0.25);
  }
}

@media (prefers-reduced-motion: reduce) {
  .scroll-seq--live .scroll-seq__step { transition: none; }
}

```

## File: assets/scroll-sequence.js

```js
/* scroll-sequence.js
   Scroll scrubbed image sequence for scroll-sequence.css. Plain JS with
   IntersectionObserver and requestAnimationFrame, no dependencies.

   Markup:
     <section class="scroll-seq" data-scroll-seq
              data-frames="frames/latte-{n}.jpg" data-count="60" data-pad="3"
              data-frames-small="frames-720/latte-{n}.jpg"   (optional, phones)
              data-fit="cover">                               (or "contain")
       <div class="scroll-seq__stage">
         <div class="scroll-seq__media">
           <canvas class="scroll-seq__canvas" aria-hidden="true"></canvas>
           <img class="scroll-seq__still" src="frames/latte-001.jpg"
                width="720" height="960" alt="An iced latte in a tall glass">
         </div>
         <div class="scroll-seq__side">
           <div class="scroll-seq__step" data-at="0">...</div>
           <div class="scroll-seq__step" data-at="0.4">...</div>
           <div class="scroll-seq__step" data-at="0.8">...</div>
         </div>
       </div>
     </section>

   {n} is replaced by the frame number, starting at 1 and zero padded to
   data-pad digits, which is exactly what ffmpeg's %03d writes. The <img> is
   the poster: frame 1 shows the moment HTML arrives, before any JS, and it
   is also the still that reduced motion and no JS users get (point its src
   at the finished frame if that reads better as a still).

   Each .scroll-seq__step shows from its data-at (0 to 1 through the
   section) until the next step's data-at. Keep all real copy in the steps,
   never baked into the frames, so the page ranks and screen readers read it.

   Making the frames from a video (ffmpeg is at ~/.local/bin/ffmpeg):
     ffmpeg -i latte.mp4 -vf "fps=24,scale=1280:-2" -q:v 4 frames/latte-%03d.jpg
   WebP is about a third smaller:
     ffmpeg -i latte.mp4 -vf "fps=24,scale=1280:-2" -c:v libwebp -quality 72 frames/latte-%03d.webp
   Phone set: same again with scale=720:-2 into frames-720/. Aim for 60 to
   120 frames and under 5 MB in total; every frame is a download.

   GSAP ScrollTrigger works too, if the site already loads it from cdnjs:
     ScrollTrigger.create({ trigger: section, start: 'top top', end: 'bottom bottom',
       scrub: true, onUpdate: function (self) { ScrollSequence.seek(section, self.progress); } });
   and pass data-driver="external" so this file stops listening to scroll.
*/
(function () {
  var reduce = window.matchMedia && matchMedia('(prefers-reduced-motion: reduce)').matches;
  var instances = [];

  function pad(n, w) { n = String(n); while (n.length < w) n = '0' + n; return n; }

  function Seq(root) {
    this.root = root;
    this.canvas = root.querySelector('.scroll-seq__canvas');
    this.ctx = this.canvas.getContext('2d');
    this.still = root.querySelector('.scroll-seq__still');
    this.steps = [].slice.call(root.querySelectorAll('.scroll-seq__step'))
      .map(function (el) { return { el: el, at: parseFloat(el.dataset.at) || 0 }; })
      .sort(function (a, b) { return a.at - b.at; });
    this.count = parseInt(root.dataset.count, 10) || 1;
    var small = root.dataset.framesSmall && window.matchMedia('(max-width: 760px)').matches;
    this.pattern = small ? root.dataset.framesSmall : root.dataset.frames;
    this.padW = parseInt(root.dataset.pad, 10) || 3;
    this.fit = root.dataset.fit || 'cover';
    this.frames = new Array(this.count);
    this.loaded = new Array(this.count);
    this.current = -1;
    this.target = 0;
    this.progress = 0;
    this.started = false;
    this.ticking = false;

    root.classList.add('scroll-seq--live');
    this.size();
    this.update();

    var self = this;
    // frame 1 first, so the canvas can take over from the poster straight away
    this.load(0, function () { self.draw(true); });

    // load the rest only once the section is within two screens
    var io = new IntersectionObserver(function (entries) {
      entries.forEach(function (e) {
        self.near = e.isIntersecting;
        if (e.isIntersecting && !self.started) { self.started = true; self.preload(); }
      });
    }, { rootMargin: '200% 0px' });
    io.observe(root);

    if ('ResizeObserver' in window) new ResizeObserver(function () { self.size(); self.draw(true); }).observe(this.canvas);
    else window.addEventListener('resize', function () { self.size(); self.draw(true); });
  }

  Seq.prototype.src = function (i) {
    return this.pattern.replace('{n}', pad(i + 1, this.padW));
  };

  Seq.prototype.load = function (i, cb) {
    if (this.frames[i]) { if (cb && this.loaded[i]) cb(); return; }
    var self = this, img = new Image();
    img.decoding = 'async';
    this.frames[i] = img;
    img.onload = function () {
      var finish = function () { self.loaded[i] = true; if (cb) cb(); self.draw(); };
      img.decode ? img.decode().then(finish, finish) : finish();
    };
    img.onerror = function () { if (cb) cb(); };
    img.src = this.src(i);
  };

  // coarse to fine: every 16th frame, then every 8th, 4th, 2nd, then the
  // rest, so a fast scroller always has a near frame to show
  Seq.prototype.preload = function () {
    var order = [], seen = {}, n = this.count;
    [16, 8, 4, 2, 1].forEach(function (step) {
      for (var i = 0; i < n; i += step) if (!seen[i]) { seen[i] = 1; order.push(i); }
    });
    if (!seen[n - 1]) order.push(n - 1);
    var self = this, live = 0, max = 6, k = 0;
    (function next() {
      while (live < max && k < order.length) {
        var i = order[k++];
        if (self.frames[i]) continue;
        live++;
        self.load(i, function () { live--; next(); });
      }
    })();
  };

  Seq.prototype.size = function () {
    var dpr = Math.min(window.devicePixelRatio || 1, 2);   // 3x phones gain nothing visible at 3x the pixels
    var w = this.canvas.clientWidth, h = this.canvas.clientHeight;
    if (!w || !h) return;
    this.canvas.width = Math.round(w * dpr);
    this.canvas.height = Math.round(h * dpr);
  };

  Seq.prototype.nearest = function (i) {
    if (this.loaded[i]) return i;
    for (var d = 1; d < this.count; d++) {
      if (i - d >= 0 && this.loaded[i - d]) return i - d;
      if (i + d < this.count && this.loaded[i + d]) return i + d;
    }
    return -1;
  };

  Seq.prototype.draw = function (force) {
    var i = this.nearest(this.target);
    if (i < 0 || (i === this.current && !force)) return;
    var img = this.frames[i], c = this.canvas, cw = c.width, ch = c.height;
    if (!cw || !ch) return;
    var iw = img.naturalWidth, ih = img.naturalHeight;
    var s = this.fit === 'contain' ? Math.min(cw / iw, ch / ih) : Math.max(cw / iw, ch / ih);
    var dw = iw * s, dh = ih * s;
    this.ctx.clearRect(0, 0, cw, ch);
    this.ctx.drawImage(img, (cw - dw) / 2, (ch - dh) / 2, dw, dh);
    this.current = i;
    if (!this.drawn) { this.drawn = true; this.root.classList.add('is-drawn'); }
  };

  Seq.prototype.update = function () {
    var r = this.root.getBoundingClientRect();
    var run = r.height - window.innerHeight;
    var p = run > 0 ? Math.min(1, Math.max(0, -r.top / run)) : 0;
    this.seek(p);
  };

  Seq.prototype.seek = function (p) {
    this.progress = p;
    this.target = Math.round(p * (this.count - 1));
    this.root.style.setProperty('--seq-p', p.toFixed(4));
    this.draw();
    var active = 0;
    for (var k = 0; k < this.steps.length; k++) if (p >= this.steps[k].at) active = k;
    for (var j = 0; j < this.steps.length; j++) {
      var on = j === active;
      if (this.steps[j].el.classList.contains('is-active') !== on) this.steps[j].el.classList.toggle('is-active', on);
    }
  };

  function onScroll() {
    instances.forEach(function (s) {
      if (s.root.dataset.driver === 'external' || s.near === false || s.ticking) return;
      s.ticking = true;
      requestAnimationFrame(function () { s.ticking = false; s.update(); });
    });
  }

  function init() {
    if (reduce) return;   // reduced motion keeps the still and the stacked steps
    document.querySelectorAll('[data-scroll-seq]').forEach(function (root) {
      if (!root.querySelector('.scroll-seq__canvas') || !root.dataset.frames) return;
      instances.push(new Seq(root));
    });
    if (instances.length) {
      window.addEventListener('scroll', onScroll, { passive: true });
      window.addEventListener('resize', onScroll, { passive: true });
    }
  }

  window.ScrollSequence = {
    seek: function (root, p) {
      for (var i = 0; i < instances.length; i++) if (instances[i].root === root) instances[i].seek(p);
    }
  };

  if (document.readyState === 'loading') document.addEventListener('DOMContentLoaded', init);
  else init();
})();

```

## File: references/bencho.md

# Bencho, reading a block

bencho.dev has no public repo (github.com/lorenzo04us/Bencho is private) and
no API. The only way in is the site. Blocks are MIT (bencho.dev/licence), so
taking the code is fine.

## The 29 blocks (Sep 2026), with their `?c=` slugs

Deep link is `https://bencho.dev/?c=<slug>&theme=dark` (or `light`). The
slugs are not the display names, so use this table.

| Block | slug | Block | slug |
|---|---|---|---|
| Magnetic select | `magnet-select` | Inline confirm | `confirm` |
| Search | `seek` | Pull to refresh | `pull` |
| Escape button | `escape` | Reorder list | `liq-arrange` |
| Create menu | `liq-create` | Range dial | `sleep` |
| Radial menu | `radial` | Dragging ball | `drag-ball` |
| Now playing | `sound` | Slide to confirm | `slide-confirm` |
| Icon bar | `icon-bar` | Carousel | `carousel` |
| Progress ticks | `progress` | Slosh slider | `slosh` |
| Command bar | `command` | Palette | `palette` |
| Liquid toggle | `liq-toggle` | Drag stepper | `stepper` |
| Checklist | `checklist` | Notify | `toasts` |
| Assignees | `picker` | Magnifying dock | `dock` |
| Aspect ratio | `aspect` | Wheel | `humidity` |
| Canvas toolbar | `toolbar` | Selection list | `roster` |
| Tilt card | `tilt` | | |

New ones ship weekly. To refresh this table, open `https://bencho.dev/` and
run in `javascript_tool`: click each `button[aria-label^="Edit "]`, read
`new URL(location.href).searchParams.get('c')`, click the `Close` button.

## What each block gives you

Open a block and there are two tabs on the right panel:

- **Sliders tab.** Two to four named values (Tilt card has tilt, shade,
  corner). Tune them here, the preview updates live. What you set is what the
  Usage copy will carry.
- **Code tab** with two copy buttons:
  - **Usage**: the React API only, `<TiltCard tilt={10} shade={60} corner={20} />`
    plus a typed props list with the range of each value. Not the source.
  - **CSS**: the real payload. The stylesheet with the author's reasoning as
    comments, the transform maths, the gradients, the hover and reduced-motion
    splits. This is what you port.

Read the CSS comments before porting. They explain *why* (for Tilt card: the
card sinks under the cursor instead of lifting, and the two radial gradients do
as much work as the transform). That reasoning is the value; the numbers alone
are not.

## Pulling it with the built-in browser

1. `navigate` to `https://bencho.dev/?c=<slug>&theme=dark` using the table
   above. The panel opens on load. A wrong slug silently drops you on the
   wall; if that happens, `find` "Edit <Block name>" and click it.
2. Set the sliders to what the page needs (`Controls` tab, the default). Note
   the values.
3. Click the `Code` tab: `find` "Code" gives the ref, or click the `</>` icon
   top right of the panel.
4. The copy buttons write to the clipboard, which the tool cannot read, so
   shim it and click both with `javascript_tool`:

```js
window.__cap = [];
Object.defineProperty(navigator, 'clipboard', {
  value: { writeText: (t) => { window.__cap.push(t); return Promise.resolve(); } },
  configurable: true
});
document.querySelectorAll('.dtl-copy').forEach(b => b.click());
await new Promise(r => setTimeout(r, 1200));
window.__cap
```

   Returns two strings: `[0]` Usage, `[1]` CSS. Save the CSS to the
   scratchpad, then port.

5. The React component itself (`src/lab/<Name>.tsx` in the private repo) is
   not available. The JS is small enough to write from the CSS plus the
   behaviour you can see in the preview: read the CSS custom properties it
   expects (`--tilt`, `--x`, `--y` or similar) and write the pointer handler
   that feeds them.

## Porting notes

- The CSS already splits `@media (hover: hover)` from touch and carries a
  `prefers-reduced-motion` rule. Keep both.
- Bencho blocks are dark-first. Check contrast when dropping one onto a light
  small business site.
- Tell the user `Bencho <block name>` and the slider values you used.


## File: references/code-xr-login.md

# Login card: the @code.xr "Login form V5" reel

Ten-second Instagram reel by @code.xr (credit line on screen: "XNIMXS · Let
Him Cook"), 20 Sep 2026. First filed as "nothing, no lesson", then
reconsidered: stock library forms work better, but this one looks better.
So it was ported.

## What is on screen

- Near-black ground with thin red circuit lines: right-angled paths with a
  small glowing dot at each corner and end, a warm red pool bottom right.
- One glass card, about 740px wide, 18px radius, thin light border, heavy
  drop shadow with a faint red bloom.
- The card is split by a slanted divider. Left panel is darker glass:
  "HELLO, FRIEND!" bold uppercase, then "Enter your personal details to
  start your journey with us." in muted grey, both centred.
- Right panel: "Sign Up" title, three inputs (user, mail, lock icons on
  the left, placeholder text, dark fill, thin border). The focused input
  has a red border and a red icon.
- A full-width red gradient pill button with a red glow under it, then
  "Already have an account? Login" with Login in red.
- The code shown underneath is React boilerplate (useState, a fake 1.5s
  submit). Nothing in it produces the look; the look is all CSS.

## The port

`assets/login-card.css` plus `assets/login-card-demo.html`. Plain CSS, no
JS needed for the component itself. One token, `--lc-accent`, drives the
lines, focus ring, button and link; the demo has swatches to prove the
swap (reel red, orange, teal). Poppins from Google Fonts, or the
project's own sans through `--lc-font`. The slant is a skewed `::before`
so the text stays straight. On phones it stacks and drops the slant.

Verified 20 Sep 2026 in the browser pane at desktop and 375px, including
the focus state and the orange swap.

## Where it belongs

A login, sign-up or gated-area page on a dark site, or a client portal
landing. Not on a small business brochure site: those are light and have no login.
The circuit lines are decoration; leave them out when the page already has
a hero.


## File: references/feeds.md

# The two feeds

These are for taste, not code. Use them **before** designing, to pick the bar,
and **after** a build that is technically done but flat, to see what the good
ones do differently. Ten minutes, one or two references, then back to work.

Never scrape a feed wholesale. Never download the media into a project. Take
a link and a written description of what the reference does.

## Inspora, inspora.design

Curated design work, mostly short video so the motion is visible. Updated
hourly. Categories: Web, Branding, Product, Motion, Illustration, 3D, Print.

Server-rendered, so Jina reads it without a browser:

```bash
curl -s "https://r.jina.ai/https://inspora.design/?category=Web" | grep -o 'https://inspora.design/posts/[a-z0-9-]*' | sort -u | head -40
curl -s "https://r.jina.ai/https://inspora.design/posts/<slug>"
```

Each post page names the creator, the category and links the mp4 at
`media.inspora.design/posts/<id>.mp4`. To actually watch the motion, open
the post in the built-in browser, or run the `watch` skill on the mp4 URL
for frames plus a description.

Which category for which job:

- **Web**: landing pages and marketing sites. The default for a small business site.
- **Product**: app screens, dashboards, settings, onboarding. Use for
  business tools (quoting, scheduling, job tracking).
- **Motion**: page transitions and hero motion. Pair with React Bits.
- **Branding**: logo, type, colour systems. Use with the Awesome DESIGN.md
  brand files, never lift a real brand's palette.
- **3D, Illustration, Print**: rarely needed for web work. Skip unless asked.

## Best Designs on X, bestdesignsonx.com

Hand-picked design posts from X, updated hourly, with side tabs for
Instagram, Fonts, Dribbble, Behance, App Icons and OG Images. Every card links
the creator's X profile and carries the original image or mp4.

Client-rendered, so Jina returns an empty shell. Use the built-in browser:

1. `navigate` to `https://bestdesignsonx.com/`, wait 4 seconds for cards.
2. Search box at the top (`⌘K`): `find` "Search designs" and type a phrase
   (`pricing page`, `mobile onboarding`, `dark dashboard`).
3. Pull the visible cards with `javascript_tool`:

```js
[...document.querySelectorAll('video, img')]
  .map(m => m.src || m.querySelector?.('source')?.src)
  .filter(s => s && /cdn\.bestdesignsonx\.com\/(media|amplify_video)/.test(s))
  .slice(0, 12)
```

   and the creators with
   `[...document.querySelectorAll('a[href*="x.com/"]')].map(a => a.href)`.

4. Open one or two that fit the job, screenshot, describe.

The **Fonts** tab is worth its own visit when picking type: real pairings in
use, not a foundry's specimen. The **OG Images** tab is the reference for
share-preview images when a launch check flags a missing one.

## Writing the reference down

A link is not a reference. Before using one, write four lines:

- **Space**: how much air, how wide the gutters, how the sections breathe.
- **Weight**: type scale and contrast, where the heavy elements sit.
- **Motion**: what moves, on what trigger, how long, what curve.
- **Colour**: how many, where the accent lands, light or dark base.

That description is what goes into the build and into the reply to the user.
The link can die; the four lines cannot.


## File: references/juicelab-dashboard.md

# Source: juicelab.uiux "Leadly AI" dashboard concept

Instagram carousel, 6 slides, posted 10 Sep 2026, 900 likes.
https://www.instagram.com/p/DdEb9grklX1/

Studied on 18 Sep 2026 for what could be used. It is a concept
mock by a UI agency (Jucie Lab), not a shipped product: the copy has a
"Dashbaord" typo and a "Water production" label on a sales screen. Take the
styling, not the layout.

Instagram is not wired into agent-reach. What worked: `yt-dlp --dump-single-json`
for the caption, then the built-in browser for the images (close the sign-up
dialog, click the carousel Next arrow, read `img[src]` from the DOM, curl the
CDN URLs). Slides are 3277x4096 JPEGs.

## What the design does

One light-grey screen, a browser chrome mock, a pill tab nav with one blue
active pill. Then a grid of cards in two families:

1. **Glow tiles.** Rounded (about 26px) tiles with a radial gradient: fully
   saturated at the centre, fading to a pale frosted rim. White text. Big
   number in a dot-matrix typeface. Under it a thin dashed baseline, a curved
   sparkline, a hollow ring at the start and a filled dot at the current
   point. Pink, blue, green, purple, one hue each.
2. **White cards.** Same radius, very light grey. Small title, big dot-matrix
   number, muted caption. Coloured dot clusters as a tiny legend. Under a
   hairline, two secondary numbers.

Charts:

- **Sales Activity**: dense grey vertical hairlines (target) with sparser black
  bars (actual) sitting in front. No legend needed, reads as planned vs done.
- **Sales Pipeline**: purple tile, white curve over a hatched band, dashed
  target line, big "View Pipeline" pill button.
- **Deal Forecast**: candle chart. Trading desk styling, left out.

Presentation: tilted monitor render, a giant faded "Dashboard" word behind it,
small corner labels ("Light Shot", "Dashboard Design", "UI UX Design").

## What was ported

`assets/dashboard-lift.css` and `assets/dashboard-lift.js`, demo in
`assets/dashboard-lift-demo.html`.

| Block | From the reference | Notes |
|---|---|---|
| `.dot-num` | dot-matrix numerals | Doto, Google Fonts, OFL. Load with the `ROND` axis or the dots come out square: `family=Doto:wght,ROND@400..900,0..100`. Numbers only, never body text. |
| `.glow-tile` | glow tiles | One HSL triple per tile in `--h`. The rim frost is a second radial gradient in `::after`. |
| `.kpi-card` | white cards, dot clusters | Dot clusters share the `--h` variable. |
| `.ghost-bars` | Sales Activity chart | Built by JS from `data-groups="target:actual,..."`. |
| `.showcase-post` | the Instagram frame | For posting your own builds, not for client sites. |
| count-up | not in the reference | Magic UI `number-ticker`, spring swapped for ease-out cubic, reduced-motion sets the final value immediately. |

Left out: candle chart, the "Earth" tooltip, the avatar stack, the browser
chrome mock.

## Where it fits

- A product pitch page: the top stat row, so it does not look like every
  other admin panel.
- Any dashboard build (job tracking, stock, quoting): the KPI row and ghost
  bars for target vs actual.
- Instagram posts of finished client work: the showcase frame.

Two or three glow tiles per screen at most. More than that
reads as a lolly shop.


## File: references/source-post.md

# Source post

Instagram carousel by @shai.hq, posted 13 Sep 2026, studied 17 Sep 2026.
https://www.instagram.com/p/DdMCJyagZgR/

Caption: "If you're vibecoding apps using Claude code then these are gold
mine of inspiration and libraries. Bookmark these before they disappear. It's
time that you stop building AI slops!"

## The seven slides

1. **Cover.** "If you're vibecoding an app, you need this! Your app doesn't
   look bad. You're just building every component from scratch."
2. **01 / Components. SmoothUI**, smoothui.dev. "130 animated components that
   drop straight into shadcn. One command each."
3. **02 / Blocks. Bencho**, bencho.dev. "Interactive blocks you can tweak
   before you take them. Everything's live, not mocked."
4. **03 / Micro-interactions. Amicro**, amicro.vercel.app. "Tiny transitions
   and mono charts, one CLI command. The details nobody has time to build."
5. **04 / Inspiration. Inspora**, inspora.design. "Motion, product, branding,
   3D, sorted by category and updated hourly. Mostly video, so you see the
   interaction."
6. **05 / Inspiration. Best Designs on X**, bestdesignsonx.com. "The good
   stuff from design Twitter, without the doomscroll. Logos, UI and branding,
   hand-picked hourly."
7. **The point.** "Stop building from scratch. Steal the polish. Three
   libraries for the components. Two feeds for the taste. All free."

## What was checked on 17 Sep 2026

- SmoothUI: MIT, github.com/educlopez/smoothui, 133 component folders in
  the repo, 130 in the public catalogue. Has a no-auth REST API and
  `llms.txt`. Clone it for the source locally (14 MB).
- Bencho: MIT per bencho.dev/licence. Repo github.com/lorenzo04us/Bencho is
  private. 29 blocks on the site. Code is the CSS tab only, read via the
  browser.
- Amicro: MIT, github.com/Subhan-code/Amicro--Micro-transitions-. 163
  registry entries. Clone it for the source locally (5 MB).
- Inspora: feed, server-rendered, Jina reads it. Posts carry an mp4.
- Best Designs on X: feed, client-rendered, needs the browser. Cards link to
  creator X profiles and carry the media on cdn.bestdesignsonx.com.

The reel's shadcn framing does not apply to plain HTML, CSS and JS sites,
so there "one command each" becomes "read it and port it", same rule as
React Bits.


## File: scripts/smoothui.sh

```bash
#!/usr/bin/env bash
# SmoothUI public API helper. No auth needed. Needs curl and node.
#   smoothui.sh suggest "what you need in plain words"   # ranked matches
#   smoothui.sh search  "keyword"                        # keyword search
#   smoothui.sh source  <component-name>                 # metadata + full TSX
#   smoothui.sh list                                     # every component, one line each
# Docs: https://smoothui.dev/docs/guides/api
set -euo pipefail
BASE="https://smoothui.dev"
cmd="${1:-}"; arg="${2:-}"
enc() { node -e 'process.stdout.write(encodeURIComponent(process.argv[1]))' "$1"; }

case "$cmd" in
  suggest)
    [ -n "$arg" ] || { echo 'usage: smoothui.sh suggest "need"' >&2; exit 1; }
    curl -sf -m 30 "$BASE/api/v1/suggest?need=$(enc "$arg")" | node -e '
      const d=JSON.parse(require("fs").readFileSync(0,"utf8"));
      for (const s of d.suggestions||[]) console.log(String(s.relevanceScore).padStart(3), s.name.padEnd(34), s.description);'
    ;;
  search)
    [ -n "$arg" ] || { echo 'usage: smoothui.sh search "keyword"' >&2; exit 1; }
    curl -sf -m 30 "$BASE/api/v1/components/search?q=$(enc "$arg")" | node -e '
      const d=JSON.parse(require("fs").readFileSync(0,"utf8"));
      const items=Array.isArray(d)?d:(d.components||d.results||d.data||[]);
      for (const s of items) console.log(s.name.padEnd(34), s.description||"");'
    ;;
  source)
    [ -n "$arg" ] || { echo 'usage: smoothui.sh source <name>' >&2; exit 1; }
    curl -sf -m 30 "$BASE/api/v1/components/$arg?include=source" | node -e '
      const d=JSON.parse(require("fs").readFileSync(0,"utf8")); const c=d.component;
      console.log("#", c.displayName, "|", c.category, "|", c.complexity, "| deps:", (c.dependencies||[]).join(", ")||"none", "| reduced-motion:", c.hasReducedMotion);
      console.log("#", c.description);
      for (const h of c.compositionHints||[]) console.log("# hint:", h);
      console.log("# docs:", c.docUrl); console.log();
      console.log(d.source||"");'
    ;;
  list)
    curl -sf -m 30 "$BASE/llms.txt" | grep '^- ' | sed 's/ — install:.*//'
    ;;
  *)
    sed -n '2,7p' "$0"; exit 1 ;;
esac

```
