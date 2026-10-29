# Influencer Monitor design lab

Working prototypes and usability test material for Influencer Monitor, one
folder per feature, all on one shared design system. Several people work here,
each with their own Claude. `README.md` describes the folder structure and the
naming rules; read it before creating or moving anything.

## Procedures

- **Starting a feature, or prototyping a product screen this repo has not built
  yet**: follow [`docs/new-feature.md`](docs/new-feature.md). It covers the
  skeleton rule: a new screen is built from screenshots, saved HTML or Figma
  frames of the real product, never from a description alone.
- **Preparing or running a usability test round**: follow
  [`docs/test-round.md`](docs/test-round.md).

## Who changes what

Each feature's `README.md` names its owner. The repository owner is
`vicentesarmento-deus`.

- **Inside your own feature folder** (`features/<feature>/`, where you are the
  owner): commit and push directly to `main`. Pages redeploys on every push.
- **Starting a new feature**: the same, plus one row for it in the root
  `index.html` and in the Features table of `README.md`.
- **Everything else** — another person's feature, `_ds/`, `shared/`,
  `features/_template/`, `docs/`, `.github/`, this file, or the rest of the
  root `README.md` and `index.html`: work on a branch named
  `<github-user>/<short-topic>` and open a pull request for the repository
  owner to review.
- The repository owner commits shared changes directly to `main`, without a
  branch or pull request.

Check the current git user (`gh api user -q .login`) against these rules before
the first commit of a session.

## Rules that hold everywhere

- **The repository and the site are public.** Participant names, customer data
  and anything confidential stay out of commits, file names and commit
  messages. Scripts carry the field `[PARTICIPANT NAME]`, filled in on screen
  during a session.
- **The design system in `_ds/monitoring-design-system-…/` is a Claude Design
  export.** Read it, build on its tokens and components, and request changes
  to it through the repository owner, who syncs it from Claude Design.
- **Prototypes use design system tokens for every colour, size and space.**
- **File names are kebab-case.** After moving or renaming a file, find every
  reference to the old path and open each affected page to confirm it loads.
- **A prototype is done when it runs.** Serve the repository
  (`python3 -m http.server 4175`), open each page you changed, and confirm the
  console and network show no errors before you commit.
