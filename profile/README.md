<div align="center">
  <img src="./banner.svg" alt="Akari-OS — 個人クリエイターのための AI OS" width="100%">
</div>

# Akari-OS

> **AI っぽくない。あなたらしさを。**
> "Not AI-ish. Yours."

スマホに iOS があるように、クリエイター PC に **AKARI OS** がある。
Writer / Video / SNS Sender といったアプリが、AI チーム・記憶層・プロトコルを共有して動く
**個人クリエイターのための AI OS**。

---

## 解きたい問題

> ひとりのクリエイターが、チームを雇わずに、
> 自分らしいものを、妥協せずに作り続けられたら？

動画を作る。ブログに載せる。SNS で告知する。分析して改善する。
今これをやるには、4 人以上のチームか、10 個のバラバラなツールが要る。
そして時短のために AI に投げると、**誰が作ったかわからない量産コンテンツ**になる。

AKARI は違う。**あなたの素材・判断・感性**を中心に置いたまま、AI が雑務を引き受ける。

---

## 3P Loop — 意図は人、手は AI

AKARI の作業フローは、3 つのフェーズが往復するループで進む。

```
  ┌─────────────────────────────────────┐
  │                                     │
  ▼                                     │
  [提案 Propose]  AI が素材を読み、叩き台を出す   │
      ↓                                 │
  [方向 Point]   あなたが画面上で指し示す         │
      ↓                                 │
  [仕上げ Polish] AI が方向通りに仕上げる         │
      └─────────────────────────────────┘
```

人間の役割は「意図の注入」と「仕上がりの確認」の 2 つだけ。
解析・ドラフト・整形・並行生成は AI が引き受ける。

---

## あなた専属の AI チーム

開いたら、この 7 人がいる。名前も顔もある。設定は不要。

| 役割 | 担当 |
|---|---|
| 🎯 **パートナー** | あなたの窓口。指示を受けてチームに振る |
| 🎬 **スタジオ担当** | 動画 / デザイン / 記事を作る |
| 📱 **オペレーター** | SNS / メール / 決済を動かす（寝てる間に） |
| 🔍 **リサーチャー** | 必要な情報を集めてくる |
| 🛡 **ガーディアン** | 品質とブランドを守る |
| 🧠 **メモリスト** | 好みと経験を覚える |
| 📊 **アナリスト** | 結果を測って改善を提案する |

7 人は **reference defaults** — OS に何もアプリが入っていない状態での顔ぶれ。
アプリが独自のエージェントを連れてくることもできる（着せ替え可能、固定ではない）。

---

## 3 つの原則

### 1. 決め打ちだけど開いている
エージェントは開いたときから動いている（設定不要）。
必要なら、アプリが独自のエージェントを連れてくることができる。

### 2. 見える
エージェントが何をしているかが**視覚的にわかる**。
チャットログを読み返す必要はない。全員の状態が一目でわかる。

### 3. 寄り添う
完全自動ではない。**あなたの感性を最大化するツール**。
AI が得意なこと（大量処理・解析）は AI に。
人間が得意なこと（「これが好き」「ここをこう変えたい」）は画面上で視覚的に指示する。

---

## リポジトリ一覧

### Core 層 — プロトコルと共有データ

