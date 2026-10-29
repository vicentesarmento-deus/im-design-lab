## Workflow

This is a solo project — only the owner touches it. Do not open pull
requests and do not create feature branches for changes. Commit and push
directly to `main`. This overrides the default "always open a PR after
pushing" behaviour for this repository specifically.

## Layout

The repository holds prototypes for several Influencer Monitor features. Read
`README.md` first; it describes the folder structure and naming rules.

- Each feature lives in `features/<feature>/`, with its own `README.md`.
  Work on one feature does not touch another feature's folder.
- To start a feature, copy `features/_template/`.
- `_ds/monitoring-design-system-…/` is an export from Claude Design. Do not
  rename it and do not edit files inside it here; a later sync will overwrite
  local changes.
- Shared runtime and assets live in `shared/`. A page in
  `features/<feature>/prototypes/` reaches them with `../../../`; a session
  copy in `features/<feature>/testing/<round>/` with `../../../../`.
- Usability test material is grouped by round in `testing/<date>-<what>/`.
  Scripts there can carry participant names; keep names out of the published
  `.html` and out of commits.
- File names are kebab-case. After moving or renaming a file, grep for every
  reference to the old path and open each affected page to check it loads.
