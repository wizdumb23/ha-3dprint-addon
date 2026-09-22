# ha-3dprint-addon

Session state: .claude/STATE.md (written by /handoff) — read it first when resuming; if absent, no handoff has run here yet.

A standalone HA addon for BambuBuddy — forked from
[Spegeli/homeassistant-app-bambuddy](https://github.com/Spegeli/homeassistant-app-bambuddy)
with no upstream tracking. The goal is to own the addon wrapper while continuing
to pull the BambuBuddy app from its upstream image
([maziggy/bambuddy](https://github.com/maziggy/bambuddy)).

## Why this fork exists

BambuBuddy's Virtual Printer feature does not work on HAOS. The VP services fail
to start. The root cause has not been fully confirmed yet — this repo exists to
investigate and fix it properly.

Do NOT arrive at conclusions before investigating. Read the Dockerfile and addon
structure first to understand what's happening before proposing changes.

## Environment

- HA install: Home Assistant OS running as a Hyper-V VM
- HA VM IP: `192.168.250.20` (eth1 / management), `10.1.1.12` (eth0 / LAN)
- This Ubuntu VM hosts Claude Code; edits flow via git commit/push → user pulls on HA VM
- SSHFS mounts: `/mnt/ha/config/` → HA VM `/homeassistant/`, `/mnt/ha/addon_configs/` → HA VM `/addon_configs/`
- SSH alias `ha` → `root@192.168.250.20:22222` using `~/.ssh/ha_ed25519`

## Currently installed addon

- Source repo forked from: `https://github.com/Spegeli/homeassistant-app-bambuddy`
- Installed slug: `33558673_bambuddy_daily`
- Addon data lives at: `/mnt/ha/addon_configs/33558673_bambuddy_daily/`
- SQLite DB: `/mnt/ha/addon_configs/33558673_bambuddy_daily/data/bambuddy.db`

## Virtual Printer — current state

The VP "VP Sharkie" (`id=1`) is configured in queue mode targeting the physical P2S.

- `bind_ip` was written directly to the SQLite DB as `192.168.250.220` — this was
  a workaround attempt during troubleshooting, not a confirmed fix
- VP is `enabled=0` — intentionally left disabled
- A secondary IP `192.168.250.220/24` was added to eth1 on the HA host persistently
  via `ha network update` and is confirmed present

## Known symptom

BambuBuddy logs show warnings from `backend.app.services.network_utils` at startup.
The VP bind interface dropdown in the UI does not show the alias IP `192.168.250.220`.
VP services do not start when the VP is enabled.

Investigate the addon and app code to understand why before touching anything.

## Workflow rules (same as main HA config project)

- Edits flow: Claude Code (here) → commit & push → user pulls on HA VM
- Claude never SSH-pulls on the HA VM — user owns the pull step
- Never modify `.storage/`
- Validate before committing

## Memory practices
- The user runs concurrent Claude Code sessions across repos/projects (e.g. this repo + homeassistant-config at the same time) — memory is the shared thread that keeps them honest with each other
- Keep memory constantly up to date: after any meaningful change, decision, or discovery, write/update the relevant memory file (don't batch it for "later")
- Check memory regularly during a session, not just at the start — especially before git operations (commit/push/reconcile) where another concurrent session may have already moved the remote
- Memory reflects point-in-time notes, not live state — always verify against git/the actual repo before assuming a memory is still current

## graphify

This project has a knowledge graph at graphify-out/ with god nodes, community structure, and cross-file relationships.

Rules:
- For codebase questions, first run `graphify query "<question>"` when graphify-out/graph.json exists. Use `graphify path "<A>" "<B>"` for relationships and `graphify explain "<concept>"` for focused concepts. These return a scoped subgraph, usually much smaller than GRAPH_REPORT.md or raw grep output.
- If graphify-out/wiki/index.md exists, use it for broad navigation instead of raw source browsing.
- Read graphify-out/GRAPH_REPORT.md only for broad architecture review or when query/path/explain do not surface enough context.
- After modifying code, run `graphify update .` to keep the graph current (AST-only, no API cost).
## Workflow: Forge
This project uses **Forge** for non-trivial work — the framework at
`~/projects/claude-harness-loop/` (the `/forge` lever). Mid-brainstorm, `/forge`
captures the conversation, frames it into a pieces-board (HAVE/BUILD/UNKNOWN) + a
quantifiable goal, runs an advisor-refined interview, freezes a spec, then builds +
verifies. Efforts land in `efforts/<slug>/` here and mirror to
`~/projects/claude-harness-loop/efforts/`. (Forge supersedes specforge.)
