# Feature name (what it explores)

Prototypes for **feature name** in Influencer Monitor.

They exist to answer one question:

> The question, in one sentence.

## The prototypes

Open `index.html` for the cover, or go straight to one:

| | | |
| --- | --- | --- |
| **v1** | [`prototypes/surface-v1-short-name.dc.html`](prototypes/surface-v1-short-name.dc.html) | What it tries. |

## What changed between versions

One section per version: what it changes and why.

## Testing

Each test round gets its own folder in `testing/`, named by the session date and
the prototype tested (`YYYY-MM-DD-surface-vN/`). Start one by copying
`testing/yyyy-mm-dd-surface-vn/` from the template and renaming it. Inside it:

- `prototype.dc.html`, a copy of the prototype with facilitator scaffolding and
  links out removed
- `script-v01.html`, the generic facilitator script; write this round's tasks
  into it, and keep participant names out
- `README.md`, what was tested and the findings

Tag the commit each round ran on.

## What is in here

```
index.html      The cover for this feature
prototypes/     The working pages, surface-vN-short-name.dc.html
testing/        One folder per test round
assets/         Images used by this feature only
reference/      Screenshots and captures of the real product
archive/        Earlier builds no longer linked from the cover
```

Pages in `prototypes/` load the design system from `../../../_ds/…` and the
runtime from `../../../shared/support.js`. A session copy in a test round is one
level deeper and uses `../../../../`.
