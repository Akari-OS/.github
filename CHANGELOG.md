# Changelog — Akari-OS

公開正典（`Akari-OS/.github`）の更新履歴。

---

## 2026-04-22 — Governance & Visibility Overhaul

### Added
- **Ownership unification**: all AKARI code consolidated under `Akari-OS` org. Former personal org repos migrated.
- **Visibility 3-tier policy**: 永続 public (protocol specs + SDK) / 永続 private (strategy/credentials) / 段階公開 (implementation repos)
- **Governance SOPs**: `publish-workflow.md`, `new-public-repo-checklist.md`, `visibility-policy.md`
- `voice` and `lp` designated as permanent private (competitive copy/UX protection)

### Changed
- Repo renames: `(former personal org)/akari-os` → `Akari-OS/hub`, `(former personal org)/akari-pool` → `Akari-OS/pool-impl` (public docs: `Akari-OS/pool`)
- `akari-cloud` / `akari-lp` moved to `Akari-OS` (private)

### Archived
- `(former personal org)/agent-memory-protocol` (正典は `Akari-OS/amp`)
- `(former personal org)/m2c-protocol` (正典は `Akari-OS/m2c`)
- `(former personal org)/memu-v3-mcp` (ecosystem 外)

Hub SOP: see private `akari-os` repository's `docs/governance/`.

---

## 2026-04-19 — VISION v2 Release

- iOS metaphor and 5-layer architecture officially adopted
- 3P Loop (Propose → Point → Polish) introduced
- 7 agents redefined as "reference defaults" (swappable, not fixed)
- Video First direction retired. AkariVideo is one of several official apps
- MVP redefined: AKARI Core foundation itself (Writer is a reference implementation)
- App SDK with Tier system (Full / MCP-Declarative) announced
- CAA (Costume Agent Architecture) concept introduced
- Edge-Native positioning clarified
- Tagline updated: "AI っぽくない。あなたらしさを。" / "Not AI-ish. Yours."

---

## 2026-04-14 — ACE v0.1-draft 公開

- ACE (Agent Context Engineering Framework) v0.1-draft 公開
- AKARI Protocol Suite に ACE を追加（4 番目のプロトコル）

---

## 2026-04-09 — Platform-First 転換

- Phase 0 を Video First から Platform-First（Core 基盤 MVP）に転換
- Shell + App モデルの採用
- Akari Video は Phase 3 で Shell アーキ上に移植する方針に変更

---

## 2026-04-09 以前

- pool: Phase 1-6 完了（Universal Knowledge Store 基盤確立）
- amp: v0.1 Draft 公開
- m2c: v0.2 Draft 進行中
- cloud: Phase 4 完了（認証 / クレジット / マーケット基盤）
