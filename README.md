# subtle-motion

An agent skill for adding **subtle, jank-free animations** to any website (plain HTML/CSS, React, Next, Vue, Svelte, Astro). The goal is a site that feels smooth without looking "animated", and that stays responsive at 60fps on low-end phones.

It turns the 12 Disney animation principles and the motion-relevant Laws of UX (Fitts, Hick, Miller, Doherty, Postel, Jakob, frequency of use) into concrete rules and CSS-first patterns:

- press squash and 44px hit areas, toggle stretch, send anticipation
- card staging and lift, a 3D tilt capped at 6°, fly-to-cart arcs
- error shake, success ring, pending states that respect the 400ms Doherty threshold
- scroll reveals that never hide content without JS, chunked stagger
- native `<dialog>`/`popover` overlays, folder tabs, accordions via `interpolate-size`, directional steps, toast stacks
- hard rules: animate only `transform` and `opacity`, keep UI motion under 300ms, respect `prefers-reduced-motion`, hover only on `(hover: hover)`, CSS-first (anime.js only if you say yes, in framework projects)
- a required verification pass: reduced motion, 4x CPU throttle, slow-mo, touch, no-JS, Lighthouse

## Install

```bash
npx skills add noni-was-taken/subtle-motion
```

Or copy `skills/subtle-motion/` into `~/.claude/skills/` (Claude Code) or your agent's skills folder.

## Use

```
/subtle-motion
```

Or ask: "add subtle animations", "make the site feel smoother", "polish the motion".

## Demo

`index.html` is a single-file lab with one tile per pattern. Open it in a browser, and use **Motion off** to compare before and after.

## Credits

Inspired by Charles Liu's reel "20 Principles of UX & Animation", Disney's *The Illusion of Life*, Jon Yablonski's *Laws of UX*, Raphael Salaja's userinterface.wiki, and Emil Kowalski's work on web animation.

## License

MIT
