# Quickstart — GitHub 訪問者向け（5 分版）

> **正典 (Layer A)**: 本ディレクトリは AKARI Onboarding の公開正典です。
> Hub (`akari-os/docs/onboarding/`) は同期コピー（編集は本ディレクトリで行う）。
>
> **対象読者**: GitHub の [Akari-OS org](https://github.com/Akari-OS) に来た人 / AKARI を初めて知った人
> **所要時間**: 約 5 分
> **前提知識**: 不要
> **次に読むもの**: 自分の興味で分岐 → §4 を参照

---

## このページの目的

「AKARI とは何で、自分は次に何をすればいいか」を 5 分で判断していただくための入口です。
詳しい技術解説は別ドキュメントに譲り、ここでは **概要 → できること → 違い → 次の一歩** だけに絞ります。

---

## 1. AKARI を 30 秒で

> **AI っぽくない。あなたらしさを。**
> "Not AI-ish. Yours."

AKARI は **個人クリエイターのための AI OS** です。スマホに iOS があるように、クリエイター PC に AKARI OS がある — そんなイメージです。

エディタ・動画編集・SNS 投稿のような「クリエイターアプリ」が、共通の **AI チーム・記憶層・プロトコル** を共有して動きます。バラバラのツールを行き来する代わりに、**1 つの環境で 1 人が全工程を回せる** ことを目指しています。

中心にあるのは **3P Loop**:

```
[提案 Propose]  AI が素材を読み、叩き台を出す
   ↓
[方向 Point]   あなたが画面上で指し示す（選択 / D&D / 範囲指定）
   ↓
[仕上げ Polish] AI が方向通りに仕上げる
   ↑                                          │
   └──────────────────────────────────────────┘
```

人間がやるのは **「意図を伝える」と「仕上がりを確認する」** の 2 つだけ。
解析・ドラフト・整形・並行生成は AI が引き受けます。

---

## 2. 何が「できる」のか（1 分）

AKARI の上で動くアプリ群（公式リファレンス実装）です。**いずれもプレビュー段階** で、機能・UI ともに変化していきます。

| アプリ | 概要 | 状態 |
|---|---|---|
| **Writer** ([writer](https://github.com/Akari-OS)) | 公式テキストエディタ。140/280 字の SNS 投稿から長文記事まで、Tiptap ベースで統合。Pool / Workflow / Inspector を備える | Phase 1 完了 / Phase 2 進行中 |
| **Video** ([video](https://github.com/Akari-OS)) | 動画編集アプリ。タイムライン編集 + SNS 比率別の export（YouTube 16:9 / TikTok 9:16 等）+ AI 編集スキル | Phase 2 完了 / Phase 3 進行中 |
| **Design** ([design](https://github.com/Akari-OS)) | Canva 系の汎用デザインアプリ。1 マスター → AI が比率・配置違いを一瞬派生 | Phase 0〜3 完了 / Phase 4 進行中 |
| **Voice** ([voice.akari-oss.app](https://voice.akari-oss.app)) | コミュニティフィードバック窓口（Web 公開済） | 公開済 |

> 全リポ一覧は [Akari-OS org](https://github.com/Akari-OS) を参照。
> 共有基盤（Pool / M2C / AMP / ACE）の詳細は [`../../VISION.md`](../../VISION.md) に書かれています。

---

## 3. なぜ「他と違う」か（1 分）

ひと言で言うと **「素材があなたの手元にあるまま、AI が脇で働く」** という設計です。
よく似た製品との違いは、おおまかに 3 点：

### 3-1. ローカルファースト + Pool

素材（写真・動画・文章・録音）は **あなたの PC 上の Pool** に置きます。原本がクラウドへ常時アップされる前提ではありません。
AI が見ているのは Pool 上のメタデータ・特徴量であり、必要な部分だけを必要な瞬間に推論に渡します。

> Pool については [`./glossary.md`](./glossary.md) と [`./concept-map.md`](./concept-map.md) を参照。

### 3-2. Open Core（コードは公開、コア体験はクラウドで）

基盤コード（Shell / Pool 実装 / Agent ランタイム / プロトコル仕様）は **OSS** です。誰でも fork・self-host・改変できます。
収益は **AKARI Cloud**（hosted service）と素材ライブラリ（subscription）で取る Open Core モデルです。

> ライセンスの詳細は [`./faq.md` §ライセンス・料金](./faq.md#ライセンス料金) と [親 README §License](../../profile/README.md) を参照。

### 3-3. AI が「あなたの代わりに作る」のではなく「あなたの仕事を助ける」

AI 動画生成や AI ライターのように **AI がゼロから生成する** のとは方向が違います。
AKARI で出てくるものは **「あなたが撮った / 書いた / 作った素材を、AI が磨いたもの」**。出口は AI 生成物ではなく、あなたの作品です。

```
AI 生成ツール:  プロンプト → AI が架空の素材を作る → 「誰が作ったか不明な」コンテンツ
AKARI:        あなたの素材 → 3P Loop で意図注入 → あなたらしい作品
```

---

## 4. 自分は次に何をすればいいか（1 分）

興味の方向で分岐してください。

| あなたが… | 次に読むもの | 所要 |
|---|---|---|
| **使ってみたい** | [`quickstart-end-user.md`](./quickstart-end-user.md) — インストールから最初の Variant 出力まで | 約 30 分 |
| **コードを書きたい / App を作りたい** | [`quickstart-developer.md`](./quickstart-developer.md) — App SDK / 開発者モード / contribute フロー | 約 1 日 |
| **概念をもっと知りたい** | [`concept-map.md`](./concept-map.md) — 1 枚図 + 用語集 [`glossary.md`](./glossary.md) | 10 分 |
| **疑問をまとめて解消したい** | [`faq.md`](./faq.md) — 30+ 問の横断 FAQ | 10 分 |
| **GitHub Org をブラウズしたい** | [Akari-OS org](https://github.com/Akari-OS) | 自由 |

> 迷ったら [`concept-map.md`](./concept-map.md) → [`faq.md`](./faq.md) の順がおすすめです。

---

## 5. 公式リンク

| 種別 | リンク |
|---|---|
| 🌐 公開正典（VISION / ROADMAP / CoC / SECURITY） | [Akari-OS/.github](https://github.com/Akari-OS/.github) |
| 📦 公開 SDK（App 開発者向け） | [Akari-OS/sdk](https://github.com/Akari-OS) |
| 📡 プロトコル仕様 | [m2c](https://github.com/Akari-OS) / [amp](https://github.com/Akari-OS) / [ace](https://github.com/Akari-OS) |
| 📚 Pool 公開ドキュメント | [Akari-OS/pool](https://github.com/Akari-OS) |
| 🏠 ホームページ | [akari-oss.app](https://akari-oss.app) |
| 💬 フィードバック / 質問 | [Akari-OS Discussions](https://github.com/Akari-OS/.github/discussions) / [Voice](https://voice.akari-oss.app) |

---

> **AKARI は「ツール」ではない。**
> **個人クリエイター専用の、エッジで動く AI OS。**
> **AI っぽくない。あなたらしさを。**
