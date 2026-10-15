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

Three things are still placeholders. None of them stop the page working,
but all of them would be visible to a real visitor:

1. **CTA links** — three buttons are marked `(demo link)` and call
   `return false`. Search `demo link` in `index.html`. They need real
   booking URLs.
2. **CRM webhook** — the form currently logs its payload to the console
   and fakes a success after 450ms. Search for `CRM INTEGRATION POINT`
   in `index.html`; the `fetch()` is written out and commented, and it is
   the only function that changes depending on which CRM you use. The
   payload carries the quiz segment and the visitor's weakest dimension,
   so automation can branch on them.
3. **Hero still** — `media/hero-still-1600.jpg` is a 693px web export
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

Type is Founders Grotesk (Klim Type Foundry), Regular and Semibold,
self-hosted from `media/fonts/`. It is a commercial face: the web licence
is per domain, so confirm INCO's licence covers wherever this page is
hosted before it goes public.

GSAP 3.15.0, self-hosted in `media/vendor/gsap/`: core, ScrollTrigger,
ScrollSmoother and SplitText (free under GSAP's standard licence since
3.13). Bundled rather than loaded from a CDN so the motion can't be
knocked out by a blocked or slow CDN, and so it works when `index.html`
is opened straight from disk. No other libraries. If the scripts still
fail, the page renders as a plain scrolling document; the progress line,
counter, quiz and form don't depend on GSAP.

## Animated edition (branch `claude/animated-landing`)

Same content, brand and integrations as `main`'s page; rebuilt around
motion. Preview locally:

```
git checkout claude/animated-landing
python3 -m http.server 8000
# open http://localhost:8000
```

Opening `index.html` directly also works.

What moves, and why:

- **Cover**: the photo opens from a framed viewport to full bleed, the
  title rises line by line, chrome fades in. Plays once on load.
- **Smooth scroll**: ScrollSmoother on mouse/trackpad only. Touch keeps
  native momentum. No scroll snapping on this edition.
- **Sheet rhythm**: the page background crossfades between slate, stone
  and olive as each sheet arrives; header, counter and progress ink
  follow whatever colour is under them. The sheet counter rolls.
- **Text**: headings rise out of a line mask (SplitText); the
  "Conditions" statement fills in word by word as you scroll.
- **Roadmap**: on wide screens the five questions are pinned and travel
  horizontally, with a ruler that ticks off each question. Stacked on
  phones.
- **Imagery**: the plate and portrait unmask and drift with light
  parallax; the INCO wordmark rises into the footer.
- **Cursor**: a small dot (fine pointers only) that swells over links
  and becomes a labelled disc over quiz answers. Primary buttons are
  magnetic. The native cursor stays in text fields.

`prefers-reduced-motion: reduce` turns off the intro, smoothing,
parallax, scrubbing, custom cursor and magnetic buttons; everything is
visible on load and in-page links jump instead of glide.

## Browser notes (main page)

- Scroll snapping is JS-driven, not CSS `scroll-snap`. Wheel, touch and
  keyboard input all cancel an in-flight glide.
- `prefers-reduced-motion` is respected: parallax, scrub and magnetic
  buttons all switch off.
- `html { overflow-x: clip }` is deliberate. `overflow-x: hidden` on
  `body` makes body a scroll container and breaks the snap logic.
