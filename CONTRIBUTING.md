# Contributing to Akari-OS

Akari-OS への貢献を検討してくれてありがとう。このガイドは **Akari-OS org 全体**に
適用される。各リポジトリ固有のルールは、それぞれの README / CONTRIBUTING を参照。

---

## はじめに

Akari-OS は「個人のための AI OS」を作るエコシステムプロジェクト。
まずは [Vision](./VISION.md) と [Roadmap](./ROADMAP.md) を読んで、方向性に共感してもらえると嬉しい。

---

## 貢献の種類

| 貢献の形 | 歓迎度 | 始め方 |
|---|:-:|---|
| 🐛 バグ報告 | ⭐⭐⭐ | Issue を立てる（該当リポで） |
| 💡 機能提案 | ⭐⭐⭐ | Issue → Discussion で議論 |
| 📖 ドキュメント改善 | ⭐⭐⭐ | 誤字修正でも PR 歓迎 |
| 🌍 翻訳 | ⭐⭐⭐ | 英語版の整備を特に歓迎 |
| 💻 コード PR | ⭐⭐ | まず Issue で方向性確認してから |
| 🎨 UI / デザイン | ⭐⭐ | video リポで |
| 🧪 Analyzer プラグイン | ⭐⭐⭐ | pool リポの `analyzers/` 参照 |

---

## 貢献フロー

### 1. Issue を立てる（or 既存 Issue を選ぶ）

いきなり PR を送る前に、**まず Issue で議論**してほしい。特に大きな変更の場合。

- バグ: 再現手順 / 期待動作 / 実際の動作 / 環境情報
- 機能: なぜ必要か / どう動くべきか / 代替案があるか
- 質問: Discussions の方が向いてるかも

### 2. Fork してブランチを切る

```bash
git clone https://github.com/<your-username>/<repo>.git
cd <repo>
git checkout -b feature/your-feature-name
```

**ブランチ名**:
- `feature/xxx` — 新機能
- `fix/xxx` — バグ修正
- `docs/xxx` — ドキュメント
- `refactor/xxx` — リファクタ

### 3. 実装 + テスト

- 既存のコードパターンに従う
- 新しいパターンを導入する前に既存を確認
- テストは実装と同時に書く
- エッジケース・エラーケースもカバー

### 4. コミット

**コミットメッセージは日本語**（コードコメントも日本語）。
以下のプレフィックスを付ける（Akari-OS 共通規約）：

| プレフィックス | 用途 |
|---|---|
| `[機能追加]` | 新機能 |
| `[機能改善]` | 既存機能の改善 |
| `[バグ修正]` | バグ修正 |
| `[リファクタ]` | 動作を変えないコード改善 |
| `[ドキュメント]` | ドキュメントのみの変更 |
| `[テスト]` | テスト追加・修正のみ |
| `[依存更新]` | 依存パッケージの更新 |

例:
```
[機能追加] Pool に PDF Analyzer を追加

PDF ファイルを LlamaParse で構造化テキストに変換し、
Pool のコンテキストとして保存できるようにした。
```

### 5. PR を送る

- PR タイトルは簡潔に
- PR 本文に以下を含める:
  - **何を変えたか** (What)
  - **なぜ変えたか** (Why) — 背景・動機
  - **どうテストしたか** (How)
  - 関連 Issue への参照 (`Closes #123`)

### 6. レビュー → マージ

- メンテナーがレビューする
- フィードバックがあれば議論 → 修正
- マージされたら `main` に反映、リリースで公開

---

## コーディング規約

### 共通

- **変数名・関数名・ファイル名は英語**
- **コメント・コミットメッセージ・ドキュメントは日本語**
- **過度な抽象化を避ける** — 1 回しか使わないヘルパーは作らない
- **不要なコードは完全に削除**（コメントアウトで残さない）

### TypeScript (public apps)

- ESLint + Prettier に従う
- 型を明示する（`any` は原則禁止）
- React コンポーネントは関数コンポーネント + Hooks

### Rust (pool / select apps)

- `cargo fmt` + `cargo clippy` に従う
- エラーは `thiserror` で型定義、`anyhow` は CLI / 統合層のみ
- 新しい dependency は最小限に

---

## コミュニケーション

- 🐛 **バグ報告**: 該当リポの Issues
- 💡 **機能提案**: 該当リポの Issues or Discussions
- 💬 **質問・雑談**: GitHub Discussions
- 🔒 **セキュリティ脆弱性**: [SECURITY.md](./SECURITY.md) を参照（公開 Issue を立てない）

**行動規範**: [Code of Conduct](./CODE_OF_CONDUCT.md) に従うこと。

---

## ライセンスと著作権

貢献するコードは、各リポジトリのライセンス（Open Core 4 層モデル）で公開される：

- **基盤コード** (`shell` / `pool-impl` / `agents`): **Apache 2.0**
- **プロトコル仕様** (`m2c` / `amp` / `ace`): **Apache 2.0**
- **SDK + Full Tier apps** (`sdk` / `design` / `writer` / `video` / `voice`): **MIT**
- **公開ドキュメント** (`pool` (docs) / `.github`): **CC BY 4.0**
- **収益層** (`cloud` / `lp` / `materials`): **proprietary**（外部 contribution 不可）
- **その他**: 各リポジトリ参照

PR を送ることは、そのライセンスでの公開に同意したとみなす（CLA なし、DCO なし、シンプルに）。

---

## 感謝

すべての貢献 — バグ報告 1 つでも、typo 修正 1 つでも — を歓迎してる。
Akari-OS が成長できるのは、あなたのおかげ。ありがとう。
