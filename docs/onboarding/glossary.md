# AKARI 用語集

> **正典 (Layer A)**: 本ディレクトリは AKARI Onboarding の公開正典です。
> Hub (`akari-os/docs/onboarding/`) は同期コピー（編集は本ディレクトリで行う）。
>
> **対象読者**: AKARI のドキュメント / コード / 議論を読むときにリファレンスとして開く全員
> **所要時間**: 5 分（必要な語だけ引く）
> **前提知識**: 不要。逆に、本ファイルが「他のドキュメントを読むための前提」を作ります
> **次に読むもの**: [`concept-map.md`](./concept-map.md)（全体像） / 公開リポの README

---

## 使い方

- カテゴリ別にまとめています（**コア概念** / **アプリ** / **AI 系** / **基盤・プロトコル** / **データ系** / **ガバナンス** / **ライセンス層**）
- 各語は 1〜2 行の最小定義 + **正典へのリンク** を持ちます。**ここで読み切るのではなく、正典へ飛ぶための索引**として使ってください
- アルファベット順を併用したい場合は本ファイル内検索（Cmd/Ctrl+F）を使ってください

---

## 🌟 コア概念

### AKARI / AKARI OS

個人クリエイターのための AI OS。スマホに iOS があるように、クリエイター PC に AKARI OS がある。
ホスト OS（Mac / Windows / Linux）の上に被さる **オーバーレイ型メタ OS**。
> [`../../VISION.md`](../../VISION.md)

### Shell

