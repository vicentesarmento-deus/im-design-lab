# Running a usability test round

A **round** is one set of sessions on one prototype version, on one date. Each
round has its own folder, so it keeps a frozen record of exactly what
participants saw and heard, even after the prototype and the design system move
on.

## 1. Create the round folder

Copy `features/_template/testing/yyyy-mm-dd-surface-vn/` to
`features/<feature>/testing/<date>-<surface>-v<N>/`, using the first session's
date and the prototype version being tested (`2026-10-01-admin-v2`).

Done when: the folder exists with its `README.md` and `script-v01.html`.

## 2. Make the session copy

Copy the tested prototype into the round folder as `prototype.dc.html`, then
take out everything a participant should not see:

- the dark facilitator bar and any key that toggles it;
- links to other prototypes and versions, and rows or buttons that open them;
- version numbers in the page title and on screen.

Add the welcome screen, so participants wait on a neutral page until asked to
start: copy it from
`features/labels/testing/2026-10-01-admin-v2/prototype.dc.html` (the
`.welcome` styles and the `welcome` block). The session copy sits one level
deeper than `prototypes/`, so its shared paths start with `../../../../` and
its feature assets with `../../assets/`.

The copy is a fork: fixes to the prototype afterwards are not carried over, and
fixes to the copy are not carried back.

Done when: the copy opens on the welcome screen, shows no version or
facilitator controls, and runs without console or network errors.

## 3. Write the script

`script-v01.html` is the generic facilitator script: introduction, recording
consent, first impression, wrap-up and facilitation rules, with one placeholder
where the tasks go. In this round's copy:

- fill in `[FEATURE]` and `[N]` in the header;
- replace the placeholder task with one `<li class="task">` per task, each
  phrased as a goal the participant has, never as the steps or labels on
  screen; a follow-up question goes in a `<p class="task__follow">`;
- adjust the agenda's times if the number of tasks changes the length.

Leave `[PARTICIPANT NAME]` as it is; it is filled in on screen during each
session. When the script changes between sessions, save it as the next version
(`script-v02.html`) and keep the earlier one. Working copies in `.docx` or
`.md` stay local; `.gitignore` keeps them out of commits.

When a change belongs in every future test, not only this one, make it in the
template's `script-v01.html` too, following "Who changes what" in `CLAUDE.md`.

Done when: every task is written in, the header is filled in, and the script
contains no participant or customer names.

## 4. Fill in the round's README

State what is tested and why, link the prototype and the script, and list the
participants' roles and the number of sessions — never their names.

## 5. After the sessions

- Tag the commit the sessions ran on: `<feature>-test-<date>`
  (`labels-test-2026-10-01`), and push the tag.
- Write the findings into the round's `README.md` under **Findings**, or link
  the document that holds them.
- Update the flag on the feature's row in the root `index.html` (for example
  "Tested 1 Oct 2026").
