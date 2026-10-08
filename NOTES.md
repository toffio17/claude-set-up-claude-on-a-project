# Notes: Claude Code setup

## What's in CLAUDE.md, and what isn't
I kept it to what Claude can't easily work out from the code, or might get wrong:
the three commands (and that CI runs lint and then tests), the conventions that
keep new code consistent (CommonJS, node:test + supertest, data access only
through db/store.js, the JSON error shape and status codes), and a short
architecture note.

I left out anything the code or ESLint already makes obvious, file-by-file
descriptions, the course instructions and anything sensitive. A short file
costs less context each session and is more likely to be followed.

## Permission rules
- **Allow:** `npm test`, `npm run lint` and read-only git commands (`status`,
  `diff`, `log`). They're safe and I run them all the time, so approving each
  one is just noise.
- **Ask:** `git push` and `npm install`. Pushing publishes work, and installing
  changes dependencies, so I want to confirm both every time.
- **Deny:** reading `.env` / `.env.local`, `git push --force` / `-f`,
  `git reset --hard` and `rm -rf`. Without the `.env` rule, Claude could read
  real secrets into the conversation, where they might end up in logs or in
  generated code. Without the force-push rule, one mistaken command could
  overwrite shared history on the remote.

## .gitignore fix
Lines 2–7 of `.gitignore` started with three spaces. Git treats leading spaces
as part of the pattern, so `.env` and `.claude/settings.local.json` weren't
actually ignored. I removed the indentation and checked with
`git check-ignore -v`.

## Verification
- `/memory` shows `CLAUDE.md` loaded: _(to confirm)_
- `/permissions` shows the allow / ask / deny rules: _(to confirm)_
- Asked "How do I run the tests here?" and Claude answered `npm test` from `CLAUDE.md`: _(to confirm)_
