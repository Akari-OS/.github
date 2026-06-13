# Akari-OS/.github

> **この特殊な `.github` リポジトリは、`Akari-OS` GitHub org の「正面玄関」です。**

ここには 2 種類のコンテンツが集まっています:

1. **org プロフィール** — `profile/README.md` が GitHub の org トップページ（<https://github.com/Akari-OS>）にそのまま表示される
2. **エコシステム公開正典** — VISION / ROADMAP / CODE_OF_CONDUCT / SECURITY / CONTRIBUTING / CHANGELOG。個別のアプリリポではなく、エコシステム全体のルールとビジョンをここにまとめる

実装コードは含まれません。各アプリの実装は `Akari-OS` org 配下の個別リポジトリ（`pool`, `m2c`, `amp`, `ace`, `sdk`, …）にあります。

---

## このリポジトリのファイル

### 👋 org プロフィール（GitHub に表示される）

| ファイル | 内容 |
|---|---|
| [`profile/README.md`](./profile/README.md) | org トップページに表示される公式プロフィール |
| [`profile/banner.svg`](./profile/banner.svg) | バナー画像 |

### 📜 公開正典ドキュメント

| ファイル | 内容 |
|---|---|
| [`VISION.md`](./VISION.md) | Akari-OS の構想・原則・iOS 比喩・5 層アーキテクチャ |
| [`ROADMAP.md`](./ROADMAP.md) | Phase 0〜5 の段階的ロードマップ |
| [`CODE_OF_CONDUCT.md`](./CODE_OF_CONDUCT.md) | 行動規範（全 org 共通） |
| [`SECURITY.md`](./SECURITY.md) | 脆弱性報告ポリシーとフロー |
| [`CONTRIBUTING.md`](./CONTRIBUTING.md) | 貢献ガイドライン |
| [`CHANGELOG.md`](./CHANGELOG.md) | エコシステム全体のリリースノート |
| [`FUNDING.yml`](./FUNDING.yml) | GitHub Sponsors / 応援チャネル |
| [`docs/`](./docs/) | 追加ドキュメント（メモリアーキ等） |

---

## 更新フロー

このリポジトリは **公開正典（配置層: 公開正典 / ライセンス: Layer 4 CC BY 4.0）** です。直接編集せず、**非公開ハブ (`akari-os` リポ)** で先に整理 → 同期する運用が推奨されます。

```
akari-os (private hub, SSOT)
   │   下書き・戦略メモ・横断ドキュメント
   │
   ▼  整理・レビュー後に切り出し
akari-dotgithub (このリポ, public canon)
   │
   ▼  push to GitHub
Akari-OS/.github  ← 世界に対する公式文書
```

詳細なフローは（非公開ハブ側の）`akari-os/docs/governance/publish-workflow.md` を参照。

---

## 関連リポジトリ

Akari-OS org 全体のリポジトリ一覧とカテゴリ分類は [`profile/README.md`](./profile/README.md) の「リポジトリ一覧」セクションが SSOT です。

---

## ライセンス

このリポジトリのドキュメントは [`LICENSE`](./LICENSE)（Creative Commons Attribution 4.0 International）で配布されます。各兄弟リポジトリのソースコードは Open Core 4 層モデル（Apache 2.0 / MIT / CC BY 4.0 / proprietary）に従い、それぞれ独自のライセンスで配布されます。詳細は [`profile/README.md`](./profile/README.md) の License 節を参照。

---

> **AI っぽくない。あなたらしさを。**
> *Not AI-ish. Yours.*
