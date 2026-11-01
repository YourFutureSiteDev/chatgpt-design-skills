# ui-signals knowledge file

This file holds the whole `ui-signals` skill: SKILL.md first, then every reference, asset and script it points to. Each one starts with a `## File:` heading that gives its path inside the skill.

## File: SKILL.md

# UI Signals


> **Note for ChatGPT.** This skill was first written for Claude Code. Where it names Claude, a subagent, a slash command or a tool you do not have (a terminal, a browser preview, the user's own files), do that step yourself in the conversation, or give the user the exact code or command to run and say which step you could not run. Where it says to copy a file from `assets/` into the project, show the user that file's contents so they can paste it.

The video that seeded this skill (a @designmotionhq reel, breakdown in
`references/source-reel.md`) makes one argument in five scenes: **an interface
has to answer back.** A dropzone that does nothing when a file is dragged over
it, a spinner that hides how long is left, an upload that dies at 90% and makes
you start over, a bare filename instead of a preview, a batch where one failure
blocks the rest. Each scene fixes one of those with a small, specific motion.

This skill is that idea generalised into twenty-three signals, and one rule that
matters more than any of them. Signals 21 and 22 came from two more reels on
20 Sep 2026 (destructive actions by @designmotionhq, empty states by
Katherine Gilligan), and signal 23 from a Code & Chill reel on 21 Sep 2026
(the Verso login and sign-up card), all in `references/source-reel.md`.

## Sits after the module's behaviour

Decide how a module behaves and which states it needs (upload, button, toast, undo, empty state) before reaching for motion; this skill then places the feedback motion on the elements that exist. If a UX pattern skill is installed, run it first and treat its spec as the source of truth for states, with this catalogue supplying the motion values.

## The placement rule

**Every signal belongs to exactly one kind of element.** Drag feedback belongs
on a dropzone. Honest progress belongs on an operation that takes longer than a
second and has a measurable length. Count-up belongs on a stat. If the page has
no element of that kind, that signal does not ship, no matter how good it looks.

The short version is "only put them in relevant places." The failure this
guards against is the page where everything glows, everything slides in,
and nothing means anything. A signal that is on the wrong element is not a
small mistake, it is the whole mistake. So the workflow starts with an
inventory, not with the catalogue.

The same rule applies to the files: `signals.css` and `signals.js` go next to
the project's own stylesheet and script (its existing `css/`, `js/` or
`assets/` folder), never a new top-level folder, and a single-file site gets
only the blocks it uses inlined into its one stylesheet.

## Workflow

### 1. Inventory the page

Before opening the catalogue, list what is actually on the page by element
kind. Be literal: read the HTML, do not guess from the brief.

| Kind | What counts |
|---|---|
| Hero headline | The one H1 above the fold |
| Primary nav | The site's main links |
| Buttons | `<button>`, and `<a>` styled as a button |
| Text inputs | input, textarea, select |
| Form submit | The button that sends the form |
| File dropzone | Anywhere a file can be dragged or picked |
| Long operation | Anything that takes over ~1s: upload, export, payment, search |
| Batch operation | More than one long operation at once |
| Clickable card | A card that is itself a link |
| Static card | A card that is not |
| Tag / chip list | Features, benefits, badges, filters |
| Stat | A number the page is proud of |
| Async content | Content that arrives after first paint |
| Below-fold section | Sections the user scrolls to |
| Transient message | Toast, snackbar, "saved" |
| Modal / drawer | Overlays |
| Data table / body text | Things people read |
| Destructive control | Delete, remove, cancel subscription, close account |
| Empty container | A list, dashboard, inbox or search result that can have nothing in it |
| Auth card | One card that holds both login and sign-up |

Write the inventory down in the reply, briefly. It is the evidence for what
was left out.

### 2. Run the gate

For each candidate, run the two questions from `animate`: how often will a
user see this (100+ a day means no animation), and what is the purpose
(feedback, state, spatial, preventing a jarring change, delight only on rare
moments). If you cannot name the purpose in one word, the signal does not
ship. Read `animate/SKILL.md` if the values below need justifying.

### 3. Pick from the catalogue

Match inventory rows to signals. A normal page lands **four to eight**. A
form-heavy or upload-heavy app lands more; a marketing page with no forms
lands fewer. Never all twenty-three.

| Signal | Belongs on | Never on | Trigger |
|---|---|---|---|
| `drop-answer` | File dropzone | Anything that is not a dropzone | dragenter / dragleave / drop |
| `honest-progress` | Long op with known length | Ops of unknown length (use `skeleton`) | progress events |
| `inline-retry` | Long op that can fail | Ops that cannot fail | error |
| `upload-preview` | After a file upload succeeds | Text-only confirmations | success |
| `own-lane-queue` | Batch operation | Single operations | each item's progress |
| `press` | Buttons, button-styled links | Nav text links, body links | pointerdown |
| `focus-glow` | Text inputs | Buttons, cards | focus |
| `submit-states` | Form submit button | Any other button | submit → loading → done / error |
| `field-error` | Invalid form field | The whole form | validation |
| `copy-swap` | Any label whose text changes with state | Static copy | state change |
| `state-glow` | Exactly one active element at a time | More than one at once, decoration | active state |
| `tick-draw` | Success states | Neutral or error states | success |
| `toast` | Transient message | Persistent content | show / hide |
| `chip-stagger` | Tag / chip list entering view | Lists the user is reading through | intersection, once |
| `headline-emphasis` | The hero H1, one phrase | Any other heading, body copy | load, once |
| `section-reveal` | Below-fold sections | Above-the-fold content, first section | intersection, once |
| `card-lift` | Clickable cards | Static cards, table rows | hover, hover-gated |
| `nav-underline` | Primary nav links | Footer links, in-text links | hover, active |
| `count-up` | Stats section, 2s or slower | Prices in a checkout, table cells | intersection, once |
| `skeleton` | Async content | Content already in the HTML | until loaded |
| `danger-confirm` | Destructive control | Anything reversible, log out, cancel-this-dialog | pointerdown held for `--sig-dur-hold` |
| `empty-state` | Empty container, first run, zero results, inbox zero | Containers that always have content | container has no items |
| `auth-sweep` | The one card holding both login and sign-up | A lone login form, settings tabs, any modal | click on the switch link; forms swap at half travel |

Recipes, values and the HTML they expect are in `references/catalogue.md`.
Read the entry for every signal you ship; do not write the CSS from memory.

### 4. Install the assets in the project's own structure

`assets/signals.css` and `assets/signals.js` are dependency-free and written
for hand-written HTML, CSS and JS static sites. Never add React or a motion library to a static site to use them.

