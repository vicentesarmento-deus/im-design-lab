From: vicentesarmento-deus
To: ritacarqueija14052000
Date: 2026-11-06

# `main` was rewritten: reset the clone before the next push

## What happened

- On 30 October, a push to `main` added
  `scripts/usability_test_labels_script--v03.html`, a copy of the 2026-10-01
  round's script with a participant's name filled in. The public site
  published it.
- On 6 November, `vicentesarmento-deus` removed that commit by rewriting the
  history of `main`. `ritacarqueija14052000/admin-v3` was rebuilt on the new
  `main` with the same 7 commits, and PR #7 was then squash-merged into `main`.
- Rita's clone still holds the old history. A push from it puts the file back
  on `main` and on the public site.

## Steps

1. Point `origin` at the repository's current name, then fetch:

   ```
   git remote set-url origin https://github.com/vicentesarmento-deus/im-design-lab.git
   git fetch --prune origin
   ```

   Done when: `git remote get-url origin` prints that URL.

2. List local commits that are not on GitHub:

   ```
   git log --oneline origin/main..main
   git log --oneline origin/main..ritacarqueija14052000/admin-v3
   ```

   Expected: "Publish the labels admin usability test script (v03)", merge
   commits, and the 7 "Admin v3" / "Add admin v3" commits, which reached `main`
   through PR #7. Any other commit is unpushed work: show it to Rita, and with
   her OK cherry-pick it onto a new branch made from `origin/main`.

   Done when: every listed commit is one of the expected ones or is on a new
   branch made from `origin/main`.

3. Reset `main`:

   ```
   git switch main
   git reset --hard origin/main
   ```

   Done when: `git status -sb` prints `## main...origin/main`, with nothing
   ahead or behind.

4. Delete the local branch, now merged:

   ```
   git branch -D ritacarqueija14052000/admin-v3
   ```

   Ask Rita whether to delete it on GitHub too, with
   `git push origin --delete ritacarqueija14052000/admin-v3`.

   Done when no commit left in the clone has a participant's name filled in;
   this prints nothing:

   ```
   git log --all --oneline -G 'aria-label="Participant name">[^[]'
   ```

## What changed for Rita

- She now commits and pushes directly to `main` inside any feature folder. See
  "Who changes what" in `CLAUDE.md`.
- `features/labels/admin-v3-developer-handoff.md` sits at the root of
  `features/labels/`, and the structure in `README.md` has no place for it.
  Ask Rita where it belongs. A new subfolder changes that shared structure,
  so it goes to `vicentesarmento-deus` as a pull request.
