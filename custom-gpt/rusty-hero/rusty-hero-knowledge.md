# rusty-hero knowledge file

This file holds the whole `rusty-hero` skill: SKILL.md first, then every reference, asset and script it points to. Each one starts with a `## File:` heading that gives its path inside the skill.

## File: SKILL.md

# Rusty hero


> **Note for ChatGPT.** This skill was first written for Claude Code. Where it names Claude, a subagent, a slash command or a tool you do not have (a terminal, a browser preview, the user's own files), do that step yourself in the conversation, or give the user the exact code or command to run and say which step you could not run. Where it says to copy a file from `assets/` into the project, show the user that file's contents so they can paste it.

Seven of Rusty's reels are the same move on a different product, and the
move is what people stop scrolling for: **the product owns the page.** Pick
the Fanta can and the page goes orange. Pick Sprite and it goes green. One
rounded card is the stage, the product floats in the middle of it, and every
control is a pill. The breakdown of the reels is in
`references/source-reels.md`; the reels themselves are on Instagram at
@ui_ux_rusty_freelancer.

The port keeps exactly that: one colour in, a whole palette out, and a
switch that slides the old product away and the new one in while the page
crossfades. Everything else in Rusty's frames (orbit ring, ghosted word, size
chips) is garnish that ships only when there is a real product to sit on.

## The placement rule

The hero needs **a choice**. Two or more subjects a visitor picks between,
each with its own colour. Products, plans, services, bots, agents, tiers,
packs. If a page has one thing to sell, or nothing to choose, this skill does
not apply, and no amount of liking the look changes that. A colour takeover
with nothing to switch is a coloured box.

It goes on **one section per page**, the one where the choice happens.
Usually the hero. Never the whole site, never every card, never the footer.
The rest of the page keeps its own design; the hero is the one place that
argues loudly.

Which mode, by what the page is:

| Page is | Mode | What happens |
|---|---|---|
| Marketing site, shop, landing page, portfolio, demo | `full` | The page background recolours with the subject. Rusty's actual move. |
| Web app, dashboard, console, internal tool | `tint` | Only the card recolours; the page gets a 12% tint of the colour so data around it stays readable. |

The `tint` mode exists because a dashboard full of numbers turning orange is
unreadable, and dashboards were picked for this deliberately.
Give them the card and the selector, not the takeover.

## Workflow

### 1. Find the choice

Read the page's HTML, not the brief. Write down, in the reply, the subjects
this hero will switch between and the colour each one owns. Sources for the
colour, in order: the product's own packaging or logo, the brand's colour if
it is a third party (ServiceM8 is teal, Tradify is blue), otherwise a colour
from the project's own palette per subject. Two subjects sharing a colour
defeats the point; pick again.

If there is no honest list of two or more, stop and say so. Do not invent
subjects to have something to switch.

### 2. Decide what the media is

Rusty's frames work because a real product image sits in the middle. In
order of preference:

- A cut-out product photo or render (PNG with transparency, or a photo that
  reads as an object).
- The subject's logo, large, if the subject is a third-party brand.
- A big typographic mark: the plan name, the price, the bot's ticker. Set it
  in `.rusty-ghost` style at full opacity as the media itself and drop the
  separate ghost word.
- A screenshot in a rounded frame, last resort.

Never an emoji, never a stock icon. If the best available media is weaker
than the copy, make the copy the media and keep the card clean.

### 3. Install in the project's own structure

`assets/rusty.css` and `assets/rusty.js` are dependency free and written for
hand-written HTML, CSS and JS static sites. Never add React, Tailwind or a motion library to use them.

- Copy both files into the folder the project already keeps CSS and JS in.
  Link `rusty.css` after the project's own stylesheet.
- Set `--rusty-display` and `--rusty-body` on the scope to the project's
  own fonts. The demo uses Georgia; a client site must not. Set them on a
  selector that beats `[data-rusty]` (for example `.hero[data-rusty]`): a
  bare class rule ties on specificity and loses on order.
- A typographic mark instead of an image: give the mark element the
  `rusty-product` class and `data-field="mark"`; it then slides in and out
  like a product would.
- Mark the scope with `data-rusty="full"` or `data-rusty="tint"`. For `full`
  the scope is usually `<body>` or `<main>`; for `tint` it is the section
  around the card.
- Put the subjects in a `<script type="application/json" data-rusty-subjects>`
  block inside the scope. Copy the shape from `assets/demo.html`. Fields you
  leave out are hidden, so a plan switcher with no image simply has no image.
- The card markup is in `assets/demo.html`. Keep the grid areas; drop the
  blocks you have no content for (a dashboard has no quantity stepper).
- A single-file page inlines only what it uses.

If the site already has a hero, this replaces it, it does not stack under
it. Two heroes is one too many.

### 4. Make the colours honest

`rusty.js` derives page, card, edge, glow and text colour from the one
`colour` per subject. Check the result on every subject, not just the first:
a very light subject colour (cream, pale yellow) gives dark text on a light
card, which is correct but reads differently from the orange frame. If a
derived text colour fails contrast on a real subject, darken that subject's
`colour` rather than patching the CSS.

Chips, arrows and the CTA inherit from the derived ink. Never hard-code a
colour into the card.

### 5. Wire the switch to something real

The switcher fires `rusty:change` on the scope with the subject. Use it. On
a shop the price and cart link change; on a plan switcher the checkout link
changes; on a dashboard the selected bot's live numbers load. A switch that
only changes the colour is decoration, and someone will ask what it is for.

Deep links work out of the box (`#rusty=<id>`), so a nav link or an email
can land on a specific subject.

### 6. Verify in the browser

