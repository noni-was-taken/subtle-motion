---
name: subtle-motion
description: Add subtle, polished animations to any website (plain HTML/CSS, React, Next, Vue, Svelte, Astro) so it feels better without feeling "animated", while staying responsive and jank-free at 60fps on low-end phones. Applies the 12 Disney animation principles adapted for UI plus the motion-relevant Laws of UX (Fitts, Hick, Miller, Doherty, Postel, Jakob, frequency of use). Leans toward more motion rather than less. Use when the user types /subtle-motion, or asks to "make the site feel nicer/smoother", "add subtle animations", "add micro-interactions", "polish the motion", "add scroll reveals", "add hover effects", or "make it feel less static". Prefer this over `animate` unless the user wants Framer Motion / React-specific patterns. Uses anime.js (after asking) for JS-driven motion in framework projects.
---

# Subtle Motion

Motion is seasoning. The user should feel the site is "smooth", never notice "an animation".

**Lean toward more motion, not less.** Missing motion makes a site feel flat and nobody asks for it; extra motion is easy to tone down later. When unsure whether something should animate, animate it, keeping it small and fast. The hard rules below are about *how* things move (performance, accessibility, timing), not about how many things move.
Sources: Disney's *The Illusion of Life*, Jon Yablonski's *Laws of UX*, Raphael Salaja's userinterface.wiki, Emil Kowalski (frequency of use).

## Hard rules (never break)

