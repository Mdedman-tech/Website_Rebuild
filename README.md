# naturalstatemeds.com — rebuild

Static rebuild of the Natural State Medicinals site. Every page is a self-contained
HTML design component that opens directly in a browser. No build step, no framework,
no package manager.

## Pages

| File | Page |
|---|---|
| `Natural State Medicinals.dc.html` | Landing (Arkansas topo intro, survey-marker nav) |
| `About Us.dc.html` | About us |
| `How to Get a Card.dc.html` | How to get a card (tabbed) |
| `Check Your Allotment.dc.html` | Check your allotment (14-day ledger) |

## Supporting scripts

| File | Purpose |
|---|---|
| `support.js` | Design-component runtime. Required by every `.dc.html`. |
| `arkansas-intro.js` | Landing intro: state outline draw, relief, zoom to White Hall |
| `ar-elevation.js` | Arkansas elevation data used by the intro |
| `topo-texture.js` | Topographic watermark generator |
| `image-slot.js` | Drag-and-drop image placeholder component |

## Standalone builds

`Natural State Medicinals - *.html` are single-file offline builds with every asset
inlined. They are large (3 to 8 MB) and are generated output, not source. Regenerate
them from the `.dc.html` files rather than editing them.

`*.src.html` files are the inliner inputs for those builds.

## Assets

`assets/` holds all photography and brand marks, grouped by subject: `brand/`,
`team/`, `grow/`, `facility/`, `flower/`, `lab/`, `kitchen/`, `packaging/`, `extract/`.

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

## Not yet built

Find our product, and Contact. Education and product locator currently point to
live external URLs.