Fastest route: `node ~/.claude/skills/rusty-hero/scripts/shots.mjs <page.html or url> <outdir>`
clicks every subject headless and writes desktop, phone and reduced-motion
screenshots plus the scope background colour per subject (first run:
`npm i --no-save playwright-core@1.47` inside the skill folder). Look at the
images, do not just trust the log. Or by hand: open the page in the browser pane. Click through every subject and screenshot
each one: the colour must change, the media must slide, the copy must swap,
the CTA must point at the right place. Then the phone width, then reduced
motion. A hero that only works on the first subject has not shipped.

### 7. Report

Keep it short: the subjects and their colours, one line; what the media
is, one line; what the switch actually changes, one line; what was
deliberately left out and why. Screenshots. Name the source ("Rusty hero,
from @ui_ux_rusty_freelancer") so the user can look at the original.

## Values

```css
--rusty-radius: 28px;
--rusty-ease: cubic-bezier(0.23, 1, 0.32, 1);
--rusty-dur-colour: 600ms;   /* page and card crossfade */
--rusty-dur-move: 520ms;     /* product out and in */
```

Colour transitions run on registered custom properties (`@property`), so
the gradient itself crossfades, not just a flat background. Product motion
is `transform` and `opacity` only. Hover motion is gated behind
`(hover: hover) and (pointer: fine)`. Reduced motion keeps the colour change
and drops the slide.

## What this is not

It is not a theme, a colour system, or permission to recolour a site. It is
one section that changes colour when a visitor makes a choice. Motion for the
rest of the page goes through `ui-signals`; hero text effects come from
React Bits; whole components from an open source library such as Preline or daisyUI.

## Done means

Before handing this back, check every line. If any fails, fix it and check
again. Do not report the work as finished until all of them pass.

- [ ] The reply lists two or more real subjects and the colour each owns, and no two share a colour
- [ ] The hero is on exactly one section per page, the one where the choice is made
- [ ] Mode is `full` on marketing pages and `tint` on apps and dashboards
- [ ] The media is a product image, a brand logo or a typographic mark, never an emoji or stock icon
- [ ] `rusty.css` and `rusty.js` sit in the project's existing asset folder, `rusty.css` is linked after the project stylesheet, and `--rusty-display` and `--rusty-body` are the project's own fonts, not the demo's Georgia
- [ ] No React, Tailwind or motion library was added
- [ ] The switch changes something real (price, link, loaded data), not only the colour
- [ ] Every subject was clicked in the browser pane and screenshotted, and the page recoloured on each
- [ ] Phone width was checked and the card stacks without horizontal scroll
- [ ] Reduced motion was checked and the colour still changes with the slide removed
- [ ] The old hero, if there was one, is gone, not stacked above or below this one
- [ ] No em dashes, no emojis, in the reply or in any copy written into the site

If a line cannot be checked without the user, say which one and why, rather
than assuming it passes.


## File: assets/demo.html

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>rusty-hero demo</title>
<link rel="stylesheet" href="rusty.css">
<style>
  body { margin: 0; font-family: system-ui, sans-serif; min-height: 100vh; display: grid; place-items: center; padding: 32px 16px; box-sizing: border-box; }
  [data-rusty] { --rusty-display: "Georgia", serif; --rusty-body: system-ui, sans-serif; width: min(960px, 100%); }
  .wrap { width: 100%; }
  p.note { text-align: center; font-size: 0.8rem; opacity: 0.6; margin-top: 18px; }
</style>
</head>
<body data-rusty="full" data-rusty-start="fanta">
<div class="wrap">
  <div class="rusty-hero" data-rusty-hero tabindex="0" aria-label="Product showcase">
    <nav class="rusty-nav">
      <a class="rusty-logo" href="#">Fresh</a>
      <div class="rusty-pills"><a href="#" aria-current="page">Products</a><a href="#">Contact</a></div>
      <div class="rusty-icons"><a href="#" aria-label="Account">&#9679;</a><a href="#" aria-label="Cart">&#9633;</a></div>
    </nav>

    <div class="rusty-copy">
      <span class="rusty-kicker rusty-swap" data-field="kicker"></span>
      <h1 class="rusty-title rusty-swap" data-field="title"></h1>
      <p class="rusty-desc rusty-swap" data-field="desc"></p>
      <div class="rusty-chips rusty-swap" data-field="chips"></div>
    </div>

    <div class="rusty-media">
      <div class="rusty-ghost rusty-swap" data-field="ghost" aria-hidden="true"></div>
      <div class="rusty-orbit" aria-hidden="true"></div>
      <img class="rusty-product" data-field="image" alt="">
    </div>

    <div class="rusty-side">
      <div class="rusty-variants" data-field="variants" role="group" aria-label="Size"></div>
      <div class="rusty-thumbs" data-thumbs role="group" aria-label="Choose product"></div>
    </div>

    <div class="rusty-price">
      <div class="rusty-amount rusty-swap"><span data-field="amount"></span><small data-field="amountNote"></small></div>
      <div class="rusty-arrows">
        <button type="button" data-prev aria-label="Previous">&#8592;</button>
        <button type="button" data-next aria-label="Next">&#8594;</button>
      </div>
      <div class="rusty-dots" data-dots aria-hidden="true"></div>
    </div>

    <div class="rusty-buy">
      <div class="rusty-stepper"><button type="button" aria-label="Fewer">&minus;</button><span>2</span><button type="button" aria-label="More">+</button></div>
      <a class="rusty-cta" data-field="cta" href="#"></a>
    </div>
  </div>

  <script type="application/json" data-rusty-subjects>
  [
    { "id": "fanta", "colour": "#f28c28", "kicker": "Orange flavour", "title": "Fanta taste",
      "desc": "Experience the perfect blend of fresh, juicy oranges in every sip.",
      "image": "data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 120 240'><rect x='20' y='10' width='80' height='220' rx='18' fill='%23ff9d3c'/><rect x='20' y='10' width='80' height='220' rx='18' fill='url(%23g)'/><circle cx='60' cy='120' r='38' fill='%232b4fb5'/><text x='60' y='128' font-size='22' font-family='Arial' font-weight='900' fill='white' text-anchor='middle'>FANTA</text><defs><linearGradient id='g' x1='0' x2='1'><stop offset='0' stop-color='white' stop-opacity='.25'/><stop offset='.5' stop-color='white' stop-opacity='0'/><stop offset='1' stop-color='black' stop-opacity='.25'/></linearGradient></defs></svg>",
      "chips": ["Sugar free", "Vegan"], "variants": ["500", "250", "100"], "variant": 1,
      "amount": "$79.50", "amountNote": "per case", "ghost": "FANTA", "cta": { "label": "Add to cart", "href": "#" } },
    { "id": "sprite", "colour": "#2fb84a", "kicker": "Lemon lime", "title": "Sprite taste",
      "desc": "Crisp, clean and cold, with the lemon lime bite you know.",
      "image": "data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 120 240'><rect x='20' y='10' width='80' height='220' rx='18' fill='%233fcf5c'/><rect x='20' y='10' width='80' height='220' rx='18' fill='url(%23g)'/><circle cx='60' cy='120' r='38' fill='%231f5fbf'/><text x='60' y='128' font-size='20' font-family='Arial' font-weight='900' fill='white' text-anchor='middle'>Sprite</text><defs><linearGradient id='g' x1='0' x2='1'><stop offset='0' stop-color='white' stop-opacity='.25'/><stop offset='.5' stop-color='white' stop-opacity='0'/><stop offset='1' stop-color='black' stop-opacity='.25'/></linearGradient></defs></svg>",
      "chips": ["Sugar free", "Vegan"], "variants": ["500", "250", "100"], "variant": 1,
      "amount": "$79.50", "amountNote": "per case", "ghost": "SPRITE", "cta": { "label": "Add to cart", "href": "#" } },
    { "id": "coke", "colour": "#d9282e", "kicker": "Original", "title": "Coca Cola taste",
      "desc": "The one and only, served ice cold, share a Coke.",
      "image": "data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 120 240'><rect x='20' y='10' width='80' height='220' rx='18' fill='%23e63946'/><rect x='20' y='10' width='80' height='220' rx='18' fill='url(%23g)'/><text x='60' y='128' font-size='18' font-family='Georgia' font-style='italic' font-weight='900' fill='white' text-anchor='middle'>Coca Cola</text><defs><linearGradient id='g' x1='0' x2='1'><stop offset='0' stop-color='white' stop-opacity='.25'/><stop offset='.5' stop-color='white' stop-opacity='0'/><stop offset='1' stop-color='black' stop-opacity='.25'/></linearGradient></defs></svg>",
      "chips": ["Classic", "Chilled"], "variants": ["500", "250", "100"], "variant": 1,
      "amount": "$79.50", "amountNote": "per case", "ghost": "COKE", "cta": { "label": "Add to cart", "href": "#" } }
  ]
  </script>
  <p class="note">Arrows, thumbnails or left/right keys switch product. The page recolours with it. Add <code>#rusty=coke</code> to the URL to land on one.</p>
</div>
<script src="rusty.js"></script>
</body>
</html>

```

## File: assets/rusty.css

```css
/* rusty.css
   Product-colour hero lifted from @ui_ux_rusty_freelancer.
   Dependency free. Drop next to the project's own stylesheet, link it after it.
   Everything hangs off one colour: rusty.js sets --rusty-base on the scope
   and derives page, card, glow and ink from it. Nothing else to configure.

   Two modes on the scope element:
     data-rusty="full"  the page recolours with the product (marketing sites)
     data-rusty="tint"  only the card recolours, the page gets a 6% tint (apps, dashboards)
*/

@property --rusty-page  { syntax: '<color>'; inherits: true; initial-value: #ece9e4; }
@property --rusty-card  { syntax: '<color>'; inherits: true; initial-value: #6b6b6b; }
@property --rusty-deep  { syntax: '<color>'; inherits: true; initial-value: #4a4a4a; }
@property --rusty-glow  { syntax: '<color>'; inherits: true; initial-value: #8a8a8a; }
@property --rusty-ink   { syntax: '<color>'; inherits: true; initial-value: #ffffff; }
@property --rusty-cta   { syntax: '<color>'; inherits: true; initial-value: #0b0b0b; }
@property --rusty-cta-ink { syntax: '<color>'; inherits: true; initial-value: #ffffff; }

[data-rusty] {
  --rusty-radius: 28px;
  --rusty-ease: cubic-bezier(0.23, 1, 0.32, 1);
  --rusty-dur-colour: 600ms;
  --rusty-dur-move: 520ms;
  --rusty-display: inherit;   /* set to the project's display face */
  --rusty-body: inherit;      /* set to the project's body face */
  transition: --rusty-page var(--rusty-dur-colour) var(--rusty-ease),
              background-color var(--rusty-dur-colour) var(--rusty-ease);
}
/* A site-wide img{display:block} would defeat the hidden attribute rusty.js uses */
[data-rusty] [hidden] { display: none !important; }
[data-rusty="full"] { background-color: var(--rusty-page); }
[data-rusty="tint"] { background-color: color-mix(in oklab, var(--rusty-page) 12%, transparent); }

/* The stage */
.rusty-hero {
  position: relative;
  display: grid;
  grid-template-columns: 1fr minmax(0, 1.4fr) 1fr;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "nav nav nav"
    "copy media side"
    "price media buy";
  gap: 16px 24px;
  min-height: 520px;
  padding: 24px 28px 28px;
  border-radius: var(--rusty-radius);
  color: var(--rusty-ink);
  font-family: var(--rusty-body);
  isolation: isolate;
  overflow: hidden;
  background:
    radial-gradient(60% 55% at 50% 48%, var(--rusty-glow) 0%, var(--rusty-card) 62%, var(--rusty-deep) 100%);
  box-shadow: 0 30px 60px -30px color-mix(in oklab, var(--rusty-deep) 60%, transparent);
  transition: --rusty-card var(--rusty-dur-colour) var(--rusty-ease),
              --rusty-deep var(--rusty-dur-colour) var(--rusty-ease),
              --rusty-glow var(--rusty-dur-colour) var(--rusty-ease),
              --rusty-ink var(--rusty-dur-colour) var(--rusty-ease),
              --rusty-cta var(--rusty-dur-colour) var(--rusty-ease),
              --rusty-cta-ink var(--rusty-dur-colour) var(--rusty-ease);
}

/* Nav inside the card */
.rusty-nav { grid-area: nav; display: flex; align-items: center; justify-content: space-between; gap: 16px; }
.rusty-nav .rusty-logo { font-family: var(--rusty-display); font-weight: 700; font-size: 1.25rem; letter-spacing: -0.01em; text-decoration: none; color: inherit; }
.rusty-pills { display: flex; gap: 2px; padding: 3px; border-radius: 999px; background: color-mix(in oklab, var(--rusty-deep) 55%, transparent); }
.rusty-pills a {
  padding: 6px 14px; border-radius: 999px; font-size: 0.75rem; text-decoration: none; color: inherit; opacity: 0.85;
  transition: background-color 150ms var(--rusty-ease), color 150ms var(--rusty-ease), opacity 150ms;
}
.rusty-pills a[aria-current], .rusty-pills a.is-active { background: #fff; color: #111; opacity: 1; }
.rusty-icons { display: flex; gap: 8px; }
.rusty-icons a, .rusty-icons button {
  width: 30px; height: 30px; border-radius: 999px; border: 0; display: grid; place-items: center;
  background: color-mix(in oklab, var(--rusty-deep) 55%, transparent); color: inherit; cursor: pointer;
}

/* Copy block */
.rusty-copy { grid-area: copy; align-self: start; display: flex; flex-direction: column; gap: 10px; max-width: 26ch; }
.rusty-kicker { font-size: 0.7rem; letter-spacing: 0.12em; text-transform: uppercase; opacity: 0.8; }
.rusty-title {
  margin: 0; font-family: var(--rusty-display); font-weight: 800; line-height: 0.95; letter-spacing: -0.02em;
  font-size: clamp(1.8rem, 3.4vw, 2.8rem);
}
.rusty-desc { margin: 0; font-size: 0.78rem; line-height: 1.45; opacity: 0.9; max-width: 30ch; }
.rusty-chips { display: flex; flex-wrap: wrap; gap: 6px; }
.rusty-chip {
  padding: 4px 10px; border-radius: 999px; font-size: 0.68rem; line-height: 1;
  border: 1px solid color-mix(in oklab, var(--rusty-ink) 45%, transparent); color: inherit; background: transparent;
}

/* Media: product, ghost word, orbit */
.rusty-media { grid-area: media; position: relative; display: grid; place-items: center; min-height: 260px; }
.rusty-ghost {
  position: absolute; inset: 0; display: grid; place-items: center; pointer-events: none; z-index: 0;
  font-family: var(--rusty-display); font-weight: 900; font-size: clamp(4rem, 14vw, 11rem); line-height: 1;
  letter-spacing: -0.04em; color: var(--rusty-ink); opacity: 0.14; white-space: nowrap; user-select: none;
}
.rusty-orbit {
  position: absolute; left: 6%; right: 6%; bottom: 12%; height: 22%; z-index: 1; pointer-events: none;
  border-radius: 50%; border: 1px solid color-mix(in oklab, var(--rusty-ink) 45%, transparent);
  border-top-color: transparent;
}
.rusty-product {
  position: relative; z-index: 2; max-width: 100%; max-height: 380px; width: auto;
  filter: drop-shadow(0 40px 40px color-mix(in oklab, var(--rusty-deep) 55%, transparent));
  transition: transform var(--rusty-dur-move) var(--rusty-ease), opacity var(--rusty-dur-move) var(--rusty-ease);
}
.rusty-product.is-out-left  { transform: translateX(-38%) scale(0.55); opacity: 0; }
.rusty-product.is-out-right { transform: translateX(38%) scale(0.55); opacity: 0; }
.rusty-product.is-in-left   { transform: translateX(-38%) scale(0.85); opacity: 0; transition: none; }
.rusty-product.is-in-right  { transform: translateX(38%) scale(0.85); opacity: 0; transition: none; }

/* Right side: variant chips and thumbnails */
.rusty-side { grid-area: side; display: flex; flex-direction: column; align-items: flex-end; gap: 14px; }
.rusty-variants { display: flex; flex-direction: column; gap: 6px; align-items: flex-end; }
.rusty-variants button {
  min-width: 36px; height: 30px; padding: 0 10px; border-radius: 999px; border: 0; cursor: pointer;
  font-size: 0.72rem; font-weight: 600; color: inherit;
  background: color-mix(in oklab, var(--rusty-deep) 45%, transparent); opacity: 0.7;
  transition: background-color 150ms var(--rusty-ease), color 150ms var(--rusty-ease), opacity 150ms;
}
.rusty-variants button[aria-pressed="true"] { background: #fff; color: #111; opacity: 1; }
.rusty-thumbs { display: flex; gap: 6px; }
.rusty-thumbs button {
  width: 44px; height: 44px; padding: 4px; border-radius: 12px; border: 2px solid transparent; cursor: pointer;
  background: color-mix(in oklab, var(--rusty-deep) 45%, transparent);
  transition: border-color 150ms var(--rusty-ease), transform 150ms var(--rusty-ease);
}
.rusty-thumbs button img { width: 100%; height: 100%; object-fit: contain; display: block; }
.rusty-thumbs button[aria-pressed="true"] { border-color: var(--rusty-ink); }
@media (hover: hover) and (pointer: fine) { .rusty-thumbs button:hover { transform: translateY(-2px); } }

/* Bottom left: price and arrows */
.rusty-price { grid-area: price; align-self: end; display: flex; flex-direction: column; gap: 10px; }
.rusty-amount { font-family: var(--rusty-display); font-weight: 700; font-size: 1.6rem; letter-spacing: -0.02em; }
.rusty-amount small { font-size: 0.7rem; font-weight: 500; opacity: 0.8; margin-left: 4px; }
.rusty-arrows { display: flex; gap: 8px; }
.rusty-arrows button {
  width: 34px; height: 34px; border-radius: 999px; cursor: pointer; display: grid; place-items: center;
  border: 1px solid color-mix(in oklab, var(--rusty-ink) 45%, transparent); background: transparent; color: inherit;
  transition: background-color 150ms var(--rusty-ease), color 150ms var(--rusty-ease), transform 120ms var(--rusty-ease);
}
.rusty-arrows button:active { transform: scale(0.94); }
@media (hover: hover) and (pointer: fine) { .rusty-arrows button:hover { background: #fff; color: #111; } }
.rusty-dots { display: flex; gap: 4px; }
.rusty-dots i { width: 18px; height: 2px; background: var(--rusty-ink); opacity: 0.35; border-radius: 2px; transition: opacity 150ms; }
.rusty-dots i.is-active { opacity: 1; }

/* Bottom right: stepper and CTA */
.rusty-buy { grid-area: buy; align-self: end; display: flex; flex-direction: column; align-items: flex-end; gap: 10px; }
.rusty-stepper { display: flex; align-items: center; gap: 10px; font-size: 0.8rem; }
.rusty-stepper button {
  width: 22px; height: 22px; border-radius: 6px; border: 1px solid color-mix(in oklab, var(--rusty-ink) 45%, transparent);
  background: transparent; color: inherit; cursor: pointer; display: grid; place-items: center; font-size: 0.8rem;
}
.rusty-cta {
  padding: 10px 18px; border-radius: 999px; border: 0; cursor: pointer; text-decoration: none;
  font-size: 0.78rem; font-weight: 700; background: var(--rusty-cta); color: var(--rusty-cta-ink);
  transition: transform 120ms var(--rusty-ease), filter 150ms var(--rusty-ease);
}
.rusty-cta:active { transform: scale(0.97); }
@media (hover: hover) and (pointer: fine) { .rusty-cta:hover { filter: brightness(1.08); } }

/* Text swap on switch */
.rusty-swap { transition: opacity 260ms var(--rusty-ease), transform 260ms var(--rusty-ease); }
.rusty-swap.is-leaving { opacity: 0; transform: translateY(6px); }

/* Phone */
@media (max-width: 720px) {
  .rusty-hero {
    grid-template-columns: 1fr 1fr;
    grid-template-areas:
      "nav nav"
      "media media"
      "copy copy"
      "side side"
      "price buy";
    padding: 18px 16px 20px; min-height: 0;
  }
  .rusty-pills { display: none; }
  .rusty-side, .rusty-variants { flex-direction: row; align-items: center; justify-content: space-between; }
  .rusty-product { max-height: 240px; }
  .rusty-title { font-size: 1.8rem; }
}

@media (prefers-reduced-motion: reduce) {
  [data-rusty], .rusty-hero, .rusty-product, .rusty-swap { transition: none !important; }
  .rusty-product.is-out-left, .rusty-product.is-out-right { transform: none; }
}

```

## File: assets/rusty.js

```js
/* rusty.js
   Product-colour hero switcher. Pairs with rusty.css.

   Markup contract (see demo.html):
     <section data-rusty="full" ...>            scope: receives the page colour
       <div class="rusty-hero" data-rusty-hero>  the card
         ... .rusty-swap elements with data-field="title|desc|amount|kicker|ghost"
         <img class="rusty-product" data-field="image">
         <div class="rusty-chips" data-field="chips">
         <div class="rusty-variants" data-field="variants">
         <div class="rusty-thumbs" data-thumbs>
         <button data-prev> <button data-next>
         <a class="rusty-cta" data-field="cta">
       </div>
       <script type="application/json" data-rusty-subjects>[ {...}, {...} ]</script>
     </section>

   Subject shape:
     { "id":"fanta", "colour":"#f28c28", "title":"Fanta taste", "kicker":"Guava flavour",
       "desc":"...", "image":"img/fanta.png", "thumb":"img/fanta.png",
       "chips":["Sugar free","Vegan"], "variants":["250","500","1000"], "variant":1,
       "amount":"$79.50", "amountNote":"per case", "ghost":"FANTA",
       "cta":{"label":"Add to cart","href":"#"} }

   Everything is optional except id, colour and title. Missing fields are hidden.
   Public API: RustyHero.init(scope?) returns { go(indexOrId), next(), prev(), current }.
*/
(function () {
  'use strict';

  function hexToHsl(hex) {
    var m = hex.replace('#', '');
    if (m.length === 3) m = m.split('').map(function (c) { return c + c; }).join('');
    var r = parseInt(m.slice(0, 2), 16) / 255, g = parseInt(m.slice(2, 4), 16) / 255, b = parseInt(m.slice(4, 6), 16) / 255;
    var max = Math.max(r, g, b), min = Math.min(r, g, b), h = 0, s = 0, l = (max + min) / 2;
    if (max !== min) {
      var d = max - min;
      s = l > 0.5 ? d / (2 - max - min) : d / (max + min);
      if (max === r) h = (g - b) / d + (g < b ? 6 : 0);
      else if (max === g) h = (b - r) / d + 2;
      else h = (r - g) / d + 4;
      h /= 6;
    }
    return { h: h * 360, s: s * 100, l: l * 100 };
  }
  function hsl(h, s, l) { return 'hsl(' + Math.round(h) + ' ' + Math.round(Math.max(0, Math.min(100, s))) + '% ' + Math.round(Math.max(0, Math.min(100, l))) + '%)'; }
  function luminance(hex) {
    var m = hex.replace('#', ''); if (m.length === 3) m = m.split('').map(function (c) { return c + c; }).join('');
    var c = [0, 2, 4].map(function (i) { var v = parseInt(m.slice(i, i + 2), 16) / 255; return v <= 0.03928 ? v / 12.92 : Math.pow((v + 0.055) / 1.055, 2.4); });
    return 0.2126 * c[0] + 0.7152 * c[1] + 0.0722 * c[2];
  }

  /* Rusty's palette from one colour: page is a lighter tint of the hue, card is the
     colour itself, deep is the card's edge, glow is the lighter centre behind the product. */
  function derive(colour) {
    var c = hexToHsl(colour);
    var dark = luminance(colour) < 0.3;
    return {
      page: hsl(c.h, c.s * 0.9, Math.min(80, c.l + 18)),
      card: colour,
      deep: hsl(c.h, c.s, c.l - 12),
      glow: hsl(c.h, c.s * 0.92, c.l + 14),
      ink: dark ? '#ffffff' : (c.l > 62 ? '#111111' : '#ffffff'),
      cta: dark ? '#ffffff' : '#0b0b0b',
      ctaInk: dark ? colour : '#ffffff'
    };
  }

  function paint(scope, hero, colour) {
    var p = derive(colour);
    scope.style.setProperty('--rusty-base', colour);
    scope.style.setProperty('--rusty-page', p.page);
    hero.style.setProperty('--rusty-card', p.card);
    hero.style.setProperty('--rusty-deep', p.deep);
    hero.style.setProperty('--rusty-glow', p.glow);
    hero.style.setProperty('--rusty-ink', p.ink);
    hero.style.setProperty('--rusty-cta', p.cta);
    hero.style.setProperty('--rusty-cta-ink', p.ctaInk);
  }

  function reduced() { return window.matchMedia && window.matchMedia('(prefers-reduced-motion: reduce)').matches; }

  function init(scope) {
    scope = scope || document.querySelector('[data-rusty]');
    if (!scope) return null;
    if (scope._rusty) return scope._rusty;      /* one instance per scope: a second init hands back the first */
    var hero = scope.querySelector('[data-rusty-hero]') || scope;
    var dataEl = scope.querySelector('[data-rusty-subjects]');
    if (!dataEl) return null;
    var subjects;
    try { subjects = JSON.parse(dataEl.textContent); } catch (e) { console.warn('rusty: bad subjects JSON', e); return null; }
    if (!subjects.length) return null;

    var fields = {};
    hero.querySelectorAll('[data-field]').forEach(function (el) { fields[el.getAttribute('data-field')] = el; });
    var thumbs = hero.querySelector('[data-thumbs]');
    var dots = hero.querySelector('[data-dots]');
    var prev = hero.querySelector('[data-prev]');
    var next = hero.querySelector('[data-next]');
    var index = Math.max(0, subjects.findIndex(function (s) { return s.id === scope.getAttribute('data-rusty-start'); }));
    var busy = false;

    function setText(name, value) {
      var el = fields[name]; if (!el) return;
      if (value == null || value === '') { el.hidden = true; return; }
      el.hidden = false; el.textContent = value;
    }
    function fillChips(name, list, pressedIndex, onPick) {
      var el = fields[name]; if (!el) return;
      el.innerHTML = '';
      if (!list || !list.length) { el.hidden = true; return; }
      el.hidden = false;
      list.forEach(function (label, i) {
        var b = document.createElement(onPick ? 'button' : 'span');
        b.className = onPick ? '' : 'rusty-chip';
        b.textContent = label;
        if (onPick) {
          b.type = 'button';
          b.setAttribute('aria-pressed', i === pressedIndex ? 'true' : 'false');
          b.addEventListener('click', function () {
            el.querySelectorAll('button').forEach(function (x) { x.setAttribute('aria-pressed', 'false'); });
            b.setAttribute('aria-pressed', 'true');
            onPick(i, label);
          });
        }
        el.appendChild(b);
      });
    }
    function render(s) {
      setText('title', s.title); setText('kicker', s.kicker); setText('desc', s.desc);
      setText('amount', s.amount); setText('ghost', s.ghost);
      if (fields.amountNote) { setText('amountNote', s.amountNote); }
      fillChips('chips', s.chips);
      fillChips('variants', s.variants, s.variant == null ? 0 : s.variant, function (i) {
        scope.dispatchEvent(new CustomEvent('rusty:variant', { detail: { subject: s, variant: s.variants[i], index: i } }));
      });
      if (fields.cta) {
        if (s.cta) { fields.cta.hidden = false; fields.cta.textContent = s.cta.label || 'Buy now'; if (s.cta.href) fields.cta.setAttribute('href', s.cta.href); }
        else fields.cta.hidden = true;
      }
      if (fields.image) {
        if (s.image) { fields.image.hidden = false; fields.image.src = s.image; fields.image.alt = s.imageAlt || s.title || ''; }
        else fields.image.hidden = true;
      }
      if (thumbs) thumbs.querySelectorAll('button').forEach(function (b, i) { b.setAttribute('aria-pressed', i === index ? 'true' : 'false'); });
      if (dots) dots.querySelectorAll('i').forEach(function (d, i) { d.classList.toggle('is-active', i === index); });
      scope.setAttribute('data-rusty-current', s.id);
    }

    function go(target, dir) {
      var to = typeof target === 'number' ? target : subjects.findIndex(function (s) { return s.id === target; });
      if (to < 0 || to >= subjects.length || to === index || busy) return;
      dir = dir || (to > index ? 1 : -1);
      var s = subjects[to];
      index = to;
      paint(scope, hero, s.colour);
      scope.dispatchEvent(new CustomEvent('rusty:change', { detail: { subject: s, index: to } }));

      var swaps = hero.querySelectorAll('.rusty-swap');
      var img = fields.image || hero.querySelector('.rusty-product'); /* a typographic mark carrying the product class slides like a product */
      if (reduced() || !img) { swaps.forEach(function (el) { el.classList.remove('is-leaving'); }); render(s); return; }

      busy = true;
      swaps.forEach(function (el) { el.classList.add('is-leaving'); });
      img.classList.add(dir > 0 ? 'is-out-left' : 'is-out-right');
      var done = false;
      function swapIn() {
        if (done) return; done = true;
        render(s);
        img.classList.remove('is-out-left', 'is-out-right');
        img.classList.add(dir > 0 ? 'is-in-right' : 'is-in-left');
        void img.offsetWidth;                       /* commit the start position before animating in */
        img.classList.remove('is-in-right', 'is-in-left');
        swaps.forEach(function (el) { el.classList.remove('is-leaving'); });
        busy = false;
      }
      img.addEventListener('transitionend', swapIn, { once: true });
      setTimeout(swapIn, 600);                      /* transitionend can be swallowed by a cached image */
    }

    if (thumbs) {
      thumbs.innerHTML = '';
      subjects.forEach(function (s, i) {
        var b = document.createElement('button'); b.type = 'button'; b.setAttribute('aria-label', s.title || s.id);
        if (s.thumb || s.image) { var im = document.createElement('img'); im.src = s.thumb || s.image; im.alt = ''; b.appendChild(im); }
        else { b.textContent = s.short || (s.title || '').slice(0, 3); b.style.color = 'inherit'; b.style.fontSize = '0.7rem'; }
        b.addEventListener('click', function () { go(i); });
        thumbs.appendChild(b);
      });
    }
    if (dots) { dots.innerHTML = ''; subjects.forEach(function () { dots.appendChild(document.createElement('i')); }); }
    if (prev) prev.addEventListener('click', function () { go((index - 1 + subjects.length) % subjects.length, -1); });
    if (next) next.addEventListener('click', function () { go((index + 1) % subjects.length, 1); });
    hero.addEventListener('keydown', function (e) {
      if (e.key === 'ArrowRight') { go((index + 1) % subjects.length, 1); }
      if (e.key === 'ArrowLeft') { go((index - 1 + subjects.length) % subjects.length, -1); }
    });

    /* Deep link: #rusty=<id> or ?rusty=<id> lands on that subject */
    var wanted = (location.hash.match(/rusty=([\w-]+)/) || location.search.match(/rusty=([\w-]+)/) || [])[1];
    if (wanted) { var w = subjects.findIndex(function (s) { return s.id === wanted; }); if (w >= 0) index = w; }

    paint(scope, hero, subjects[index].colour);
    render(subjects[index]);
    scope.classList.add('is-ready');
    var api = { go: go, next: function () { next && next.click(); }, prev: function () { prev && prev.click(); }, get current() { return subjects[index]; }, subjects: subjects };
    scope._rusty = api;
    return api;
  }

  window.RustyHero = { init: init, derive: derive };
  if (document.readyState === 'loading') document.addEventListener('DOMContentLoaded', function () { document.querySelectorAll('[data-rusty]').forEach(function (s) { init(s); }); });
  else document.querySelectorAll('[data-rusty]').forEach(function (s) { init(s); });
})();

```

## File: references/source-reels.md

# Source: @ui_ux_rusty_freelancer (Rusty Emerite)

Instagram, 64.7k followers, UI/UX and Framer/Webflow designer, Cameroon.
Thirteen reels studied 15 Sep 2026. Watch them on Instagram, not this file,
if a value below needs checking.

## What every showpiece has in common

Seven reels (Jan to Sep 2026) are the same move applied to a different
product: Fanta, Sprite and Coke cans; a Nike puffer jacket in three colours;
a milk bottle; a jam jar. Each opens on a grey wireframe ("Before") and
reveals the finished hero ("After").

1. **The page takes the product's colour.** Not an accent, the whole
   viewport. Fanta turns the page orange, Sprite turns it green, Coke turns
   it red. Switch product and the page recolours with it. This is the single
   most recognisable thing Rusty does and the reason the reels stop the scroll.
2. **One rounded hero card** (roughly 28px radius) sits on that page colour,
   in a deeper shade of the same hue, with a lighter radial glow behind the
   product. The card is the product's stage.
3. **Nav lives inside the card.** Logo top-left, a centred pill nav where the
   active item is a filled white pill, account and cart icons top-right.
4. **A fixed grid of content inside the card:**
   headline plus two-line description plus small tag chips top-left,
   product image large and centred (allowed to break the card edge),
   variant chips (250 / 500 / 1000, sizes 39 40 41) on the right,
   price and prev/next arrows bottom-left,
   quantity stepper plus one primary pill CTA bottom-right.
5. **Type is two faces:** a display face for the brand or headline (a
   Fraunces-like serif for "Fresh", a rounded fat sans for "Milk"), a
   neutral sans for everything else, body copy tiny at 10 to 12px. Product
   name in condensed all-caps.
6. **Depth cues, all cheap:** a soft drop shadow under the product, a thin
   elliptical ring under it like an orbit, the brand mark or a huge word
   ghosted behind the product at around 15% opacity (the Nike swoosh, the
   word "PLAN MILK").
7. **Switch motion:** the outgoing product shrinks and slides to a corner
   thumbnail, the incoming one slides in from the opposite side, the colour
   crossfades over about half a second, text fades up. Never a hard cut.

## What is NOT part of it

- The 2024 reels are intro and hire-me pieces, no pattern to lift.
- Two of the 2026 reels are sponsored (Runable AI). Ignored.
- Rusty builds in Figma, Framer and Rive. Nothing here needs any of them;
  the whole move is CSS custom properties, a gradient and one transition.

## Why it works, so the port keeps the point

The colour takeover makes the product feel like it owns the page, and the
switch makes the visitor want to click through every variant just to watch
it happen. That is the effect worth keeping. The chips, the orbit ring and
the ghosted word are garnish; ship them when there is a real product image
to sit on, leave them when there is not.


## File: scripts/shots.mjs

```js
// shots.mjs: screenshot every subject of a rusty-hero page, desktop and phone,
// so the "clicked every subject" line of Done means has evidence.
//
//   node ~/.claude/skills/rusty-hero/scripts/shots.mjs <url or file path> <out dir>
//
// Uses playwright-core with the Chromium already on this PC (no download).
// Writes <out>/desktop-<id>.png for every subject, <out>/phone-<id>.png for the
// first and last, and <out>/reduced-motion.png. Prints the subject list it found.

import { chromium } from 'playwright-core';
import { mkdirSync, existsSync } from 'node:fs';
import { resolve } from 'node:path';
import { pathToFileURL } from 'node:url';

const [,, target, outArg] = process.argv;
if (!target) { console.error('usage: shots.mjs <url|file> [outdir]'); process.exit(1); }
const out = resolve(outArg || 'rusty-shots');
mkdirSync(out, { recursive: true });
const url = /^https?:/.test(target) ? target : pathToFileURL(resolve(target)).href;

const candidates = [
  process.env.RUSTY_CHROME,
  `${process.env.LOCALAPPDATA}\\ms-playwright\\chromium-1234\\chrome-win64\\chrome.exe`,
  `${process.env.LOCALAPPDATA}\\ms-playwright\\chromium-1194\\chrome-win64\\chrome.exe`,
  'C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe',
].filter(Boolean);
const executablePath = candidates.find(p => existsSync(p));
if (!executablePath) { console.error('no chromium found; set RUSTY_CHROME'); process.exit(1); }

const browser = await chromium.launch({ executablePath, headless: true });
async function run(viewport, prefix, reduced = false) {
  const ctx = await browser.newContext({ viewport, reducedMotion: reduced ? 'reduce' : 'no-preference', deviceScaleFactor: 1 });
  const page = await ctx.newPage();
  const errors = [];
  page.on('pageerror', e => errors.push(String(e)));
  page.on('console', m => { if (m.type() === 'error') errors.push(m.text()); });
  await page.goto(url, { waitUntil: 'load' });
  await page.waitForTimeout(400);
  const subjects = await page.evaluate(() => {
    const s = document.querySelector('[data-rusty-subjects]');
    return s ? JSON.parse(s.textContent).map(x => ({ id: x.id, colour: x.colour, title: x.title })) : [];
  });
  if (!subjects.length) { console.log(`${prefix}: no [data-rusty-subjects] found`); await ctx.close(); return { subjects, errors }; }
  const pick = prefix === 'phone' ? [subjects[0], subjects[subjects.length - 1]] : subjects;
  for (const s of pick) {
    await page.evaluate(id => {
      const scope = document.querySelector('[data-rusty]');
      const api = window.__rusty || (window.__rusty = window.RustyHero.init(scope));
      api.go(id);
    }, s.id);
    await page.waitForTimeout(reduced ? 150 : 900);
    const bg = await page.evaluate(() => getComputedStyle(document.querySelector('[data-rusty]')).backgroundColor);
    await page.screenshot({ path: `${out}/${prefix}-${s.id}.png`, fullPage: false });
    console.log(`${prefix}-${s.id}.png  scope bg ${bg}  (${s.colour} ${s.title})`);
  }
  await ctx.close();
  return { subjects, errors };
}

const d = await run({ width: 1280, height: 800 }, 'desktop');
const p = await run({ width: 390, height: 844 }, 'phone');
const r = await run({ width: 1280, height: 800 }, 'reduced', true);
await browser.close();
const errors = [...d.errors, ...p.errors, ...r.errors];
console.log(`\n${d.subjects.length} subjects, shots in ${out}`);
if (errors.length) { console.log('page errors:'); errors.forEach(e => console.log('  ' + e)); process.exit(2); }

```