| Repository | 内容 |
|---|---|
| **[pool](https://github.com/Akari-OS/pool)** | Universal Knowledge Store（公開ドキュメント）。SQLite + FTS5 + Analyzer + MCP サーバー |
| **[m2c](https://github.com/Akari-OS/m2c)** | Media-to-Context プロトコル。メディアを構造化された AI コンテキストに変換 |
| **[amp](https://github.com/Akari-OS/amp)** | Agent Memory Protocol。記憶の保存・検索・減衰を標準化 |
| **[ace](https://github.com/Akari-OS/ace)** | Agent Context Engineering Framework。コンテキストの組み立て方と品質 Lint を標準化 |
| **[sdk](https://github.com/Akari-OS/sdk)** | AKARI App SDK monorepo。Full Tier / MCP-Declarative Tier 2 つの App 開発モデル |

#### AKARI Protocol Suite

| プロトコル | 標準化対象 | リポ |
|---|---|---|
| **MCP** | ツール呼び出し | (external) [modelcontextprotocol.io](https://modelcontextprotocol.io) |
| **M2C** | メディア → コンテキスト変換 | [m2c](https://github.com/Akari-OS/m2c) |
| **AMP** | エージェント記憶 | [amp](https://github.com/Akari-OS/amp) |
| **ACE** | エージェントコンテキスト + Lint | [ace](https://github.com/Akari-OS/ace) |

### Apps 層 — 公開済みサイト

| App | 配信先 | ソース |
|---|---|---|
| **Home page** | [akari-oss.app](https://akari-oss.app) | [lp](https://github.com/Akari-OS/lp) |
| **Voice**（フィードバック + チェンジログ） | [voice.akari-oss.app](https://voice.akari-oss.app) | [voice](https://github.com/Akari-OS/voice) |

### Preview / 開発中（private）

Akari-OS org 配下で preview として開発中。stable 到達次第 public 化予定:

- **`shell`** — Tauri v2 + React ホスト（Phase 0 実装中）
- **`agents`** — 7 Reference Agents daemon（Node.js + TypeScript）
- **`pool-impl`** — Pool 実装（Rust + SQLite + MCP）、v1.0 到達で公開候補

永続非公開（credentials を扱うため公開しない）:

- **`cloud`** — 認証・クレジット・マーケットのバックエンド

計画中（未着手）:

- **`video`** — デスクトップ動画編集（Phase 3 で Shell 統合予定）

---

## ステータス

Early development — 主要リポは Phase 開発中。Star / Issue / PR はどのリポにも大歓迎。

- **pool**: Phase 6 完了（MCP 11 ツール + Obsidian 同期）— 次は Phase 7（Shell 統合）
- **shell**: Phase 0 実装中（Tauri v2 + React）
- **agents**: preview 段階、7 Reference Agents daemon 設計・初期実装
- **m2c / amp**: v0.1〜0.2 Draft
- **ace**: v0.1-draft 公開（2026-04-14）
- **sdk**: monorepo 整備中（Module → App リネーム完了）
- **cloud**: Phase 4 完了（永続 private）

---

## ドキュメント

- 📖 **[Vision](https://github.com/Akari-OS/.github/blob/main/VISION.md)** — 構想・原則・iOS 比喩・5 層アーキ・App SDK
- 🗺 **[Roadmap](https://github.com/Akari-OS/.github/blob/main/ROADMAP.md)** — Phase 0-5 の段階的ロードマップ
- 🧠 **[Memory Architecture](https://github.com/Akari-OS/.github/blob/main/docs/memory.md)** — 記憶の 4 層モデル
- 🤝 **[Contributing](https://github.com/Akari-OS/.github/blob/main/CONTRIBUTING.md)** — 貢献方法・コーディング規約
- 📜 **[Code of Conduct](https://github.com/Akari-OS/.github/blob/main/CODE_OF_CONDUCT.md)** — 行動規範
- 🔒 **[Security Policy](https://github.com/Akari-OS/.github/blob/main/SECURITY.md)** — 脆弱性報告

---

## プロジェクトを応援する

Akari-OS は個人開発の OSS プロジェクト。

- ⭐ **各リポに Star** を付ける（タダで一番嬉しい）
- 🐛 **Issue で使い方・バグ・感想**を教えてくれる
- 💬 **[Discussions](https://github.com/Akari-OS/.github/discussions)** で議論に参加する
- ☕ **[Buy Me a Coffee](https://buymeacoffee.com/kyacharsx)** で 1 杯分の応援
- 🌐 プロジェクトを **SNS で広めてくれる**

---

> **AKARI は「ツール」ではない。**
> **個人クリエイター専用の、エッジで動く AI OS。**
> **AI っぽくない。あなたらしさを。**

---

## 名前の由来

**Akari**（あかり / 明かり）は日本語で「光・明るさ」を意味する。
"**A**kar**i**" の最初と最後の文字を取ると "**AI**"、そこに "**OS**" を加えると **"AI OS"** になる。

🇯🇵 **日本発の AI OS** — クラウド依存ではなく、ローカルで動く、個人のための AI OS。

---

*Built by [@ryoma-nakajima](https://github.com/ryoma-nakajima) and a growing community.*
