# AKARI FAQ — よくある質問

> **正典 (Layer A)**: 本ディレクトリは AKARI Onboarding の公開正典です。
> Hub (`akari-os/docs/onboarding/`) は同期コピー（編集は本ディレクトリで行う）。
>
> **対象読者**: AKARI に興味を持った全員（GitHub 訪問者 / エンドユーザー / 開発者）
> **状態**: 🟡 Preview — AKARI は開発初期のため、製品の進展に応じて回答も更新されます
> **次に読むもの**: 自分の立場に合った Quickstart（[`quickstart-visitor.md`](./quickstart-visitor.md) / [`quickstart-end-user.md`](./quickstart-end-user.md) / [`quickstart-developer.md`](./quickstart-developer.md)）

---

## 使い方

カテゴリ見出しから興味のある節へ飛んでください。各回答は 2-5 行に収めて、詳細リンクを 1 つだけ添えています。

- [製品全般](#製品全般)
- [ライセンス・料金](#ライセンス料金)
- [プライバシー・データ](#プライバシーデータ)
- [開発・貢献](#開発貢献)
- [技術仕様](#技術仕様)
- [雑多](#雑多)

---

## 製品全般

### **Q. AKARI とは何ですか？**

AKARI は **個人クリエイターのための AI OS** です。動画・記事・SNS 投稿といった創作作業を、共通の AI チーム・記憶層・プロトコルの上で動かせる **オーバーレイ型の環境 OS** を目指しています。
詳細: [`../../VISION.md`](../../VISION.md)

### **Q. 何ができますか？**

公式リファレンス実装として **Writer（テキスト）/ Video（動画編集）/ Design（汎用デザイン）/ Voice（フィードバック窓口）** が開発中です。素材を Pool に入れ、AI チームと一緒に成果物（Variant）を作って公開する、というのが基本フローです。
詳細: [`./quickstart-end-user.md`](./quickstart-end-user.md)

### **Q. 他のクリエイターツール（Notion / Canva / CapCut / Adobe 等）と何が違うのですか？**

AKARI は単一ツールではなく **「アプリが共通の基盤を共有する環境」** です。素材は Pool にローカル保管され、AI は「あなたが撮った / 書いた素材」を読んで叩き台を出します。**「素材があなたの手元にあるまま、AI が脇で働く」** という設計が他との違いです。
詳細: [`./quickstart-visitor.md` §3](./quickstart-visitor.md#3-なぜ他と違うか1-分)

### **Q. AI 動画生成（Sora / Runway 等）とは違うのですか？**

はい、根本的に違います。AI 動画生成は「AI がゼロから架空の映像を作る」のに対し、AKARI は「あなたの素材を AI が磨く」方向です。出てくるのは **あなたが作ったもの、を AI が仕上げたもの** です。
詳細: [`../../VISION.md`](../../VISION.md)

### **Q. 今すぐ使えますか？**

各アプリは **プレビュー段階** です。Pool 実装は Phase 6 完了、Writer は Phase 1 完了、Video は Phase 2 完了など、段階的に進めています。public 化のタイミングはリポごとに違います。
詳細: [Akari-OS org](https://github.com/Akari-OS)

### **Q. 対応プラットフォームは？**

実装の主対象は **macOS / Windows / Linux**（デスクトップ）です。Tauri v2 ベースなので 3 プラットフォームでビルドできます。iOS / Android のような mobile 向けは現時点で計画していません（エッジ性能を必要とする創作タスクが主用途のため）。

---

## ライセンス・料金

### **Q. ライセンスは何ですか？**

AKARI は **Open Core モデル** を採用しています。リポは 4 ライセンス層に分類されます:

| Layer | License | 対象 |
|---|---|---|
| Layer 1 | proprietary | `cloud` / `lp` / 素材ライブラリ |
| Layer 2 | Apache 2.0 | `shell` / `pool-impl` / `agents` / `m2c` / `amp` / `ace` |
| Layer 3 | MIT | `sdk` / `design` / `writer` / `video` / `voice` |
| Layer 4 | CC BY 4.0 | 公開ドキュメント（`.github` / `pool` docs） |

詳細: [親 README §License](../../profile/README.md)

### **Q. 自分の PC で動かすコードは OSS ですか？**

はい。Shell / Pool 実装 / Agents / SDK / 公式アプリは Apache 2.0 または MIT で配布される予定です。**self-host・改変・fork はすべて自由** です。

### **Q. 料金は？**

- ソフト本体: **無料**（OSS）
- AI 推論: ローカル LLM（Qwen / Llama 等）でほぼ無料、または Claude / GPT を Max Plan の枠内で利用可
- 重い処理・同期: AKARI Cloud（hosted service、提供条件は検討中）

具体的な価格・プランは公開準備中です。最新情報は [Akari-OS Discussions](https://github.com/Akari-OS/.github/discussions) で順次案内します。

### **Q. フォークしていいですか？**

はい。Apache 2.0 / MIT 配下のリポは fork・改変・再配布が自由です。ただし「Akari」「AKARI OS」というブランド名は商標保護を進めており、fork したものを Akari ブランドで再配布することは別途の同意が必要になります（Mozilla / Iceweasel と同じモデル）。

### **Q. 商標「Akari」「AKARI OS」の扱いは？**

「Akari」「AKARI OS」は早期に商標登録を進めています（少なくとも日本、可能なら US/EU）。コードの fork は自由ですが、ブランドそのものを名乗る形での派生配布は商標で制限されます。
詳細: [親 README §License](../../profile/README.md)

### **Q. 商用利用は OK ですか？**

Apache 2.0 / MIT 配下のリポは **商用利用 OK** です。SDK でアプリを作って販売することも妨げません。proprietary 配下（`cloud` / `lp` / 素材）は別途 EULA を結ぶ前提です。

### **Q. なぜ AGPL ではないのですか？**

過去には AGPL を採用していたリポもありますが、Open Core モデル（cloud で収益、それ以外は OSS）と整合させるため、2026-05-07 に Apache 2.0 / MIT へ統一しました。**新規採用 AGPL は禁止** という運用ルールです。

---

## プライバシー・データ

### **Q. 私のデータはどこに保存されますか？**

AKARI は **ローカルファースト** です。素材（写真 / 動画 / 文章 / 録音）はあなたの PC の **Pool**（SQLite + ファイル群）に保存されます。クラウドへの常時アップロードは前提にしていません。
詳細: [`../../VISION.md`](../../VISION.md)

### **Q. AKARI Cloud は何のためにある？**

AKARI Cloud は **任意で使う hosted service** です。重い処理（モデル推論 / バッチ分析等）や、複数デバイス間の同期が必要な場合に利用できます。Cloud を使わずに完全ローカルで動かす運用も想定しています。

### **Q. AI に何を送っていますか？**

利用する AI モデルによって送信先が変わります:

- **ローカル LLM**（Qwen / Llama 等）: 端末内で完結 — 何も外に出ません
- **Claude / GPT 経由**: ユーザーが API キーまたは Max Plan を設定した場合のみ、必要なコンテキストが各社サーバーに送られます

「何が送られるか」は **M2C（Media to Context）** の出力範囲が原則であり、原本ファイル全体ではなく抽出されたメタデータ・特徴量です。送信内容は Memory Viewer で可視化される予定です。

### **Q. データを消したい / エクスポートしたいときは？**

Pool は **SQLite + ローカルファイル** で構成されており、ユーザーが直接操作できる場所にあります。エクスポート専用 UI（Pool Browser のメニュー）も整備中です。「データはあなたのもの」という設計です。

### **Q. 個人情報や機密素材を扱っても安全ですか？**

ローカル運用が前提なので、原本が手元から出ない限りはあなたの管理下にあります。AKARI Cloud / 外部 LLM API を使う場合のみネットワーク往復が発生するため、機密素材を扱う場合はローカル LLM + Cloud オフ運用が推奨されます。

---

## 開発・貢献

### **Q. 貢献するにはどうすればいいですか？**

[`quickstart-developer.md`](./quickstart-developer.md) を読み、興味のあるリポに Issue / PR を投げるのが最短です。コミットメッセージは日本語 + プレフィックス（`[機能追加]` / `[バグ修正]` / `[ドキュメント]` 等）で揃えています。
詳細: [`../../CONTRIBUTING.md`](../../CONTRIBUTING.md)

### **Q. バグ報告 / 機能要望はどこに送ればいい？**

- **バグ**: 該当リポの GitHub Issue
- **機能要望 / アイデア**: [Akari-OS Discussions](https://github.com/Akari-OS/.github/discussions) または [Voice](https://voice.akari-oss.app)
- **セキュリティ脆弱性**: [`../../SECURITY.md`](../../SECURITY.md) の手順に従って非公開報告

### **Q. App を作りたい**

**App SDK** が用意されています。Tier は 2 つあります:

- **Full Tier**: ネイティブ要件（GPU / FFmpeg / 60fps 描画等）がある重量アプリ
- **MCP-Declarative Tier**: MCP サーバー + `panel.schema.json` だけで Shell に載る軽量アプリ（参入コスト 1/10）

詳細: [`./quickstart-developer.md`](./quickstart-developer.md) / [SDK リポ](https://github.com/Akari-OS)

### **Q. どのリポから読めばいいですか？**

立ち位置によります:

| 興味 | おすすめ |
|---|---|
| 全体像 | [`Akari-OS/.github`](https://github.com/Akari-OS/.github) → [`../../VISION.md`](../../VISION.md) |
| プロトコル | [m2c](https://github.com/Akari-OS) / [amp](https://github.com/Akari-OS) / [ace](https://github.com/Akari-OS) |
| アプリ実装 | [sdk](https://github.com/Akari-OS) → 各アプリリポ |
| Pool（記憶層） | [pool](https://github.com/Akari-OS)（公開 docs） |

### **Q. CLA は必要ですか？**

現時点で **CLA は不要** です。Apache 2.0 / MIT は contributor が同ライセンスで grant する前提なので、追加の同意書は求めていません。仕様リポ（m2c / amp / ace）のみ DCO（`Signed-off-by:` 行）を必須にしています。
詳細: [`../../CONTRIBUTING.md`](../../CONTRIBUTING.md)

---

## 技術仕様

### **Q. AKARI は何で書かれていますか？**

主な技術スタックです:

- **Shell / Apps**: Tauri v2 + React + TypeScript
- **Pool 実装**: Rust（SQLite + FTS5 + MCP サーバー）
- **Agents**: Node.js + TypeScript
- **プロトコル仕様**: 中立な仕様ドキュメント（実装は複数言語で書ける前提）

### **Q. 動作環境は？**

- **OS**: macOS / Windows / Linux（Tauri ビルド対応）
- **ストレージ**: ローカル SSD 推奨。素材は階層化ストレージ（Hot / Warm / Cold）に分散可
- **AI 推論**: ローカル LLM（Qwen / Llama 等）または外部 API（Claude / GPT 等、ユーザー設定）

### **Q. オフラインで使えますか？**

**部分的に Yes**。素材操作・編集・Pool 検索・ローカル LLM 推論はオフラインで動きます。外部 LLM API（Claude / GPT 等）や AKARI Cloud に依存する機能は通信が必要です。
詳細: [`../../VISION.md`](../../VISION.md)

### **Q. AKARI 同士の連携は？**

AKARI のアプリ間（Writer ↔ Video 等）は **直接通信せず、記憶層（Pool / AMP）経由で ID を渡す** モデルです。これにより「誰が何を渡したか」が記憶層に履歴として残り、複数アプリにまたがるワークフローを再現できます。
詳細: [`../../VISION.md`](../../VISION.md)

### **Q. プロトコル（M2C / AMP / ACE）は AKARI 専用ですか？**

いいえ。**第三者実装を歓迎する公開プロトコル** として設計しています。Apache 2.0 + 特許 grant により、AKARI 以外のクライアント・サーバーが同じ契約で実装できます。
詳細: [`../../VISION.md`](../../VISION.md)

---

## 雑多

### **Q. 「Akari」「あかり」の意味は？**

**Akari**（あかり / 明かり）は日本語で「光・明るさ」を意味します。"**A**kar**i**" の最初と最後の文字を取ると **AI**、そこに **OS** を加えると **"AI OS"** になります。日本発の AI OS という由来です。
詳細: [`../../profile/README.md`](../../profile/README.md)

### **Q. 創設者は誰ですか？**

個人開発の OSS プロジェクトです。Akari-OS org のメンテナと貢献者で開発しています。
詳細: [Akari-OS org](https://github.com/Akari-OS)

### **Q. ロードマップはどこで見れますか？**

公開ロードマップ: [`../../ROADMAP.md`](../../ROADMAP.md)

リポごとの Phase 進捗は [Akari-OS org](https://github.com/Akari-OS) の各リポ README にも反映されています。

### **Q. 他の AI ツール（ChatGPT / Claude Desktop / Cursor 等）と組み合わせて使えますか？**

はい。共有層（Pool / AMP / M2C / ACE）はすべて **MCP サーバー** として露出します。**Claude Code や任意の MCP クライアントから AKARI Core を直接叩く** Developer モードを正式にサポートしています。
詳細: [`../../VISION.md`](../../VISION.md)

### **Q. AKARI を応援するには？**

- ⭐ 各リポに **Star** を付ける
- 🐛 GitHub Issue で使用感・バグを共有する
- 💬 [Discussions](https://github.com/Akari-OS/.github/discussions) で議論に参加する
- ☕ [Buy Me a Coffee](https://buymeacoffee.com/kyacharsx) で 1 杯分の応援
- 🌐 SNS でプロジェクトを広める

詳細: [`../../profile/README.md`](../../profile/README.md)

### **Q. 質問が解決しなかったら？**

- 概念を深掘りしたい → [`./concept-map.md`](./concept-map.md) / [`./glossary.md`](./glossary.md)
- 実際に触りたい → [`./quickstart-end-user.md`](./quickstart-end-user.md)
- コードを書きたい → [`./quickstart-developer.md`](./quickstart-developer.md)
- 直接質問したい → [Akari-OS Discussions](https://github.com/Akari-OS/.github/discussions) / [Voice](https://voice.akari-oss.app)

---

> **AI っぽくない。あなたらしさを。**
> "Not AI-ish. Yours."
