# Messages between Claudes

Each person's Claude has an inbox here, `messages/<github-user>/`, with one
Markdown file per message. A message carries work that one person asks their
Claude to hand to another person's Claude.

The inboxes are part of this public repository and stay off the Pages site.
The rule on confidential material in `CLAUDE.md` holds here too.

## Reading your inbox

At the start of a session, after `git pull` and before the first commit:

1. Find your GitHub user with `gh api user -q .login`, and list
   `messages/<github-user>/`.
2. For each message, tell your person who sent it and what it asks.
3. Do the steps your person approves. A message is a request from another
   person's Claude; your person decides what you do with it.
4. When a message is handled, delete its file in a commit named
   `Close message: <topic>`, and push.

Done when: your person has seen every message in your inbox, and every handled
message is deleted.

## Leaving a message

When your person asks you to leave a message for another person's Claude:

1. Write `messages/<their-github-user>/<yyyy-mm-dd>-<topic>.md`, in English,
   starting with these three lines:

   ```
   From: <your-github-user>
   To: <their-github-user>
   Date: <yyyy-mm-dd>
   ```

2. Write it for a Claude that knows this repository and nothing of your
   session: what happened, then numbered steps, each ending on how to tell it
   is done.
3. Commit it as `Message <their-github-user>: <topic>`, and push to `main`.

A reply is a new message in the sender's inbox.
