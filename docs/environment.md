# Environment — Cave Bot

Last verified: 2026-07-25.

## Runtime

| Item | Value |
|---|---|
| OS (verified) | Linux Mint 22.3, kernel 6.17.0-35-generic, x86_64 |
| Python | 3.12.3 (`/usr/bin/python3.12`) |
| Virtualenv | `.venv/` in the repository root, git-ignored |
| Package manager | `pip` inside `.venv` |

This project uses the system Python through a project-local virtualenv. There
is no version manager involved. Do not introduce pyenv, mise, or asdf for it
without a specific reason.

## Declared dependencies

`requirements.txt` lists three packages, unpinned:

```
flask
requests
anthropic
```

Installed in `.venv` at last verification: Flask 3.1.3, requests 2.34.2,
anthropic 0.116.0.

## Test command (verified)

Tests use the standard library `unittest`. **pytest is not installed and is not
a dependency.**

```
cd ~/git/cave-bot
PYTHONPATH=src .venv/bin/python -m unittest discover -s tests
```

`PYTHONPATH=src` is required because there is no `pyproject.toml`, `setup.py`,
`pytest.ini`, or `conftest.py` to make the package importable.

Verified 2026-07-25: **62 tests, OK**, 0.044s.

The error-path test prints `AI reply failed for persona 'cavebot': boom` to
stdout. That is expected output from a deliberate failure test, not a problem.

## Run command (NOT verified)

```
cd ~/git/cave-bot
PYTHONPATH=src .venv/bin/python -m cave_bot.groupme_app
```

Derived from `src/cave_bot/groupme_app.py:205-207`. **This has not been
executed** — verify before relying on it.

Serves on `0.0.0.0:5000` by default; `PORT` overrides. Endpoints:

- `GET /` — health check, returns `{"status": "ok", "service": "cave-bot"}`
- `POST /groupme/callback` — GroupMe webhook target

GroupMe must reach the callback over the public internet, so real end-to-end
use requires a tunnel or a host. No tunnelling tool is installed or documented
on this machine.

## Environment variables

Names only. Never record values in this file.

| Name | Purpose | Default |
|---|---|---|
| `GROUPME_BOT_ID` | GroupMe bot posting credential | unset — posting disabled |
| `BOT_TRIGGER` | Word that triggers a reply | `cavebot` |
| `CAVE_BOT_AI_ENABLED` | Enables Anthropic-generated replies | unset — disabled |
| `CAVE_BOT_MODEL` | Model ID used for persona replies | `claude-opus-4-8` |
| `CAVE_BOT_ADMIN_IDS` | Comma-separated GroupMe user IDs allowed to switch personas | empty — nobody |
| `PORT` | Flask listen port | `5000` |
| `ANTHROPIC_API_KEY` | Read implicitly by the `anthropic` SDK | unset |

There is no `.env.example` in this repository. This table is currently the only
inventory of these names.

## Known compatibility notes

- `.devcontainer/devcontainer.json` is a leftover from the repository's
  previous life as a Linux/Ansible lab. It is named "Cave Linux Lab", installs
  ansible, shellcheck, and yamllint, and installs none of this project's
  dependencies. It does not describe this environment.
