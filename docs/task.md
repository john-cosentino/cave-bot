# Current task — Cave Bot

**Status: no active task.**

There is no work in progress. The working tree is clean at `73a070c` and the
test suite passes. This file is a placeholder until the next piece of work is
agreed.

## Structure to use when work begins

Replace everything below with:

- **Objective** — one sentence.
- **Inputs** — files, documents, and data the work depends on.
- **Deliverables** — what must exist when this is done.
- **In scope** — explicit list.
- **Out of scope** — explicit list.
- **Constraints** — versions, boundaries, things not to touch.
- **Acceptance criteria** — verifiable statements, not intentions.
- **Manual verification steps** — exact commands and their expected output.

## Candidate work

Surfaced while documenting the repository on 2026-07-25. Listed so they are not
lost — none has been selected or prioritised.

1. Rewrite `README.md` to describe the GroupMe bot instead of the Linux lab.
2. Decide the fate of `src/cave_bot/main.py`: delete it, or repurpose it as a
   local reply-testing harness.
3. Replace or delete `.devcontainer/devcontainer.json`.
4. Add `.env.example` listing the seven environment variable names.
5. Confirm whether `claude-opus-4-8` is the intended default model.
6. Remove the dead commented block at `src/cave_bot/groupme_app.py:159-163`.
7. Decide whether active-persona state should survive a restart.
8. Verify the run command in `docs/environment.md` and record the result.