AKARI の「器」。Tauri v2 + React で動く 1 つのホストアプリ。App を動的ロードする。
> [`Akari-OS/shell`](https://github.com/Akari-OS)（段階公開）

### Pool

全アプリ共有の Knowledge Store（素材 + 文脈の SSOT）。Rust + SQLite + FTS5 + MCP。
> 公開ドキュメント: [`Akari-OS/pool`](https://github.com/Akari-OS)

### Work（仕事の単位）

ユーザーが「一つの目的」で作る成果物の集合体（記事 1 本 / 動画 1 本 / キャンペーン 1 つ）。
複数の App をまたぐ単位（`Work.apps[]`）。

### Asset（途中成果物・素材）

Work に投入される再利用可能な部品（画像 / 動画 / 音 / テキスト / スライド / 引用）。
**immutable**（取り込んだら書き換えない）。

### Variant（最終出力フォーマット）

Work の最終出力単位（X 投稿用 / 縦長動画 / 短文版）。
**Variant が出力単位**であり、Asset の修正は Variant-level override として保存される。

### Library

Pool 内の素材の整理単位（workspace tier）。テンプレート / outputs / 取り込み素材などを分けて管理。

### Personal Pool

ユーザー 1 人につき 1 つの Pool（声色 / 嗜好 / ブランディング素材）。
**アンビエント**として全 Work に常時 attach される（Work scope から外す）。

### Campaign

Pool 内の素材をタグ的にまとめるサブグループ層（Pool ではない）。

### Scratch Work

「迷ったらここ」の使い捨て Work。Pool を選ばずにすぐ書き始めるための受け皿。

---

## 📱 アプリ

### Writer

公式テキストエディタ App（Full Tier）。140/280 字 SNS から長文記事まで統合。Tiptap ベース。
> [`Akari-OS/writer`](https://github.com/Akari-OS)（段階公開）

### Video

動画編集 App（Full Tier）。Tauri + React + ffmpeg。SNS 比率別 export / 字幕 / Pool 連携。
> [`Akari-OS/video`](https://github.com/Akari-OS)（段階公開）

### Design

Canva ライク汎用デザイン App（Full Tier）。SNS / 印刷物 / プレゼン / Web を 1 マスター → AI 派生で展開。
> [`Akari-OS/design`](https://github.com/Akari-OS)（段階公開）

### Voice

コミュニティフィードバック App。`voice.akari-oss.app` で公開中。
> [voice.akari-oss.app](https://voice.akari-oss.app)

### Pool Browser

Pool 内素材の閲覧・検索 App（Shell 内軽量 viewer）。

### Hunter Test / Tomosu / Drop Helper 等

AKARI 以外の自社プロダクト。AKARI エコシステム外。

---

## 🤖 AI 系

### Agent（エージェント）

LLM が着る「コスチューム」（システムプロンプト + コンテキスト + tool セット）。
**仕様は固定ファイル / 実行は揮発**（呼ばれて起動 → 仕事して死ぬ）。
> [`../../VISION.md`](../../VISION.md) §CAA — Costume Agent Architecture

### Reference Agents（7 人）

OS が何もアプリを入れていない状態でのデフォルト顔ぶれ。**固定ではなく着せ替え可能**。
Partner / Studio / Operator / Researcher / Guardian / Memory / Analyst の 7 人。
> [`../../VISION.md`](../../VISION.md) §あなた専属の AI チーム

### Partner（パートナー）

ユーザーの窓口エージェント。指示を受けてチームに振る。
> [`../../VISION.md`](../../VISION.md)

### Skill

App / Agent が提供する再利用可能な能力単位（プロンプト + tool ハンドラ + メタデータ）。

### Pipeline

Writer 等が提供する **承認駆動型 4 Phase ループ**（A 設計 → B 生成 → C QA → D 公開）。

### Workflow

Pipeline をユーザーが編集できる可視化編集グラフ（フローチャート）。

### 3P Loop

提案（Propose）→ 方向（Point）→ 仕上げ（Polish）の 3 フェーズ往復ループ。AKARI の作業単位。
> [`../../VISION.md`](../../VISION.md) §3P Loop

### CAA — Costume Agent Architecture

「専門エージェントは実在しない、コスチュームだけが存在する」という設計原則。
状態管理を **思考層 / 道具層 / 記憶層** の 3 層に分離する根拠。
> [`../../VISION.md`](../../VISION.md) §CAA

---

## 📡 基盤・プロトコル

### App SDK

AKARI Core と App の境界契約（Apple の HIG + UIKit に相当）。
**7 つの API 群** — Agent / Memory / Context / UI / Inter-App / Permission / Skill。
> [`Akari-OS/sdk`](https://github.com/Akari-OS)

### App Tier

App の参入コスト分類: **Full Tier**（ネイティブ要件あり）/ **MCP-Declarative Tier**（MCP + `panel.schema.json` だけで動く軽量）。
> [`Akari-OS/sdk`](https://github.com/Akari-OS)

### MCP（Model Context Protocol）

ツール呼び出しの外部標準（Anthropic 主導）。AKARI も全共有層を MCP サーバーとして露出。
> [modelcontextprotocol.io](https://modelcontextprotocol.io)

### M2C（Media to Context）

メディア → コンテキスト変換のプロトコル。動画・音声・画像から AI 用コンテキストを抽出。
> [`Akari-OS/m2c`](https://github.com/Akari-OS)

### AMP (Agent Memory Protocol)

エージェント記憶の保存・検索・減衰を標準化するプロトコル。
> [`Akari-OS/amp`](https://github.com/Akari-OS)

### ACE（Agent Context Engineering）

コンテキストの組み立て方と品質 Lint を標準化するフレームワーク。
> [`Akari-OS/ace`](https://github.com/Akari-OS)

### Memory Layer / Semantic Layer

AKARI Core の 5 層中の 2 層。
- Memory Layer = Pool（素材 + 文脈）+ AMP（エージェント記憶）
- Semantic Layer = M2C（特徴量抽出）+ ACE（コンテキスト構築）
> [`../../VISION.md`](../../VISION.md) §AKARI は "OS" である

### Panel Schema

Shell に App UI を宣言する schema（`panel.schema.json`）。MCP-Declarative Tier の核。
> [`Akari-OS/sdk`](https://github.com/Akari-OS)

---

## 💾 データ系

### StorageMode

Asset を Pool に登録するときの保存方式:

| モード | Semantics |
|---|---|
| **`reference`** (既定) | ユーザー元パスを参照のみ（copy しない） |
| **`copy`** | Pool 配下に実ファイル複製（quota 対象） |
| **`auto`** | サイズ閾値で自動振り分け（既定 < 10MB → copy） |

### dangling reference

`reference` モードで元ファイルが移動・削除された状態。Pool browser が lazy detect してバッジ表示。

### Tiered Storage（Hot / Warm / Cold）

素材ストレージの階層化（HOT: ローカル SSD / WARM: 外部 SSD・NAS / COLD: クラウド）。
M2C 特徴量があれば cold にあっても AI 検索可能。
> [`../../VISION.md`](../../VISION.md) §階層化ストレージ

### Provenance（来歴）

Asset / Variant がどの素材・どの編集で出来たかの履歴チェーン。Pool に記録。

### Workspace（Pool の workspace）

Pool 内の論理空間（旧 Pool 単位）。FTS5 インデックスのスコープ。

---

## 🏛 ガバナンス

### Layer A / B / C — Placement Tiers（配置層モデル）

ドキュメントを **どこに置くか** の 3 分類:

- **Layer A**: Canonical（公開正典） → `Akari-OS/.github/`（本リポ）
- **Layer B**: Hub（ローカル作業ハブ・非公開） → `akari-os/`
- **Layer C**: Repo-specific（各アプリ固有） → `<repo>/docs/`

### Schema / Wiki / Raw — 文書層モデル

ドキュメントを **誰が書くか** の 3 分類:

- **Schema** = `CLAUDE.md`（人間が書く規約）
- **Wiki** = INDEX.md / 整形 doc（LLM が maintain 可）
- **Raw** = `raw/` 配下（人間が curate する一次情報・不変）

### SSOT（Single Source of Truth）

同じ情報を 2 箇所に書かない原則。必要時は「正典 + 同期コピー」を明示。

### SDD（Spec-Driven Development）

仕様駆動開発。spec ファーストで書き、未ドキュメント機能は逆算 spec 化する。

### spec-id

仕様の追跡 ID（例: `AKARI-HUB-024`）。実装 commit / PR に含めて traceability を担保。

### ADR（Architecture Decision Record）

設計判断の記録。`ADR-{NNN}-{topic}.md` 形式で保存。状態遷移は draft → review → accepted → implemented → deprecated。

### handoff

セッション間の引き継ぎ記録（`handoff-YYYY-MM-DD-sessionNN.md`）。
当日の thread / commit / 残課題 / 次セッション提案を含む（運用の詳細は Hub 内部）。

### 配置層 vs 文書層 — 「3 層モデル」用語

「3 層モデル」は **2 つの軸** で出てくる:

- 配置層 = Layer A/B/C（**どこに置くか**）
- 文書層 = Schema/Wiki/Raw（**誰が書くか**）

### Generated Wiki

機械生成された Wiki 層。codegen 出力（型定義 / OpenAPI 等）。手書き禁止、SSOT は spec / schema 側。

---

## ⚖️ ライセンス層（4 層モデル / Open Core 戦略）

| Layer | License | 対象 |
|---|---|---|
| **Layer 1** | proprietary（All Rights Reserved + EULA） | `cloud` / `lp` / `materials`（収益源）+ Hub（永続 private 作業材料） |
| **Layer 2** | Apache 2.0 | `shell` / `pool-impl` / `agents` / `m2c` / `amp` / `ace` |
| **Layer 3** | MIT | `sdk` / `design` / `writer` / `video` / `voice` |
| **Layer 4** | CC BY 4.0 | `dotgithub`（本リポ） / `pool-docs` |

**禁止事項**: AGPL の新規採用 / 4 層から外れたライセンス（GPL / SSPL / BSL 等）の独自採用 / Layer 1 リポへの OSS license 付与。

> 公開版 LICENSE: [`../../LICENSE`](../../LICENSE)

---

## 🔗 一覧で開きたい入口

- 全体像: [`concept-map.md`](./concept-map.md)
- FAQ: [`faq.md`](./faq.md)
- Vision: [`../../VISION.md`](../../VISION.md)
- Roadmap: [`../../ROADMAP.md`](../../ROADMAP.md)
- Akari-OS org: [github.com/Akari-OS](https://github.com/Akari-OS)
