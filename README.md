# naturalstatemeds.com — rebuild

Static rebuild of the Natural State Medicinals site. Every page is a self-contained
HTML design component that opens directly in a browser. No build step, no framework,
no package manager.

## Pages

| File | Page |
|---|---|
| `Natural State Medicinals.dc.html` | Landing (Arkansas topo intro, survey-marker nav) |
| `About Us.dc.html` | About us |
| `How to Get a Card.dc.html` | How to get a card (seven chapters, fixed chapter rail) |
| `Check Your Allotment.dc.html` | Check your allotment (state Limit Meter walkthrough) |

## Supporting scripts

| File | Purpose |
|---|---|
| `support.js` | Design-component runtime. Required by every `.dc.html`. |
| `arkansas-intro.js` | Landing intro: state outline draw, relief, zoom to White Hall |
| `ar-elevation.js` | Arkansas elevation data used by the intro |
| `topo-texture.js` | Topographic watermark generator |
| `image-slot.js` | Drag-and-drop image placeholder component |

The landing page's pocket-transit compass is two PNG layers in
`assets/compass/` (`Compass_Body.png`, `Compass_Needle.png`). The needle layer
rotates north to south once the intro settles, driven by the `ns-intro-settled`
event dispatched from `arkansas-intro.js`, never a fixed timer, so SKIP INTRO and
reduced-motion both land correctly.

## Published site

GitHub Pages serves the repo root at
https://mdedman-tech.github.io/Website_Rebuild/. Four single-file builds sit there
with every asset inlined:

| URL | Page |
|---|---|
| `index.html` | Landing |
| `about.html` | About us |
| `get-a-card.html` | How to get a card |
| `allotment.html` | Check your allotment |

These are generated output, not source. Do not edit them. Regenerate from the
`.dc.html` pages: each `*.src.html` is the inliner input, and it is the `.dc.html`
plus a thumbnail template, with cross-page `.dc.html` links rewritten to the clean
published filenames above. Rebuild all four whenever any page changes, so the links
between them stay consistent.

`.nojekyll` keeps Pages from running the files through Jekyll.

## Page notes

How to get a card is one continuous read, not tabs. Seven chapters, a fixed chapter
rail beside the text on wide screens, a compact progress bar below 1080px, and a
"keep going" hand-off at the foot of every chapter. Body copy stays visible with no
JavaScript; the entrance animation only arms once the script is running.

Check your allotment does not track purchases. Arkansas holds the official count and
the dispensary register reads it, so the page explains how to find the state's own
Limit Meter. The fourteen-day figure is scroll-driven: days light as they pass and
the first day's weight steps off on day fifteen.

About us is people first, and ends in a scatter of crew polaroids that straighten as
you scroll into them. Four `image-slot` placeholders there are waiting on photos
from kitchen, extraction, packaging, and delivery, and every caption still needs a
real name and role.

## Assets

`assets/` holds all photography and brand marks, grouped by subject: `brand/`,
`team/`, `grow/`, `facility/`, `flower/`, `lab/`, `kitchen/`, `packaging/`,
`extract/`, `compass/`.

`_ds/` holds the bound Natural State design system: color and type tokens, the
marketing UI kit, and the component bundle. Pages load `colors_and_type.css` from
here, so keep the folder path intact.

## Conventions

Typography, punctuation, voice, palette, and compliance rules live in `CLAUDE.md`.
Read it before changing copy. Two rules matter most: body copy is Work Sans,
display is Burford, and there are no em dashes anywhere.

## Running locally

Serve the folder over HTTP, do not open with `file://`, since the pages fetch
sibling scripts and assets.

    python3 -m http.server 8000

Then open http://localhost:8000/Natural%20State%20Medicinals.dc.html

## Education guide

The guide lives in its own repo and is served by GitHub Pages. In-site
Education links point at that URL for now, because GitHub Pages, the demo host,
cannot serve another site under our own path.

Go-live plan on Wix: Wix has no rewrite or proxy feature, so the guide cannot be
served from naturalstatemeds.com by Wix alone. Put Cloudflare in front of the
domain instead:

1. Move DNS for naturalstatemeds.com to Cloudflare, proxied. Wix keeps serving
   the site.
2. Add a Worker on the route naturalstatemeds.com/education/* that fetches from
   mdedman-tech.github.io/Natural-State-Education-Guide/ and returns the
   response. Everything else falls through to Wix untouched.
3. Change the four Education links to /education/. They are in:
   Natural State Medicinals.dc.html, Landing Standalone.src.html,
   About Us.dc.html, How to Get a Card.dc.html.
4. Confirm the guide's own CSS, fonts, and images load. If any of its paths
   start at the site root they will 404 through the proxy and need to be made
   relative, or the guide needs its base path set to /education/.

## Not yet built

Find our product, and Contact. The product locator still points to a live
external URL.

## Open items

- Crew names and roles for the About us polaroid captions.
- Staff photos for kitchen, extraction, packaging, and delivery.
- Confirm two facts with ADH: visiting patient card length, and the caregiver
  background check fee.
- Voice pass on the landing page and About us. Allotment and the card page are done.
- Performance: subset the fonts, serve photography as WebP or AVIF, lazy-load
  below the fold. Fold this into any Wix or Cloudflare migration.
