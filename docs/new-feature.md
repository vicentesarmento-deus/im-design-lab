# Starting a feature

A feature's prototypes sit on a **skeleton**: a working copy of the real
product screen the feature lives on — its navigation, header, tables and
density — built from the design system and filled with sample data, before any
of the feature's own behaviour exists. The skeleton is what makes a participant
feel they are in Influencer Monitor, so it is built from the real product,
never from memory or a description.

Example: the labels admin is new, so there was no screen to copy. It was built
from screenshots of the product's Keyword Groups administration, the closest
existing screen, and then reshaped into labels and categories.

## 1. Set up the folder

Copy `features/_template/` to `features/<feature>/` (kebab-case). In its
`README.md`, fill in the title, the question the prototypes exist to answer,
and the **Owner** line with your GitHub username.

Done when: the folder exists and its README states the question and the owner.

## 2. Find the screen's skeleton

Decide which product screen or screens the feature lives on, then look for a
skeleton of each in `features/*/prototypes/` (a file ending in
`-v0-skeleton.dc.html`, or the earliest version of a feature on the same
screen) and in `features/*/reference/`.

- **A skeleton exists**: copy it into your `prototypes/` as
  `<surface>-v0-skeleton.dc.html`, fix its relative paths, and go to step 6.
- **None exists**: go to step 3.

Done when: each screen the feature needs either has a skeleton copied in, or is
listed as missing.

## 3. Ask for the real screen

For each missing screen, ask the user for one or more of:

- **Screenshots** of the screen, including its states that matter (empty, a
  long list, an open menu or dialog).
- **The page's saved HTML**: in the browser, File › Save Page As (Webpage,
  HTML only), or copy the `<body>` element's outer HTML from the inspector.
- **Figma link(s)** to one or more frames of the screen.

Screenshots plus saved HTML give the best result: the picture shows the
layout, the HTML gives the exact structure and copy. When no screen in the
product matches, ask for the closest one — as Keyword Groups stood in for the
labels admin — and agree with the user which parts carry over.

Done when: you hold real material for every missing screen. Wait for it; do not
start the skeleton without it.

## 4. Store the material

Put it in `features/<feature>/reference/` with kebab-case names
(`keyword-groups-list.png`, `keyword-groups-detail.html`). Before committing,
look at each file for customer names, real handles, emails, workspace names or
other personal data: the repository is public. Crop or blur screenshots, and
replace real values in saved HTML with sample ones. When in doubt, ask the
user.

Done when: every file is in `reference/` and has been checked for personal and
customer data.

## 5. Build the skeleton

Read the material:

- **Figma links**: read the frames through the Figma MCP server if one is
  connected (load the Figma design-to-code skill first when it is installed),
  taking both the design context and a screenshot of each frame. Without a
  Figma connection, ask the user to export the frames as PNG instead.
- **Saved HTML**: take the structure, the copy and the order of things from
  it; ignore its class names and styles.
- **Screenshots**: take layout, spacing and density from them.

Then write `prototypes/<surface>-v0-skeleton.dc.html`. Prototypes are
`.dc.html` pages run by `shared/support.js`; open an existing one, such as
`features/labels/prototypes/admin-v1-labels-and-categories.dc.html`, and follow
its structure: the `<x-dc>` root, the `<helmet>` block with the design system
links, and the `class Component extends DCLogic` script. Map every colour,
font size and space to a design system token, and use the design system's
components where one fits. Fill the screen with invented sample data that
looks real: plausible names and numbers, no real people or customers.

Done when: the skeleton opens in the browser without console or network
errors, and laid next to the reference material it shows the same navigation,
regions, columns and density. Show the user a screenshot of it beside the
reference and get their agreement before building on it.

## 6. Prototype the feature

Build `<surface>-v1-<short-name>.dc.html` on top of the skeleton. Each later
version is a new file with the next number; the README's table and "what
changed" sections record why each one exists. Add the versions to the
feature's `index.html` cover.

Done when: the version runs without errors, and the cover and README list it.

## 7. Publish

Add one row for the feature to the root `index.html` and to the Features table
in the root `README.md`, then commit and push following "Who changes what" in
`CLAUDE.md`.
