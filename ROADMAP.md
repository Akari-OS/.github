# Roadmap — Akari-OS

Akari-OS エコシステム全体の段階的ロードマップ。各アプリの個別フェーズは、
それぞれのリポジトリの README / docs を参照。

---

## 全体戦略

**方針**: Phase は順番に着実に全部やり切る。並列化や飛ばしはしない。
1 つの基盤が固まってから次に進む。

> **2026-04-09 方針転換**: 旧 Phase は Video First 路線だったが、
> Shell + Module モデルの採用と Platform-First 哲学に基づき、
> Phase 0 を Writer First + プラットフォーム骨格に変更。Akari Video は
> Phase 3 で Shell アーキ上に移植される（既存コードは維持）。

```
Phase 0: プラットフォーム骨格 + Tiny Writer Module + X 投稿
Phase 1: MCP Hub + Home Module + オーケストレーション可視化
Phase 2: AKARI Writer 拡張 + 複数 Module 連携
Phase 3: Akari Video を Shell アーキ上に移植
Phase 4: 自律化（夜間ジョブ + Memory 横断）
Phase 5: エコシステム拡張（サードパーティ Module 受け入れ）
```

---

## Phase 0: プラットフォーム骨格 + Tiny Writer Module（現在進行中）

> **目標**: "仕組みが動いている" ことを 1 ループで証明する。
> 「X に 140 字投稿する」という最小ループが Akari Shell → Agent daemon →
> MCP → X まで通ればプラットフォームの最小証明が完成する。

| 項目 | リポジトリ | 状態 |
|---|---|:-:|
| **Akari Shell** (Tauri + React) | 将来 `Akari-OS/shell` | ⬜ 設計完了、実装開始 |
| **Agent daemon** (L1 最小 4 人: Partner / Studio / Memory / Operator) | 将来 `Akari-OS/agents` | ⬜ 設計中 |
| **MCP クライアント基盤** | agents 内 | ⬜ |
| **Tiny Writer Module** (140/280 字 X 投稿専用) | Shell 内 | ⬜ spec 完了 |
| Universal Knowledge Store 基盤 | [pool](https://github.com/Akari-OS/pool) | 🚧 Phase 4.5 |
| Media-to-Context プロトコル | [m2c](https://github.com/Akari-OS/m2c) | 🚧 v0.2 Draft |
| Agent Memory Protocol | [amp](https://github.com/Akari-OS/amp) | 🚧 v0.1 Draft |
| クラウドバックエンド | [cloud](https://github.com/Akari-OS/cloud) | 🚧 Phase 4 完了 |
| ランディングページ | [lp](https://github.com/Akari-OS/lp) | 🚧 |
| コミュニティフィードバック | [voice](https://github.com/Akari-OS/voice) | 🚧 |
| **Akari Video** (独立アプリ、Phase 0 は維持のみ) | [video](https://github.com/Akari-OS/video) | 🚧 維持のみ (Phase 3 移植予定) |

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
| 7: Shell Module 統合 | Shell Module (Pool Browser / Writer 等) から pool-core 利用 | ⬜ |
| 8: エコシステム拡大 | Shell Module 生態系の成熟（Bundled Module 配布） | ⬜ |

---

## Phase 1: MCP Hub + Home Module + オーケストレーション可視化

> **目標**: ユーザーが自分で MCP サーバを追加でき、ダッシュボードで全体を俯瞰できるようにする。

- [ ] **MCP Hub Module** — ユーザーが自分で MCP サーバを追加・管理
- [ ] **Home Module** — 7 エージェントの状態・ジョブ・サマリの俯瞰
- [ ] **Flow 可視化 Module** — L1/L2/L3 の協調を "見える化"
- [ ] 他 SNS 対応（公式 MCP が出次第、Instagram / Threads / Bluesky 等）
- [ ] Pool Browser Module / Memory Viewer Module（最小版）

---

## Phase 2: AKARI Writer 拡張 + 複数 Module 連携

> **目標**: Tiny Writer Module を AKARI Writer へ段階的に拡張し、複数 Module 間連携を成立させる。

- [ ] AKARI Writer を長文・本・アウトライン・Pool 検索対応に
- [ ] Module 間ハンドオフ（Writer で書いた文章が Memory Viewer / Analyst に反映）
- [ ] Analyst Reports Module
- [ ] Guardian の本格稼働（公開前チェック・ブランドガード）
- [ ] L3 サブエージェント拡充（Studio 配下の writer-sub, researcher-sub 等）

---

## Phase 3: Akari Video を Shell アーキ上に移植

> **目標**: 既存の Akari Video 資産を Phase 0-2 で固まった Shell + Agent アーキの上に載せる。

- [ ] Akari Video (new arch) — 独立アプリのまま、Agent daemon / Pool / AMP を共有
- [ ] Video → Writer 連携（動画のシーン要約 → 記事 / SNS 投稿に派生）
- [ ] Studio/video-editor-sub を L3 サブエージェントとして統合
- [ ] M2C Layer 2 を本格活用（動画コンテキスト化の完全実装）

---

## Phase 4: 自律化

> **目標**: 寝てる間に仕事が進む状態を作る。

- [ ] **Backstage Job System**（夜間自律実行）
- [ ] Memory Store の Module 横断共有
- [ ] 承認フロー + 朝のブリーフィング
- [ ] Pool の Filed back ループ（関係性自動推論）

---

## Phase 5: エコシステム拡張

> **目標**: サードパーティが Bundled Module を寄与できる OS へ。

- [ ] **Module SDK** の公式化（サードパーティが Module を書けるドキュメント + CLI）
- [ ] **Module マーケットプレイス**（公式 / コミュニティ Bundled Module の配布・発見）
- [ ] プラグインシステム（Analyzer の外部登録）
- [ ] サードパーティ Module の sandbox 化（iframe 化マイグレーション）
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
