# Labels admin v3 — handoff for front-end development

For the developer who builds the labels admin in the real product.
The prototype is the specification for behaviour and wording. This file says what
to read, which rules to keep, and what the prototype leaves open.

**Prototype:** [`prototypes/admin-v3-labels-and-categories.dc.html`](prototypes/admin-v3-labels-and-categories.dc.html)
**Owner:** `vicentesarmento-deus` · **Other docs:** [`README.md`](README.md) (history of every round)

## 1. What this is

A screen where the label catalogue of Influencer Monitor is managed: create,
rename, move, duplicate, merge and delete labels and **label groups**, and see which
posts carry a label. It mirrors the product's Keyword Groups screen on purpose:

| Keywords | Here |
| --- | --- |
| keyword group | **label group** |
| keyword | **label** |

A label always belongs to at most one group. A label without a group is allowed
("No label group").

## 2. How to read the prototype

- It is a single working page, not a picture. Open it and use every action.
- The dark bar at the top (Reset, Hide, "[v4] Labels in a post") is test
  scaffolding. **Do not build it.**
- The sample data is invented. Names, posts and dates are placeholders.
- Code names still say `category` (routes, state, functions). **The product says
  "label group"** everywhere a user can read it. Use "label group" in code you
  write, and in the routes if the product allows it. The prototype's routes are
  `#/labels`, `#/categories`, `#/category/:id`, `#/label/:id`.
- `window.__labelsAdmin` exposes the prototype's state in the browser console. It
  is useful for checking what a rule does, for example `__labelsAdmin.state.data`.
- Look at the real Keyword Groups screen first. The layout, table, pager and
  dialogs should feel like the same product.

## 3. Design system

- Use the product's design system components. The prototype uses the Claude Design
  export in [`_ds/`](../../_ds/) (Button, IconButton, Dialog, Menu, Select,
  TextField, Radio, Checkbox, Tooltip, Alert, Snackbar, Icon).
- **Every colour, size and space comes from a token** (`--space-*`, `--fs-*`,
  `--color-*`, `--radius-*`, `--grid-*`). No hard-coded values. If a token is
  missing, ask the design system owner. Do not invent one.
- Icons used: `UnfoldMoreOutlined` (unsorted column), `ArrowUpwardFilled` and
  `ArrowDownwardFilled` (sorted), `AddFilled`, `EditFilled`, `DeleteFilled`,
  `InfoOutlined`, `FolderFilled`, `ChevronLeftFilled`, `ChevronRightFilled`.

## 4. Data model

| Entity | Fields the UI needs |
| --- | --- |
| **Label group** | id, name, created by, created at, last edited at |
| **Label** | id, group id (or none), name, created by, created at, last edited at |
| **Post** | id, preview image, creator, platform and format ("IG: Story"), posting time |
| **Label–post link** | post id, label id, added by, added at |

Counts shown in lists (labels per group, posts per label or group) are derived.

**Name matching.** Two names are the same when they are equal after trimming,
collapsing inner spaces and ignoring case. The first spelling stays as written.
Every "already exists" rule below uses this.

## 5. Screens and URLs

Each view has its own URL, so the browser's Back button works.

| View | URL | Notes |
| --- | --- | --- |
| **All labels** | `#/labels` | **Opens here.** First tab. |
| **Label groups** | `#/categories` | Second tab. |
| **One label group** | `#/category/:id` or `#/category/none` | Its labels. `none` is the "No label group" page. |
| **One label** | `#/label/:id` | Every post that carries it. |

On the two tab views: the title is **Labels**, then Export and the add button, then
search, then the first-visit banner, then the tabs. Tabs show their count:
"All labels (43)", "Label groups (9)". **No total is shown under the title.**
On a group's page the breadcrumb is the title ("Labels › Campaigns"), and the
total and creation info sit beside it.

### Tables

- Columns, **All labels:** Label, Label group, Posts, Created, Created by, Last edited.
- Columns, **Label groups:** Title, Labels, Posts, Created, Last edited.
- Columns, **one group:** the same as All labels, without the Label group column.
- Columns, **one label:** Preview, Creator, Posting time, Platform, Label added by, Added.
- **Default sort:** All labels and one group by name A–Z; Label groups by created,
  newest first; one label by added, newest first.
