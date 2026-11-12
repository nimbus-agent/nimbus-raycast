# Build prompt — `nimbus-raycast`

> **Historical.** This build prompt is kept as written. The repository was retired on 2026-11-12;
> nothing should be built from it. See the note at the top of `README.md`.

Copy everything below into a fresh Claude Code session opened in a clone of `nimbus-agent/nimbus-raycast`, with the main `nimbus-agent/Nimbus` repo available locally for reference.

---

I'm building **`nimbus-raycast`**, a Raycast extension that is a thin client over the local Nimbus gateway for quick "ask Nimbus" and index search on macOS.

**Read first in the main Nimbus repo so the IPC usage is correct:**
- `packages/client/` — the `@nimbus-dev/client` typed IPC wrapper: how it connects to the gateway socket and which methods/notifications it exposes. The extension should use this rather than re-implementing the transport.
- The skill `.claude/commands/nimbus-ipc.md` — the JSON-RPC method namespaces, the notification/streaming contract, and which methods are safe for a thin client.
- The CLI ask/search path under `packages/cli/src` — how it calls the gateway and consumes a streamed answer, and how it surfaces HITL consent prompts.
- Note the Tauri allowlist posture (invariant I7) — mirror it: ask/search only, no RCE-class methods (no `vault.*`, no `extension.install`).

**Goal:** hotkey → ask/search Nimbus → streamed result, with consent prompts handled gracefully.

**Deliverables:**
1. A Raycast extension (`@raycast/api` + React + TypeScript) with commands: **Ask Nimbus** and **Search Index** (plus optionally "recent results").
2. Connect to the local gateway via `@nimbus-dev/client`; **stream** the answer using the gateway's notification/streaming contract.
3. **HITL handling** — when an action requires consent, render the approval prompt and route the user's decision back.
4. A graceful **"gateway not running"** state with guidance to start it.
5. Publish-ready for the Raycast store (manifest, icons, metadata).

**Acceptance criteria:**
- With a running local gateway, "Ask Nimbus" streams a real answer; "Search Index" returns indexed results.
- A query that would trigger a gated action surfaces the consent prompt (not a silent failure).
- Only read/ask methods are used — no RCE-class IPC surface.

**Process:** brainstorm → spec → plan first. Confirm the exact streaming + consent IPC contract against `@nimbus-dev/client` and the `nimbus-ipc` skill before building. Verify against a real running gateway before opening the PR. License is MIT.
