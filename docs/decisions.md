# Decisions — Cave Bot

Durable decisions and their consequences.

**Read this first:** the entries below were reconstructed by reading the code on
2026-07-25. The *what* is observable in the source. The *why* is marked
**unconfirmed** wherever no record of the reasoning exists. Do not treat an
unconfirmed rationale as established, and replace it when you know the real
reason.

---

## D1 — Personas are data, not code

**Decided:** a persona is a JSON file in `data/personas/profiles/`, validated at
load time against required fields (`key`, `display_name`, `aliases`,
`system_prompt`).

**Why:** *unconfirmed.* Consistent with wanting to add characters without
touching Python.

**Consequences:** adding a character is a data change. Invalid definitions raise
`PersonaError` at load time rather than at reply time.

**Date:** on or before 2026-07-13 (`73a070c`).

---

## D2 — The AI client is injected, never constructed inside reply logic

**Decided:** `build_reply()` and `generate_persona_reply()` both receive the
client as a parameter. `groupme_app.py` constructs the real client lazily in
`get_anthropic_client()`.

**Why:** *unconfirmed.* The effect is that the full suite runs with mocks and no
network access.

**Consequences:** tests need no API key and make no network calls. Moving client
construction into the reply path would destroy that property.

**Date:** on or before 2026-07-13 (`73a070c`).

---

## D3 — Active persona is in-memory only

**Decided:** `PersonaRegistry` holds the active key in an instance attribute
with no persistence. The class docstring states this explicitly.

**Why:** *unconfirmed.* The explicit docstring suggests a deliberate choice
rather than an oversight.

**Consequences:** restarting the service resets the active persona to `cavebot`.
A multi-worker deployment would give different workers different active
personas.

**Date:** on or before 2026-07-13 (`73a070c`).

---

## D4 — Persona switching is admin-gated; listing and status are not

**Decided:** `cavebot character <key>` requires the sender's GroupMe user ID to
appear in `CAVE_BOT_ADMIN_IDS`. `characters` and bare `character` are open to
everyone. An unset admin list denies everyone.

**Why:** *unconfirmed.*

**Consequences:** with no `CAVE_BOT_ADMIN_IDS` configured, nobody can switch
personas. This fail-closed behaviour is covered by a test.

**Date:** on or before 2026-07-13 (`73a070c`).

---

## D5 — AI replies are opt-in

**Decided:** `CAVE_BOT_AI_ENABLED` defaults to off. When off, a persona reply
returns the persona's greeting plus "(I can't answer questions yet.)".

**Why:** *unconfirmed.* The effect is that no API cost is incurred by default.

**Consequences:** the bot is functional without an Anthropic API key.

**Date:** on or before 2026-07-13 (`73a070c`).
