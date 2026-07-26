# Current state — Cave Bot

- **Last verified:** 2026-07-25
- **Last verified commit:** `73a070c` (2026-07-13, "Add persona-driven
  character system with AI-powered replies")
- **Working tree at verification:** clean, branch `main`

## Verification actually performed

| What | Command | Result |
|---|---|---|
| Test suite | `PYTHONPATH=src .venv/bin/python -m unittest discover -s tests` | 62 tests, OK, 0.044s |

Nothing else below was executed. The remaining statements come from reading the
source on 2026-07-25 and are labelled where the distinction matters.

## Confirmed working (covered by passing tests)

- Persona JSON loading and validation, including rejection of missing fields,
  non-lowercase keys, empty alias lists, and duplicate keys.
- Persona registry: default selection, listing sorted by key, switching, reset
  to default, and case-insensitive alias lookup.
- Admin handling: comma-separated ID parsing, whitespace stripping,
  deduplication, empty-set denial, and denial of `None`/empty sender IDs.
- Reply routing: alias-prefix matching, the trigger-word gate, `help`, `ping`,
  and the `characters` / `character` / `character <key>` commands with admin
  gating on switches.
- AI routing: uses the injected client when enabled, skips it when disabled,
  skips it when there is no question, and falls back to the persona greeting
  when the client raises.

## Not verified

- **The service has never been started** as part of this documentation work.
  The run command in `docs/environment.md` is derived from source, not tested.
- No end-to-end GroupMe delivery has been verified. `send_groupme_message()` is
  not exercised by any test.
- No real Anthropic API call has been made. Every AI test uses a mock client.
- Whether `claude-opus-4-8` (the `CAVE_BOT_MODEL` default) is the intended or
  current model ID has not been confirmed.

## Known issues

1. **`src/cave_bot/main.py` is orphaned.** A 61-line interactive CLI left over
   from the repository's previous life. Nothing imports it. Its docstring
   claims "This version does not use an external AI API yet", which is false.
2. **`README.md` describes a different project** — "Personal Linux Lab", listing
   directories (`notes/`, `labs/`, `ansible/`, `scratch/`) that do not exist.
3. **`.devcontainer/devcontainer.json` is stale** — named "Cave Linux Lab",
   installs ansible/shellcheck/yamllint, installs no project dependencies.
4. **`.gitignore` retains lab-era entries** — `*.retry`, `labs/**/results/`,
   `labs/**/playground/`.
5. **Dead commented code** at `src/cave_bot/groupme_app.py:159-163` duplicates
   the live `vibe` and `rules` handlers immediately above it.
6. **No `.env.example`**, so the seven environment variable names are
   discoverable only by reading source or `docs/environment.md`.
7. **Active persona is process-local.** `PersonaRegistry` holds it in memory, so
   a restart resets it to `cavebot`. This appears intentional — see
   `docs/decisions.md` D3.

## Work in progress

None. The working tree is clean and there is no open checkpoint.

## Not started

See the candidate list in `docs/task.md`. Nothing there has been selected.
