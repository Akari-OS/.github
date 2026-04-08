# Roadmap — Akari-OS

Akari-OS エコシステム全体の段階的ロードマップ。各アプリの個別フェーズは、
それぞれのリポジトリの README / docs を参照。

---

## 全体戦略

**方針**: Phase は順番に着実に全部やり切る。並列化や飛ばしはしない。
1 つの基盤が固まってから次に進む。

```
Phase 0: 基盤        ── Pool + Video MVP
Phase 1: 2 アプリ連携 ── Video + CMS
Phase 2: 自律化      ── Backstage Job System
Phase 3: エコシステム ── サードパーティ + マーケット
```

---

## Phase 0: 基盤（現在進行中）

> **目標**: 動画編集アプリと汎用 Knowledge Store を完成させ、エコシステムの基盤を作る。

| 項目 | リポジトリ | 状態 |
|---|---|:-:|
| Universal Knowledge Store 基盤 | [pool](https://github.com/Akari-OS/pool) | 🚧 Phase 4.5 |
| 動画編集 MVP (CLI + GUI + MCP) | [video](https://github.com/Akari-OS/video) | 🚧 Step 2 完了 |
| Media-to-Context プロトコル | [m2c](https://github.com/Akari-OS/m2c) | 🚧 v0.2 Draft |
| Agent Memory Protocol | [amp](https://github.com/Akari-OS/amp) | 🚧 v0.1 Draft |
| クラウドバックエンド | [cloud](https://github.com/Akari-OS/cloud) | 🚧 Phase 4 完了 |
| ランディングページ | [lp](https://github.com/Akari-OS/lp) | 🚧 |
| コミュニティフィードバック | [voice](https://github.com/Akari-OS/voice) | 🚧 |

### Pool の Phase 詳細

| Phase | 内容 | 状態 |
|---|---|:-:|
| 0: 基盤 | プロジェクト構造、設計書、Cargo workspace | ✅ |
| 1: pool-core | SQLite スキーマ、CRUD、基本 CLI | ✅ |
| 2: workspace | Workspace 隔離、横断検索 | ✅ |
| 3: analyzer | Analyzer trait、ArticleAnalyzer MVP | ✅ |
| 3.5: 制限解消 | FTS5 trigram、HTML 対応、LLM リトライ | ✅ |
| 4: wiki + lint | Wiki compile、Relations CRUD、Linter (2/4 種) | ✅ |
| **4.5: lint 完成** | **Inconsistency / ConnectionGap + Relation CLI** | 🟡 **次** |
| 5: filed back | Obsidian 双方向同期、関係性自動推論 | ⬜ |
| 6: MCP server | rmcp で MCP server 化 | ⬜ |
| 7: Video 統合 | Video 側を pool-core 依存に切り替え | ⬜ |
| 8: 別アプリ実装 | AkariCMS / AkariNotes / AkariSearch | ⬜ |

### Video の Step 詳細

| Step | 内容 | 状態 |
|---|---|:-:|
| 1: CLI + GUI + SKILL.md | akari-cli で全操作、GUI は人間用、SKILL.md で AI 連携 | ✅ |
| 2: MCP サーバー追加 | 109 ツールを MCP で公開 | ✅ |
| 3: パートナー（内蔵 AI）追加 | GUI 内 AI チャット。視覚的指示対応 | ⬜ |
| 4: AI OS 化 | 複数アプリ連携、7 エージェント体制、夜間自律実行 | ⬜ |

---

## Phase 1: 2 アプリ連携

> **目標**: Video と CMS を連携させ、「アプリ間でローカル直結」を実証する。

- [ ] **Akari CMS** を新規開発（Pool の記事ビュー）
- [ ] Video → CMS の自動連携（動画 → 記事化）
- [ ] 7 エージェント体制の UI 実装
- [ ] Memory Store の初期実装

---

## Phase 2: 自律化

> **目標**: 寝てる間に仕事が進む状態を作る。

- [ ] **Backstage Job System**（夜間自律実行）
- [ ] Memory Store のアプリ横断共有
- [ ] 承認フロー + 朝のブリーフィング
- [ ] Pool の Filed back ループ（関係性自動推論）

---

## Phase 3: エコシステム

> **目標**: サードパーティが拡張できる OS へ。

- [ ] サードパーティアプリの受け入れ（Akari SDK）
- [ ] マーケットプレイス（Akari Cloud 上）
- [ ] プラグインシステム（Analyzer の外部登録）
- [ ] **Akari Box**（軽量推論デバイス）/ **Akari Station**（GPU 推論サーバー）のハードウェア化検討

---

## コンテキスト表現の段階的進化

データの内部表現も並行して進化させる：

| Phase | Layer | 内容 | 状態 |
|---|---|---|:-:|
| 現在 | Layer 2 | 構造化 JSON（M2C） | 🚧 |
| 次 | Layer 2 拡張 | ToMe + LLMLingua-2 圧縮 | ⬜ |
| 将来 | Layer 3 | Qwen3-VL-Embedding ベクトル | ⬜ |

---

## 貢献したい方へ

- 🗂 [Issues](https://github.com/Akari-OS) — 各リポの Issue を見てください
- 🤝 [Contributing Guide](./CONTRIBUTING.md) — 貢献方法
- 💬 GitHub Discussions — 大きな方向性の議論はここで
- 📖 [Vision](./VISION.md) — なぜこれを作っているか

**特に協力が欲しい領域**:

- Pool の Analyzer プラグイン（Video / Audio / PDF / Code 等、モダリティごと）
- Video の UI 改善（React + Tailwind）
- M2C / AMP プロトコル仕様への feedback
- 翻訳（英語版ドキュメント）
- ドキュメント改善全般