- **Every sortable column always shows a sort icon**, grey and unsorted until used,
  so users know they can sort. Posts, Created and Last edited start descending on
  the first click; the others start ascending. This is how users find labels
  with no posts, so there is no "Posts" filter.
- Rows per page: 10, 25, 50.
- **"No label group"** is a pinned row after the last page of the Label groups
  table. It is not counted and not selectable.
- A row has a checkbox and a ⋮ menu. The ⋮ menu offers the same actions as the bar
  for that one row.
- Empty states exist for: no labels, no groups, an empty group, and no search
  results. Wording is in the prototype.

### Action bar

A bar sits at the top of the table. **It is always visible** (it does not appear
on selection, because the table would jump).

- With nothing selected it reads "Select labels to act on them" (or "label groups")
  and the buttons are disabled.
- With a selection it reads "N rows selected", and "Clear selection" appears.
- **Labels:** Move, Merge, Rename, Duplicate, Delete. **Label groups:** Merge,
  Rename, Delete. Delete is red.
- Disabled buttons say why in a tooltip, for example "Select two or more label
  groups to merge" or "Delete one label group at a time".

### First-visit banner

On All labels and Label groups, above the tabs: "Labels help you track brands
across evaluations, assign posts to your team and group ads by product category."
"Learn more" expands three examples; "Got it" hides it.

- **In the prototype it shows every time the page opens**, so that test participants
  always see it. **For the product, decide the real behaviour** (typically: shown
  once, remembered per user). See section 9.

## 6. Actions and their rules

Every change **says what it will touch before it happens** (in the dialog), and
every change **can be undone** from the snackbar that follows it (or with `Z`).
Changes apply immediately for everyone. There is no save bar here.

### Add label or label group (one dialog, from everywhere)

- The button reads "Add label or label group". **Inside a group's page it reads
  "Add label to <group name>"**, and the group field is filled in.
- Fields: "Label group (optional)" and the label list. Labels are typed and added
  one by one. A comma or Enter adds the entry. Pasting a list splits on commas,
  new lines and tabs. Backspace in the empty field brings the last entry back to
  edit. **Whatever is still in the field on confirm is added too.**
- `Group / Label` typed in the list puts that label in that group, even a new one.
- An empty group field means labels with no group. A new group name creates the
  group (with or without labels). An existing name adds to that group. The
  helper text says which of these will happen.
- **A label that already exists elsewhere** (another group, or no group) shows its
  row as "Already exists in <place>" with two choices: **Move to <group>**
  (the existing label, with its posts, moves here) or **Keep both** (creates a
  second label; labels in different groups are different things). The user must
  answer every such row before confirming, and may change an answer.
- A repeated name inside the same group is skipped, and the row says so.
- The confirm button says exactly what happens, for example "Add 1 label, move 1 label".

### Move

A list of destinations: **+ New label group** first, then the groups, then "No label
group". Each row shows its first 5 labels and a "+N" with a tooltip listing the
rest. If a moved label has the same name as a label already in the destination,
those two **merge** (the dialog says so, per row). Moving never merges across
different groups on its own.

### Rename

Inline, in the row or in the page title. Enter saves, Esc cancels. The helper text
says how many posts or labels will show the new name. Renaming to a name that
already exists **in the same group** offers a merge instead.

Renaming several selected labels at once gives them all one spelling. It is never
a merge, so it is only offered when they are in **different** groups.

### Duplicate

Creates an independent copy of a label in another group, **with no posts**. A
checkbox opts in to linking the copy to the same posts.

### Merge labels

Only when every selected label is **in the same group**. Choose the name to keep
(one of the labels, or another name). The posts of the others move to the kept
label; **a post that already has the kept label is not duplicated.** The dialog
lists the posts affected first.

### Merge label groups

Choose the name to keep: one of the groups, or another name. If another name
matches a group that exists, that group is merged in as well, and the dialog warns.
The others are deleted, and their labels move to the kept group.

**Labels with the same name in the merged groups become one label.** The dialog
shows them in a table:

- One column per group (the group name is in the header), and a green **Becomes**
  column with the label that stays and its post count.
