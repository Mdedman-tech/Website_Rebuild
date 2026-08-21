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
| `Contact Us.dc.html` | Contact us (form posts to a Google Sheet) |
| `Your Paperwork.dc.html` | Your paperwork (form checklist and downloads) |

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
https://mdedman-tech.github.io/Website_Rebuild/. Six single-file builds sit there with every asset inlined, plus the education
guide, which stays as loose files under `education/`:

| URL | Page |
|---|---|
| `index.html` | Landing |
| `about.html` | About us |
| `get-a-card.html` | How to get a card |
| `allotment.html` | Check your allotment |
| `contact.html` | Contact us |
| `paperwork.html` | Your paperwork |
| `education/index.html` | Education guide (routes to desktop or mobile) |

These are generated output, not source. Do not edit them. Regenerate from the
`.dc.html` pages: each `*.src.html` is the inliner input, and it is the `.dc.html`
plus a thumbnail template, with cross-page `.dc.html` links rewritten to the clean
published filenames above. Rebuild all six whenever any page changes, so the links
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

The guide now lives in this repo under `education/`, so it is part of the site
instead of a link out to another host. In-site Education links point at
`education/index.html`.

- `education/index.html` routes phones to `mobile.html`, everything else to
  `desktop.html`. `?v=desktop` or `?v=mobile` forces one.
- `education/support.js` and `education/guide-data.js` belong to the guide.
  `support.js` at the root is the site's own runtime. They are different files.
- Guide images, fonts, and design tokens resolve up one level, into the shared
  `assets/` and `_ds/` trees. The guide's own copies were merged in, so nothing
  is duplicated.
- `_ds/overbuilt-design-system-3b590326.../` is the guide's token set, kept
  alongside the site's design system. Both are needed.
- `.nojekyll` at the root is what lets `_ds/` serve on GitHub Pages. Do not
  remove it.

Deep links still work: `education/index.html#strain/dogtown` survives the
redirect. The Cloudflare Worker plan is no longer needed.

## Not yet built

Find our product. The locator still points to a live external URL.

## The contact form is not connected

`Contact Us.dc.html` posts to a Google Apps Script web app. The URL is a single
constant at the top of the logic class:

    const SHEET_ENDPOINT = '';

While it is empty the form refuses to send and says so plainly, rather than
swallowing a note. To connect it: make a Sheet, Extensions then Apps Script, a
`doPost(e)` that appends `e.parameter` as a row, deploy as a web app with access
set to anyone, then paste the `/exec` URL into that constant and rebuild
`contact.html`. Posts arrive as `route`, `sent`, `name`, `email`, `message`,
and on the complaint path also `dispensary`, `purchased`, and `lot`.

## Open items

- Crew names and roles for the About us polaroid captions.
- Staff photos for kitchen, extraction, packaging, and delivery.
- Confirm two facts with ADH: visiting patient card length, and the caregiver
  background check fee.
- Voice pass on the landing page and About us. Allotment and the card page are done.
- Performance: subset the fonts, serve photography as WebP or AVIF, lazy-load
  below the fold. Fold this into any Wix or Cloudflare migration.
