# Influencer Monitor design lab

Working prototypes and usability test material for **Influencer Monitor**, a
social-media influencer and brand monitoring product. Each feature we explore
gets its own folder; all of them share one design system.

Every prototype is a working page, not a picture of one. Open it and use it as
you would the real product.

## Features

| Feature | Folder | Question it answers |
| --- | --- | --- |
| **Labels** | [`features/labels/`](features/labels/) | Should label changes on a post resolve in place or inside a modal, and how is the label catalogue managed? |

Each feature folder has its own `README.md` with the full story.

## Structure

```
index.html                  The hub: one row per feature
features/<feature>/         One folder per feature
  index.html                  Its cover
  README.md                   What it explores, what changed between versions
  prototypes/                 The working pages, vN-short-name.dc.html
  testing/                    Clean copies for sessions, test scripts
  assets/                     Images used by this feature only
  reference/                  Screenshots and captures of the real product
features/_template/         Starting point for a new feature
_ds/monitoring-design-…/    The Monitoring Design System, shared by every feature
shared/support.js           Runtime for the .dc.html pages
shared/assets/              Assets used by more than one feature (the IM mark)
.nojekyll                   Stops a GitHub Pages build from excluding _ds/
```

## Adding a feature

1. Copy `features/_template/` to `features/<feature-name>/`, in kebab-case.
2. Fill in its `README.md` and the cover in its `index.html`.
3. Put prototypes in `prototypes/`. A page there is three levels below the
   root, so it loads shared files as `../../../_ds/…` and
   `../../../shared/support.js`.
4. Add a row for the feature to the root `index.html` and to the table above.

File names are kebab-case: no spaces, brackets or commas, so links need no
URL encoding. Prototypes carry their version first (`v1-…`, `admin-v2-…`), and
session copies carry the session date (`2026-10-01-…`).

## Design system

Everything is built on the **Monitoring Design System** in `_ds/` — see its own
`readme.md` for scope and caveats. In short: Material UI's Figma kit with
Influencer Monitor's theme layered on top. Primary is IM Blue `#1C00B7`, type is
Rubik for body and Epilogue for display, spacing is on an 8px base with a 4px
half-step, and the radius is a flat 4px.

The folder keeps the name it was exported with from Claude Design, so it can be
synced again. Files inside it come from that export; change them there, not
here, or the next sync will overwrite the change.

The design system is shared, so changing a token changes every feature,
including old prototypes. To keep a record of exactly what a participant saw,
tag the commit a session ran on (for example `labels-test-2026-10-01`).

## Where it lives

Published at **<https://vicentesarmento-deus.github.io/im-design-lab/>**.

Every push to `main` redeploys it, via `.github/workflows/pages.yml`. That
workflow assembles the site into `_site` so `.git` and the workflow itself are
not published, and carries `.nojekyll` through — which is what stops a Jekyll
build from excluding the underscore-prefixed `_ds/` directory.

## Running it locally

Opening `index.html` straight from disk works — the pages are built to run
from `file://`, and the design system loads by relative path.

To serve them over HTTP instead:

```
python3 -m http.server 4175
```

Then open <http://localhost:4175/>. A matching launch configuration lives in
`.claude/launch.json`.

**An internet connection is required.** The `.dc.html` pages load React and
Babel from unpkg at runtime, and the design system pulls the Epilogue title font
from Google Fonts. Offline, the prototypes will not boot and headings fall back
to Rubik.
