# Roadmap — Akari-OS

Akari-OS エコシステム全体の段階的ロードマップ。各アプリの個別フェーズは、
それぞれのリポジトリの README / docs を参照。

---

## 全体方針

**MVP = AKARI Core 基盤そのもの**（Shell + Agent Runtime + Memory Layer + Semantic Layer + Protocol Suite）。

Writer を同時に作っているのは「基盤が本当に動くかを検証する reference implementation」だから。
基盤が固まれば、同じ契約で Video も SNS Sender も動く。

```
Phase 0 ─ Core 基盤 MVP + Tiny Writer（reference impl）    ← いまここ
Phase 1 ─ MCP Hub + Home App + オーケストレーション可視化
Phase 2 ─ AKARI Writer 拡張 + 複数 App 連携
Phase 3 ─ Video を Shell アーキ上に移植
Phase 4 ─ 自律化（夜間ジョブ + Memory 横断）
Phase 5 ─ エコシステム拡張（App SDK / サードパーティ受け入れ）
```

---

## Phase 0: Core 基盤 MVP + Tiny Writer（現在進行中）

> **目標**: "仕組みが動いている" ことを 1 ループで証明する。
> 「X に 140 字投稿する」という最小ループが AKARI Shell → Agent Runtime →
> MCP → X まで通れば Core 基盤の最小証明が完成する。
> Tiny Writer はこの基盤の上で動く reference implementation。

| 項目 | リポジトリ | 状態 |
|---|---|:-:|
| **AKARI Shell** (Tauri + React) | 将来 `Akari-OS/shell` | ⬜ 設計完了、実装開始 |
| **Agent Runtime**（Partner / Studio / Memory / Operator の 4 人最小構成） | 将来 `Akari-OS/agents` | ⬜ 設計中 |
| **MCP クライアント基盤** | agents 内 | ⬜ |
| **Tiny Writer App** (140/280 字 X 投稿専用) | Shell 内 | ⬜ spec 完了 |
| Universal Knowledge Store 基盤 | [pool](https://github.com/Akari-OS/pool) | 🚧 Phase 6 完了・Phase 7 準備中 |
| Media-to-Context プロトコル | [m2c](https://github.com/Akari-OS/m2c) | 🚧 v0.2 Draft |
| Agent Memory Protocol | [amp](https://github.com/Akari-OS/amp) | 🚧 v0.1 Draft |
| Agent Context Engineering Framework | [ace](https://github.com/Akari-OS/ace) | 🆕 v0.1-draft 公開 (2026-04-14) |
| クラウドバックエンド | [cloud](https://github.com/Akari-OS/cloud) | 🚧 Phase 4 完了 |
| ホームページ | [lp](https://github.com/Akari-OS/lp) | 🚧 |
| **Akari Video**（独立アプリ、Phase 0 は維持のみ） | [video](https://github.com/Akari-OS/video) | 🚧 維持のみ（Phase 3 移植予定） |

---

## Phase 1: MCP Hub + Home App + オーケストレーション可視化

> **目標**: ユーザーが自分で MCP サーバを追加でき、ダッシュボードで全体を俯瞰できるようにする。

- [ ] **MCP Hub App** — ユーザーが自分で MCP サーバを追加・管理
- [ ] **Home App** — 7 エージェントの状態・ジョブ・サマリの俯瞰
- [ ] **Flow 可視化 App**（最小版） — オーケストレーションの状態を "見える化"
- [ ] 他 SNS 対応（公式 MCP が出次第、Instagram / Threads / Bluesky 等）
- [ ] Pool Browser App / Memory Viewer App（最小版）

---

## Phase 2: AKARI Writer 拡張 + 複数 App 連携

> **目標**: Tiny Writer App を AKARI Writer へ拡張し、複数 App 間連携を成立させる。

- [ ] AKARI Writer — 長文・アウトライン・Pool 検索対応
- [ ] App 間ハンドオフ（Writer で書いた文章が Memory Viewer / Analyst に反映）
- [ ] Analyst Reports App
- [ ] Guardian の本格稼働（公開前チェック・ブランドガード）
- [ ] Agent 追加（Studio 配下の専門エージェント）

---

## Phase 3: Akari Video を Shell アーキ上に移植

> **目標**: 既存の Akari Video 資産を Phase 0-2 で固まった Shell + Agent アーキの上に載せる。

- [ ] Akari Video (new arch) — 独立アプリのまま、Agent Runtime / Pool / AMP を共有
- [ ] Video → Writer 連携（動画のシーン要約 → 記事 / SNS 投稿に派生）
- [ ] video-editor サブエージェント統合
- [ ] M2C Layer 2 を本格活用（動画コンテキスト化の完全実装）

---

## Phase 4: 自律化

> **目標**: 寝てる間に仕事が進む状態を作る。

- [ ] **Backstage Job System**（夜間自律実行）
- [ ] Memory Store の App 横断共有
- [ ] 承認フロー + 朝のブリーフィング
- [ ] コンテキスト圧縮（ToMe / LLMLingua-2 / プロンプトキャッシュ 3 段ロケット）

---

## Phase 5: エコシステム拡張

> **目標**: サードパーティが App を寄与できる OS へ。

- [ ] **App SDK 正式リリース** — サードパーティが App を書けるドキュメント + `akari-app-cli`
  - **Full Tier** / **MCP-Declarative Tier** の 2 Tier 制度（参入コストを大幅削減）
  - `akari.toml` manifest + 3 層 Certification（Lint / Contract Test / Manual Review）
- [ ] **Panel Schema v1 安定化** — UI 定義スキーマの正式リリース
  - Widget Catalog 拡充、式言語確定
- [ ] **Tiered Storage 本番化** — Hot / Warm / Cold + Storage Backend Adapter
  - ローカル / 外部 SSD / NAS / akari-cloud / Google Drive / S3 対応
  - 安い PC でも全素材ライブラリを扱える
- [ ] **Declarative Capability Apps 拡充** — Publishing / Documents / Design / Asset Generation 等 11 カテゴリ参考実装
  - Publishing（SNS: X / LINE / Note / Threads / Bluesky 等）
  - Documents（Microsoft 365 / Google Workspace / Notion / Airtable 等）
- [ ] **App マーケットプレイス** — 公式 / コミュニティ App の配布・発見
- [ ] **Skill / テンプレート配布** — ユーザーが専門エージェントを共有できる
- [ ] **AKARI Box / Station** — 軽量推論デバイス / GPU 推論サーバーのハードウェア検討

---

## フェーズ間の共通原則

いつの時点でも守るもの:

1. **ローカルファースト** — データはユーザーのもの。オフラインで全機能動作
2. **MVP = Core 基盤** — Writer / Video は reference impl であって、基盤の主役ではない
3. **SSOT** — VISION は `Akari-OS/.github/VISION.md` が正典

---

## 貢献したい方へ

- 🗂 [Issues](https://github.com/Akari-OS) — 各リポの Issue を見てください
- 🤝 [Contributing Guide](./CONTRIBUTING.md) — 貢献方法
- 💬 GitHub Discussions — 大きな方向性の議論はここで
- 📖 [Vision](./VISION.md) — なぜこれを作っているか

**特に協力が欲しい領域**:

- Pool の Analyzer プラグイン（Video / Audio / PDF / Code 等）
- M2C / AMP プロトコル仕様への feedback
- App SDK の設計提案
- ドキュメント・翻訳の改善