- Copy both files into the folder the project already keeps its CSS and JS
  in. Link `signals.css` last, after the project's own stylesheet and any
  inline `<style>`, so its state rules (an error button going red, a focused
  input's ring) beat the project's base button and input styles at equal
  specificity. The demo page does this on purpose.
- Set the three tokens at the top of `signals.css` (`--sig-accent`,
  `--sig-danger`, `--sig-surface`) from the project's palette. Never leave
  the demo teal on a client site.
- If the project has one stylesheet and no JS file, inline only the blocks
  for signals that shipped. Unused blocks are dead weight.
- If a signal needs a component the site does not have (a real toast, a
  modal), take it from an open source library such as Preline or daisyUI
  rather than hand-rolling one, and name the component in the reply.

### 5. Wire only matching elements

Each signal is opted into by a class or data attribute on the element it
belongs to. Nothing is applied globally except the easing tokens. If you find
yourself adding `data-sig-reveal` to every section including the hero, stop
and re-read the placement rule.

### 6. Verify in the browser

Open the page in the browser pane. For every signal that shipped, trigger its
state (drag a file, submit the form, scroll to the section, hover the card)
and screenshot it in that state. Then toggle reduced motion (DevTools
rendering panel, or `resize_window` with the emulation) and confirm the page
still reads correctly with the motion gone. A signal that only works in your
head has not shipped.

### 7. Report

Keep it short: one line per signal that landed, naming the element it is
on; one line listing what was deliberately left out and why; the name of the
source (this reel, a React Bits component, a Preline component) so the user can look at
the original. Show the screenshots. No essay.

## The "plus more"

This catalogue is the feedback layer. Two other sources cover the rest, and
they are read, not reinvented:

- **Hero motion, text effects, animated backgrounds:** React Bits
  (reactbits.dev). Port by hand into CSS and JS; name the
  component you took it from.
- **Modals, drawers, dropdowns, accordions:** `animate/RECIPES.md`. (Hold-to-confirm
  now lives here as `danger-confirm`.)
- **Whole components (docks, navs, pricing tables):** an open source library such as Preline or daisyUI.

When a request is "animate the site" with no further detail, the split is:
this skill for every interactive element, React Bits for the hero only if the
page is a marketing page, and `animate` as the referee for every value.

## Values

All timings and curves come from `animate`. The ones this catalogue uses:

```css
--sig-ease-out: cubic-bezier(0.23, 1, 0.32, 1);     /* entrances, state changes */
--sig-ease-in-out: cubic-bezier(0.77, 0, 0.175, 1); /* on-screen movement */
--sig-dur-press: 120ms;
--sig-dur-state: 150ms;    /* focus, hover, copy swap */
--sig-dur-enter: 220ms;    /* toast, error row, preview card */
--sig-dur-reveal: 400ms;   /* below-fold sections, marketing tier */
--sig-dur-count: 2000ms;   /* count-up, deliberately slow, fast ones read as fake */
```

Progress bars move with `transform: scaleX()` on a `linear` curve, never
`width`. Nothing enters from `scale(0)`. Nothing uses `transition: all`.
Hover motion is gated behind `@media (hover: hover) and (pointer: fine)`.
Reduced motion keeps colour and opacity changes and drops movement.

## Done means

Before handing this back, check every line. If any fails, fix it and check
again. Do not report the work as finished until all of them pass.

- [ ] The reply contains the page inventory, and every shipped signal maps to a row in it
- [ ] No signal is on an element kind listed in its "Never on" column
- [ ] The hero and any above-the-fold content have no `section-reveal`
- [ ] At most one element carries `state-glow` at any time
- [ ] Every destructive control is a `danger-confirm` with a verb label ("Delete project", never "Yes" or "Confirm"), sits away from the primary action, and `--sig-danger` red appears on nothing else on the page (a red log-out button is a fail)
- [ ] Every container that can be empty has a `sig-empty` that says why it is empty and gives one next action; no blank panels on first run
- [ ] A page with both login and sign-up puts them on one `sig-auth` card with real forms and real hrefs on the switch links; a page with only one of the two gets no sweep
- [ ] `signals.css` and `signals.js` live in the project's existing asset folder, not a new one, and the tokens are set to the project's palette (no demo teal on a client site)
- [ ] No React or motion library was added to a static site
- [ ] Every shipped signal was triggered in the browser pane and screenshotted in its animated state
- [ ] Reduced motion was checked and the page still reads with movement removed
- [ ] No `transition: all`, no `scale(0)` entrance, no `ease-in`, no animated `width`/`height`/`top`/`left` (progress bars use `scaleX`)
- [ ] The reply names what was left out and why, and names the source of anything lifted (this reel, a React Bits component, a Preline or daisyUI component)
- [ ] Every count-up runs for at least two seconds (a 0.8s version was rejected in testing: numbers should load much slower than everything else)
- [ ] No em dashes, no emojis, in the reply or in any copy written into the site

If a line cannot be checked without the user (for example, whether a card is
meant to be clickable), say which one and why, rather than assuming it
passes.

When the user sends something back, the fix is a new line here, not a rewrite of
the instructions above.


## File: assets/demo.html

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>UI Signals</title>
<style>
  /* Demo layout only. Nothing here belongs in a client site.
     signals.css is linked AFTER this block on purpose: its state rules must
     beat the base button and panel styles, which is the install rule. */
  :root {
    --bg: #0c0e12;
    --ink: #e6e9ee;
    --muted: #8a93a3;
    --panel: #15181d;
    --line: rgba(255,255,255,0.09);
  }
  * { box-sizing: border-box; }
  html, body { margin: 0; }
  body {
    font: 15px/1.5 -apple-system, BlinkMacSystemFont, "Segoe UI", Inter, Roboto, sans-serif;
    color: var(--ink);
    background:
      linear-gradient(rgba(255,255,255,0.035) 1px, transparent 1px) 0 0 / 40px 40px,
      linear-gradient(90deg, rgba(255,255,255,0.035) 1px, transparent 1px) 0 0 / 40px 40px,
      var(--bg);
  }
  main { max-width: 720px; margin: 0 auto; padding: 72px 20px 120px; }
  h1 { font-size: 40px; line-height: 1.1; letter-spacing: -0.02em; margin: 0 0 12px; }
  .lede { color: var(--muted); margin: 0 0 8px; max-width: 48ch; }
  .eyebrow { font-size: 11px; letter-spacing: 0.18em; text-transform: uppercase; color: var(--muted); margin: 0 0 8px; }
  section { margin-top: 72px; }
  section h2 { font-size: 24px; letter-spacing: -0.01em; margin: 0 0 6px; }
  section h2 em { font-style: normal; color: var(--sig-accent); }
  section > p { color: var(--muted); margin: 0 0 20px; max-width: 52ch; }
  .rule { font-size: 12px; color: var(--muted); border-left: 2px solid var(--sig-accent); padding-left: 10px; margin: 0 0 20px; }
  .panel { background: var(--panel); border: 1px solid var(--line); border-radius: 14px; padding: 18px; }
  .row { display: flex; align-items: center; gap: 12px; flex-wrap: wrap; }
  .btn {
    appearance: none; cursor: pointer; font: inherit; color: var(--ink);
    background: #1d2129; border: 1px solid var(--line); border-radius: 10px; padding: 9px 16px;
  }
  .btn.primary { background: var(--sig-accent); color: #06221f; border-color: transparent; font-weight: 600; }
  .btn.ghost { background: transparent; }
  .demo-actions { margin-top: 14px; display: flex; gap: 8px; flex-wrap: wrap; }
  .demo-actions .btn { font-size: 13px; padding: 7px 12px; }
  .muted { color: var(--muted); }
  .small { font-size: 12px; }
  .mono { font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; }
  input.field, textarea.field {
    width: 100%; font: inherit; color: var(--ink); background: #0f1216;
    border: 1px solid var(--line); border-radius: 10px; padding: 10px 12px;
  }
  label { display: block; font-size: 13px; color: var(--muted); margin: 0 0 6px; }
  .grid2 { display: grid; grid-template-columns: 1fr 1fr; gap: 14px; }
  @media (max-width: 600px) { .grid2 { grid-template-columns: 1fr; } }

  /* dropzone content */
  .dz { min-height: 180px; display: grid; place-items: center; text-align: center; padding: 28px; cursor: pointer; }
  .dz .sig-icon { width: 34px; height: 34px; color: var(--muted); }
  .dz h3 { margin: 10px 0 2px; font-size: 17px; }
  .dz p { margin: 0; color: var(--muted); font-size: 13px; }
  .dz input { display: none; }

  /* progress card */
  .up-head { display: flex; align-items: center; gap: 12px; }
  .up-ico { width: 40px; height: 40px; border-radius: 10px; background: #1d2129; display: grid; place-items: center; color: var(--muted); }
  .up-name { font-weight: 600; }
  .up-sub { color: var(--muted); font-size: 12px; }
  .up-pct { margin-left: auto; font-size: 30px; font-weight: 700; letter-spacing: -0.02em; }
  .sig-bar { margin: 12px 0 8px; }
  .up-status { font-size: 13px; color: var(--muted); }
  .sig-progress-meta > span { font-size: 12px; padding: 4px 8px; border: 1px solid var(--line); border-radius: 6px; }
  .sig-retry-card { padding: 10px 12px; margin-top: 10px; display: flex; align-items: center; gap: 12px; }
  .sig-retry-card b { color: var(--sig-danger); display: block; font-size: 13px; }
  .sig-retry-card span { color: var(--muted); font-size: 12px; }
  .sig-retry-card .btn { margin-left: auto; }

  /* preview card */
  .sig-preview { display: grid; grid-template-columns: 120px 1fr; gap: 16px; }
  .sig-thumb { width: 120px; height: 120px; display: grid; place-items: center; color: rgba(255,255,255,0.7); position: relative; overflow: hidden; }
  .sig-thumb img { width: 100%; height: 100%; object-fit: cover; }
  .sig-thumb small { position: absolute; right: 6px; bottom: 6px; background: rgba(0,0,0,0.55); padding: 2px 6px; border-radius: 4px; font-size: 10px; }
  .pv-name { font-weight: 600; margin: 4px 0 8px; }
  .pv-proof { display: flex; align-items: center; gap: 8px; color: var(--muted); font-size: 13px; margin-top: 12px; border-top: 1px solid var(--line); padding-top: 10px; }
  .pv-proof .sig-tick { color: var(--sig-accent); }
  .pv-actions { grid-column: 1 / -1; display: flex; gap: 8px; border-top: 1px solid var(--line); padding-top: 12px; }

  /* queue */
  .q-head { display: flex; align-items: center; justify-content: space-between; margin-bottom: 12px; }
  .q-head b { font-weight: 600; }
  .q-count { font-size: 12px; padding: 3px 8px; border: 1px solid var(--line); border-radius: 6px; color: var(--muted); }
  .sig-queue-item { display: grid; grid-template-columns: 32px 1fr 56px; align-items: center; gap: 12px; padding: 10px 12px; border: 1px solid var(--line); border-radius: 10px; margin-bottom: 8px; }
  .sig-queue-item .q-ico { width: 32px; height: 32px; border-radius: 8px; background: #1d2129; display: grid; place-items: center; color: var(--muted); }
  .sig-queue-item .q-name { font-size: 13px; font-weight: 600; }
  .sig-queue-item .q-name small { color: var(--muted); font-weight: 400; margin-left: 6px; }
  .sig-queue-item .sig-bar { margin: 6px 0 0; height: 3px; }
  .q-end { position: relative; text-align: right; font-size: 13px; }
  .q-end .sig-tick-wrap { position: absolute; right: 0; top: 50%; transform: translateY(-50%); color: var(--sig-accent); }

  /* tick svg sizing */
  .sig-tick { width: 20px; height: 20px; display: block; }

  /* toast look */
  .sig-toast { background: #1d2129; border: 1px solid var(--line); color: var(--ink); padding: 10px 14px; border-radius: 10px; font-size: 14px; box-shadow: 0 10px 30px -12px rgba(0,0,0,0.8); }

  /* chips */
  .sig-chips { display: flex; gap: 8px; flex-wrap: wrap; list-style: none; padding: 0; margin: 0; }
  .sig-chips li { padding: 6px 10px; border: 1px solid var(--line); border-radius: 8px; font-size: 13px; }
  .sig-chips li b { color: var(--sig-accent); font-weight: 600; margin-right: 6px; }

  /* cards */
  .card { display: block; color: inherit; text-decoration: none; padding: 16px; border: 1px solid var(--line); border-radius: 12px; background: var(--panel); }
  .card h4 { margin: 0 0 4px; font-size: 15px; }
  .card p { margin: 0; color: var(--muted); font-size: 13px; }

  /* nav */
  .sig-nav { display: flex; gap: 22px; font-size: 14px; }
  .sig-nav a { color: var(--ink); padding: 4px 0; }

  /* stats */
  .stats { display: grid; grid-template-columns: repeat(3, 1fr); gap: 14px; }
  .stat { text-align: center; }
  .stat b { display: block; font-size: 32px; letter-spacing: -0.02em; }
  .stat span { color: var(--muted); font-size: 13px; }

  /* skeleton */
  .sk-line { height: 14px; margin: 8px 0; }
  .sk-line.w60 { width: 60%; } .sk-line.w80 { width: 80%; } .sk-line.w40 { width: 40%; }

  /* glow demo */
  .step { padding: 14px; border: 1px solid var(--line); border-radius: 12px; cursor: pointer; }
  .step b { display: block; font-size: 14px; }
  .step span { color: var(--muted); font-size: 12px; }
  .step.is-active { border-color: var(--sig-accent); }

  /* submit stack */
  .sig-submit { min-width: 120px; }

  footer { margin-top: 96px; color: var(--muted); font-size: 12px; }

  /* auth card (signal 23): the reel's cream, deep green and gold. Demo only. */
  .auth { --sig-auth-card: #f4efe6; --sig-auth-ink: #16201b; --sig-auth-band-bg: #0f3d2e; --sig-auth-band-ink: #f4efe6; --sig-auth-seam: #d4a53a; box-shadow: 0 30px 60px rgba(0,0,0,0.35); }
  .auth .auth-title { margin: 0 0 22px; font-size: 22px; letter-spacing: -0.01em; }
  .auth .auth-title::after { content: ""; display: block; width: 44px; height: 2px; margin-top: 8px; background: #d4a53a; }
  .auth label { color: rgba(22,32,27,0.62); }
  .auth .field { background: transparent; border: 0; border-bottom: 1px solid rgba(22,32,27,0.28); border-radius: 0; padding: 10px 0; color: #16201b; }
  .auth .sig-focus:focus-visible { border-bottom-color: #d4a53a; box-shadow: 0 1px 0 0 #d4a53a; }
  .auth .sig-field { margin-bottom: 6px; }
  .auth .auth-row { display: flex; align-items: center; justify-content: space-between; gap: 12px; margin: 6px 0 18px; font-size: 12.5px; color: rgba(22,32,27,0.75); }
  .auth .auth-row a { white-space: nowrap; }
  .auth .auth-row label { display: flex; align-items: center; gap: 6px; margin: 0; color: inherit; white-space: nowrap; }
  .auth .auth-row input { accent-color: #0f3d2e; }
  .auth a { color: #16201b; font-weight: 600; text-decoration: underline; text-underline-offset: 3px; text-decoration-color: #d4a53a; }
  .auth .btn.auth-cta { width: 100%; padding: 13px 16px; border-radius: 999px; border-color: transparent; background: #0f3d2e; color: #f4efe6; font-weight: 600; }
  .auth .auth-alt { margin: 16px 0 0; font-size: 13px; color: rgba(22,32,27,0.75); }
  .auth .auth-hint { margin: 6px 0 0; font-size: 12px; color: rgba(22,32,27,0.55); }
</style>
<link rel="stylesheet" href="signals.css">
</head>
<body>
<main>

  <p class="eyebrow">ui-signals</p>
  <h1 data-sig-headline>An interface has to <em>answer back.</em></h1>
  <p class="lede">Twenty-three feedback and state signals. Each belongs on one kind of element. This page is the test bench: trigger every state and watch it.</p>
  <nav class="sig-nav" aria-label="Demo">
    <a href="#s01" aria-current="page">Upload</a>
    <a href="#s06">Forms</a>
    <a href="#s13">Page</a>
    <a href="#s19">Loading</a>
    <a href="#s23">Sign in</a>
  </nav>

  <!-- 01 drop-answer -->
  <section id="s01">
    <p class="eyebrow">Signal 01 · Drag feedback</p>
    <h2>Nothing reacts. It has to <em>answer back.</em></h2>
    <p>Three signals before the drop: border, glow, copy. Drag any file over the box.</p>
    <p class="rule">Belongs on: a file dropzone. Never on: anything that is not one.</p>
    <div class="sig-dropzone sig-glow dz" data-sig-dropzone id="dz">
      <div class="sig-swap">
        <div data-when="idle">
          <svg class="sig-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"><path d="M7 18a4 4 0 0 1-.5-7.97A6 6 0 0 1 18 9a3.5 3.5 0 0 1 .5 7"/><path d="M12 12v8m0-8-3 3m3-3 3 3"/></svg>
          <h3>Drop your file</h3>
          <p>PNG, JPG, PDF, up to 50 MB</p>
        </div>
        <div data-when="over">
          <svg class="sig-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"><path d="M12 19V5m0 0-5 5m5-5 5 5"/><path d="M5 21h14"/></svg>
          <h3>Release to upload</h3>
          <p data-sig-dropcopy>1 file</p>
        </div>
      </div>
      <input type="file" multiple>
    </div>
    <div class="demo-actions">
      <button class="btn sig-press" data-dz="over">Show "over" state</button>
      <button class="btn sig-press" data-dz="idle">Back to idle</button>
    </div>
  </section>

  <!-- 02 honest-progress + 03 inline-retry -->
  <section id="s02">
    <p class="eyebrow">Signal 02 · Honest progress</p>
    <h2>A spinner <em>hides the truth.</em></h2>
    <p>Percent, time left, rate. The user decides whether to wait or walk away.</p>
    <p class="rule">Belongs on: an operation over one second with a measurable length. Unknown length: use the skeleton instead.</p>

    <p class="eyebrow" style="margin-top:28px">Signal 03 · Inline retry</p>
    <h2>It dies at <em>ninety percent.</em></h2>
    <p>The bar turns red where it stopped, the error grows in with a Retry, the file stays. Retry resumes from 90%. Never make them start over.</p>
    <p class="rule">Belongs on: any network action that can fail. Never on: actions that cannot fail.</p>

    <div class="panel sig-progress" id="up1" data-state="idle">
      <div class="up-head">
        <div class="up-ico"><svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><rect x="3" y="5" width="18" height="14" rx="2"/><path d="M7 5v14M17 5v14M3 10h4M17 10h4M3 14h4M17 14h4"/></svg></div>
        <div><div class="up-name">demo-recording.mp4</div><div class="up-sub">48 MB, video/mp4</div></div>
        <div class="up-pct sig-progress-pct" data-pct>0%</div>
      </div>
      <div class="sig-bar"><i class="sig-bar-fill"></i></div>
      <div class="up-status" data-status>Waiting</div>
      <div class="sig-progress-meta" style="margin-top:10px">
        <span data-eta></span>
        <span data-rate></span>
      </div>
      <div class="sig-retry-row">
        <div>
          <div class="sig-retry-card">
            <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="var(--sig-danger)" stroke-width="1.8" stroke-linecap="round"><path d="M12 9v4m0 4h.01M10.3 3.9 1.8 18a2 2 0 0 0 1.7 3h17a2 2 0 0 0 1.7-3L13.7 3.9a2 2 0 0 0-3.4 0z"/></svg>
            <div><b>Upload failed</b><span data-error-msg>Connection lost</span></div>
            <button type="button" class="btn sig-press" data-retry>Retry</button>
          </div>
        </div>
      </div>
    </div>
    <div class="demo-actions">
      <button class="btn primary sig-press" id="up1-go">Simulate upload</button>
      <button class="btn sig-press" id="up1-fail">Fail at 90%</button>
      <button class="btn ghost sig-press" id="up1-reset">Reset</button>
    </div>
  </section>

  <!-- 04 upload-preview -->
  <section id="s04">
    <p class="eyebrow">Signal 04 · Upload preview</p>
    <h2>A filename is <em>not feedback.</em></h2>
    <p>Thumbnail, type, size, proof, and the two actions that matter next.</p>
    <p class="rule">Belongs on: the moment after a file upload succeeds. Never as: a text-only line.</p>
    <div class="panel muted small mono" style="margin-bottom:12px">Text only: &#10003; IMG_4032.jpg uploaded</div>
    <div class="panel sig-preview" id="pv1" hidden>
      <div class="sig-thumb">
        <img data-thumb alt="" hidden>
        <svg width="34" height="34" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="3" y="4" width="18" height="16" rx="2"/><circle cx="9" cy="10" r="1.6"/><path d="M21 16l-5-5-8 8"/></svg>
        <small class="mono">4032 × 3024</small>
      </div>
      <div>
        <div class="pv-name" data-name>IMG_4032.jpg</div>
        <div class="row"><span class="sig-chip" data-type>JPG</span><span class="small mono" data-size>2.4 MB</span></div>
        <div class="pv-proof">
          <svg class="sig-tick" viewBox="0 0 24 24"><circle class="sig-tick-ring" cx="12" cy="12" r="10"/><path class="sig-tick-path" d="M7 12.5l3.2 3.2L17 9"/></svg>
          Uploaded just now
        </div>
      </div>
      <div class="pv-actions">
        <button class="btn sig-press">Replace</button>
        <button class="btn ghost sig-press">Remove</button>
      </div>
    </div>
    <div class="demo-actions">
      <button class="btn primary sig-press" id="pv1-show">Show preview</button>
      <button class="btn ghost sig-press" id="pv1-hide">Hide</button>
    </div>
  </section>

  <!-- 05 own-lane-queue -->
  <section id="s05">
    <p class="eyebrow">Signal 05 · Independent queue</p>
    <h2>Every file, <em>its own lane.</em></h2>
    <p>One failure never blocks the others.</p>
    <p class="rule">Belongs on: a batch of long operations. Never on: a single one.</p>
    <div class="panel" id="q1">
      <div class="q-head"><b>Uploading 5 files</b><span class="q-count mono" data-done>0 of 5 done</span></div>
      <div class="sig-queue-item" data-sig-queue-item data-state="idle">
        <div class="q-ico">&#9636;</div>
        <div><div class="q-name">brief.pdf<small>1.2 MB</small></div><div class="sig-bar"><i class="sig-bar-fill"></i></div></div>
        <div class="q-end"><span class="sig-queue-pct" data-pct>0%</span><span class="sig-tick-wrap"><svg class="sig-tick" viewBox="0 0 24 24"><circle class="sig-tick-ring" cx="12" cy="12" r="10"/><path class="sig-tick-path" d="M7 12.5l3.2 3.2L17 9"/></svg></span></div>
      </div>
      <div class="sig-queue-item" data-sig-queue-item data-state="idle">
        <div class="q-ico">&#9635;</div>
        <div><div class="q-name">hero-banner.png<small>2.4 MB</small></div><div class="sig-bar"><i class="sig-bar-fill"></i></div></div>
        <div class="q-end"><span class="sig-queue-pct" data-pct>0%</span><span class="sig-tick-wrap"><svg class="sig-tick" viewBox="0 0 24 24"><circle class="sig-tick-ring" cx="12" cy="12" r="10"/><path class="sig-tick-path" d="M7 12.5l3.2 3.2L17 9"/></svg></span></div>
      </div>
      <div class="sig-queue-item" data-sig-queue-item data-state="idle">
        <div class="q-ico">&#9654;</div>
        <div><div class="q-name">demo-recording.mp4<small>48 MB</small></div><div class="sig-bar"><i class="sig-bar-fill"></i></div></div>
        <div class="q-end"><span class="sig-queue-pct" data-pct>0%</span><span class="sig-tick-wrap"><svg class="sig-tick" viewBox="0 0 24 24"><circle class="sig-tick-ring" cx="12" cy="12" r="10"/><path class="sig-tick-path" d="M7 12.5l3.2 3.2L17 9"/></svg></span></div>
      </div>
      <div class="sig-queue-item" data-sig-queue-item data-state="idle">
        <div class="q-ico">&#9636;</div>
        <div><div class="q-name">invoice-2026.pdf<small>340 KB</small></div><div class="sig-bar"><i class="sig-bar-fill"></i></div></div>
        <div class="q-end"><span class="sig-queue-pct" data-pct>0%</span><span class="sig-tick-wrap"><svg class="sig-tick" viewBox="0 0 24 24"><circle class="sig-tick-ring" cx="12" cy="12" r="10"/><path class="sig-tick-path" d="M7 12.5l3.2 3.2L17 9"/></svg></span></div>
      </div>
      <div class="sig-queue-item" data-sig-queue-item data-state="idle">
        <div class="q-ico">&#9635;</div>
        <div><div class="q-name">avatar-photo.jpg<small>860 KB</small></div><div class="sig-bar"><i class="sig-bar-fill"></i></div></div>
        <div class="q-end"><span class="sig-queue-pct" data-pct>0%</span><span class="sig-tick-wrap"><svg class="sig-tick" viewBox="0 0 24 24"><circle class="sig-tick-ring" cx="12" cy="12" r="10"/><path class="sig-tick-path" d="M7 12.5l3.2 3.2L17 9"/></svg></span></div>
      </div>
    </div>
    <div class="demo-actions">
      <button class="btn primary sig-press" id="q1-go">Simulate batch</button>
      <button class="btn ghost sig-press" id="q1-reset">Reset</button>
    </div>
  </section>

  <!-- 06 press, 07 focus, 08 submit, 09 field-error -->
  <section id="s06">
    <p class="eyebrow">Signals 06 to 09 · Forms</p>
    <h2>Press, focus, submit, <em>error.</em></h2>
    <p>Press is 120ms scale to 0.97. Focus is a ring, not an outline removal. Submit locks its width and moves through loading, done or error. A field error grows under that field only.</p>
    <p class="rule">Press: buttons only. Focus: text inputs only. Submit states: the form's submit button only. Field error: the one invalid field, never the whole form.</p>
    <form class="panel" id="f1" novalidate>
      <div class="grid2">
        <div class="sig-field" data-state="idle">
          <label for="name">Name</label>
          <input class="field sig-focus" id="name" name="name" placeholder="Your name">
          <div class="sig-field-msg"><div><small>Enter your name</small></div></div>
        </div>
        <div class="sig-field" data-state="idle">
          <label for="email">Email</label>
          <input class="field sig-focus" id="email" name="email" placeholder="you@example.com">
          <div class="sig-field-msg"><div><small>That email does not look right</small></div></div>
        </div>
      </div>
      <div style="margin-top:14px" class="sig-field" data-state="idle">
        <label for="msg">Message</label>
        <textarea class="field sig-focus" id="msg" name="msg" rows="3" placeholder="What do you need built?"></textarea>
        <div class="sig-field-msg"><div><small>Tell us a little about the job</small></div></div>
      </div>
      <div class="row" style="margin-top:16px">
        <button type="submit" class="btn primary sig-press sig-submit" data-state="idle">
          <span class="sig-submit-stack">
            <span class="sig-submit-label">Send enquiry</span>
            <span class="sig-spinner" aria-hidden="true"></span>
            <span class="sig-submit-done row" style="gap:6px"><svg class="sig-tick" viewBox="0 0 24 24" style="width:18px;height:18px"><path class="sig-tick-path" d="M7 12.5l3.2 3.2L17 9"/></svg> Sent</span>
            <span class="sig-submit-error">Try again</span>
          </span>
        </button>
        <label class="row small muted" style="margin:0;gap:6px"><input type="checkbox" id="f1-fail"> make the server fail</label>
      </div>
    </form>
  </section>

  <!-- 10 copy-swap, 11 state-glow, 12 tick-draw -->
  <section id="s10">
    <p class="eyebrow">Signals 10 to 12 · State</p>
    <h2>Copy swap, one glow, <em>the tick.</em></h2>
    <p>Copy crossfades when a label changes with state. The glow marks the single live element on the page, and moves rather than multiplies. The tick draws itself only on success.</p>
    <p class="rule">Glow: exactly one element at a time. Tick: success states only, never neutral or error.</p>
    <div class="grid2">
      <div class="step sig-glow is-active" data-step><b>1. Brief</b><span>Tell us the job</span></div>
      <div class="step sig-glow" data-step><b>2. Quote</b><span>Fixed price, no surprises</span></div>
      <div class="step sig-glow" data-step><b>3. Build</b><span>Preview link within the week</span></div>
      <div class="step sig-glow" data-step><b>4. Live</b><span>Pay on completion</span></div>
    </div>
    <div class="panel row" style="margin-top:14px">
      <div class="sig-swap" id="sw1" data-state="a">
        <span data-when="a" class="is-on">Saving your changes</span>
        <span data-when="b">Saved</span>
      </div>
      <span class="sig-tick" id="tk1" style="color:var(--sig-accent)"><svg class="sig-tick" viewBox="0 0 24 24"><circle class="sig-tick-ring" cx="12" cy="12" r="10"/><path class="sig-tick-path" d="M7 12.5l3.2 3.2L17 9"/></svg></span>
      <button class="btn sig-press" id="sw1-go" style="margin-left:auto">Save</button>
    </div>
  </section>

  <!-- 13 toast, 14 chips, 15 headline (top), 16 reveal, 17 card-lift, 18 nav (top) -->
  <section id="s13" data-sig-reveal>
    <p class="eyebrow">Signals 13 to 18 · Page</p>
    <h2>Toast, chips, reveal, <em>lift.</em></h2>
    <p>This section revealed on scroll because it is below the fold. The hero at the top did not, and would be refused. The headline emphasis ran once on load. The nav underline is at the top.</p>
    <p class="rule">Reveal: below-fold sections only. Chips: tag lists entering view, once. Card lift: cards that link somewhere, hover-gated. Toast: transient messages only.</p>
    <ul class="sig-chips">
      <li><b>1</b>Border</li><li><b>2</b>Glow</li><li><b>3</b>Copy</li><li><b>4</b>Percent</li><li><b>5</b>Time left</li><li><b>6</b>Retry</li><li><b>7</b>Preview</li><li><b>8</b>Own lane</li>
    </ul>
    <div class="grid2" style="margin-top:18px">
      <a class="card sig-card" href="#s01"><h4>Clickable card</h4><p>Lifts 2px on hover because it goes somewhere.</p></a>
      <div class="card"><h4>Static card</h4><p>Does not lift. It is not a link.</p></div>
    </div>
    <div class="demo-actions">
      <button class="btn sig-press" id="toast-ok">Toast</button>
      <button class="btn sig-press" id="toast-err">Error toast</button>
    </div>
  </section>

  <!-- 19 count-up, 20 skeleton -->
  <section id="s19" data-sig-reveal>
    <p class="eyebrow">Signals 19 and 20 · Numbers and loading</p>
    <h2>Count up, and <em>skeleton.</em></h2>
    <p>Stats count from zero once, when they enter view. Skeletons stand in for content that arrives after first paint, and never for content already in the HTML.</p>
    <p class="rule">Count-up: a stats section. Never: prices in a checkout, table cells. Skeleton: async content only.</p>
    <div class="panel stats">
      <div class="stat"><b data-sig-count="398">0</b><span>leads found</span></div>
      <div class="stat"><b data-sig-count="4.5" data-decimals="1" data-suffix="%">0</b><span>reply rate</span></div>
      <div class="stat"><b data-sig-count="20" data-prefix="A$" data-suffix="/yr">0</b><span>site care</span></div>
    </div>
    <div class="panel" id="sk1" style="margin-top:14px">
      <div class="sig-skeleton sk-line w40">&nbsp;</div>
      <div class="sig-skeleton sk-line w80">&nbsp;</div>
      <div class="sig-skeleton sk-line w60">&nbsp;</div>
    </div>
    <div class="demo-actions">
      <button class="btn sig-press" id="sk1-load">Load the reviews</button>
      <button class="btn ghost sig-press" id="sk1-reset">Reset</button>
    </div>
  </section>

  <section id="s21" data-sig-reveal>
    <p class="eyebrow">Signal 21 · Destructive actions</p>
    <h2>Hold to delete. <em>Release early, nothing dies.</em></h2>
    <p>The ring is the confirmation. The label is the verb. Red is spent here and nowhere else on the page, the control sits away from the primary spot, and it lives last, in a bordered danger zone.</p>
    <p class="rule">Belongs on: the one control that destroys something. Never: log out, cancel on a dialog, anything reversible.</p>
    <div class="panel">
      <section class="sig-danger-zone" style="margin-top:0">
        <h3>Danger zone</h3>
        <p style="margin:0">Deleting the project removes its 84 assets for everyone on the team.</p>
        <button class="sig-hold" type="button" id="hold1">
          <span class="sig-hold-ring" aria-hidden="true"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"><path d="M4 7h16M10 11v6M14 11v6M6 7l1 13h10l1-13M9 7V4h6v3"/></svg></span>
          <span class="sig-hold-label">Hold to delete project<small>hold 600 ms</small></span>
          <span class="sig-hold-pct">0%</span>
        </button>
      </section>
    </div>
    <div class="demo-actions">
      <span class="mono" id="hold1-out">nothing deleted</span>
      <button class="btn ghost sig-press" id="hold1-reset">Reset</button>
    </div>
  </section>

  <section id="s22" data-sig-reveal>
    <p class="eyebrow">Signal 22 · Empty states</p>
    <h2>Why it is empty, <em>what to do next.</em></h2>
    <p>The empty state is often the first thing a user sees. Say why it is empty, give one action, and for a first run add the checklist. Empty search suggests the fix. Inbox zero feels earned.</p>
    <p class="rule">Belongs on: any container that can have nothing in it. Never: as a loading state (that is skeleton).</p>
    <div class="panel">
      <div class="sig-empty" id="empty1">
        <svg class="sig-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="3" y="4" width="18" height="16" rx="3"/><path d="M8 12h8M12 8v8"/></svg>
        <h3>You have no projects yet</h3>
        <p>Projects hold your jobs, quotes and photos in one place.</p>
        <button class="btn sig-press sig-empty-cta" type="button" id="empty1-step">Create your first project</button>
        <div class="sig-empty-steps">
          <div class="sig-bar"><div class="sig-bar-fill"></div></div>
          <small data-steps-label>0 of 3 complete</small>
          <ol><li>Add your business details</li><li>Create a project</li><li>Invite a teammate</li></ol>
        </div>
      </div>
    </div>
    <div class="panel" style="margin-top:14px">
      <div class="sig-empty" data-kind="search">
        <h3>No results for "prple shoes"</h3>
        <p>Did you mean <a href="#s22">purple shoes</a>?</p>
      </div>
    </div>
    <div class="panel" style="margin-top:14px">
      <div class="sig-empty" data-kind="achievement">
        <svg class="sig-tick sig-icon is-on" viewBox="0 0 24 24"><circle class="sig-tick-ring" cx="12" cy="12" r="10"/><path class="sig-tick-path" d="M7 12.5l3.2 3.2L17 9"/></svg>
        <h3>Inbox zero</h3>
        <p>Nothing waiting on you.</p>
      </div>
    </div>
  </section>

  <section id="s23" data-sig-reveal>
    <p class="eyebrow">Signal 23 · Login and sign-up</p>
    <h2>One blade. <em>The forms trade places under the dark.</em></h2>
    <p>Login to sign-up without a single fade. One raked rectangle, one translateX. Sign-up is sign-in mirrored, so the band never rotates: it travels one way, covers the whole card halfway across, and that is where the two forms swap. Real forms underneath, transform and opacity only.</p>
    <p class="rule">Belongs on: the one card that holds both login and sign-up. Never: a lone login form, a settings page with tabs, anything that is not two states of one card.</p>
    <div class="sig-auth auth" data-sig-auth data-mode="signin" id="auth1">
      <div class="sig-auth-band" aria-hidden="true">
        <span class="sig-auth-seam" data-edge="lead"></span>
        <span class="sig-auth-seam" data-edge="trail"></span>
        <div class="sig-auth-band-inner">
          <div class="sig-auth-band-page">
            <div class="sig-auth-brand" data-side="right">
              <small>Verso</small>
              <h3>Welcome <em>back.</em></h3>
              <p>Your boards, your drafts and your people are exactly where you left them.</p>
            </div>
            <div class="sig-auth-brand" data-side="left">
              <small>Verso</small>
              <h3>Start the <em>first page.</em></h3>
              <p>One account for every board, every draft and every device you own.</p>
            </div>
          </div>
        </div>
      </div>
      <form class="sig-auth-pane" data-mode="signin" id="auth1-in" novalidate>
        <h3 class="auth-title">Sign in</h3>
        <div class="sig-field" data-state="idle">
          <label for="a1-user">Username or email</label>
          <input class="field sig-focus" id="a1-user" name="user" type="text" autocomplete="username">
          <div class="sig-field-msg"><div><small>Enter your username or email</small></div></div>
        </div>
        <div class="sig-field" data-state="idle">
          <label for="a1-pass">Password</label>
          <input class="field sig-focus" id="a1-pass" name="pass" type="password" autocomplete="current-password">
          <div class="sig-field-msg"><div><small>Enter your password</small></div></div>
        </div>
        <div class="auth-row">
          <label><input type="checkbox" name="keep" checked> Keep me signed in</label>
          <a href="#s23">Forgot password?</a>
        </div>
        <button type="submit" class="btn auth-cta sig-press sig-submit" data-state="idle">
          <span class="sig-submit-stack">
            <span class="sig-submit-label">Sign in</span>
            <span class="sig-spinner" aria-hidden="true"></span>
            <span class="sig-submit-done row" style="gap:6px"><svg class="sig-tick" viewBox="0 0 24 24" style="width:18px;height:18px"><path class="sig-tick-path" d="M7 12.5l3.2 3.2L17 9"/></svg> Signed in</span>
            <span class="sig-submit-error">Try again</span>
          </span>
        </button>
        <p class="auth-alt">New to Verso? <a href="#s23" data-sig-auth-to="signup">Create an account</a></p>
      </form>
      <form class="sig-auth-pane" data-mode="signup" id="auth1-up" novalidate>
        <h3 class="auth-title">Create account</h3>
        <div class="sig-field" data-state="idle">
          <label for="a1-name">Full name</label>
          <input class="field sig-focus" id="a1-name" name="name" type="text" autocomplete="name">
          <div class="sig-field-msg"><div><small>Enter your name</small></div></div>
        </div>
        <div class="sig-field" data-state="idle">
          <label for="a1-email">Email address</label>
          <input class="field sig-focus" id="a1-email" name="email" type="email" autocomplete="email">
          <div class="sig-field-msg"><div><small>That email does not look right</small></div></div>
        </div>
        <div class="sig-field" data-state="idle">
          <label for="a1-newpass">Password</label>
          <input class="field sig-focus" id="a1-newpass" name="newpass" type="password" autocomplete="new-password">
          <p class="auth-hint">Use 8 characters or more.</p>
          <div class="sig-field-msg"><div><small>Use 8 characters or more</small></div></div>
        </div>
        <button type="submit" class="btn auth-cta sig-press sig-submit" data-state="idle" style="margin-top:14px">
          <span class="sig-submit-stack">
            <span class="sig-submit-label">Create account</span>
            <span class="sig-spinner" aria-hidden="true"></span>
            <span class="sig-submit-done row" style="gap:6px"><svg class="sig-tick" viewBox="0 0 24 24" style="width:18px;height:18px"><path class="sig-tick-path" d="M7 12.5l3.2 3.2L17 9"/></svg> Account created</span>
            <span class="sig-submit-error">Try again</span>
          </span>
        </button>
        <p class="auth-alt">Already have an account? <a href="#s23" data-sig-auth-to="signin">Sign in</a></p>
      </form>
    </div>
    <div class="demo-actions">
      <button class="btn ghost sig-press" id="auth1-in-btn">Show sign in</button>
      <button class="btn ghost sig-press" id="auth1-up-btn">Show sign up</button>
      <span class="mono" id="auth1-out">mode: signin</span>
    </div>
  </section>

  <footer>
    Sources: @designmotionhq reels "FutureVille · realization" (five upload signals) and "Destructive actions" (signal 21); Katherine Gilligan, Building With Good UX part 8 (signal 22); Code &amp; Chill, Verso component 91, login and sign-up (signal 23). The rest follow the same rule: one signal, one kind of element. Values from the <span class="mono">animate</span> skill.
  </footer>
</main>

<script src="signals.js"></script>
<script>
  Signals.init();
  // 21 danger-confirm
  var holdOut = document.getElementById('hold1-out');
  var hold1 = Signals.hold(document.getElementById('hold1'), function () { holdOut.textContent = 'project deleted (callback fired once)'; });
  document.getElementById('hold1-reset').addEventListener('click', function () { hold1.reset(); holdOut.textContent = 'nothing deleted'; });

  // 22 empty-state
  var e1 = document.getElementById('empty1'), e1n = 0;
  document.getElementById('empty1-step').addEventListener('click', function () { e1n = Math.min(3, e1n + 1); e1._sig.steps(e1n); });


  // 23 auth-sweep: both forms validate for real; the links inside the card drive the swap
  var auth1 = document.getElementById('auth1');
  var authOut = document.getElementById('auth1-out');
  auth1._sig = Signals.auth(auth1, { onChange: function (m) { authOut.textContent = 'mode: ' + m; } });
  document.getElementById('auth1-in-btn').addEventListener('click', function () { auth1._sig.go('signin'); });
  document.getElementById('auth1-up-btn').addEventListener('click', function () { auth1._sig.go('signup'); });
  [['auth1-in', ['user', 'pass'], 'Signed in'], ['auth1-up', ['name', 'email', 'newpass'], 'Account created']].forEach(function (spec) {
    var form = document.getElementById(spec[0]);
    var fs = Array.prototype.map.call(form.querySelectorAll('.sig-field'), Signals.field);
    Signals.submit(form, function (data) {
      var bad = false;
      spec[1].forEach(function (key, i) {
        var v = data.get(key) || '';
        if (!v || (key === 'email' && !/.+@.+\..+/.test(v)) || (key === 'newpass' && v.length < 8)) { fs[i].error(); bad = true; }
      });
      if (bad) return Promise.reject();
      return new Promise(function (res) { setTimeout(res, 900); }).then(function () { Signals.toast(spec[2]); });
    });
  });

  // 01 dropzone: demo buttons force the state so it can be screenshotted without a real drag.
  var dz = document.getElementById('dz');
  dz._sig = dz._sig || Signals.dropzone(dz, { onFiles: function (files) { Signals.toast(files.length + ' file' + (files.length === 1 ? '' : 's') + ' dropped'); } });
  dz.addEventListener('click', function (e) { if (e.target.tagName !== 'INPUT') dz.querySelector('input').click(); });
  document.querySelectorAll('[data-dz]').forEach(function (b) {
    b.addEventListener('click', function () {
      var s = b.getAttribute('data-dz');
      if (s === 'over') { dz.querySelector('[data-sig-dropcopy]').textContent = 'hero-banner.png, 2.4 MB'; Signals.glow(dz); }
      else dz.classList.remove('is-active');
      Signals.setState(dz, s);
    });
  });

  // 02 + 03 progress and retry
  var up1 = document.getElementById('up1');
  var sim1;
  var ctrl1 = Signals.progress(up1, {
    onResume: function (from) { sim1 = Signals.simulateUpload(ctrl1, { sizeMB: 48, rateMBps: 2.4, from: from, speed: 0.15 }); },
    onDone: function () { Signals.toast('demo-recording.mp4 uploaded'); }
  });
  function resetUp1() { if (sim1) sim1.cancel(); up1.setAttribute('data-state', 'idle'); up1.style.setProperty('--p', '0'); up1.querySelector('[data-pct]').textContent = '0%'; up1.querySelector('[data-status]').textContent = 'Waiting'; up1.querySelectorAll('.sig-progress-meta > span').forEach(function (s) { s.classList.remove('is-on'); }); }
  document.getElementById('up1-go').addEventListener('click', function () { resetUp1(); sim1 = Signals.simulateUpload(ctrl1, { sizeMB: 48, rateMBps: 2.4, speed: 0.15 }); });
  document.getElementById('up1-fail').addEventListener('click', function () { resetUp1(); sim1 = Signals.simulateUpload(ctrl1, { sizeMB: 48, rateMBps: 2.4, speed: 0.15, failAt: 90 }); });
  document.getElementById('up1-reset').addEventListener('click', resetUp1);

  // 04 preview
  var pv1 = document.getElementById('pv1'), pvApi;
  document.getElementById('pv1-show').addEventListener('click', function () { pvApi = Signals.preview(pv1); });
  document.getElementById('pv1-hide').addEventListener('click', function () { if (pvApi) pvApi.hide(); });

  // 05 queue
  var q1 = document.getElementById('q1');
  var lanes = Signals.queue(q1);
  var qSims = [];
  function resetQ1() {
    qSims.forEach(function (s) { s.cancel(); }); qSims = [];
    q1.querySelectorAll('[data-sig-queue-item]').forEach(function (r) { r.setAttribute('data-state', 'idle'); r.style.setProperty('--p', '0'); r.querySelector('[data-pct]').textContent = '0%'; });
    lanes = Signals.queue(q1);
  }
  document.getElementById('q1-go').addEventListener('click', function () {
    resetQ1();
    var sizes = [1.2, 2.4, 48, 0.34, 0.86];
    lanes.forEach(function (lane, i) {
      var ctrl = { set: function (p) { lane.set(p); }, fail: function () { lane.fail(); }, done: function () { lane.done(); } };
      qSims.push(Signals.simulateUpload(ctrl, { sizeMB: sizes[i], rateMBps: 0.9 + Math.random() * 0.4, speed: sizes[i] > 10 ? 0.15 : 1.2 }));
    });
  });
  document.getElementById('q1-reset').addEventListener('click', resetQ1);

  // 06 to 09 form
  var f1 = document.getElementById('f1');
  var fields = Array.prototype.map.call(f1.querySelectorAll('.sig-field'), Signals.field);
  Signals.submit(f1, function (data) {
    var bad = false;
    if (!data.get('name')) { fields[0].error(); bad = true; }
    if (!/.+@.+\..+/.test(data.get('email') || '')) { fields[1].error(); bad = true; }
    if (!data.get('msg')) { fields[2].error(); bad = true; }
    if (bad) return Promise.reject();
    var fail = document.getElementById('f1-fail').checked;
    return new Promise(function (res, rej) { setTimeout(fail ? rej : res, 1400); }).then(function () { Signals.toast('Enquiry sent'); }, function () { Signals.toast('Server did not answer', { kind: 'error' }); throw new Error(); });
  });

  // 10 to 12 glow, swap, tick
  document.querySelectorAll('[data-step]').forEach(function (s) { s.addEventListener('click', function () { Signals.glow(s); }); });
  var sw1 = document.getElementById('sw1'), tk1 = document.getElementById('tk1');
  document.getElementById('sw1-go').addEventListener('click', function () {
    tk1.classList.remove('is-on'); Signals.setState(sw1, 'a');
    setTimeout(function () { Signals.setState(sw1, 'b'); tk1.classList.add('is-on'); }, 900);
  });

  // 13 toast
  document.getElementById('toast-ok').addEventListener('click', function () { Signals.toast('Changes saved'); });
  document.getElementById('toast-err').addEventListener('click', function () { Signals.toast('Could not save, try again', { kind: 'error' }); });

  // 20 skeleton
  var sk1 = document.getElementById('sk1');
  var skHtml = sk1.innerHTML;
  document.getElementById('sk1-load').addEventListener('click', function () {
    setTimeout(function () {
      sk1.innerHTML = '<p style="margin:0 0 6px"><b>Review text arrives here once the request returns.</b></p><p class="muted small" style="margin:0">Reviewer name, business</p>';
    }, 1200);
  });
  document.getElementById('sk1-reset').addEventListener('click', function () { sk1.innerHTML = skHtml; });
</script>
</body>
</html>

```

## File: assets/signals.css

```css
/* ui-signals: feedback and state animations for hand-written sites.
   Dependency free. Link after the project's own stylesheet.
   Set the first three tokens from the project's palette before shipping.
   Every signal is opt-in by class or data attribute; nothing here is global
   except the tokens. Recipes and placement rules: references/catalogue.md */

:root {
  --sig-accent: #2dd4bf;   /* set from the project palette, never leave the demo teal on a client site */
  --sig-danger: #f0506e;
  --sig-surface: #15181d;

  --sig-ease-out: cubic-bezier(0.23, 1, 0.32, 1);
  --sig-ease-in-out: cubic-bezier(0.77, 0, 0.175, 1);
  --sig-dur-press: 120ms;
  --sig-dur-state: 150ms;
  --sig-dur-enter: 220ms;
  --sig-dur-reveal: 400ms;
  --sig-dur-count: 2000ms;  /* numbers must load much slower than the rest, never under 2s */
  --sig-dur-hold: 600ms;    /* danger-confirm hold. The source reel used 300ms; on a phone that is a slow tap, so 600 */
  --sig-dur-sweep: 640ms;   /* auth-sweep travel. The two forms trade places at half, under the band */

  /* auth-sweep card. Set the five colours from the site's palette; the demo shows the reel's green and gold */
  --sig-auth-rake: 12deg;
  --sig-auth-band: 1.5;     /* band width as a multiple of the card. Wider than 1 so it covers the whole card for the middle fifth of the travel */
  --sig-auth-card: #f4efe6;
  --sig-auth-ink: #16201b;
  --sig-auth-band-bg: var(--sig-surface);
  --sig-auth-band-ink: #f4efe6;
  --sig-auth-seam: var(--sig-accent);

  --sig-accent-soft: color-mix(in srgb, var(--sig-accent) 22%, transparent);
  --sig-accent-faint: color-mix(in srgb, var(--sig-accent) 10%, transparent);
  --sig-danger-soft: color-mix(in srgb, var(--sig-danger) 18%, transparent);
  --sig-line: color-mix(in srgb, currentColor 14%, transparent);
  --sig-line-strong: color-mix(in srgb, currentColor 26%, transparent);
}

/* ------------------------------------------------------------------ */
/* copy-swap: one container, several [data-when] children, JS marks one
   .is-on. Old copy fades down, new copy rises in. Used by the dropzone. */

.sig-swap { display: grid; }
.sig-swap > [data-when] {
  grid-area: 1 / 1;
  opacity: 0;
  transform: translateY(4px);
  pointer-events: none;
  transition: opacity var(--sig-dur-state) var(--sig-ease-out),
              transform var(--sig-dur-state) var(--sig-ease-out);
}
.sig-swap > [data-when].is-on {
  opacity: 1;
  transform: none;
  pointer-events: auto;
}

/* ------------------------------------------------------------------ */
/* state-glow: a soft radial behind the one element that is live.
   JS (Signals.glow) removes it from everything else first. */

.sig-glow { position: relative; isolation: isolate; }
.sig-glow::before {
  content: "";
  position: absolute;
  inset: 0;
  z-index: -1;
  border-radius: inherit;
  background: radial-gradient(60% 70% at 50% 50%, var(--sig-accent-soft), transparent 72%);
  opacity: 0;
  transition: opacity var(--sig-dur-state) var(--sig-ease-out);
  pointer-events: none;
}
.sig-glow.is-active::before,
.sig-glow[data-state="over"]::before,
.sig-glow[data-state="active"]::before { opacity: 1; }

/* ------------------------------------------------------------------ */
/* drop-answer: the dropzone answers a drag with border, glow and copy. */

.sig-dropzone {
  position: relative;
  border: 1.5px dashed var(--sig-line-strong);
  border-radius: 14px;
  background: var(--sig-surface);
  overflow: hidden;
  transition: border-color var(--sig-dur-state) var(--sig-ease-out);
}
.sig-dropzone[data-state="over"] { border-color: var(--sig-accent); }
.sig-dropzone .sig-icon {
  display: inline-block;
  transition: transform var(--sig-dur-state) var(--sig-ease-out), color var(--sig-dur-state) var(--sig-ease-out);
}
.sig-dropzone[data-state="over"] .sig-icon { transform: translateY(-3px); color: var(--sig-accent); }

/* ------------------------------------------------------------------ */
/* honest-progress: percent, time left, rate. Bar moves with scaleX on a
   linear curve. Never animate width. */

.sig-progress { --p: 0; }
.sig-progress-pct {
  font-variant-numeric: tabular-nums;
  color: var(--sig-accent);
  transition: color var(--sig-dur-state) var(--sig-ease-out);
}
.sig-bar {
  height: 4px;
  border-radius: 99px;
  background: var(--sig-line);
  overflow: hidden;
}
.sig-bar-fill {
  display: block;
  height: 100%;
  border-radius: inherit;
  background: var(--sig-accent);
  transform-origin: left center;
  transform: scaleX(var(--p));
  transition: transform 200ms linear, background-color var(--sig-dur-state) var(--sig-ease-out);
}
.sig-progress-meta {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
  font-variant-numeric: tabular-nums;
}
.sig-progress-meta > span {
  opacity: 0;
  transform: translateY(4px);
  transition: opacity var(--sig-dur-enter) var(--sig-ease-out), transform var(--sig-dur-enter) var(--sig-ease-out);
}
.sig-progress-meta > span.is-on { opacity: 1; transform: none; }
.sig-progress[data-state="error"] .sig-progress-pct { color: var(--sig-danger); }
.sig-progress[data-state="error"] .sig-bar-fill { background: var(--sig-danger); }
.sig-progress[data-state="done"] .sig-bar-fill { background: var(--sig-accent); }

/* ------------------------------------------------------------------ */
/* inline-retry: the error row grows in under the bar. The file stays.
   Height via grid-template-rows is the one sanctioned height animation. */

.sig-retry-row {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows var(--sig-dur-enter) var(--sig-ease-out);
}
.sig-retry-row > * {
  min-height: 0;
  overflow: hidden;
  opacity: 0;
  transform: translateY(-4px);
  transition: opacity var(--sig-dur-enter) var(--sig-ease-out), transform var(--sig-dur-enter) var(--sig-ease-out);
}
.sig-progress[data-state="error"] .sig-retry-row { grid-template-rows: 1fr; }
.sig-progress[data-state="error"] .sig-retry-row > * { opacity: 1; transform: none; }
.sig-retry-card {
  border: 1px solid var(--sig-danger-soft);
  background: color-mix(in srgb, var(--sig-danger) 8%, transparent);
  border-radius: 10px;
}

/* ------------------------------------------------------------------ */
/* upload-preview: the card that replaces "filename uploaded". */

.sig-preview {
  opacity: 0;
  transform: translateY(8px);
  transition: opacity var(--sig-dur-enter) var(--sig-ease-out), transform var(--sig-dur-enter) var(--sig-ease-out);
}
.sig-preview.is-in { opacity: 1; transform: none; }
.sig-preview .sig-thumb {
  background: linear-gradient(135deg, var(--sig-accent-soft), var(--sig-accent-faint));
  border-radius: 10px;
}
.sig-chip {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  padding: 2px 8px;
  border-radius: 6px;
  border: 1px solid var(--sig-accent-soft);
  color: var(--sig-accent);
  font-size: 0.75em;
  letter-spacing: 0.02em;
}

/* ------------------------------------------------------------------ */
/* own-lane-queue: each row owns its bar. The counter is plain text. */

.sig-queue-item { --p: 0; }
.sig-queue-item .sig-queue-pct {
  font-variant-numeric: tabular-nums;
  transition: opacity var(--sig-dur-state) var(--sig-ease-out);
}
.sig-queue-item[data-state="done"] .sig-queue-pct { opacity: 0; }
.sig-queue-item .sig-tick-wrap {
  opacity: 0;
  transition: opacity var(--sig-dur-state) var(--sig-ease-out);
}
.sig-queue-item[data-state="done"] .sig-tick-wrap { opacity: 1; }
.sig-queue-item[data-state="error"] .sig-bar-fill { background: var(--sig-danger); }
.sig-queue-item[data-state="error"] .sig-queue-pct { color: var(--sig-danger); }

/* ------------------------------------------------------------------ */
/* press: buttons and button-styled links only. */

.sig-press {
  transition: transform var(--sig-dur-press) var(--sig-ease-out),
              background-color var(--sig-dur-state) var(--sig-ease-out),
              border-color var(--sig-dur-state) var(--sig-ease-out);
}
.sig-press:active { transform: scale(0.97); }

/* ------------------------------------------------------------------ */
/* focus-glow: text inputs only. The ring replaces the outline, it does
   not remove focus visibility. */

.sig-focus {
  transition: border-color var(--sig-dur-state) var(--sig-ease-out),
              box-shadow var(--sig-dur-state) var(--sig-ease-out);
}
.sig-focus:focus-visible {
  outline: none;
  border-color: var(--sig-accent);
  box-shadow: 0 0 0 4px var(--sig-accent-soft);
}

/* ------------------------------------------------------------------ */
/* submit-states: idle, loading, done, error. Width is locked by JS so
   the button never jumps. Label, spinner and tick share one grid cell. */

.sig-submit { position: relative; }
.sig-submit .sig-submit-stack { display: grid; place-items: center; }
.sig-submit .sig-submit-stack > * {
  grid-area: 1 / 1;
  transition: opacity var(--sig-dur-state) var(--sig-ease-out), transform var(--sig-dur-state) var(--sig-ease-out);
}
.sig-submit .sig-submit-label { opacity: 1; }
.sig-submit .sig-spinner,
.sig-submit .sig-submit-done,
.sig-submit .sig-submit-error { opacity: 0; transform: translateY(4px); }
.sig-submit[data-state="loading"] .sig-submit-label,
.sig-submit[data-state="done"] .sig-submit-label,
.sig-submit[data-state="error"] .sig-submit-label { opacity: 0; transform: translateY(-4px); }
.sig-submit[data-state="loading"] .sig-spinner,
.sig-submit[data-state="done"] .sig-submit-done,
.sig-submit[data-state="error"] .sig-submit-error { opacity: 1; transform: none; }
.sig-submit[data-state="error"] {
  animation: sig-shake 300ms var(--sig-ease-out);
  background: var(--sig-danger);
  border-color: var(--sig-danger);
}
.sig-submit[data-state="loading"] { cursor: progress; }
@keyframes sig-shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-3px); }
  75% { transform: translateX(3px); }
}

/* spinner: a ring. Constant motion, linear. */
.sig-spinner {
  width: 1.1em;
  height: 1.1em;
  border-radius: 50%;
  border: 2px solid color-mix(in srgb, currentColor 30%, transparent);
  border-top-color: currentColor;
  animation: sig-spin 700ms linear infinite;
}
@keyframes sig-spin { to { transform: rotate(360deg); } }

/* ------------------------------------------------------------------ */
/* field-error: the message grows under the field. Only that field. */

.sig-field .sig-field-msg {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows var(--sig-dur-enter) var(--sig-ease-out);
}
.sig-field .sig-field-msg > * {
  min-height: 0;
  overflow: hidden;
  color: var(--sig-danger);
  opacity: 0;
  transform: translateY(-4px);
  transition: opacity var(--sig-dur-enter) var(--sig-ease-out), transform var(--sig-dur-enter) var(--sig-ease-out);
}
.sig-field[data-state="error"] .sig-field-msg { grid-template-rows: 1fr; }
.sig-field[data-state="error"] .sig-field-msg > * { opacity: 1; transform: none; }
.sig-field[data-state="error"] .sig-focus { border-color: var(--sig-danger); }
.sig-field[data-state="error"] .sig-focus:focus-visible { box-shadow: 0 0 0 4px var(--sig-danger-soft); }

/* ------------------------------------------------------------------ */
/* tick-draw: a checkmark that draws itself on success. */

.sig-tick { overflow: visible; }
.sig-tick .sig-tick-path {
  fill: none;
  stroke: currentColor;
  stroke-width: 2;
  stroke-linecap: round;
  stroke-linejoin: round;
  stroke-dasharray: 24;
  stroke-dashoffset: 24;
  transition: stroke-dashoffset 300ms var(--sig-ease-out) 60ms;
}
.sig-tick .sig-tick-ring {
  fill: none;
  stroke: currentColor;
  stroke-width: 1.5;
  opacity: 0.6;
  transform: scale(0.9);
  transform-origin: center;
  transition: transform var(--sig-dur-enter) var(--sig-ease-out);
}
.sig-tick.is-on .sig-tick-path,
.is-in .sig-tick .sig-tick-path,
.is-on .sig-tick .sig-tick-path,
[data-state="done"] .sig-tick .sig-tick-path { stroke-dashoffset: 0; }
.sig-tick.is-on .sig-tick-ring,
.is-in .sig-tick .sig-tick-ring,
.is-on .sig-tick .sig-tick-ring,
[data-state="done"] .sig-tick .sig-tick-ring { transform: none; }

/* ------------------------------------------------------------------ */
/* toast: enters from the bottom, leaves the same way. Transition, not
   keyframes, so a second toast retargets instead of restarting. */

.sig-toast {
  position: fixed;
  left: 50%;
  bottom: 24px;
  transform: translate(-50%, 16px);
  opacity: 0;
  pointer-events: none;
  transition: transform var(--sig-dur-enter) var(--sig-ease-out), opacity var(--sig-dur-enter) var(--sig-ease-out);
  z-index: 50;
}
.sig-toast.is-on { transform: translate(-50%, 0); opacity: 1; pointer-events: auto; }
.sig-toast[data-kind="error"] { border-color: var(--sig-danger-soft); }

/* ------------------------------------------------------------------ */
/* chip-stagger: tags arrive in sequence, once, when the list enters view.
   JS sets --i on each child. */

.sig-chips > * {
  opacity: 0;
  transform: translateY(6px);
  transition: opacity var(--sig-dur-enter) var(--sig-ease-out), transform var(--sig-dur-enter) var(--sig-ease-out);
  transition-delay: calc(var(--i, 0) * 40ms);
}
.sig-chips.is-in > * { opacity: 1; transform: none; }

/* ------------------------------------------------------------------ */
/* headline-emphasis: the hero H1 settles, then one phrase takes the
   accent. Once per load. Wrap the phrase in <em>. */

[data-sig-headline] {
  opacity: 0;
  transform: translateY(8px);
  transition: opacity var(--sig-dur-reveal) var(--sig-ease-out), transform var(--sig-dur-reveal) var(--sig-ease-out);
}
[data-sig-headline] em {
  font-style: normal;
  color: inherit;
  transition: color var(--sig-dur-reveal) var(--sig-ease-out) 350ms;
}
[data-sig-headline].is-in { opacity: 1; transform: none; }
[data-sig-headline].is-in em { color: var(--sig-accent); }

/* ------------------------------------------------------------------ */
/* section-reveal: below-fold sections only. JS refuses above-fold ones. */

[data-sig-reveal] {
  opacity: 0;
  transform: translateY(12px);
  transition: opacity var(--sig-dur-reveal) var(--sig-ease-out), transform var(--sig-dur-reveal) var(--sig-ease-out);
}
[data-sig-reveal].is-in { opacity: 1; transform: none; }

/* ------------------------------------------------------------------ */
/* card-lift: clickable cards, hover gated. */

@media (hover: hover) and (pointer: fine) {
  .sig-card {
    transition: transform var(--sig-dur-state) var(--sig-ease-out),
                box-shadow var(--sig-dur-state) var(--sig-ease-out),
                border-color var(--sig-dur-state) var(--sig-ease-out);
  }
  .sig-card:hover {
    transform: translateY(-2px);
    border-color: var(--sig-line-strong);
    box-shadow: 0 10px 30px -12px color-mix(in srgb, #000 60%, transparent);
  }
}

/* ------------------------------------------------------------------ */
/* nav-underline: grows from the left on hover, stays on the active link. */

.sig-nav a { position: relative; text-decoration: none; }
.sig-nav a::after {
  content: "";
  position: absolute;
  left: 0;
  right: 0;
  bottom: -4px;
  height: 1.5px;
  background: currentColor;
  transform: scaleX(0);
  transform-origin: left center;
  transition: transform 180ms var(--sig-ease-out);
}
.sig-nav a[aria-current="page"]::after { transform: scaleX(1); }
@media (hover: hover) and (pointer: fine) {
  .sig-nav a:hover::after { transform: scaleX(1); }
}

/* ------------------------------------------------------------------ */
/* count-up: stats only. JS animates the number; CSS keeps it steady. */

[data-sig-count] { font-variant-numeric: tabular-nums; }

/* ------------------------------------------------------------------ */
/* skeleton: for content that arrives after first paint. */

.sig-skeleton {
  position: relative;
  overflow: hidden;
  border-radius: 8px;
  background: var(--sig-line);
  color: transparent;
}
.sig-skeleton::after {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg, transparent, color-mix(in srgb, currentColor 10%, transparent), transparent);
  transform: translateX(-100%);
  animation: sig-shimmer 1.4s linear infinite;
}
@keyframes sig-shimmer { to { transform: translateX(100%); } }

/* ------------------------------------------------------------------ */
/* danger-confirm: hold to delete. The ring is the confirmation; release
   early and nothing fires. JS drives --hold from 0 to 1 over
   --sig-dur-hold. Red lives here and in .sig-danger-zone and nowhere else. */

@property --hold { syntax: "<number>"; inherits: true; initial-value: 0; }
.sig-hold {
  --hold: 0;
  --hold-dur: 0ms;
  position: relative;
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 10px 16px 10px 12px;
  border: 1px solid var(--sig-danger-soft);
  border-radius: 12px;
  background: transparent;
  color: inherit;
  font: inherit;
  cursor: pointer;
  user-select: none;
  -webkit-user-select: none;
  touch-action: none;
  transition: --hold var(--hold-dur) linear,
              border-color var(--sig-dur-state) var(--sig-ease-out),
              background-color var(--sig-dur-state) var(--sig-ease-out);
}
.sig-hold-ring {
  position: relative;
  width: 28px;
  height: 28px;
  border-radius: 50%;
  background: conic-gradient(var(--sig-danger) calc(var(--hold) * 360deg), var(--sig-danger-soft) 0);
  display: grid;
  place-items: center;
}
.sig-hold-ring::after {
  content: "";
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: var(--sig-surface);
}
.sig-hold-ring > svg {
  position: absolute;
  width: 12px;
  height: 12px;
  color: var(--sig-danger);
}
.sig-hold-label { display: grid; line-height: 1.2; }
.sig-hold-label small { opacity: 0.6; font-size: 0.8em; }
.sig-hold-pct { margin-left: auto; opacity: 0.5; font-variant-numeric: tabular-nums; font-size: 0.85em; }
.sig-hold[data-state="holding"] { border-color: var(--sig-danger); background: var(--sig-danger-soft); }
.sig-hold[data-state="done"] { border-color: var(--sig-danger); background: var(--sig-danger); color: #fff; }
.sig-hold[data-state="done"] .sig-hold-ring::after { background: var(--sig-danger); }
.sig-hold[data-state="done"] .sig-hold-ring > svg { color: #fff; }
.sig-hold:focus-visible { outline: 2px solid var(--sig-danger); outline-offset: 2px; }

/* danger zone: bordered, labelled, last on the page. Geography is friction. */
.sig-danger-zone {
  border: 1px solid var(--sig-danger-soft);
  border-radius: 14px;
  padding: 16px;
  margin-top: 48px;
}
.sig-danger-zone > h2,
.sig-danger-zone > h3 {
  margin: 0 0 4px;
  font-size: 0.8em;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--sig-danger);
}
.sig-danger-zone .sig-hold { margin-top: 12px; }

/* ------------------------------------------------------------------ */
/* empty-state: why it is empty, what to do next, never looks broken.
   One rise on entry. Copy does the work; the motion just says "on purpose". */

.sig-empty {
  display: grid;
  justify-items: center;
  text-align: center;
  gap: 8px;
  padding: 40px 24px;
  border: 1px dashed var(--sig-line);
  border-radius: 14px;
  opacity: 0;
  transform: translateY(6px);
  transition: opacity var(--sig-dur-enter) var(--sig-ease-out),
              transform var(--sig-dur-enter) var(--sig-ease-out);
}
.sig-empty.is-on { opacity: 1; transform: none; }
.sig-empty > .sig-icon { width: 36px; height: 36px; opacity: 0.55; }
.sig-empty > h3 { margin: 4px 0 0; font-size: 1.05em; }
.sig-empty > p { margin: 0; opacity: 0.7; max-width: 36ch; }
.sig-empty > .sig-empty-cta { margin-top: 8px; }
.sig-empty[data-kind="achievement"] { border-style: solid; border-color: var(--sig-accent-soft); background: var(--sig-accent-faint); }
.sig-empty[data-kind="achievement"] > .sig-icon { opacity: 1; color: var(--sig-accent); }
.sig-empty[data-kind="search"] > p > a { color: var(--sig-accent); text-decoration: underline; text-underline-offset: 2px; }

/* first-run checklist: the gamified steps. Fill moves with scaleX. */
.sig-empty-steps { --p: 0; width: 100%; max-width: 360px; margin-top: 12px; text-align: left; }
.sig-empty-steps > .sig-bar { margin-bottom: 10px; }
.sig-empty-steps > ol { margin: 0; padding: 0 0 0 20px; display: grid; gap: 4px; }
.sig-empty-steps > ol > li[data-done] { opacity: 0.5; text-decoration: line-through; }

/* ------------------------------------------------------------------ */
/* auth-sweep: login and sign-up on one card, swapped by one raked band.
   The band is a window (overflow hidden) over a fixed brand layer: its
   inner counter-transform puts that layer back in card co-ordinates, so
   the copy under it never skews. The band is wider than the card, so at
   mid-travel it covers all of it, and that is where the two forms trade
   places. Transform and opacity only, no fade between states.
   From Code & Chill, Verso component 91 (references/source-reel.md). */

.sig-auth {
  --sweep: 0%;
  position: relative;
  overflow: hidden;
  isolation: isolate;
  min-height: 440px;
  border-radius: 18px;
  background: var(--sig-auth-card);
  color: var(--sig-auth-ink);
}
.sig-auth[data-mode="signup"] { --sweep: -100%; }

.sig-auth-band {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 50%;
  width: calc(var(--sig-auth-band) * 100%);
  overflow: hidden;
  z-index: 2;
  background: var(--sig-auth-band-bg);
  color: var(--sig-auth-band-ink);
  transform: skewX(var(--sig-auth-rake)) translateX(var(--sweep));
  transition: transform var(--sig-dur-sweep) var(--sig-ease-in-out);
  will-change: transform;
}
.sig-auth-band-inner {
  position: absolute;
  inset: 0;
  transform: translateX(calc(-1 * var(--sweep))) skewX(calc(-1 * var(--sig-auth-rake)));
  transition: transform var(--sig-dur-sweep) var(--sig-ease-in-out);
}
.sig-auth-band-page {
  position: absolute;
  top: 0;
  bottom: 0;
  width: calc(100% / var(--sig-auth-band));
  left: calc(-50% / var(--sig-auth-band));
}
.sig-auth-seam {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 2px;
  z-index: 1;
  background: linear-gradient(180deg, transparent 0%, var(--sig-auth-seam) 18%, var(--sig-auth-seam) 82%, transparent 100%);
  box-shadow: 0 0 14px color-mix(in srgb, var(--sig-auth-seam) 55%, transparent);
}
.sig-auth-seam[data-edge="lead"] { left: 0; }
.sig-auth-seam[data-edge="trail"] { right: 0; }

.sig-auth-brand {
  position: absolute;
  top: 50%;
  width: 38%;
  padding: 0 6%;
  transform: translateY(-50%);
}
.sig-auth-brand[data-side="right"] { right: 0; }
.sig-auth-brand[data-side="left"] { left: 0; }
.sig-auth-brand > small { display: block; margin: 0 0 14px; font-size: 11px; letter-spacing: 0.22em; text-transform: uppercase; color: var(--sig-auth-seam); }
.sig-auth-brand > h3 { margin: 0 0 12px; font: 400 34px/1.1 Georgia, "Times New Roman", serif; letter-spacing: -0.01em; }
.sig-auth-brand > h3 em { font-style: italic; color: var(--sig-auth-seam); }
.sig-auth-brand > p { margin: 0; font-size: 14px; line-height: 1.5; opacity: 0.8; }

.sig-auth-pane {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 50%;
  z-index: 1;
  display: grid;
  align-content: center;
  padding: 36px 8%;
  margin: 0;
  opacity: 0;              /* no transition on purpose: the swap happens under the band */
  visibility: hidden;
}
.sig-auth-pane.is-on { opacity: 1; visibility: visible; }
.sig-auth-pane[data-mode="signin"] { left: 0; }
.sig-auth-pane[data-mode="signup"] { right: 0; }

/* one column: the band starts fully off the right edge (the rake leans in by
   half the card height times tan(rake), so it parks at 120%), wipes the whole
   card and leaves off the left. The brand copy steps aside. */
@media (max-width: 640px) {
  .sig-auth { --sig-auth-band: 2; --sig-auth-rake: 8deg; min-height: 0; }
  .sig-auth-band { left: 120%; }
  .sig-auth[data-mode="signup"] { --sweep: -170%; } /* band width 2, so the covered window is again the middle fifth */
  .sig-auth-band-page { width: calc(100% / var(--sig-auth-band)); left: calc(-120% / var(--sig-auth-band)); }
  .sig-auth-brand { display: none; }
  .sig-auth-pane { position: relative; width: 100%; padding: 28px 22px; }
  .sig-auth-pane:not(.is-on) { display: none; }
}

/* ------------------------------------------------------------------ */
/* reduced motion: keep colour and opacity, drop movement. Not zero. */

@media (prefers-reduced-motion: reduce) {
  .sig-swap > [data-when],
  .sig-progress-meta > span,
  .sig-retry-row > *,
  .sig-field .sig-field-msg > *,
  .sig-preview,
  .sig-chips > *,
  [data-sig-headline],
  [data-sig-reveal],
  .sig-toast,
  .sig-empty,
  .sig-submit .sig-submit-stack > * { transform: none !important; transition-duration: 1ms !important; transition-delay: 0ms !important; }
  .sig-press:active,
  .sig-card:hover,
  .sig-dropzone[data-state="over"] .sig-icon { transform: none; }
  .sig-submit[data-state="error"] { animation: none; }
  .sig-skeleton::after { animation: none; }
  .sig-bar-fill { transition-duration: 1ms; }
  .sig-tick .sig-tick-path { transition-duration: 1ms; stroke-dashoffset: 0; }
  .sig-tick .sig-tick-ring { transform: none; }
  .sig-hold { transition-duration: 1ms; } /* ring jumps to full at confirm; the timer still takes the full hold */
  .sig-auth-band, .sig-auth-band-inner { transition-duration: 1ms; } /* the band jumps; the transform itself must stay, it is the layout */
}

```

## File: assets/signals.js

```js
/* ui-signals: the JS half. Dependency free, no build step.
   Load after signals.css. Call Signals.init() once, or wire pieces by hand.
   Every helper only touches the element it is given; nothing is global. */

(function () {
  'use strict';

  var reduce = window.matchMedia && window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  function $(sel, root) { return (root || document).querySelector(sel); }
  function $$(sel, root) { return Array.prototype.slice.call((root || document).querySelectorAll(sel)); }
  function nextFrame(fn) { requestAnimationFrame(function () { requestAnimationFrame(fn); }); }
  function fmtBytes(b) {
    if (b == null) return '';
    if (b >= 1048576) return (b / 1048576).toFixed(1) + ' MB';
    if (b >= 1024) return Math.round(b / 1024) + ' KB';
    return b + ' B';
  }
  function fmtSecs(s) {
    if (s == null || !isFinite(s)) return '';
    if (s < 1) return 'under 1s left';
    if (s < 60) return Math.ceil(s) + 's left';
    return Math.ceil(s / 60) + 'm left';
  }

  /* copy-swap: mark the [data-when] child matching `state` inside `el`. */
  function setState(el, state) {
    el.setAttribute('data-state', state);
    $$('.sig-swap', el).concat(el.classList.contains('sig-swap') ? [el] : []).forEach(function (swap) {
      $$(':scope > [data-when]', swap).forEach(function (child) {
        child.classList.toggle('is-on', child.getAttribute('data-when') === state);
      });
    });
  }

  /* state-glow: only one live element at a time, page wide. */
  function glow(el) {
    $$('.sig-glow.is-active').forEach(function (other) { if (other !== el) other.classList.remove('is-active'); });
    if (el) el.classList.add('is-active');
  }

  /* drop-answer: the dropzone answers a drag. onFiles(files) on drop or pick. */
  function dropzone(el, opts) {
    opts = opts || {};
    var input = $('input[type="file"]', el);
    var depth = 0;
    var overCopy = $('[data-when="over"] [data-sig-dropcopy]', el);
    setState(el, 'idle');

    function describe(dt) {
      if (!dt || !dt.items || !dt.items.length) return '';
      var n = dt.items.length;
      var type = dt.items[0].type || '';
      var kind = type.indexOf('image/') === 0 ? 'image' : type === 'application/pdf' ? 'PDF' : type.indexOf('video/') === 0 ? 'video' : 'file';
      return n === 1 ? '1 ' + kind : n + ' files';
    }

    el.addEventListener('dragenter', function (e) {
      e.preventDefault();
      depth++;
      if (overCopy) overCopy.textContent = describe(e.dataTransfer);
      setState(el, 'over');
      if (el.classList.contains('sig-glow')) glow(el);
    });
    el.addEventListener('dragover', function (e) { e.preventDefault(); });
    el.addEventListener('dragleave', function () {
      depth = Math.max(0, depth - 1);
      if (depth === 0) { setState(el, 'idle'); el.classList.remove('is-active'); }
    });
    el.addEventListener('drop', function (e) {
      e.preventDefault();
      depth = 0;
      setState(el, 'idle');
      el.classList.remove('is-active');
      if (opts.onFiles) opts.onFiles(Array.prototype.slice.call(e.dataTransfer.files));
    });
    if (input) {
      input.addEventListener('change', function () {
        if (opts.onFiles) opts.onFiles(Array.prototype.slice.call(input.files));
      });
    }
    return { setState: function (s) { setState(el, s); } };
  }

  /* honest-progress + inline-retry: a controller for one .sig-progress. */
  function progress(el, opts) {
    opts = opts || {};
    var pct = $('[data-pct]', el);
    var eta = $('[data-eta]', el);
    var rate = $('[data-rate]', el);
    var status = $('[data-status]', el);
    var retry = $('[data-retry]', el);
    var last = 0;

    function set(p, meta) {
      meta = meta || {};
      last = Math.max(0, Math.min(100, p));
      el.style.setProperty('--p', (last / 100).toString());
      if (pct) pct.textContent = Math.round(last) + '%';
      if (eta) { eta.textContent = fmtSecs(meta.secondsLeft); eta.classList.toggle('is-on', !!eta.textContent); }
      if (rate) { rate.textContent = meta.rate || ''; rate.classList.toggle('is-on', !!meta.rate); }
      if (status && meta.status != null) status.textContent = meta.status;
      if (el.getAttribute('data-state') !== 'uploading') el.setAttribute('data-state', 'uploading');
    }
    function fail(message) {
      el.setAttribute('data-state', 'error');
      if (status) status.textContent = 'Paused at ' + Math.round(last) + '%, file kept in memory';
      var msg = $('[data-error-msg]', el);
      if (msg && message) msg.textContent = message;
      if (eta) eta.classList.remove('is-on');
      if (rate) rate.classList.remove('is-on');
    }
    function resume() {
      el.setAttribute('data-state', 'uploading');
      if (status) status.textContent = 'Resuming from ' + Math.round(last) + '%';
      if (opts.onResume) opts.onResume(last);
    }
    function done() {
      set(100, { status: 'Uploaded' });
      el.setAttribute('data-state', 'done');
      if (eta) eta.classList.remove('is-on');
      if (rate) rate.classList.remove('is-on');
      if (opts.onDone) opts.onDone();
    }
    if (retry) retry.addEventListener('click', resume);
    return { set: set, fail: fail, resume: resume, done: done, value: function () { return last; } };
  }

  /* upload-preview: reveal a .sig-preview card after success. */
  function preview(card, file) {
    if (file) {
      var name = $('[data-name]', card); if (name) name.textContent = file.name;
      var size = $('[data-size]', card); if (size) size.textContent = fmtBytes(file.size);
      var type = $('[data-type]', card); if (type) type.textContent = (file.name.split('.').pop() || '').toUpperCase();
      var img = $('img[data-thumb]', card);
      if (img && file.type && file.type.indexOf('image/') === 0 && window.URL) img.src = URL.createObjectURL(file);
    }
    card.hidden = false;
    nextFrame(function () { card.classList.add('is-in'); });
    return {
      hide: function () { card.classList.remove('is-in'); setTimeout(function () { card.hidden = true; }, 240); }
    };
  }

  /* own-lane-queue: one controller per row, plus a done counter. */
  function queue(list) {
    var rows = $$('[data-sig-queue-item]', list);
    var counter = $('[data-done]', list);
    var doneCount = 0;
    function refresh() { if (counter) counter.textContent = doneCount + ' of ' + rows.length + ' done'; }
    refresh();
    return rows.map(function (row) {
      var pct = $('[data-pct]', row);
      return {
        el: row,
        set: function (p) {
          p = Math.max(0, Math.min(100, p));
          row.style.setProperty('--p', (p / 100).toString());
          if (pct) pct.textContent = Math.round(p) + '%';
          if (row.getAttribute('data-state') !== 'uploading') row.setAttribute('data-state', 'uploading');
        },
        done: function () {
          row.style.setProperty('--p', '1');
          if (row.getAttribute('data-state') !== 'done') { doneCount++; row.setAttribute('data-state', 'done'); refresh(); }
        },
        fail: function () { row.setAttribute('data-state', 'error'); }
      };
    });
  }

  /* submit-states: form submit button, width locked so it never jumps.
     handler(formData) returns a promise; resolve = done, reject = error. */
  function submit(form, handler, opts) {
    opts = opts || {};
    var btn = $('.sig-submit', form) || $('[type="submit"]', form);
    if (!btn) return;
    form.addEventListener('submit', function (e) {
      e.preventDefault();
      if (btn.getAttribute('data-state') === 'loading') return;
      btn.style.minWidth = btn.offsetWidth + 'px';
      btn.setAttribute('data-state', 'loading');
      btn.disabled = true;
      Promise.resolve(handler(new FormData(form))).then(function () {
        btn.setAttribute('data-state', 'done');
        setTimeout(function () { btn.setAttribute('data-state', 'idle'); btn.disabled = false; }, opts.doneMs || 1800);
      }, function () {
        btn.setAttribute('data-state', 'error');
        setTimeout(function () { btn.setAttribute('data-state', 'idle'); btn.disabled = false; }, opts.errorMs || 1600);
      });
    });
  }

  /* field-error: one field, one message. */
  function field(wrap) {
    var msg = $('.sig-field-msg > *', wrap);
    var input = $('input, textarea, select', wrap);
    var api = {
      error: function (text) { if (msg && text) msg.textContent = text; wrap.setAttribute('data-state', 'error'); },
      clear: function () { wrap.setAttribute('data-state', 'idle'); }
    };
    if (input) input.addEventListener('input', api.clear);
    return api;
  }

  /* toast: one element reused, so a second call retargets the transition. */
  var toastEl, toastTimer;
  function toast(text, opts) {
    opts = opts || {};
    if (!toastEl) {
      toastEl = document.createElement('div');
      toastEl.className = 'sig-toast' + (opts.className ? ' ' + opts.className : '');
      toastEl.setAttribute('role', 'status');
      document.body.appendChild(toastEl);
    }
    toastEl.textContent = text;
    toastEl.setAttribute('data-kind', opts.kind || 'info');
    nextFrame(function () { toastEl.classList.add('is-on'); });
    clearTimeout(toastTimer);
    toastTimer = setTimeout(function () { toastEl.classList.remove('is-on'); }, opts.ms || 2800);
  }

  /* section-reveal: below-fold only. Above-fold elements are shown at once
     and a warning is logged, because that placement is a mistake. */
  function reveal(root) {
    var els = $$('[data-sig-reveal]', root);
    if (!els.length) return;
    var vh = window.innerHeight;
    var io = new IntersectionObserver(function (entries) {
      entries.forEach(function (en) {
        if (en.isIntersecting) { en.target.classList.add('is-in'); io.unobserve(en.target); }
      });
    }, { threshold: 0.15 });
    els.forEach(function (el) {
      if (el.getBoundingClientRect().top < vh * 0.9) {
        el.style.transition = 'none';
        el.classList.add('is-in');
        console.warn('ui-signals: [data-sig-reveal] is on an above-the-fold element, revealed instantly. Remove the attribute from it.', el);
        return;
      }
      io.observe(el);
    });
  }

  /* chip-stagger: number the children, reveal once in view. */
  function chips(root) {
    var lists = $$('.sig-chips', root);
    if (!lists.length) return;
    var io = new IntersectionObserver(function (entries) {
      entries.forEach(function (en) {
        if (en.isIntersecting) { en.target.classList.add('is-in'); io.unobserve(en.target); }
      });
    }, { threshold: 0.2 });
    lists.forEach(function (list) {
      $$(':scope > *', list).forEach(function (c, i) { c.style.setProperty('--i', i); });
      io.observe(list);
    });
  }

  /* headline-emphasis: once, on load. */
  function headline(root) {
    $$('[data-sig-headline]', root).forEach(function (h) { nextFrame(function () { h.classList.add('is-in'); }); });
  }

  /* count-up: stats only. Reduced motion jumps straight to the value. */
  function countUp(root) {
    var els = $$('[data-sig-count]', root);
    if (!els.length) return;
    function run(el) {
      var target = parseFloat(el.getAttribute('data-sig-count'));
      var prefix = el.getAttribute('data-prefix') || '';
      var suffix = el.getAttribute('data-suffix') || '';
      var decimals = (el.getAttribute('data-decimals') | 0);
      // Slow on purpose. Fast count-ups read as fake; 2s is the floor, the
      // token in signals.css or data-duration on the element can go longer.
      var tokenDur = parseFloat(getComputedStyle(document.documentElement).getPropertyValue('--sig-dur-count')) || 2000;
      var dur = Math.max(2000, parseFloat(el.getAttribute('data-duration')) || tokenDur);
      function paint(v) { el.textContent = prefix + v.toLocaleString(undefined, { minimumFractionDigits: decimals, maximumFractionDigits: decimals }) + suffix; }
      if (reduce) { paint(target); return; }
      var start = performance.now();
      function tick(now) {
        var t = Math.min(1, (now - start) / dur);
        var eased = 1 - Math.pow(1 - t, 3);
        paint(target * eased);
        if (t < 1) requestAnimationFrame(tick);
      }
      requestAnimationFrame(tick);
    }
    var io = new IntersectionObserver(function (entries) {
      entries.forEach(function (en) { if (en.isIntersecting) { run(en.target); io.unobserve(en.target); } });
    }, { threshold: 0.5 });
    els.forEach(function (el) { io.observe(el); });
  }

  /* Demo and mockup helper only: fakes an upload against a progress
     controller. Never ship this on a real form. */
  function simulateUpload(ctrl, opts) {
    opts = opts || {};
    var sizeMB = opts.sizeMB || 48;
    var rateMB = opts.rateMBps || 6;
    var failAt = opts.failAt;
    var from = opts.from || 0;
    var totalMs = Math.max((sizeMB / rateMB) * 1000 * (opts.speed || 0.25) * ((100 - from) / 100), from > 0 ? 1500 : 0);
    var start = performance.now();
    var cancelled = false;
    function step(now) {
      if (cancelled) return;
      var p = from + ((now - start) / totalMs) * (100 - from);
      if (failAt != null && p >= failAt) { ctrl.set(failAt, { secondsLeft: 0 }); ctrl.fail(opts.failMessage || 'Connection lost'); return; }
      if (p >= 100) { ctrl.done(); return; }
      var left = ((100 - p) / 100) * (sizeMB / rateMB);
      ctrl.set(p, { secondsLeft: left, rate: rateMB.toFixed(1) + ' MB/s', status: from > 0 ? undefined : 'Uploading' });
      requestAnimationFrame(step);
    }
    requestAnimationFrame(step);
    return { cancel: function () { cancelled = true; } };
  }

  // danger-confirm: hold to confirm. A timer owns the decision and a CSS
  // transition on --hold owns the ring, so neither depends on rAF being
  // delivered. Release early and the ring drains back with nothing fired.
  // Space or Enter held works too.
  function hold(el, onConfirm, opts) {
    opts = opts || {};
    var dur = opts.duration || parseFloat(getComputedStyle(el).getPropertyValue('--sig-dur-hold')) || 600;
    var pct = $('.sig-hold-pct', el);
    var timer = null, ticker = null, start = 0, done = false, keyHeld = false;
    function paintPct(p) { if (pct) pct.textContent = Math.round(p * 100) + '%'; }
    function begin(e) {
      if (done || el.disabled || el.dataset.state === 'holding') return;
      if (e && e.type === 'pointerdown' && e.button !== 0) return;
      start = Date.now();
      setState(el, 'holding');
      el.style.setProperty('--hold-dur', dur + 'ms');
      el.style.setProperty('--hold', '1');
      clearInterval(ticker);
      ticker = setInterval(function () { paintPct(Math.min(1, (Date.now() - start) / dur)); }, 40);
      clearTimeout(timer);
      timer = setTimeout(finish, dur);
    }
    function cancel() {
      if (done || el.dataset.state !== 'holding') return;
      clearTimeout(timer); clearInterval(ticker);
      setState(el, 'cancelled');
      el.style.setProperty('--hold-dur', '150ms');
      el.style.setProperty('--hold', '0');
      paintPct(0);
      setTimeout(function () { if (el.dataset.state === 'cancelled') setState(el, 'idle'); }, 200);
    }
    function finish() {
      done = true;
      clearInterval(ticker);
      el.style.setProperty('--hold', '1');
      paintPct(1);
      setState(el, 'done');
      if (onConfirm) onConfirm(el);
    }
    el.addEventListener('pointerdown', begin);
    el.addEventListener('pointerup', cancel);
    el.addEventListener('pointerleave', cancel);
    el.addEventListener('pointercancel', cancel);
    el.addEventListener('keydown', function (e) {
      if ((e.key === ' ' || e.key === 'Enter') && !keyHeld) { keyHeld = true; e.preventDefault(); begin(); }
    });
    el.addEventListener('keyup', function (e) {
      if (e.key === ' ' || e.key === 'Enter') { keyHeld = false; cancel(); }
    });
    el.addEventListener('blur', function () { keyHeld = false; cancel(); });
    el.addEventListener('click', function (e) { e.preventDefault(); }); // a tap never confirms
    el.style.setProperty('--hold', '0');
    paintPct(0);
    setState(el, 'idle');
    return { reset: function () { done = false; clearTimeout(timer); clearInterval(ticker); el.style.setProperty('--hold-dur', '0ms'); el.style.setProperty('--hold', '0'); paintPct(0); setState(el, 'idle'); } };
  }

  // empty-state: mark the container on so it rises once; tick the first-run
  // checklist by calling steps(n).
  function empty(el) {
    nextFrame(function () { el.classList.add('is-on'); });
    var steps = $('.sig-empty-steps', el);
    function setSteps(n) {
      if (!steps) return;
      var items = $$('ol > li', steps);
      items.forEach(function (li, i) { if (i < n) li.setAttribute('data-done', ''); else li.removeAttribute('data-done'); });
      var fill = $('.sig-bar-fill', steps);
      if (fill) fill.style.transform = 'scaleX(' + (items.length ? n / items.length : 0) + ')';
      var label = $('[data-steps-label]', steps);
      if (label) label.textContent = n + ' of ' + items.length + ' complete';
    }
    return { steps: setSteps };
  }

  // auth-sweep: one raked band sweeps across the card. At half its travel
  // it covers the whole card, and that is when the two forms trade places,
  // so the swap is never in view. The hidden form is inert.
  function auth(el, opts) {
    opts = opts || {};
    function sweepMs() {
      if (opts.duration) return opts.duration;
      var v = (getComputedStyle(el).getPropertyValue('--sig-dur-sweep') || '').trim();
      var n = parseFloat(v);
      if (!n) return 640;
      return /ms$/.test(v) ? n : n * 1000;
    }
    var panes = {};
    $$('.sig-auth-pane', el).forEach(function (p) { panes[p.getAttribute('data-mode')] = p; });
    var mode = el.getAttribute('data-mode') || 'signin';
    var timer = null;
    function show(next, focus) {
      Object.keys(panes).forEach(function (k) {
        var on = k === next;
        panes[k].classList.toggle('is-on', on);
        panes[k].inert = !on;
        if (on) panes[k].removeAttribute('aria-hidden'); else panes[k].setAttribute('aria-hidden', 'true');
      });
      if (focus) { var first = $('input, select, textarea, button', panes[next]); if (first) first.focus({ preventScroll: true }); }
    }
    function go(next) {
      if (!panes[next] || next === mode) return;
      mode = next;
      el.setAttribute('data-mode', next);
      clearTimeout(timer);
      timer = setTimeout(function () { show(next, true); if (opts.onChange) opts.onChange(next); }, reduce ? 0 : sweepMs() / 2);
    }
    $$('[data-sig-auth-to]', el).forEach(function (a) {
      a.addEventListener('click', function (e) { e.preventDefault(); go(a.getAttribute('data-sig-auth-to')); });
    });
    el.setAttribute('data-mode', mode);
    show(mode, false);
    return { go: go, mode: function () { return mode; } };
  }

  function init(root) {
    root = root || document;
    reveal(root);
    chips(root);
    headline(root);
    countUp(root);
    $$('[data-sig-dropzone]', root).forEach(function (el) { if (!el._sig) el._sig = dropzone(el); });
    $$('.sig-empty', root).forEach(function (el) { if (!el._sig) el._sig = empty(el); });
    $$('[data-sig-auth]', root).forEach(function (el) { if (!el._sig) el._sig = auth(el); });
  }

  window.Signals = {
    init: init,
    setState: setState,
    glow: glow,
    dropzone: dropzone,
    progress: progress,
    preview: preview,
    queue: queue,
    submit: submit,
    field: field,
    toast: toast,
    reveal: reveal,
    chips: chips,
    headline: headline,
    countUp: countUp,
    hold: hold,
    empty: empty,
    auth: auth,
    simulateUpload: simulateUpload,
    fmtBytes: fmtBytes
  };
})();

```

## File: references/catalogue.md

# The catalogue

Twenty signals. Each entry: what it belongs on, what must never carry it, the
HTML it expects, and the values. The CSS and JS are in `../assets/`; the
snippets here show the shape, not the full rules. Read the entry for every
signal you ship. `demo.html` has a live example of each.

Contents: 01 drop-answer · 02 honest-progress · 03 inline-retry ·
04 upload-preview · 05 own-lane-queue · 06 press · 07 focus-glow ·
08 submit-states · 09 field-error · 10 copy-swap · 11 state-glow ·
12 tick-draw · 13 toast · 14 chip-stagger · 15 headline-emphasis ·
16 section-reveal · 17 card-lift · 18 nav-underline · 19 count-up ·
20 skeleton

The first five are the reel. The rest are the same rule applied to the parts
of a page the reel did not cover.

---

## 01 drop-answer

**The reel:** "Nothing reacts. It has to answer back. Three signals, before
the drop: border, glow, copy." A file dragged over a dead dropzone makes users
hesitate.

**Belongs on:** a file dropzone. **Never on:** anything else. A hero with a
glow behind it is not this signal, it is decoration.

**What moves, on dragenter:** the dashed border goes from muted to accent
(150ms), a radial accent glow fades up behind the content (150ms), the icon
swaps and rises 3px, the copy crossfades from "Drop your file" to "Release to
upload" plus what is being dragged. All four reverse on dragleave. Drop
returns to idle and hands the files on.

```html
<div class="sig-dropzone sig-glow" data-sig-dropzone>
  <div class="sig-swap">
    <div data-when="idle"> <svg class="sig-icon">…</svg> <h3>Drop your file</h3> <p>PNG, JPG, PDF, up to 50 MB</p> </div>
    <div data-when="over"> <svg class="sig-icon">…</svg> <h3>Release to upload</h3> <p data-sig-dropcopy></p> </div>
  </div>
  <input type="file" hidden>
</div>
```
```js
Signals.dropzone(el, { onFiles: function (files) { /* start the upload */ } });
```

Browsers do not expose filenames during dragover, only count and type, so
the over copy says "1 image" or "3 files". The reel's filename chip is a mock.

---

## 02 honest-progress

**The reel:** "A spinner hides the truth." Mystery: a ring with a question
mark and "Uploading…". Honest: 62%, 7s left, 2.4 MB/s. "Let them decide,
wait or walk away."

**Belongs on:** an operation over about one second whose length is known
(bytes, rows, steps). **Never on:** an operation of unknown length. A fake
percentage that stalls at 95% is worse than a spinner; use `skeleton` there.

**What moves:** the fill scales on X with `transition: transform 200ms
linear` (progress is constant motion, so linear). The percent is large,
tabular-nums, accent. Time left and rate arrive as small chips once they are
known (220ms rise), and leave when they are not.

```html
<div class="sig-progress" data-state="idle">
  <span class="sig-progress-pct" data-pct>0%</span>
  <div class="sig-bar"><i class="sig-bar-fill"></i></div>
  <div data-status></div>
  <div class="sig-progress-meta"><span data-eta></span><span data-rate></span></div>
</div>
```
```js
var ctrl = Signals.progress(el);
xhr.upload.onprogress = function (e) {
  ctrl.set(e.loaded / e.total * 100, { secondsLeft: est, rate: mbps + ' MB/s', status: 'Uploading' });
};
ctrl.done();
```

---

## 03 inline-retry

**The reel:** "It dies at ninety percent." The bar turns red where it
stopped, the status reads "Paused at 90%, file kept in memory", an error row
grows in under it with a Retry button. One tap resumes from 90%. "Never make
them start over."

**Belongs on:** any network action that can fail: upload, form submit,
payment. **Never on:** actions that cannot fail, and never as a full-page
error state that throws away what the user had.

**What moves:** on `fail()`, the fill and percent go danger colour (150ms),
the error row expands (grid-template-rows 0fr to 1fr, 220ms, the one
sanctioned height animation) with its content rising in. On `resume()` the
row collapses, colours return, and the bar carries on from its last value.
The element the user typed into or dropped is never removed.

```html
<div class="sig-retry-row">
  <div>
    <div class="sig-retry-card"> <b>Upload failed</b> <span data-error-msg>Connection lost</span> <button data-retry>Retry</button> </div>
  </div>
</div>
```
`data-retry` is wired by `Signals.progress`; pass `onResume(fromPercent)` to
restart the transfer with a range header or your own resume logic.

Only promise a resume the server can keep. A plain form post to a static
host cannot pick up at 90%, so "Resuming from 90%" there is a lie. In that
case the honest version is `submit-states` going to "Try again" with
everything the user typed and attached still in place, and no percentage.

---

## 04 upload-preview

**The reel:** "A filename is not feedback." Text only: "IMG_4032.jpg
uploaded". Preview: a card with the thumbnail, a JPG chip, 2.4 MB, a drawn
tick and "Uploaded just now", then Replace and Remove. "Thumbnail · Type ·
Size · Proof."

**Belongs on:** the moment after any file upload succeeds. **Never as:** a
line of text, and never before the upload has actually finished.

**What moves:** the card enters with opacity 0 to 1 and translateY 8px to 0
(220ms ease-out). The tick inside draws itself (see 12). Nothing else
animates; the content is the feedback.

```html
<div class="sig-preview" hidden>
  <div class="sig-thumb"><img data-thumb alt=""></div>
  <b data-name></b> <span class="sig-chip" data-type></span> <span data-size></span>
  <svg class="sig-tick">…</svg> Uploaded just now
  <button>Replace</button> <button>Remove</button>
</div>
```
```js
var pv = Signals.preview(card, file);  // fills name, size, type, image thumb
pv.hide();
```

On a form where the file only goes up when the form is sent (most contact
forms), show the card at attach time instead, because a bare filename is
still worse. Then the proof line says "Attached and ready to send", not
"Uploaded", and the tick waits for the real success.

---

## 05 own-lane-queue

**The reel:** "Every file, its own lane." Five rows, each with its own bar
and percent, a "4 of 5 done" counter, ticks appearing per row as each
finishes while the 48 MB video is still at 82%. "One failure never blocks the
others."

**Belongs on:** a batch of long operations. **Never on:** a single
operation, and never as one combined bar for the batch.

**What moves:** each row is its own `honest-progress` at 3px. On a row's
`done()` its percent fades out and its tick fades in and draws. The counter
is plain text that updates. A failed row goes danger and stays put; the
others carry on.

```html
<div class="sig-queue"> <span data-done></span>
  <div class="sig-queue-item" data-sig-queue-item> … <div class="sig-bar"><i class="sig-bar-fill"></i></div> <span data-pct></span> <span class="sig-tick-wrap"><svg class="sig-tick">…</svg></span> </div>
</div>
```
```js
var lanes = Signals.queue(list); lanes[i].set(40); lanes[i].done(); lanes[j].fail();
```

---

## 06 press

**Belongs on:** buttons, and links styled as buttons. **Never on:** nav
links, links in text, cards.

**What moves:** `transform: scale(0.97)` on `:active`, 120ms ease-out in
and out. Background and border colour changes share the 150ms state
duration. It is felt, not seen.

```html
<button class="sig-press">Send</button>
```

---

## 07 focus-glow

**Belongs on:** text inputs, textareas, selects. **Never on:** buttons,
cards, links.

**What moves:** on `:focus-visible` the border goes accent and a 4px soft
ring fades in (150ms). The default outline is replaced by the ring, not
removed; the ring has contrast. An errored field's ring is danger.

```html
<input class="sig-focus">
```

---

## 08 submit-states

**Belongs on:** the form's submit button. **Never on:** any other button.
One form, one button that goes through states.

**What moves:** on submit the button's width is locked to its current
width so it never jumps; the label fades up and out and a ring spinner fades
in (150ms). Success: spinner out, tick draws plus "Sent", holds 1.8s, then
idle. Error: one 300ms shake, background goes danger with "Try again", holds
1.6s, then idle. The shake is keyframed because it fires rarely and must
run to completion.

```html
<button type="submit" class="sig-submit" data-state="idle">
  <span class="sig-submit-stack">
    <span class="sig-submit-label">Send enquiry</span>
    <span class="sig-spinner"></span>
    <span class="sig-submit-done"><svg class="sig-tick">…</svg> Sent</span>
    <span class="sig-submit-error">Try again</span>
  </span>
</button>
```
```js
Signals.submit(form, function (formData) { return fetch(url, { method: 'POST', body: formData }).then(function (r) { if (!r.ok) throw r; }); });
```

---

## 09 field-error

**Belongs on:** the one field that failed validation. **Never on:** the
whole form. The reel's rule about never making them start over applies:
the user's other answers stay untouched.

**What moves:** the message grows under the field (grid rows, 220ms) with
its text rising in, the field border goes danger. Typing clears it.

```html
<div class="sig-field" data-state="idle">
  <input class="sig-focus">
  <div class="sig-field-msg"><div>That email does not look right</div></div>
</div>
```
```js
var f = Signals.field(wrap); f.error('That email does not look right'); f.clear();
```

---

## 10 copy-swap

**Belongs on:** any label whose text changes with state: a dropzone, a
button, a status line. **Never on:** static copy.

**What moves:** all variants sit in one grid cell; the outgoing one fades
and drops 4px, the incoming rises from 4px (150ms ease-out). No layout
shift because the cell holds the tallest variant.

```html
<div class="sig-swap" data-state="a"> <span data-when="a" class="is-on">Saving</span> <span data-when="b">Saved</span> </div>
```
```js
Signals.setState(el, 'b');
```

---

## 11 state-glow

**Belongs on:** exactly one live element at a time: the active step, the
dropzone being dragged over, the card being edited. **Never on:** more
than one element at once, and never as decoration on a hero or a section.

**What moves:** a radial accent gradient behind the element fades in at
150ms. `Signals.glow(el)` removes it from every other `.sig-glow` first,
which is how the rule is enforced.

```html
<div class="sig-glow">…</div>
```
```js
Signals.glow(el);
```

---

## 12 tick-draw

**Belongs on:** success states: upload done, form sent, step complete.
**Never on:** neutral or error states, and never as a static icon that
happens to animate on load.

**What moves:** the check path draws with stroke-dashoffset 24 to 0 over
300ms ease-out after a 60ms pause; the ring around it settles from scale
0.9 to 1 at 220ms. Triggered by `.is-on` on the svg or an ancestor, or
`data-state="done"` on an ancestor.

```html
<svg class="sig-tick" viewBox="0 0 24 24"><circle class="sig-tick-ring" cx="12" cy="12" r="10"/><path class="sig-tick-path" d="M7 12.5l3.2 3.2L17 9"/></svg>
```

---

## 13 toast

**Belongs on:** transient confirmations: saved, sent, copied. **Never
on:** anything the user needs to act on or read later; that is inline
content, not a toast.

**What moves:** enters from the bottom, translateY 16px to 0 with opacity
(220ms ease-out), and leaves by the same path. Transitions, not keyframes,
so a second toast retargets the one element instead of restarting. Auto
hides at 2.8s.

```js
Signals.toast('Changes saved'); Signals.toast('Could not save', { kind: 'error' });
```
For a stacked or dismissable toast use the Preline overlay or a daisyUI
toast rather than extending this one.

---

## 14 chip-stagger

**Belongs on:** a list of tags, features, benefits or badges as it enters
view. **Never on:** lists the user is reading through (a menu, a table, a
FAQ), and never a second time.

**What moves:** each child rises from 6px with opacity, 220ms ease-out,
40ms apart. Runs once when the list is 20% in view.

```html
<ul class="sig-chips"><li>Border</li><li>Glow</li><li>Copy</li></ul>
```
`Signals.init()` wires every `.sig-chips`.

---

## 15 headline-emphasis

**The reel's title cards:** "Nothing reacts." with "reacts" in accent;
"It has to answer back." with "answer back" in accent. The phrase tints
after the line has settled.

**Belongs on:** the hero H1, one phrase, once per load. **Never on:** any
other heading, and never on more than one phrase.

**What moves:** the H1 rises 8px with opacity over 400ms; the `<em>` phrase
tints from inherit to accent over 400ms starting at 350ms, so the sentence
reads first and the emphasis lands second.

```html
<h1 data-sig-headline>An interface has to <em>answer back.</em></h1>
```

---

## 16 section-reveal

**Belongs on:** sections below the fold. **Never on:** the hero, the first
section, or anything visible at load. `Signals.reveal()` refuses above-fold
elements: it shows them instantly and logs a warning naming the element, so
the mistake is visible in the console rather than as a blank hero.

**What moves:** opacity 0 to 1 and translateY 12px to 0 over 400ms
ease-out, once, at 15% visibility. This is the marketing tier, so 400ms is
allowed; nothing the user is waiting on should take that long.

```html
<section data-sig-reveal>…</section>
```

---

## 17 card-lift

**Belongs on:** cards that are themselves links. **Never on:** static
cards, table rows, list items. If it does not go anywhere, it does not lift.

**What moves:** translateY(-2px), a stronger border and a soft shadow on
hover, 150ms ease-out, inside `@media (hover: hover) and (pointer: fine)`
so touch devices never see a stuck hover.

```html
<a class="card sig-card" href="/service">…</a>
```

---

## 18 nav-underline

**Belongs on:** the primary nav's links. **Never on:** footer links, links
in body text.

**What moves:** an underline scales from the left, 180ms ease-out, on
hover (gated) and stays on `aria-current="page"`.

```html
<nav class="sig-nav"><a href="/" aria-current="page">Home</a><a href="/work">Work</a></nav>
```

---

## 19 count-up

**Belongs on:** a stats section: leads found, years trading, jobs done.
**Never on:** prices in a checkout, totals, table cells, anything the user
is about to act on.

**What moves:** the number eases from 0 to its value over 2 seconds (cubic
ease-out, so the last digits settle slowly), once, at 50% visibility, with
tabular-nums so the width holds. Reduced motion jumps straight to the value.

**The rule: numbers load much slower than everything else.**
2s is the floor, not the default. `signals.js` clamps anything shorter back
up to 2s; make it longer with `--sig-dur-count` in `signals.css` or
`data-duration="3000"` on the element. A count-up that flicks to its value
in under a second reads as fake.

```html
<b data-sig-count="398">0</b> <b data-sig-count="4.5" data-decimals="1" data-suffix="%">0</b> <b data-sig-count="20" data-prefix="A$" data-suffix="/yr" data-duration="3000">0</b>
```

---

## 20 skeleton

**Belongs on:** content that arrives after first paint: reviews from an
API, a feed, a map. **Never on:** content that is already in the HTML, and
never as a substitute for `honest-progress` when the length is known.

**What moves:** a linear shimmer sweeps left to right every 1.4s. Reduced
motion shows a static block. Replace the skeleton with the real content in
place; do not animate the swap.

```html
<div class="sig-skeleton">&nbsp;</div>
```

---

## 21 danger-confirm

**Belongs on:** the one control that destroys something: delete project,
remove member, cancel subscription, close account. **Never on:** anything
reversible, log out, "cancel" on a dialog, or as a novelty on a normal
button. Red (`--sig-danger`) is a budget: spend it on this and on
`.sig-danger-zone`, nowhere else on the page, or delete starts to look
routine.

**What moves:** on pointerdown a conic ring fills over `--sig-dur-hold`
(600ms; the reel used 300ms, which on a phone is a slow tap). Release early
and the ring drains back and nothing fires. Held to the end, the button
fills red and the callback runs once. A click never confirms. Space or
Enter held does the same for keyboard users.

**The six rules from the reel, all of which are placement, not motion:**

1. Hold to confirm; the ring replaces the "Are you sure?" dialog.
2. The label is the verb: "Delete project" and "Keep project", never
   "Yes" and "No". Nobody reads the sentence above the buttons.
3. Off the happy path: never where the primary button sits. Muscle memory
   clicks primary spots blind.
4. Red budget: red on destruction only.
5. Danger zone: bordered, labelled, last on the page. Geography is friction.
6. Cooldown for the big ones: "Account scheduled for deletion, 14 days to
   cancel" with a Cancel deletion button. Time is the last line of defence.
   That one is a copy and server pattern, not CSS.

```html
<section class="sig-danger-zone">
  <h3>Danger zone</h3>
  <p>Deleting the project removes its 84 assets for everyone on the team.</p>
  <button class="sig-hold" type="button" id="del">
    <span class="sig-hold-ring" aria-hidden="true"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M4 7h16M10 11v6M14 11v6M6 7l1 13h10l1-13M9 7V4h6v3"/></svg></span>
    <span class="sig-hold-label">Hold to delete project<small>hold 600 ms</small></span>
    <span class="sig-hold-pct">0%</span>
  </button>
</section>
```
```js
Signals.hold(document.getElementById('del'), function (btn) { /* fire the delete */ });
```

---

## 22 empty-state

**Belongs on:** any container that can have nothing in it: a dashboard on
first run, a list with no items, a search with no results, an inbox at zero.
**Never on:** containers that always have content, and never as a loading
state (that is `skeleton`).

**What moves:** almost nothing, on purpose. The block rises 6px with a fade
once when it appears, so it reads as designed rather than broken. The
first-run checklist bar moves with `scaleX`. Everything else is copy.

**The four rules from the reel:**

1. Say why it is empty ("You have no projects yet"), never just a blank.
2. Give one next action as a real button ("Create your first project").
3. For a first run, add the step-by-step checklist with progress; for every
   other empty section, one line on what the section is for and how to
   start it.
4. Empty search suggests the fix: "No results for prple shoes. Search for
   purple shoes?" with the link. When empty is the goal (inbox zero), make
   it feel earned: `data-kind="achievement"`.

```html
<!-- first run -->
<div class="sig-empty">
  <svg class="sig-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="3" y="4" width="18" height="16" rx="3"/><path d="M8 12h8M12 8v8"/></svg>
  <h3>You have no projects yet</h3>
  <p>Projects hold your jobs, quotes and photos in one place.</p>
  <a class="sig-empty-cta sig-press" href="/projects/new">Create your first project</a>
  <div class="sig-empty-steps">
    <div class="sig-bar"><div class="sig-bar-fill"></div></div>
    <small data-steps-label>0 of 3 complete</small>
    <ol><li>Add your business details</li><li>Create a project</li><li>Invite a teammate</li></ol>
  </div>
</div>

<!-- empty search -->
<div class="sig-empty" data-kind="search">
  <h3>No results for "prple shoes"</h3>
  <p>Did you mean <a href="?q=purple+shoes">purple shoes</a>?</p>
</div>

<!-- inbox zero -->
<div class="sig-empty" data-kind="achievement">
  <svg class="sig-tick sig-icon is-on" viewBox="0 0 24 24"><circle class="sig-tick-ring" cx="12" cy="12" r="10"/><path class="sig-tick-path" d="M7 12.5l3.2 3.2L17 9"/></svg>
  <h3>Inbox zero</h3>
  <p>Nothing waiting on you.</p>
</div>
```
```js
var first = Signals.empty(document.querySelector('.sig-empty')); first.steps(1);
```
`Signals.init()` picks up every `.sig-empty` automatically.

---

## 23 auth-sweep

**Belongs on:** the one card that holds both login and sign-up, where the
user switches between them. **Never on:** a lone login form, a modal that
only signs in, tabs on a settings page, anything that is not two states of
one card.

**What moves:** one raked rectangle (the band) and nothing else. It sits
over the right half of the card in sign-in and slides left to cover the
left half in sign-up, one `translateX`, one curve. Sign-up is sign-in
mirrored, so the band never rotates. It is wider than the card
(`--sig-auth-band: 1.5`), so for the middle fifth of the travel it covers the whole card,
and that is where JS swaps the two forms. No fade between states: the
hidden form is `opacity: 0` with no transition, plus `inert`, so the swap
happens in the dark and is never on screen.

**How the copy stays straight:** the band is a window (`overflow: hidden`)
over a fixed brand layer. `.sig-auth-band-inner` carries the reverse
transform (`translateX(-sweep) skewX(-rake)`), so `.sig-auth-band-page` is
back in card co-ordinates: the "Welcome back" block sits on the right,
"Start the first page" on the left, and the moving window simply reveals
whichever is under it. Two gold seams live on the band's edges and travel
with it.

**Timing:** `--sig-dur-sweep` 640ms on `--sig-ease-in-out`; the forms swap
at half. Reduced motion: the band jumps, the forms swap immediately, focus
still moves to the first field of the new form.

**Phones:** under 640px the card is one column. The band parks off the
right edge (`left: 120%`, the rake eased to 8deg so the lean never pokes
into the card), wipes the whole card right to left (band width 2, sweep `-170%`) and
leaves off the left. The brand copy is hidden and the hidden form takes no
space, so the card is only as tall as the form that is showing.

```html
<div class="sig-auth" data-sig-auth data-mode="signin">
  <div class="sig-auth-band" aria-hidden="true">
    <span class="sig-auth-seam" data-edge="lead"></span>
    <span class="sig-auth-seam" data-edge="trail"></span>
    <div class="sig-auth-band-inner"><div class="sig-auth-band-page">
      <div class="sig-auth-brand" data-side="right"><small>Brand</small><h3>Welcome <em>back.</em></h3><p>Your jobs and quotes are where you left them.</p></div>
      <div class="sig-auth-brand" data-side="left"><small>Brand</small><h3>Start the <em>first page.</em></h3><p>One account for every job.</p></div>
    </div></div>
  </div>
  <form class="sig-auth-pane" data-mode="signin">
    <!-- real fields, sig-field + sig-focus as usual, sig-submit on the button -->
    <p>New here? <a href="/signup" data-sig-auth-to="signup">Create an account</a></p>
  </form>
  <form class="sig-auth-pane" data-mode="signup">
    <p>Already have an account? <a href="/login" data-sig-auth-to="signin">Sign in</a></p>
  </form>
</div>
```
```js
var card = Signals.auth(document.querySelector('.sig-auth'), { onChange: function (mode) { history.replaceState(null, '', mode === 'signup' ? '/signup' : '/login'); } });
card.go('signup');   // or let the data-sig-auth-to links do it
```
`Signals.init()` picks up every `[data-sig-auth]` automatically. Set
`--sig-auth-card`, `--sig-auth-ink`, `--sig-auth-band-bg`,
`--sig-auth-band-ink` and `--sig-auth-seam` from the site's palette; the
defaults use `--sig-surface` for the band and `--sig-accent` for the seam.
The links keep real hrefs, so with JS off each state is still a page. The
inputs take `focus-glow` and the buttons `submit-states` as on any form.

Source: Code & Chill, "Login and Signup Animation", Verso component 91
(references/source-reel.md). Our own implementation of the idea.

---

## Reduced motion, in one place

`signals.css` ends with a `prefers-reduced-motion: reduce` block that keeps
every colour and opacity change and removes every transform, delay and
keyframe. Ticks appear drawn, counts appear at their value, the shimmer
stops, sections are simply visible. Test it once per page.


## File: references/source-reel.md

# The source reel

Instagram reel by @designmotionhq, series "FutureVille · realization".
56 seconds, 720x1280, no speech. The brief that came with it: make a skill
that uses animations like this plus more, applied whenever someone asks to
animate a website or UI, but only in the places they belong.

The file is not committed here (it is their work, and this folder is a public
repo). This page is the breakdown so future sessions do not need the mp4.

## Look

Near-black background (`#0c0e12`) with a faint 40px grid. Panels one step
lighter (`#15181d`) with a 1px low-contrast border and 14px radius. Teal
accent (`#2dd4bf` region) for everything live, a pink-red for failure. Inter
style sans, one large headline per scene with the key phrase tinted accent
or danger. Small mono chips along the bottom summarise the scene's rule.
Every motion is short, ease-out, and tied to a state change; nothing loops
for decoration.

## Scenes

| t | Scene | What happens on screen | The rule it states |
|---|---|---|---|
| 0:00 | Cold open | A `brief.pdf` chip is dragged over a dead dropzone (top) and a live one (bottom): dashed teal border, teal glow behind, cloud icon becomes an arrow. | |
| 0:03 | Signal 01 · Drag feedback | "Nothing reacts." Dropzone: "Drop your file, PNG JPG PDF up to 50 MB". A `hero-banner.png 2.4 MB` chip approaches. Caption: "Users hesitate when nothing moves." | |
| 0:07 | | "It has to answer back." Border goes dashed teal, radial glow fades up, copy becomes "Release to upload" with "hero-banner.png, 2.4 MB" under it, icon becomes an up arrow. | |
| 0:13 | | Chips: 1 Border, 2 Glow, 3 Copy. Caption: "Three signals, before the drop." | A drag gets three answers before the drop. |
| 0:14 | Signal 02 · Honest progress | "A spinner hides the truth." Left card MYSTERY: red ring with "?", "Uploading…", time left ??, progress ??. Right card HONEST: `demo-recording.mp4 48 MB`, big teal 8%, thin bar. | |
| 0:18 | | Honest card at 62% with chips "7s left" and "2.4 MB/s". Bottom chips: % percent, time left, wait or walk away. Caption: "Let them decide, wait or walk away." | Show percent, time left and rate. |
| 0:22 | Signal 03 · Inline retry | "It dies at ninety percent." Upload card at 73%, then 90%. | |
| 0:25 | | Bar and percent go red at 90%. "Paused at 90%, file kept in memory". An error row grows in: warning icon, "Upload failed, Connection lost", a Retry button. Chips: connection lost, file kept loaded. Caption: "Never make them start over." | |
| 0:28 | | After Retry: bar teal again at 94%, "Resuming from 90%…". Third chip: one tap resumes. | Fail in place, keep the file, resume from where it stopped. |
| 0:31 | Signal 04 · Upload preview | "A filename is not feedback." TEXT ONLY: "✓ IMG_4032.jpg uploaded" in mono. | |
| 0:32 | | PREVIEW card grows in: filename, then a drawn tick "Uploaded just now". | |
| 0:34 | | Thumbnail fills in (teal gradient, image icon, "4032 × 3024" badge), JPG chip. Bottom: Thumbnail · Type · Size · Proof, each lighting as it appears. | |
| 0:36 | | "2.4 MB" size appears, then Replace and Remove buttons. All four bottom words lit. | Thumbnail, type, size, proof, then the next two actions. |
| 0:39 | Signal 05 · Independent queue | "Every file, its own lane." "Uploading 5 files", "0 of 5 done". Five rows: brief.pdf 20%, hero-banner.png 9%, demo-recording.mp4 17%, invoice-2026.pdf 37%, avatar-photo.jpg 6%. | |
| 0:45 | | "4 of 5 done". Four rows show teal ticks; the 48 MB video is still at 82%. Caption: "One failure never blocks the others." | One bar per item, one counter for the batch. |
| 0:48 | Outro | Three done chips stack, "Follow for more UX engineering. Save this for your next upload flow." Save button with a teal glow. | |
| 0:52 | | Fade to the handle. | |

## What the skill took from it

- The five signals, as 01 to 05 in the catalogue, with the same names for
  their states (over, uploading, error, done).
- The rule that motion is a state answer, never decoration, which became the
  placement rule in `SKILL.md`.
- The headline treatment (one tinted phrase after the line settles) as 15.
- The chip row that lights up as each point lands, as 14.
- The values: 150ms for state colour and copy, 220ms for things entering,
  linear for the bar, glow behind exactly one live element.


# The second and third reels (20 Sep 2026)

Both arrived on 20 Sep 2026 in a batch of seven videos. Neither mp4 is committed here.

## Destructive actions, @designmotionhq

`ca1106cfbd324a99813e8ef5fe1f337a.mp4`, 59 seconds, same FutureVille look
as the first reel. Six patterns, spoken and captioned:

1. Hold to delete: "release early, nothing dies. The ring is the
   confirmation. 300 milliseconds of commitment replace a dialogue."
2. Labels: "Are you sure? Yes. Nobody reads that. Name the action: Delete
   project, Keep project. The verb is the warning." Shown with a "clicked
   in 0.4s, read nothing" tag on the Yes button.
3. Off the happy path: "Destructive buttons never sit where confirm lives.
   Muscle memory clicks primary spots blind."
4. The red budget: "Red is a budget, spend it on destruction only. A red
   log out button cries wolf, then delete looks routine."
5. Danger zone: "GitHub buries deletion in a danger zone. Bordered,
   labelled, last position on the page. Geography is friction."
6. Cooldown: "Deletion scheduled, 14 days to cancel. Time is the last line
   of defence." Shown as an account card with 14 day boxes and a full-width
   Cancel deletion button.

Closing card: "Danger is a design language. Save this before you ship a
delete button." Became signal 21, `danger-confirm`, and the two red-budget
lines in Done means.

## Empty states, Katherine Gilligan

instagram.com/reel/DY5PkUjyhMv, 69 seconds, part 8 of her "Building With
Good UX" series, talking head plus phone mockups. The argument: the empty
state is often the first thing a user sees, so make it a good impression.

- First-run dashboard: "You have no projects" leaves the user with no
  action. Add "Create your first project" with a button, then a
  step-by-step gamified checklist to get them started.
- Every section with no content yet: say what the section is for and how
  to start using it, never leave it blank.
- Empty search: "No results" is okay, but "No results for purple shoes.
  What about purple shoes?" with a link to that search keeps the user
  moving.
- When empty is the goal (inbox zero): make it feel like an achievement,
  add a nice animation, a background they look forward to seeing.
- Summary: "A good empty state tells a user why it's empty, shows them
  what to do next, and doesn't feel broken."

Became signal 22, `empty-state`. Her next video is on partial states.

## Login and sign-up blade, Code & Chill

instagram.com/reel/Db7fSM1v8Yg (@_code_and_chill_, 5 seconds, 5K likes,
21 Sep 2026). A cream auth card with a deep green raked band
on its right, "VERSO, Welcome back." in serif with a gold italic. Click
"Create an account": the band slides left, covers the card, and the
sign-up form is on the left-hand side when it clears, with "Start the
first page." now showing on the band. Three code panes under the card:

- `index.html`: `.auth__band` ("one blade, two lit seams") holding
  `.band__edge--lead`, then `.band__inner` ("its transform, backwards")
  and `.band__page` ("back in card co-ordinates") with the welcome title.
- `app.css`: `.auth__band { overflow: hidden; transform: skewX(var(--rake))
  translateX(var(--sweep)); }` and `.band__inner { transform:
  translateX(calc(-1 * var(--sweep))) skewX(calc(-1 * var(--rake))); }`.
- `app.js`: set `auth.dataset.mode`, add `is-on` to the next pane, then
  after `T.cover` flip `inert` on the two panes: "the blade covers the card
  for a fifth of its travel, and the two states trade places under that
  dark".

Caption: "Login to Sign up, without a single fade. One rectangle. One
translateX. Sign up is just sign in mirrored, so the blade never has to
rotate: it travels one way and covers the card completely halfway across.
That dark is where the two states trade places. Real forms underneath.
Zero dependencies. Transform and opacity only."

Became signal 23, `auth-sweep`. The Verso name, copy and palette stay in
the demo only; a client site sets its own tokens.


## File: scripts/test-demo.mjs

```js
// Headless regression check of every signal on assets/demo.html.
// Needs: the demo served on http://localhost:8123 (npx -y serve -l 8123 assets),
// playwright-core (npm i --no-save playwright-core in the skill folder) and a
// Chromium it can launch (npx playwright install chromium). Run: node scripts/test-demo.mjs
// Prints a pass/fail line per signal and saves a screenshot of each state to scripts/shots/.
import { createRequire } from 'node:module';
import path from 'node:path';
import fs from 'node:fs';
const require = createRequire(import.meta.url);
const { chromium } = require('playwright-core');

const outDir = path.join(path.dirname(new URL(import.meta.url).pathname.replace(/^\/([A-Za-z]:)/, '$1')), 'shots');
fs.mkdirSync(outDir, { recursive: true });
const shot = async (page, name, sel) => {
  const el = sel ? page.locator(sel).first() : null;
  if (el) await el.scrollIntoViewIfNeeded();
  await page.waitForTimeout(150);
  await (el ? el.screenshot({ path: path.join(outDir, name + '.png') }) : page.screenshot({ path: path.join(outDir, name + '.png') }));
};
const results = [];
const check = (name, ok, detail) => results.push({ name, ok: !!ok, detail });

const browser = await chromium.launch({ headless: true });
const page = await browser.newPage({ viewport: { width: 1000, height: 900 } });
const errors = [];
page.on('console', m => { if (m.type() === 'error' && !/favicon/.test(m.location()?.url || '')) errors.push(m.text() + ' @ ' + (m.location()?.url || '')); });
page.on('pageerror', e => errors.push(e.message));
await page.goto('http://localhost:8123/demo', { waitUntil: 'networkidle' });
await page.waitForTimeout(1000);

// 15 headline emphasis
const em = await page.evaluate(() => { const h = document.querySelector('[data-sig-headline]'); return { isIn: h.classList.contains('is-in'), color: getComputedStyle(h.querySelector('em')).color, opacity: getComputedStyle(h).opacity }; });
check('15 headline-emphasis: h1 visible and phrase teal after settle', em.isIn && em.opacity === '1' && em.color === 'rgb(45, 212, 191)', JSON.stringify(em));
await shot(page, '15-headline', 'h1');

// 16 section reveal: below-fold sections hidden at load, no warning for hero
const rv0 = await page.evaluate(() => [...document.querySelectorAll('[data-sig-reveal]')].map(e => ({ id: e.id, isIn: e.classList.contains('is-in'), op: getComputedStyle(e).opacity })));
check('16 section-reveal: below-fold sections still hidden at load', rv0.every(r => !r.isIn && r.op === '0'), JSON.stringify(rv0));

// 01 dropzone over
await page.click('[data-dz="over"]');
await page.waitForTimeout(400);
const dz = await page.evaluate(() => { const dz = document.getElementById('dz'); const over = dz.querySelector('[data-when="over"]'); return { state: dz.dataset.state, border: getComputedStyle(dz).borderColor, glow: getComputedStyle(dz, '::before').opacity, over: getComputedStyle(over).opacity, idle: getComputedStyle(dz.querySelector('[data-when="idle"]')).opacity, copy: dz.querySelector('[data-sig-dropcopy]').textContent }; });
check('01 drop-answer: border teal, glow on, copy swapped', dz.border === 'rgb(45, 212, 191)' && dz.glow === '1' && dz.over === '1' && dz.idle === '0' && dz.copy.includes('hero-banner'), JSON.stringify(dz));
await shot(page, '01-dropzone-over', '#dz');
await page.click('[data-dz="idle"]');
await page.waitForTimeout(300);
const dz2 = await page.evaluate(() => { const dz = document.getElementById('dz'); return { state: dz.dataset.state, glow: getComputedStyle(dz, '::before').opacity }; });
check('01 drop-answer: returns to idle, glow off', dz2.state === 'idle' && dz2.glow === '0', JSON.stringify(dz2));

// 02 honest progress mid-flight
await page.click('#up1-go');
await page.waitForTimeout(1200);
const pr = await page.evaluate(() => { const u = document.getElementById('up1'); return { state: u.dataset.state, pct: u.querySelector('[data-pct]').textContent, eta: u.querySelector('[data-eta]').textContent, rate: u.querySelector('[data-rate]').textContent, etaOn: u.querySelector('[data-eta]').classList.contains('is-on'), scale: getComputedStyle(u.querySelector('.sig-bar-fill')).transform }; });
check('02 honest-progress: percent, time left and rate shown, bar scaled', pr.state === 'uploading' && parseInt(pr.pct) > 0 && /left/.test(pr.eta) && /MB\/s/.test(pr.rate) && pr.etaOn && pr.scale !== 'none', JSON.stringify(pr));
await shot(page, '02-progress', '#up1');
await page.waitForTimeout(2500);
const prDone = await page.evaluate(() => document.getElementById('up1').dataset.state);
check('02 honest-progress: reaches done', prDone === 'done', prDone);

// 03 inline retry
await page.click('#up1-fail');
await page.waitForTimeout(3600);
const fl = await page.evaluate(() => { const u = document.getElementById('up1'); return { state: u.dataset.state, pct: u.querySelector('[data-pct]').textContent, status: u.querySelector('[data-status]').textContent, fill: getComputedStyle(u.querySelector('.sig-bar-fill')).backgroundColor, rows: getComputedStyle(u.querySelector('.sig-retry-row')).gridTemplateRows, cardOp: getComputedStyle(u.querySelector('.sig-retry-card')).opacity }; });
check('03 inline-retry: red at 90%, error row open, file kept', fl.state === 'error' && fl.pct === '90%' && fl.fill === 'rgb(240, 80, 110)' && parseFloat(fl.rows) > 40 && /kept/.test(fl.status), JSON.stringify(fl));
await shot(page, '03-retry', '#up1');
await page.click('#up1 [data-retry]');
await page.waitForTimeout(500);
const rs = await page.evaluate(() => { const u = document.getElementById('up1'); return { state: u.dataset.state, status: u.querySelector('[data-status]').textContent, pct: u.querySelector('[data-pct]').textContent, rows: getComputedStyle(u.querySelector('.sig-retry-row')).gridTemplateRows }; });
check('03 inline-retry: resumes from 90, row collapses', rs.state === 'uploading' && /Resuming from 90/.test(rs.status) && parseInt(rs.pct) >= 90 && parseFloat(rs.rows) < 1, JSON.stringify(rs));
await page.waitForTimeout(2000);
const rsDone = await page.evaluate(() => document.getElementById('up1').dataset.state);
check('03 inline-retry: completes after retry', rsDone === 'done', rsDone);

// 04 preview
await page.click('#pv1-show');
await page.waitForTimeout(600);
const pv = await page.evaluate(() => { const pv = document.getElementById('pv1'); return { hidden: pv.hidden, op: getComputedStyle(pv).opacity, tf: getComputedStyle(pv).transform, tick: getComputedStyle(pv.querySelector('.sig-tick-path')).strokeDashoffset }; });
check('04 upload-preview: card in, tick drawn', !pv.hidden && pv.op === '1' && pv.tf === 'none' && pv.tick === '0px', JSON.stringify(pv));
await shot(page, '04-preview', '#pv1');

// 05 queue
await page.click('#q1-go');
await page.waitForTimeout(2200);
const qMid = await page.evaluate(() => { const q = document.getElementById('q1'); return { counter: q.querySelector('[data-done]').textContent, states: [...q.querySelectorAll('[data-sig-queue-item]')].map(r => r.dataset.state) }; });
await shot(page, '05-queue-mid', '#q1');
await page.waitForTimeout(8000);
const qEnd = await page.evaluate(() => { const q = document.getElementById('q1'); return { counter: q.querySelector('[data-done]').textContent, states: [...q.querySelectorAll('[data-sig-queue-item]')].map(r => r.dataset.state), tick: getComputedStyle(q.querySelector('[data-state="done"] .sig-tick-path')).strokeDashoffset }; });
check('05 own-lane-queue: rows finish independently, counter counts, ticks draw', qMid.states.includes('uploading') && qEnd.states.every(s => s === 'done') && qEnd.counter === '5 of 5 done' && qEnd.tick === '0px', JSON.stringify({ qMid, qEnd }));
await shot(page, '05-queue-done', '#q1');

// 07 focus glow
await page.focus('#name');
await page.keyboard.press('Tab'); await page.keyboard.press('Shift+Tab');
await page.waitForTimeout(300);
const fg = await page.evaluate(() => { const i = document.getElementById('name'); return { focused: document.activeElement === i, shadow: getComputedStyle(i).boxShadow, border: getComputedStyle(i).borderColor }; });
check('07 focus-glow: ring and accent border on focus-visible', fg.focused && fg.shadow !== 'none' && fg.border === 'rgb(45, 212, 191)', JSON.stringify(fg));
await shot(page, '07-focus', '#f1');

// 09 field-error + 08 submit error path
await page.click('#f1 .sig-submit');
await page.waitForTimeout(500);
const fe = await page.evaluate(() => { const f = document.querySelector('#f1 .sig-field'); const b = document.querySelector('#f1 .sig-submit'); return { field: f.dataset.state, rows: getComputedStyle(f.querySelector('.sig-field-msg')).gridTemplateRows, btn: b.dataset.state, btnBg: getComputedStyle(b).backgroundColor, minW: b.style.minWidth }; });
check('09 field-error: message open under the empty field', fe.field === 'error' && parseFloat(fe.rows) > 10, JSON.stringify(fe));
check('08 submit-states: error state, width locked', fe.btn === 'error' && fe.btnBg === 'rgb(240, 80, 110)' && /px/.test(fe.minW), JSON.stringify(fe));
await shot(page, '08-09-submit-error', '#f1');
await page.waitForTimeout(1800);
await page.fill('#name', 'Test'); await page.fill('#email', 'test@example.com'); await page.fill('#msg', 'Hello');
await page.click('#f1 .sig-submit');
await page.waitForTimeout(300);
const ld = await page.evaluate(() => { const b = document.querySelector('#f1 .sig-submit'); return { btn: b.dataset.state, spinner: getComputedStyle(b.querySelector('.sig-spinner')).opacity, label: getComputedStyle(b.querySelector('.sig-submit-label')).opacity, cleared: [...document.querySelectorAll('#f1 .sig-field')].every(f => f.dataset.state === 'idle') }; });
check('08 submit-states: loading shows spinner, hides label, errors cleared on input', ld.btn === 'loading' && ld.spinner === '1' && ld.label === '0' && ld.cleared, JSON.stringify(ld));
await shot(page, '08-submit-loading', '#f1');
await page.waitForTimeout(1600);
const dn = await page.evaluate(() => { const b = document.querySelector('#f1 .sig-submit'); return { btn: b.dataset.state, done: getComputedStyle(b.querySelector('.sig-submit-done')).opacity, tick: getComputedStyle(b.querySelector('.sig-submit-done .sig-tick-path')).strokeDashoffset, toast: document.querySelector('.sig-toast')?.classList.contains('is-on'), toastText: document.querySelector('.sig-toast')?.textContent }; });
check('08 submit-states: done with tick, toast fired', dn.btn === 'done' && dn.done === '1' && dn.tick === '0px' && dn.toast && /sent/i.test(dn.toastText), JSON.stringify(dn));
await shot(page, '08-submit-done-13-toast', 'body');

// 11 state glow: only one active
await page.click('[data-step]:nth-of-type(3)');
await page.waitForTimeout(300);
const gl = await page.evaluate(() => ({ active: [...document.querySelectorAll('.sig-glow.is-active')].length, third: document.querySelectorAll('[data-step]')[2].classList.contains('is-active') }));
check('11 state-glow: exactly one active after click', gl.active === 1 && gl.third, JSON.stringify(gl));
await shot(page, '11-glow', '#s10 .grid2');

// 10 + 12 copy-swap and tick
await page.click('#sw1-go');
await page.waitForTimeout(1400);
const sw = await page.evaluate(() => ({ b: getComputedStyle(document.querySelector('#sw1 [data-when="b"]')).opacity, a: getComputedStyle(document.querySelector('#sw1 [data-when="a"]')).opacity, tick: getComputedStyle(document.querySelector('#tk1 .sig-tick-path')).strokeDashoffset }));
check('10 copy-swap + 12 tick-draw: Saved shown, Saving gone, tick drawn', sw.b === '1' && sw.a === '0' && sw.tick === '0px', JSON.stringify(sw));

// 16 reveal + 14 chips + 17 card lift on scroll
await page.locator('#s13').scrollIntoViewIfNeeded();
await page.waitForTimeout(900);
const rv = await page.evaluate(() => { const s = document.getElementById('s13'); const chips = s.querySelector('.sig-chips'); return { isIn: s.classList.contains('is-in'), op: getComputedStyle(s).opacity, chipsIn: chips.classList.contains('is-in'), lastChipOp: getComputedStyle(chips.lastElementChild).opacity, delays: [...chips.children].map(c => c.style.getPropertyValue('--i')).join(',') }; });
check('16 section-reveal: section revealed on scroll', rv.isIn && rv.op === '1', JSON.stringify(rv));
check('14 chip-stagger: chips numbered and revealed', rv.chipsIn && rv.lastChipOp === '1' && rv.delays === '0,1,2,3,4,5,6,7', JSON.stringify(rv));
await page.hover('#s13 a.sig-card');
await page.waitForTimeout(300);
const cl = await page.evaluate(() => ({ link: getComputedStyle(document.querySelector('#s13 a.sig-card')).transform, static: getComputedStyle(document.querySelector('#s13 div.card')).transform }));
check('17 card-lift: clickable card lifts, static card does not', /matrix\(1, 0, 0, 1, 0, -2\)/.test(cl.link) && cl.static === 'none', JSON.stringify(cl));
await shot(page, '14-16-17-page', '#s13');

// 18 nav underline
const nv = await page.evaluate(() => ({ active: getComputedStyle(document.querySelector('.sig-nav a[aria-current]'), '::after').transform, other: getComputedStyle(document.querySelectorAll('.sig-nav a')[1], '::after').transform }));
check('18 nav-underline: active link underlined, others not', nv.active === 'matrix(1, 0, 0, 1, 0, 0)' && /matrix\(0,/.test(nv.other), JSON.stringify(nv));

// 19 count-up + 20 skeleton
await page.locator('#s19').scrollIntoViewIfNeeded();
const t0 = Date.now();
await page.waitForTimeout(900);
const early = await page.evaluate(() => document.querySelector('[data-sig-count]').textContent);
check('19 count-up: still counting at 0.9s (slow on purpose)', early !== '398', 'at 0.9s: ' + early);
await page.waitForTimeout(1800);
const cu = await page.evaluate(() => [...document.querySelectorAll('[data-sig-count]')].map(e => e.textContent));
check('19 count-up: stats land on their values with prefix and suffix', cu[0] === '398' && cu[1] === '4.5%' && cu[2] === 'A$20/yr', JSON.stringify(cu));
const sk = await page.evaluate(() => ({ anim: getComputedStyle(document.querySelector('.sig-skeleton'), '::after').animationName }));
check('20 skeleton: shimmer running', sk.anim === 'sig-shimmer', JSON.stringify(sk));
await shot(page, '19-20-stats', '#s19');

// 23 auth-sweep
await page.locator('#s23').scrollIntoViewIfNeeded();
await page.waitForTimeout(700);
const a0 = await page.evaluate(() => { const c = document.getElementById('auth1'); const pg = c.querySelector('.sig-auth-band-page'); const cr = c.getBoundingClientRect(), pr = pg.getBoundingClientRect(); return { mode: c.dataset.mode, inOn: document.getElementById('auth1-in').classList.contains('is-on'), upOn: document.getElementById('auth1-up').classList.contains('is-on'), upInert: document.getElementById('auth1-up').inert, pageLeft: Math.round(pr.left - cr.left), pageW: Math.round(pr.width), cardW: Math.round(cr.width) }; });
check('23 auth-sweep: sign-in on, sign-up hidden and inert, brand layer sits exactly on the card', a0.mode === 'signin' && a0.inOn && !a0.upOn && a0.upInert && Math.abs(a0.pageLeft) <= 1 && Math.abs(a0.pageW - a0.cardW) <= 1, JSON.stringify(a0));
await shot(page, '23-auth-signin', '#auth1');
await page.click('#auth1 [data-sig-auth-to="signup"]');
await page.waitForTimeout(160);
const a1 = await page.evaluate(() => { const c = document.getElementById('auth1'); return { mode: c.dataset.mode, inOn: document.getElementById('auth1-in').classList.contains('is-on'), tf: getComputedStyle(c.querySelector('.sig-auth-band')).transform }; });
check('23 auth-sweep: band moving, forms not yet swapped early in the travel', a1.mode === 'signup' && a1.inOn && /^matrix\(/.test(a1.tf), JSON.stringify(a1));
await shot(page, '23-auth-mid', '#auth1');
await page.waitForTimeout(700);
const a2 = await page.evaluate(() => { const c = document.getElementById('auth1'); const b = c.querySelector('.sig-auth-band'); const pg = c.querySelector('.sig-auth-band-page'); const m = new DOMMatrix(getComputedStyle(b).transform); return { inOn: document.getElementById('auth1-in').classList.contains('is-on'), upOn: document.getElementById('auth1-up').classList.contains('is-on'), inInert: document.getElementById('auth1-in').inert, tx: Math.round(m.e), bandW: Math.round(b.offsetWidth), pageLeft: Math.round(pg.getBoundingClientRect().left - c.getBoundingClientRect().left), focused: document.activeElement && document.activeElement.id }; });
check('23 auth-sweep: forms swapped under the band, band travelled its own width, brand layer still on the card, focus in the first field', a2.upOn && !a2.inOn && a2.inInert && Math.abs(a2.tx + a2.bandW) <= 2 && Math.abs(a2.pageLeft) <= 1 && a2.focused === 'a1-name', JSON.stringify(a2));
await shot(page, '23-auth-signup', '#auth1');
await page.click('#auth1-up button[type="submit"]');
await page.waitForTimeout(300);
const a3 = await page.evaluate(() => ({ fields: [...document.querySelectorAll('#auth1-up .sig-field')].map(f => f.dataset.state), btn: document.querySelector('#auth1-up .sig-submit').dataset.state }));
check('23 auth-sweep: the sign-up form validates for real (three field errors, button error)', a3.fields.every(s => s === 'error') && a3.btn === 'error', JSON.stringify(a3));

// reduced motion
await page.emulateMedia({ reducedMotion: 'reduce' });
await page.reload({ waitUntil: 'networkidle' });
await page.waitForTimeout(600);
await page.click('[data-dz="over"]');
await page.locator('#s19').scrollIntoViewIfNeeded();
await page.waitForTimeout(400);
const rm = await page.evaluate(() => ({ h1op: getComputedStyle(document.querySelector('[data-sig-headline]')).opacity, h1tf: getComputedStyle(document.querySelector('[data-sig-headline]')).transform, icon: getComputedStyle(document.querySelector('#dz [data-when="over"] .sig-icon')).transform, count: document.querySelector('[data-sig-count]').textContent, shimmer: getComputedStyle(document.querySelector('.sig-skeleton'), '::after').animationName, s19: getComputedStyle(document.getElementById('s19')).opacity, border: getComputedStyle(document.getElementById('dz')).borderColor }));
check('reduced motion: content visible, transforms gone, colour kept, count jumps', rm.h1op === '1' && rm.h1tf === 'none' && rm.icon === 'none' && rm.count === '398' && rm.shimmer === 'none' && rm.s19 === '1' && rm.border === 'rgb(45, 212, 191)', JSON.stringify(rm));

await page.locator('#s23').scrollIntoViewIfNeeded();
await page.click('#auth1 [data-sig-auth-to="signup"]');
await page.waitForTimeout(80);
const rmA = await page.evaluate(() => ({ upOn: document.getElementById('auth1-up').classList.contains('is-on'), inOn: document.getElementById('auth1-in').classList.contains('is-on'), dur: getComputedStyle(document.querySelector('#auth1 .sig-auth-band')).transitionDuration }));
check('23 auth-sweep, reduced motion: forms swap at once, band jumps', rmA.upOn && !rmA.inOn && rmA.dur === '0.001s', JSON.stringify(rmA));

check('no console or page errors', errors.length === 0, errors.join(' | ') || 'clean');
await browser.close();

let fails = 0;
for (const r of results) { if (!r.ok) fails++; console.log((r.ok ? 'PASS ' : 'FAIL ') + r.name + (r.ok ? '' : '\n      ' + r.detail)); }
console.log(`\n${results.length - fails}/${results.length} passed. Screenshots in ${outDir}`);
process.exit(fails ? 1 : 0);

```
