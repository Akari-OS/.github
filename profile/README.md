# Akari-OS

> **Your personal AI team, visible at work.**
> A local-first, AI-native app constellation for independent creators.

---

## What is Akari-OS?

Akari-OS is an ecosystem of specialized apps — video editor, CMS, knowledge store, and more — that run locally and talk to each other. Instead of one giant AI-powered app, you get a **team of AI agents** working across apps, sharing memory, and seeing the same context.

- 🎬 **Local-first** — your data never leaves your machine
- 🧠 **AI-native** — every app understands context from day one
- 🔗 **Constellation** — apps are independent but share a common data and memory layer
- 🆓 **Open source** — AGPL / Apache licensed, always free to run

## The 3-layer AI stack

Most "AI-powered" tools only touch the prompt layer. Akari-OS is built on all three:

```
┌─────────────────────────────────────────┐
│ Harness       — how AI is orchestrated  │
│ Job system, approval flow, autonomy     │
├─────────────────────────────────────────┤
│ Context       — what AI sees            │
│ M2C, Pool, Memory (AMP)                 │
├─────────────────────────────────────────┤
│ Prompt        — how AI is instructed    │
│ Skill manifests, agent definitions      │
└─────────────────────────────────────────┘
```

## Ecosystem

### 🧬 Core layer — protocols and shared data

| Repository | What it is |
|---|---|
| **[pool](https://github.com/Akari-OS/pool)** | Universal Knowledge Store. SQLite + FTS5 + Analyzer plugins. MCP server. |
| **[m2c](https://github.com/Akari-OS/m2c)** | Media-to-Context protocol. Turns media into structured AI context. |
| **[amp](https://github.com/Akari-OS/amp)** | Agent Memory Protocol. Standardizes memory storage, retrieval, and decay. |

### 📱 Apps layer — what users interact with

| Repository | What it is |
|---|---|
| **[video](https://github.com/Akari-OS/video)** | Akari Video — desktop video editor (Tauri + Rust + React) |
| **[cloud](https://github.com/Akari-OS/cloud)** | Akari Cloud — optional backend for auth, credits, marketplace |
| **[voice](https://github.com/Akari-OS/voice)** | Community feedback + changelog |
| **[lp](https://github.com/Akari-OS/lp)** | Landing page (akari-oss.app) |

### 📚 Documentation

- **Vision** — the story and principles behind Akari-OS
- **Roadmap** — where we're going
- **Contributing** — how to help

## Philosophy

> **Akari-OS is not a tool. It's an OS where your personal AI team lives.**
> Open the app and they're all there. Already working. You just say "do this".

No configuration. No agent setup. No prompt engineering. Just a team that understands your work, remembers your preferences, and keeps going while you sleep.

## Status

🚧 **Early development.** Most repos are in active Phase development. Stars, issues, and PRs welcome on any repo.

## License

Each repo carries its own license:
- **pool**: AGPL-3.0
- **m2c**, **amp**: Apache-2.0
- **video**, **cloud**, **voice**, **lp**: see individual repos

---

*Built by [@KYO-kobo](https://github.com/KYO-kobo) and a growing community.*