- **The post count counts each post once.** If one post carries both labels of a
  pair, 2 + 12 posts becomes 13, not 14. A tag "1 post has both" (or "N posts
  have both") appears under the result when this happens, and not otherwise.
- Below the table: "'Campaigns' will have 4 labels in total", where "4 labels" has
  a tooltip listing every label name that will be in the group.
- Confirm is disabled until a name is chosen.

### Delete

- **Label groups are deleted one at a time**, never in bulk (a little friction is
  intended). Choose what happens to the labels: keep them with no group, move them
  to another group, or delete them too. The affected posts are shown first.
- **Labels:** remove them from their posts, or replace them with another label from
  the same group. The affected posts are shown first.

### Export

The whole catalogue as Excel (.xlsx), Word (.docx) or JSON. The prototype builds
them in the browser. The real product may export on the server.

### Undo and messages

Every change gives a snackbar naming what happened ("Moved 2 labels to Brand (1
merged)"), with Undo. Keyboard: Esc closes a dialog or menu, Cmd/Ctrl+Enter confirms
a dialog, `Z` undoes the last change.

## 7. Wording

The prototype's text is the reference. Keep these:

- **Label group**, never "category", in everything the user reads. The one
  exception is the banner sentence "group ads by product category", where it means
  the product's own category.
- "All labels" and "Label groups" for the two tabs, in that order.
- "No label group" for labels without one.

## 8. Accessibility

Check these against the prototype and the product's standards:

- Tabs, the action bar, dialogs, menus and tables need correct roles and keyboard
  paths (Tab, Enter, Space, Esc, arrows in menus and lists).
- The sort button sets `aria-sort` on the column header.
- The Move list is a listbox. The merge table needs proper header cells.
- The "+N" and "4 labels" tooltips must also be reachable by keyboard focus, and
  have a text equivalent (the prototype sets an `aria-label` with the full list).
- Do not rely on colour alone: the green "Becomes" column also has its header and
  the orange tag has text.

## 9. What the prototype does not decide

These need an answer from product, design or the backend team. They are not
solved by the prototype, and the sections above should not be read as having
solved them.

1. **Persistence and concurrency.** The prototype keeps data in memory and resets
   on reload. Undo is a stack of snapshots. In the product, what does Undo do when
   someone else has changed the same data? Is it time-limited? Is it per user?
2. **Atomic operations.** Merge, move and delete touch many rows. They need to
   succeed or fail as one operation. The pieces of text that count posts (for
   example "13 posts") should come from the server, not be recomputed by the
   client.
3. **Scale.** The sample has 9 groups and 43 labels. Real catalogues may have
   thousands. Decide on server-side pagination, sorting and search, and how
   the dialogs list large merges and moves. The Move list shows every group.
4. **Search.** Label groups search matches the group name and the names of labels
   inside it ("Matching labels: …" is shown as a hint). Confirm the exact rule.
5. **Permissions.** Who can create, rename, merge and delete? The prototype has
   no roles.
6. **The first-visit banner.** Shown once per user, per workspace, or until
   dismissed? The prototype always shows it.
7. **Post links.** Post previews in the label page are thumbnails only. Decide
   where each one opens.
8. **Export.** Which columns, which file naming, and whether it is generated by the
   server.
9. **Telemetry and errors.** Not designed here: loading states, network errors,
   and failures during a merge.
10. **Languages.** The prototype is English only.

## 10. Order of work (suggestion)

1. The shell: tabs, tables, search, sorting and paging, with the real API.
2. One label and one group pages.
3. The action bar and the simple actions: Rename, Duplicate, Delete.
4. Add, and then Move (the "already exists" rules are the hardest part of Add).
5. The two merges, with their previews computed by the server.
6. Undo, snackbar and keyboard shortcuts.
7. The first-visit banner, once the rule in 9.6 is decided.
8. Export.

## 11. Checking your work

Open the prototype next to your build and try these:

- Add `nike` when `Brand / Nike` exists, and answer "Move" and then "Keep both".
- Merge two label groups that share a label name, and check the count (it counts
  a post once) and the tag.
- Rename a label to the name of another label in the same group (it offers a merge).
- Rename a label to the name of a label in a different group (it is just renamed).
- Delete a group with labels, in each of the three ways.
- Sort every column and check that the icon is always visible.
- Undo each change, and check the data is exactly as before.
