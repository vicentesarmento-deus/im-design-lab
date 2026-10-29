# Feature name (what it explores)

Prototypes for **feature name** in Influencer Monitor.

They exist to answer one question:

> The question, in one sentence.

## The prototypes

Open `index.html` for the cover, or go straight to one:

| | | |
| --- | --- | --- |
| **v1** | [`prototypes/v1-short-name.dc.html`](prototypes/v1-short-name.dc.html) | What it tries. |

## What changed between versions

One section per version: what it changes and why.

## Testing

Session copies go in `testing/`, named with the session date
(`YYYY-MM-DD-short-name.dc.html`), with facilitator scaffolding and links out
removed. Tag the commit each session ran on.

## What is in here

```
index.html      The cover for this feature
prototypes/     The working pages
testing/        Clean copies for sessions, and test scripts
assets/         Images used by this feature only
reference/      Screenshots and captures of the real product
```

Pages in `prototypes/` and `testing/` load the design system from
`../../../_ds/…` and the runtime from `../../../shared/support.js`.
