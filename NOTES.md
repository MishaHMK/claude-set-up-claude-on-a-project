# NOTES

## What I put in CLAUDE.md, what I left out, and why

I started from `/init` draft and then cut it down a lot. I kept a one-line description of the project, the three commands I really use (`npm run dev`, `npm test`, `npm run lint`) and also how to run only one test file, because this is the thing I ask most often. In Conventions I put rules which Claude can not guess from code alone, or which it can easy break: CommonJS and not ES modules, tests only with `node:test` + `supertest` (no Jest, never `listen()`), data only through `db/store.js`, and the error format with 400/404/201. In Architecture there is just few lines how the project is organised - `server.js`, one router file per resource, and that the store is in-memory and shared between tests, so Claude don't write tests with exact counts.

What I deliberately left out: the list of every file and every endpoint (Claude can just read the code, and this list become outdated very fast), the setup steps from README (`npm install`, how to open terminal etc.), course instructions, and anything from `.env`. I think if the file is too long, the important rules get lost between obvious things, so every line should save me one correction. And secrets should never be in a file that is committed to git.

## Which permission rules I added, and what could go wrong without the deny rule

- **allow** `Bash(npm test:*)` and `Bash(npm run lint:*)` - this I run all the time and it is safe, it don't change anything, so I don't want click "yes" every time.
- **ask** `Bash(git commit:*)` and `Bash(git push:*)` - I want to see and confirm by myself before something goes to the repo, specially push, because it is visible for other people.
- **deny** `Read(./.env)` and `Bash(git push --force:*)`.

Without the deny rules two bad things can happen. Claude could read `.env` while "looking around" the project, and then real secrets (like database password) go into the conversation context or even get copied into some file or commit message. And a force-push can rewrite the history on remote branch and delete work of other people, and this is very hard to undo. I prefer that this things are blocked at all, not depend on me reading the prompt carefully when I am tired.

## Verification

I started a fresh session and ran `/memory` - the project `CLAUDE.md` is shown as loaded. Then I ran `/permissions` and my allow / ask / deny rules are listed there. I also asked "How do I run the tests here?" and Claude answered with `npm test` and `node --test tests/users.test.js` straight from the file, without me explaining.
