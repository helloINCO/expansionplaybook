# The Expansion Playbook — landing page

Lead-magnet landing page for INCO Studio. Single self-contained HTML file
plus local media. No build step, no package manager, no framework.

## Structure

```
index.html            the entire page: markup, CSS and JS in one file
media/                photography and portrait
media/logos/          client marks, pre-processed (see below)
.nojekyll             stops GitHub Pages running the files through Jekyll
```

## Running it

Open `index.html` in a browser, or serve the folder:

```
python3 -m http.server 8000
```

## Deploying on GitHub Pages

Push the contents of this folder to the repo root (not nested in a
subfolder), then Settings → Pages → Source: `main` / `/ (root)`.
`index.html` at the root is what Pages serves.

If you'd rather keep it in a `docs/` folder, move everything there and
pick `/docs` as the Pages source instead. Either works — what breaks is
putting `index.html` in a subfolder and pointing Pages at the root.

## Before this goes public

Four things are still placeholders. None of them stop the page working,
but all of them would be visible to a real visitor:

1. **Contents card images** — the four cards in "What's in the set" pull
   from `picsum.photos`, a random-image service. These need real INCO
   project photography. They are the only remaining external image
   dependency; if picsum is ever down, the cards render empty.
2. **CTA links** — three buttons are marked `(demo link)` and call
   `return false`. Search `demo link` in `index.html`. They need real
   booking URLs.
3. **CRM webhook** — the form currently logs its payload to the console
   and fakes a success after 450ms. Search for `CRM INTEGRATION POINT`
   in `index.html`; the `fetch()` is written out and commented, and it is
   the only function that changes depending on which CRM you use. The
   payload carries the quiz segment and the visitor's weakest dimension,
   so automation can branch on them.
4. **Hero still** — `media/hero-still-1600.jpg` is a 693px web export
   upscaled to 1600px. It holds up at small sizes but softens on a large
   display. Re-export from the original if you have it.

Client permission for the La Viet imagery on the "It rarely fails loudly"
sheet has not been confirmed. That sheet sits under a failure headline,
so it's worth clearing before the page is public.

## Client logos

`media/logos/*.png` are not the raw supplied files. Each was trimmed to
its ink bounds, flattened to a single slate ink (`#1c1c1a`) on a
transparent 400×140 canvas, and scaled so every mark carries roughly the
same *ink area* — otherwise a one-line wordmark and a three-line stacked
lockup read at completely different weights on the same row.

The padding is baked into each asset, so the CSS is just a box with no
per-logo exceptions. **Consequence:** adding or removing a logo shifts
the set's median ink area, so the whole set should be regenerated
together rather than dropping a single new file in at a guessed size.

## Dependencies

GSAP 3.15 and ScrollTrigger, loaded from jsDelivr. The page degrades
gracefully if they fail — scroll animation stops, content stays readable.
Everything else is vanilla JS.

## Browser notes

- Scroll snapping is JS-driven, not CSS `scroll-snap`. Wheel, touch and
  keyboard input all cancel an in-flight glide.
- `prefers-reduced-motion` is respected: parallax, scrub and magnetic
  buttons all switch off.
- `html { overflow-x: clip }` is deliberate. `overflow-x: hidden` on
  `body` makes body a scroll container and breaks the snap logic.