1. **Animate only `transform` and `opacity`** (plus `filter`/`clip-path` sparingly). Never animate `width`, `height`, `top`, `left`, `margin`, `padding`, `box-shadow` size, or `font-size`. Need a shadow change? Fade a pseudo-element's `opacity`.
2. **Snappy timing: UI motion under 300ms.** Press 80–120ms, hover 150ms, enter 200–300ms, exit ≈ 70% of enter. Only a one-time hero may go to 500ms.
3. **Small distances.** Translate 2–16px, scale 0.97–1.03, rotate ≤ 6°. Bigger reads as "animated".
4. **Slow in & slow out.** Never `linear` on movement. Ease-out to enter, ease-in to exit.
5. **Frequency of use sets the size, not whether it moves.** Frequent clicks (filters, sort, tabs, segmented controls, list selection) still get motion, just shorter and smaller: a ≤150ms slide or a ≤200ms fade of the result. Only keyboard-repeat actions (arrow keys through a list or palette, typing in search) stay instant, because they fire many times a second.
6. **Doherty threshold: respond within 100ms, finish within 400ms.** Feedback (press state, pending state) starts on the same frame as input. Anything slower than 400ms shows progress.
7. **`prefers-reduced-motion: reduce` kills movement.** Opacity fades may stay.
8. **Hover effects only under `@media (hover: hover)`.** Touch gets sticky hovers otherwise.
9. **CSS first.** The only library this skill adds is anime.js, and only after the user says yes (see "anime.js"). A project that already has a motion library keeps using it. No JS loops when CSS can do it.
10. **Never delay or hide content.** Reveals must not hide content if JS fails, block clicks, or animate the LCP element on load.
11. **Staging: one focal motion at a time.** Cover as many touchpoints as the page has; just never two things competing for the eye in the same moment.
12. **`will-change` only during the animation** (or on ≤3 persistent elements).
13. **Interruptible by default.** State that can flip mid-flight (hover, toggle, open/close, tab indicator) uses `transition`, which retargets from wherever it is. `@keyframes` only for one-shot moments (shake, ring, send) that restart cleanly.
14. **Scale duration to size and initiator.** Small elements faster than big ones (icon 100ms, card 250ms, sheet 300ms). User-initiated = immediate; system-initiated (toast, background update) may take the slow end.
15. **No flashing more than 3×/s; anything looping > 5s needs a pause control** (or don't loop).

## The principles, as UI rules

Pick the ones that fit the touchpoint; never all at once. Each maps to a pattern below.

| # | Principle | In UI it means | Pattern |
|---|-----------|----------------|---------|
| 1 | Squash & stretch | Things squish on impact, stretch in motion: weight | Press, Toggle |
| 2 | Anticipation | Tiny pull-back before a big move (send, submit, fly-out) | Send button |
| 3 | Staging | Direct the eye to one thing: dim siblings, one motion at a time | Card focus |
| 4 | Pose to pose | Define start/end states; let the browser tween | `transition`, not frame-by-frame JS |
| 5 | Follow through | Parts settle after the body stops (small overshoot) | Toast, Badge |
| 6 | Slow in & slow out | Accelerate and decelerate, never constant speed | Tokens |
| 7 | Arcs | Travelling elements move on curves, not straight lines | Fly-to-cart |
| 8 | Secondary action | Small flourish that backs the main action | Success ring |
| 9 | Snappy timing | Duration sets the feel; UI < 300ms | Tokens |
| 10 | Exaggerated feedback | Push past real so the moment lands (errors, success) | Error shake |
| 11 | Solid drawing / 3D | Give flat things depth and volume | Tilt card |
| 12 | Appeal | The sum of every detail, consistent across the site | Verify pass |

| UX law | Motion consequence | Pattern |
|--------|--------------------|---------|
| Fitts's law | Bigger, closer targets are faster to hit | Hit areas |
| Bigger hit areas | Clickable area ≥ 44×44px, even if the visual is smaller | Hit areas |
| Hick's law | More choices = slower decisions; don't animate many options at once | Stagger cap |
| Miller's law | Chunk content (~7 items); reveal in groups, not item by item | Stagger cap |
| Doherty threshold | <100ms feedback, <400ms total, else show progress | Pending state |
| Postel's law | Accept messy input, return clean output; animate the clean-up gently | Input format |
| Jakob's law | Use conventional motion (menus drop from trigger, modals fade+scale from center) | Overlay |
| Frequency of use | Frequent = less animation | Rule 5 |

## Workflow

1. **Read the site first.** Framework, CSS approach (Tailwind, CSS modules, plain), existing motion and tokens. Reuse them. If it is a framework project with no motion library, ask once whether to add anime.js (see "anime.js").
2. **Classify each interactive element by frequency**: rare (signup, checkout, success), occasional (cards, sections, modals), frequent (nav, tabs, filters, sort, list rows), keyboard-repeat (arrow keys, typing). Rare gets the richest motion, frequent gets short slides and fades, keyboard-repeat gets none.
3. **Cover every touchpoint the page has**, starting with these (ranked by feel-per-cost), then everything else that changes state on a click:
   1. Buttons/links: press squash + hit area (principles 1, Fitts)
   2. Form feedback: error shake, success ring, pending state (10, 8, Doherty)
   3. Cards: hover lift, optional tilt (11)
   4. Sections: scroll reveal, chunked stagger (6, Miller)
   5. Overlays/toasts: enter/exit with follow-through (5, Jakob)
   6. Filters, sort and tabs: sliding selection + results fade in (rule 5)
   7. Anything that appears or disappears on click (undo rows, confirmations, empty states): fade or rise in
4. **Add the tokens** once, globally.
5. **Apply patterns** to every touchpoint from step 3. Leaving one static should be a decision with a reason (keyboard-repeat, LCP), not the default.
6. **Verify** (last section). Not done until verified.
7. **Report**: each touchpoint with file:line, the principle it applies, and how to see it.

## Tokens (add once, globally)

```css
:root {
  --ease-out: cubic-bezier(.23, 1, .32, 1);       /* enter, hover */
  --ease-in: cubic-bezier(.55, 0, 1, .45);        /* exit */
  --ease-in-out: cubic-bezier(.65, 0, .35, 1);    /* move on screen */
  --ease-settle: cubic-bezier(.34, 1.4, .64, 1);  /* follow-through: ~4% overshoot */
  /* spring feel, interruptible; browsers without linear() fall back to `ease` */
  --ease-spring: linear(0, .07 3%, .29 9%, .62 18%, .86 28%, 1.01 40%, 1.03 50%, 1.015 65%, 1);
  --dur-press: 100ms;
  --dur-fast: 150ms;
  --dur-base: 250ms;
  --dur-slow: 300ms;
  --rise: 8px;
}
@media (prefers-reduced-motion: reduce) {
  :root { --rise: 0px; }
  *, *::before, *::after {
    animation-duration: 1ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 1ms !important;
    scroll-behavior: auto !important;
  }
}
```

Tailwind project? Put these in `@theme` / `tailwind.config` `extend` instead of a new CSS file.

## Patterns

### Press: squash (1) + hit area (Fitts)

```css
.btn {
  position: relative;
  transition: transform var(--dur-fast) var(--ease-out), background-color var(--dur-fast) var(--ease-out);
}
@media (hover: hover) { .btn:hover { transform: translateY(-1px); } }
.btn:active { transform: scale(.97); transition-duration: var(--dur-press); }

/* Small icon buttons: grow the hit area, not the visual */
.icon-btn { position: relative; }
.icon-btn::before { content: ""; position: absolute; inset: 50% auto auto 50%; width: 44px; height: 44px; translate: -50% -50%; }
```

### Toggle: squash & stretch (1)

The knob stretches while travelling, settles when it lands.

```css
.switch .knob { transition: transform var(--dur-base) var(--ease-settle); }
.switch[aria-checked="true"] .knob { transform: translateX(20px); }
.switch:active .knob { transform: scaleX(1.2); transform-origin: left; }
.switch[aria-checked="true"]:active .knob { transform: translateX(20px) scaleX(1.2); transform-origin: right; }
```

### Send / submit: anticipation (2)

Only on rare "commit" actions. Pull back 2px, then go.

```css
@keyframes send { 0% { transform: none } 30% { transform: translate(-2px, 2px) } 100% { transform: translate(24px, -24px); opacity: 0 } }
.send.is-sending svg { animation: send 280ms var(--ease-in-out) forwards; }
```

### Card focus: staging (3)

```css
@media (hover: hover) {
  .grid:has(.card:hover) .card:not(:hover) { opacity: .6; }
}
.grid .card { transition: opacity var(--dur-base) var(--ease-out); }
```

Use only on dense grids where one choice matters. Skip on content people read.

### Card lift + optional tilt: solid drawing (11)

```css
.card { position: relative; transition: transform var(--dur-base) var(--ease-out); }
.card::after {
  content: ""; position: absolute; inset: 0; border-radius: inherit; pointer-events: none;
  box-shadow: 0 12px 32px -12px rgb(0 0 0 / .25);
  opacity: 0; transition: opacity var(--dur-base) var(--ease-out);
}
@media (hover: hover) {
  .card:hover { transform: translateY(-3px); }
  .card:hover::after { opacity: 1; }
}
```

Tilt (only for showcase items like a product, pricing or credit card; ≤6°, pointer only, rAF-throttled):

```js
if (matchMedia('(hover: hover) and (prefers-reduced-motion: no-preference)').matches) {
  for (const el of document.querySelectorAll('.tilt')) {
    let raf = 0;
    el.addEventListener('pointermove', (e) => {
      cancelAnimationFrame(raf);
      raf = requestAnimationFrame(() => {
        const r = el.getBoundingClientRect(); // read once per frame, not per transition
        const x = (e.clientX - r.left) / r.width - .5, y = (e.clientY - r.top) / r.height - .5;
        el.style.transform = `perspective(800px) rotateX(${-y * 6}deg) rotateY(${x * 6}deg)`;
      });
    });
    el.addEventListener('pointerleave', () => { cancelAnimationFrame(raf); el.style.transform = ''; });
  }
}
```

### Toast / badge: follow through (5)

```css
.toast { transition: transform var(--dur-slow) var(--ease-settle), opacity var(--dur-fast) var(--ease-out); }
.toast[data-state="closed"] { transform: translateY(12px); opacity: 0; transition-timing-function: var(--ease-in); }
```

Overshoot only on things that arrive (toasts, badges, counters), never on layout or text blocks.

### Fly-to-cart: arcs (7)

Split X and Y across two nested elements with different easings; the path curves.

```css
.fly-x { transition: transform 300ms linear; }
.fly-y { transition: transform 300ms var(--ease-in); }
.flying .fly-x { transform: translateX(var(--dx)); }
.flying .fly-y { transform: translateY(var(--dy)) scale(.4); }
```

Set `--dx/--dy` from one `getBoundingClientRect()` read before adding `.flying`.

### Success ring: secondary action (8)

```css
.btn.is-success::after {
  content: ""; position: absolute; inset: -4px; border-radius: inherit; border: 2px solid currentColor;
  animation: ring 400ms var(--ease-out) forwards; pointer-events: none;
}
@keyframes ring { from { opacity: .6; transform: scale(.95) } to { opacity: 0; transform: scale(1.15) } }
```

### Error shake: exaggerated feedback (10)

```css
@keyframes shake { 20%,60% { transform: translateX(-4px) } 40%,80% { transform: translateX(4px) } }
.field[aria-invalid="true"] { animation: shake 280ms var(--ease-in-out); }
```

Re-trigger by removing and re-adding the attribute. Always pair with a text message (accessibility).

### Pending state: Doherty threshold

Press feedback immediately; if the action takes >400ms, show a spinner/progress inside the button (keep its width fixed to avoid layout shift). Prefer optimistic UI: show the result, roll back on error.

### Messy input → clean output: Postel's law

Format on `blur` (phone numbers, card numbers, dates), not on every keystroke. Crossfade the value with a 150ms opacity change, no movement.

### Scroll reveal: slow in/out (6) + chunking (Miller, Hick)

Content visible by default; only hidden once JS confirms it can reveal it.

```css
.js .reveal {
  opacity: 0; transform: translateY(var(--rise));
  transition: opacity var(--dur-slow) var(--ease-out), transform var(--dur-slow) var(--ease-out);
  transition-delay: calc(min(var(--i, 0), 5) * 50ms);
}
.js .reveal.in { opacity: 1; transform: none; }
```

```js
document.documentElement.classList.add('js');
const io = new IntersectionObserver((entries) => {
  for (const e of entries) if (e.isIntersecting) { e.target.classList.add('in'); io.unobserve(e.target); }
}, { rootMargin: '0px 0px -10% 0px' });
document.querySelectorAll('.reveal').forEach((el) => io.observe(el));
```

- Stagger with `style="--i: n"`; capped at 6 steps (Miller). Long lists reveal as one group.
- Reveal once. Skip anything above the fold.
- Frameworks: same CSS; run the observer in a mount effect, disconnect on unmount.

### Overlays: Jakob's law

Conventional motion only: dropdowns grow from their trigger (`transform-origin` on the trigger side), modals fade + scale 0.98→1 from center, sheets slide from their edge. Native first: `<dialog>` / `popover` with `@starting-style`.

```css
dialog, [popover] {
  opacity: 0; transform: translateY(4px) scale(.98);
  transition: opacity var(--dur-fast) var(--ease-in), transform var(--dur-fast) var(--ease-in),
              overlay var(--dur-fast) allow-discrete, display var(--dur-fast) allow-discrete;
}
dialog[open], [popover]:popover-open {
  opacity: 1; transform: none;
  transition-timing-function: var(--ease-out); transition-duration: var(--dur-base);
}
@starting-style {
  dialog[open], [popover]:popover-open { opacity: 0; transform: translateY(4px) scale(.98); }
}
```

Command palette / menu opened by keyboard many times a day: keep the open to a ≤100ms fade; arrow-key movement inside it stays instant.

### Tabs: folder surface that slides (not an accent underline)

The AI/template tell is the **thin accent underline**, not the motion. Use a familiar physical shape (Jakob) and let that shape do the moving:

- **Folder tabs** (preferred): the selected tab's surface merges into the panel and slides to the clicked tab.
- **Segmented control**: for 2–4 options; the pressed-in segment slides the same way.

Make tabs equal width so the surface only needs `translateX` (no width or `scaleX`, which distorts radius and border). Use a ≤150ms transition so it stays interruptible. The panel content crossfades in ≤150ms (opacity only).

```css
.tabs { position: relative; display: flex; gap: 2px; border-bottom: 1px solid var(--line); }
.tabs [role=tab] { position: relative; z-index: 1; flex: 0 0 96px; background: none; border: 0;
  transition: color var(--dur-fast) var(--ease-out), transform var(--dur-press) var(--ease-out); }
.tabs [role=tab]:active { transform: translateY(1px); }
.tabs .folder { position: absolute; left: 0; top: 0; bottom: -1px; width: 96px; background: var(--panel-bg);
  border: 1px solid var(--line); border-bottom: 0; border-radius: var(--r-ctl) var(--r-ctl) 0 0;
  transition: transform var(--dur-fast) var(--ease-out); }
```

```js
const move = (tab) => folder.style.transform = `translateX(${tab.offsetLeft}px)`; // one read per click
```

Tab labels too uneven for equal widths? Let the surface jump without animating, and keep only the 1px press. Use View Transitions with a shared `view-transition-name` only for page-level swaps; they can't be interrupted.

### Sliding selection: filter chips and segmented controls

Same idea as folder tabs, for a row of filter chips or a sort toggle: one ink surface slides under the selected option, and the chips stay transparent. When the surface has no radius or border, `scaleX` on a 1px-wide element is safe, so the chips can have uneven widths and can wrap to two rows.

```css
.choices { position: relative; display: inline-flex; flex-wrap: wrap; isolation: isolate; }
.choices .chip { position: relative; z-index: 1; background: transparent; }
.choices .chip[aria-pressed="true"] { color: var(--on-ink); }
.slider { position: absolute; left: 0; top: 0; width: 1px; background: var(--ink); transform-origin: 0 0; pointer-events: none; }
.slider.ready { transition: transform var(--dur-fast) var(--ease-out); }   /* added after first paint: no slide on load */
```

```js
const slide = (group) => {
  const on = group.querySelector('[aria-pressed="true"]'), s = group.querySelector('.slider');
  s.style.height = on.offsetHeight + 'px';
  s.style.transform = `translate(${on.offsetLeft}px, ${on.offsetTop}px) scaleX(${on.offsetWidth})`;
};
new ResizeObserver(() => slide(group)).observe(group);   // re-place when chips wrap
```

### Results refresh: filter and sort

When a click changes a list (filter, sort, category), the new items fade in with a short rise and a capped stagger. Not on first load (LCP) and not on each search keystroke (keyboard-repeat).

```css
@keyframes item-in { from { opacity: 0; transform: translateY(6px); } }
.item.enter { animation: item-in 200ms var(--ease-out) both; animation-delay: calc(min(var(--i, 0), 6) * 30ms); }
```

```js
// after re-rendering the list because of a click:
[...list.children].forEach((el, i) => { el.style.setProperty('--i', i); el.classList.add('enter'); });
```

### Appear on click: undo rows, confirmations, empty states

Anything that shows up because of a click fades in (150ms), or fades and rises 8px if it's a rare moment like "order placed".

```css
@keyframes fade-in { from { opacity: 0; } }
@keyframes rise-in { from { opacity: 0; transform: translateY(8px); } }
.undo-row { animation: fade-in var(--dur-fast) var(--ease-out); }
.confirmation:not([hidden]) { animation: rise-in var(--dur-base) var(--ease-out); }
```

### Accordion / height: the one layout exception

Height can't be done with transform. Use native `<details>` and let the browser interpolate to `auto`; keep it to small subtrees.

```css
:root { interpolate-size: allow-keywords; }
details::details-content { block-size: 0; overflow: clip; opacity: 0;
  transition: block-size var(--dur-base) var(--ease-out), opacity var(--dur-base) var(--ease-out), content-visibility var(--dur-base) allow-discrete; }
details[open]::details-content { block-size: auto; opacity: 1; }
```

Unsupported browsers just open instantly. Never animate height on large lists or anything that reflows the page under the cursor.

### Multi-step flow: directional

Steps slide in from the direction of travel (forward = from the right, back = from the left), 12px, with a crossfade. Spatial consistency tells the user where they are.

```css
.step { transition: opacity var(--dur-base) var(--ease-out), transform var(--dur-base) var(--ease-out); }
.step[data-from="next"] { opacity: 0; transform: translateX(12px); }
.step[data-from="prev"] { opacity: 0; transform: translateX(-12px); }
```

### Toast stack: system-initiated

Stack with an index variable; older toasts scale down and tuck behind. Pure transform.

```css
.toast { --i: 0; transform: translateY(calc(var(--i) * -10px)) scale(calc(1 - var(--i) * .05));
  transition: transform var(--dur-slow) var(--ease-spring), opacity var(--dur-fast) var(--ease-out); }
.toasts .toast:nth-last-child(n+4) { opacity: 0; } /* max 3 visible (Miller); set --i = distance from newest */
```

### Focus

```css
:focus-visible { outline: 2px solid currentColor; outline-offset: 2px; transition: outline-offset var(--dur-fast) var(--ease-out); }
```

### Page transitions (only if asked)

View Transitions API (`@view-transition { navigation: auto; }` for MPAs, `document.startViewTransition` for SPAs). Default crossfade, ≤200ms, feature-detect.

## anime.js (only after the user says yes)

CSS stays the default. anime.js is the library this skill uses for the moments where JS-driven motion is nicer than CSS: staggered results, fly-to-cart arcs, real springs, timelines (one thing finishes, the next starts), and counting numbers.

**Ask first.** In workflow step 1, if the project uses a framework or bundler where an npm package can be added (React, Next, Vue, Nuxt, Svelte, SvelteKit, Astro, Angular, Solid, Vite...), ask the user once with AskUserQuestion:

- **Keep CSS-only (recommended for small sites)**: no new dependency; every pattern in this file works.
- **Add anime.js**: `npm i animejs`; the JS-driven moments above use it, everything else stays CSS.

Skip the question when the project already has anime.js (use it) or has another motion library already installed (GSAP, Framer Motion/Motion: use that one instead of adding a second). Plain HTML with no build step: don't ask; stay CSS-only. Never install without a yes.

Map the tokens so CSS and JS motion feel like one system (checked against anime.js 4.5; see animejs.com/documentation for other versions):

```js
import { animate, stagger, spring, createTimeline } from 'animejs';

const reduce = matchMedia('(prefers-reduced-motion: reduce)').matches;
const dur = (ms) => (reduce ? 0 : ms);                                 // rule 7
// spring({ bounce, duration }): duration is the perceived length, so it obeys the <300ms rule.
// (createSpring() is deprecated in 4.5; stiffness/damping springs settle in ~700ms+, too slow for UI.)
const snappy = spring({ bounce: .1, duration: 250 });                  // UI chrome, interruptible
const gentle = spring({ bounce: .15, duration: 300 });                 // larger surfaces

// Results refresh (filters/sort): same values as the CSS pattern
animate('.item', { opacity: { from: 0 }, y: { from: 6 }, duration: dur(200), ease: 'outQuint', delay: stagger(30) });

// Count up a total when it changes
animate(totalObj, { value: newTotal, duration: dur(300), ease: 'outQuint', onUpdate: () => (el.textContent = money(totalObj.value)) });
```

Same rules as CSS: transform and opacity only (`x`, `y`, `scale`, `rotate`, `opacity`), small distances, under 300ms, `bounce` ≤ .15 on UI chrome. In frameworks, start animations in a mount/effect hook and `revert()` them on unmount.

## Performance guardrails

- 50+ animated items? `content-visibility: auto` on sections; reveal containers, not items.
- Scroll-linked effects: CSS `animation-timeline: view()/scroll()` inside `@supports`. Never `scroll` listeners that write styles. Parallax is rarely subtle; ask first.
- Never read layout inside an animation path (only once before it starts, as in fly-to-cart).
- No `backdrop-filter: blur()` on animated elements.

## Verify (required)

1. **Reduced motion:** DevTools > Rendering > emulate `prefers-reduced-motion: reduce`. Nothing moves.
2. **Jank:** DevTools > Performance, 4x CPU throttle, scroll and hover the touchpoints. No "Layout" during hover/reveal, no long frames. Rendering > Paint flashing: only the animated element should flash.
2b. **Slow-mo:** DevTools > Animations panel at 10% speed. Check each curve, overshoot and stagger frame by frame; interrupt hover/toggle mid-flight and confirm it reverses smoothly.
3. **Doherty:** every click shows feedback in the same frame; nothing UI-level runs past 300ms.
4. **Touch:** device toolbar on, tap buttons/cards. No stuck hover; hit areas ≥ 44px. Also grep: every `:hover` rule sits inside `@media (hover: hover)` (redesigns often move them out).
5. **No JS:** disable JS, reload. All content visible.
6. **Lighthouse mobile:** CLS ≈ 0, LCP not worse than before.
7. **Appeal pass (12):** same easing and duration family everywhere; keyboard-repeat actions stay instant; and look for anything that changes state on a click with no motion at all. That's the more common miss.

If `agent-browser` is available, run 1, 4 and 5 with it and screenshot. Otherwise list the steps for the user.

## Anti-patterns (remove if found)

- `transition: all`, or animations on every element
- Whole-page fade-in on load
- Bounce/elastic on buttons or text, typing effects, infinite pulses on CTAs
- Animated keyboard-repeat actions (arrow-key movement through a list or palette, results re-animating on every search keystroke)
- Hover-only affordances with no touch equivalent
- A colored bar down the left edge of the selected/active row (palette, sidebar, list). Use a plain background fill and ink-colored text instead.
- A thin accent underline gliding under tabs. Slide a folder or segmented surface instead.
- AOS/WOW/GSAP added just for fade-ups
