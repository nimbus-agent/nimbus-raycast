# Nimbus for Raycast

> **Retired and archived (2026-11-12).** This repository held a vision and a build prompt, and
> nothing was ever built here. A quick-ask launcher is not planned. The reason, and the condition that would reopen it, are in the Nimbus roadmap's [Rejected Directions](https://github.com/nimbus-agent/Nimbus/blob/main/docs/roadmap.md#rejected-directions).
> The text below is kept as it was written.

A [Raycast](https://raycast.com) extension — quick-ask [**Nimbus**](https://github.com/nimbus-agent/Nimbus) from anywhere on macOS.

> **Status: SCAFFOLD — not yet built.** This repo holds its vision and a [build prompt](./NEW-SESSION-PROMPT.md).

## What it will be

Nimbus runs as a headless local gateway; clients (CLI, desktop, VS Code) talk to it only over local JSON-RPC 2.0 IPC. A Raycast extension is a natural **thin client**: a hotkey to ask Nimbus a question, search your local index, and see streamed answers — without leaving what you're doing.

- **Ask Nimbus** — a natural-language query, streamed back inline.
- **Search Index** — fast lookup across your indexed files, PRs, incidents, docs, people.
- **HITL aware** — if an answer would trigger a destructive/outbound action, the structural consent gate surfaces an approval prompt; the extension handles it rather than silently failing.

It connects to the local gateway socket via the typed wrapper [`@nimbus-dev/client`](https://github.com/nimbus-agent/Nimbus/tree/main/packages/client) (MIT). macOS-first, since Raycast is macOS-only.

## Scope

A read/ask-oriented client. It deliberately avoids RCE-class IPC methods (the same posture as the Tauri renderer allowlist, invariant I7) — it asks, searches, and relays consent prompts; it does not expose vault or extension-install surfaces.

## License

[MIT](./LICENSE).
