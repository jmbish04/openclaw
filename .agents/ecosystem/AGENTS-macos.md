# This Mac — services and connectivity

> Part of the workstation briefing. **`~/AGENTS.md` is the parent — read it first**;
> it carries the rules that apply everywhere (secrets, backups, how to write to
> Justin, when to flag). This file adds the rules for **macOS itself** — LaunchAgents, background services, and LAN connectivity.

---

# The launchd Local Network trap (macOS)

A LaunchAgent that reaches a LAN peer fine from your terminal but fails from
launchd with `errno 65 No route to host` is hitting a **per-binary** macOS Local
Network gate. A background job never gets the permission prompt, so the Python
enabled in System Settings is a *different* binary from the one the service runs.
Toggling Settings does nothing.

**Fix:** point the venv at an interpreter that already holds the grant
(`uv venv --python /opt/homebrew/bin/python3`) and pin `UV_PYTHON`,
`UV_PYTHON_DOWNLOADS=never`, `UV_PYTHON_PREFERENCE=only-system` in the plist, or
`uv run` re-resolves and the bug returns. **Never run
`sudo tccutil reset LocalNetwork`** — it wipes the one working grant machine-wide.

Full writeup, including the 5-row measurement table, three disproved hypotheses,
and the reproduce recipe: `~/.colby-ecosystem/reference/launchd-local-network-trap.md`.
Skill: `launchd-lan-connectivity-troubleshooting`.
