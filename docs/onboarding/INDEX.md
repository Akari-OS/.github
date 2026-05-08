# AKARI オンボーディング

> **正典 (Layer A)**: 本ディレクトリは AKARI Onboarding の公開正典です。
> Hub (`akari-os/docs/onboarding/`) は同期コピー（編集は本ディレクトリで行う）。
>
> **対象読者**: AKARI を初めて触る人（GitHub 訪問者 / エンドユーザー / 開発者）
> **所要時間**: 5 分 〜 1 日（読者層により異なる）
> **前提知識**: 不要（必要な概念は本ディレクトリで全部解説する）
> **次に読むもの**: 自分の立場に合った Quickstart を 1 つ選んで進めてください

---

## このディレクトリについて

このディレクトリは **AKARI を初めて触る人向けの導線をまとめた「玄関」** です。
エコシステム全体（公開リポ + 子リポ）にまたがる概念を、対象読者ごとに整理しています。

---

## まずここから — 共通読み物

立場を問わず、これから AKARI に触る全員が **最初の 10 分で読む** ものです。

| ファイル | 内容 | 所要 |
|---|---|---|
| [`concept-map.md`](./concept-map.md) | AKARI 全体像（1 枚の Mermaid 図 + 各概念の最小定義） | 10 分 |
| [`glossary.md`](./glossary.md) | 用語集（46 語をカテゴリ別 + アルファベット順に整理） | 5 分（リファレンス） |
| [`faq.md`](./faq.md) | よくある質問（"AI 動画生成と何が違う？" "なぜローカル？" 等） | 10 分 |

---

## 対象別 Quickstart（3 段階）

自分のゴールに一番近い 1 本を選んでください。

### 1. 5 分で AKARI を知る — GitHub 訪問者

> 「これは何？」「自分に関係あるか？」を 5 分で判断したい人向け。

→ [`quickstart-visitor.md`](./quickstart-visitor.md)

含む内容:
- AKARI が解決する問題（一言で）
- 似た製品（Adobe / Canva / Runway / Notion 等）との違い
- 4 ライセンス層の意味（OSS / proprietary の境界）
- 「もっと知りたい」場合の入口

### 2. 30 分で最初の Work を作る — エンドユーザー

> 実際に AKARI Shell を起動して、**自分の素材で 1 つの成果物**を作りたい人向け。

→ [`quickstart-end-user.md`](./quickstart-end-user.md)

含む内容:
- AKARI のインストール（Tauri アプリ）
- 最初の Pool を作る
- 素材（Asset）を入れる
- Work を作って Variant を出力する（X 投稿 / 縦長動画 等）
- 7 人の AI チームに話しかけてみる

### 3. 1 日で contribute する — 開発者

> AKARI 上で動く **自前の App を作る** か、**既存リポに PR を投げる** ところまで進みたい人向け。

→ [`quickstart-developer.md`](./quickstart-developer.md)

含む内容:
- 開発者モード（Claude Code / 任意の MCP クライアント経由で Pool / AMP を叩く）
- App SDK の Tier 選び（Full / MCP-Declarative）
- `akari-app-cli` で雛形生成 → 配布まで
- 仕様駆動開発（SDD）と spec-id の付け方
- contribute フロー（コミット規約 / PR / handoff）

---

## 関連ドキュメント

- 🌐 公開正典（VISION / ROADMAP / CoC / SECURITY）: [Akari-OS/.github](https://github.com/Akari-OS/.github)
- 📖 Vision: [`../../VISION.md`](../../VISION.md)
- 🗺 Roadmap: [`../../ROADMAP.md`](../../ROADMAP.md)
- 🤝 Contributing: [`../../CONTRIBUTING.md`](../../CONTRIBUTING.md)
- 📜 Code of Conduct: [`../../CODE_OF_CONDUCT.md`](../../CODE_OF_CONDUCT.md)
- 🔒 Security: [`../../SECURITY.md`](../../SECURITY.md)
- 🌐 Akari-OS org: [github.com/Akari-OS](https://github.com/Akari-OS)
