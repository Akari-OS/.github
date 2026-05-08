# Quickstart — エンドユーザー編

> **正典 (Layer A)**: 本ディレクトリは AKARI Onboarding の公開正典です。
> Hub (`akari-os/docs/onboarding/`) は同期コピー（編集は本ディレクトリで行う）。
>
> **対象読者**: 個人クリエイター（コードを書かない、PC の基本操作はできる）
> **所要時間**: 約 30 分
> **前提知識**: なし
> **状態**: 🟡 **Preview（dev build のみ、DMG / installer 未リリース）**
> **次に読むもの**:
> - 概念をもう少し深く知りたい → [`./concept-map.md`](./concept-map.md)
> - 用語の定義を確認したい → [`./glossary.md`](./glossary.md)
> - 困ったときの逆引き → [`./faq.md`](./faq.md)
> - 各 app の詳しい使い方 → 各リポの `docs/howto.md`（本文中で都度 pointer）

---

## ⚠ 現状について（読んでから始めてください）

AKARI は **Preview 段階** です。

- **AKARI Shell** は現在 **開発者ビルド (`pnpm tauri dev`)** でのみ動作します。
- **DMG / installer の正式配布は Phase 1+ で予定**しており、本ドキュメントの Step 1 はリリース後に差し替えます。
- バックアップを取ってから試してください。Pool に入れた素材は基本的にローカル（`~/.akari-pool/`）に保存されます。
- なお、Shell は [`Akari-OS/shell`](https://github.com/Akari-OS) として段階公開中（stable 到達後に public 化予定）。Preview 段階の dev build を試す場合は read 招待が必要になることがあります。

---

## Step 0: AKARI とは（30 秒）

> **個人クリエイターのための AI OS**。あなた専属の AI チームが、あなたの手元で働きます。
>
> - 素材は **あなたが撮った / 書いた / 作ったもの**（AI が生成したものではない）
> - AI は **読み・提案し・仕上げ**ます。あなたは **意図を伝え・仕上がりを確認**するだけ
> - 出てくるのは **「あなたが作ったもの」を AI が磨いたもの**

詳しくは [`../../VISION.md`](../../VISION.md) と [`./concept-map.md`](./concept-map.md) を参照。
ここから先は実際に手を動かします。

---

## Step 1: Shell を起動する（5 分）

AKARI のすべては **AKARI Shell**（器）から始まります。Shell はデスクトップアプリで、ここから Writer / Video / Design などのアプリを開きます。

### 1-1. 現状（Preview）— dev build で起動する

正式リリース前なので、開発者向けの方法で起動します。Terminal を使います。

1. **リポジトリを clone**
   - GitHub から `Akari-OS/shell` を clone（access 権限が必要）
2. **依存をインストール**
   - リポジトリのルートで `pnpm install`
3. **dev build で起動**
   - `pnpm tauri dev`
   - 初回はビルドに数分かかります。Tauri のウィンドウが立ち上がれば成功です。

> 詳しい手順・トラブルシュートは Shell リポの `DEVELOPMENT.md` を参照してください。

### 1-2. 将来（正式リリース後）— DMG / installer から起動する

- macOS: `.dmg` をダウンロード → アプリケーションフォルダにドラッグ → ダブルクリックで起動
- Windows / Linux: 同等の installer を予定

> このセクションは正式リリース時に差し替えます。

### 1-3. 起動直後に見える画面

Shell が立ち上がると、最初に **Works 一覧画面（ホーム）** が表示されます。画面の構造は次のとおりです。

- **左端の縦列アイコン（ActivityBar）**
  - **Home** — Works 一覧に戻る
  - **Apps** — 追加した app の管理
  - **Settings** — Agent 接続 / Pool パスなどの設定（下部）
- **左上のハンバーガーメニュー**
  - ホーム / Chat / Writer / Pool / Pool 管理 / 記憶 / 通知 / バグ / 設定の切り替え
- **中央の大きな領域**
  - 既存の Work があれば一覧表示。なければ「**新規 Work 作成**」または「**Scratch Work**」のボタンが見えます
- **右下からスライドする Chat パネル**
  - パートナー（AI の窓口）と話す場所。ここから他のエージェントに作業を振れます

> Shell の各部位の詳しい説明は Shell リポ（[`Akari-OS/shell`](https://github.com/Akari-OS)）の `docs/howto.md` を参照。

---

## Step 2: 最初の Work を作る（10 分）

### 2-1. 「Work」とは何か（30 秒）

**Work** は「ある 1 つの作品 / 1 つの仕事の入れ物」です。例えば：

- 「YouTube ショート 1 本」
- 「ブログ記事 1 本」
- 「展示用ポスター 1 枚」

Work には **素材（Asset）**・**変奏（Variant）**・**会話履歴**が紐づきます。1 つの Work の中で AI と対話しながら作品を仕上げていく、というのが基本の流れです。

> 用語の正確な定義は [`./glossary.md`](./glossary.md)、Work / Pool / Variant の関係は [`./concept-map.md`](./concept-map.md) を参照。

### 2-2. Work を作る手順

1. ホーム画面の **「新規 Work 作成」** ボタンをクリック（または `Cmd+N`）
2. **Pool を選ぶ**
   - Pool は「ある仕事の文脈の単位」です。プロダクト / 案件 / 個人メモなど、まとまった文脈ごとに 1 つ作ります
   - 既存の Pool があればリストから選択。無ければ **「+ 新規 Pool」** で作成（名前と簡単な説明を入れる）
   - 説明（例: `料理系 YouTube ショート動画の素材集`）を書いておくと、AI 分析時のヒントになります
3. **App を選ぶ**
   - 文章を書きたい → **Writer**
   - 動画を編集したい → **Video**
   - 画像 / デザインをしたい → **Design**
4. **Work のタイトル** を入れる（例: `2026-05 春のレシピ動画`）
5. **「作成」** をクリック → 選んだ App が Work と一緒に開きます

### 2-3. 作成後の画面

App ごとに 4 パネル構成になっています（共通の骨格）：

- **左 (Source)** — 素材パネル / Pool / Style など、入力リソース
- **中央 (Editor / Preview)** — 編集領域（Writer ならエディタ、Video ならタイムライン + プレビュー）
- **右 (Inspector)** — 選択中のオブジェクトの詳細・パラメータ
- **下 / 右下 (Chat)** — パートナー（AI）との対話

> どの App でも「左に素材、中央に編集、右に詳細、下に AI」という配置は共通です。

### 2-4. もう少し気軽に試したいとき — Scratch Work

「正式な Work を作るほどでもない、ちょっと書き散らしたい」というときは **Scratch Work** が便利です。

- ホーム画面の **「Scratch Work」** ボタン、または Command Palette (`Cmd+K`) > Writer > Scratch
- 常駐の Work で、Pool 選択もタイトル入力も不要

---

## Step 3: 素材を投入する（5 分）

### 3-1. 「素材（Asset）」とは

**Asset** は Pool に保存される素材アイテムです。動画 / 画像 / 音 / PDF / テキスト / Web ページなど、なんでも入れられます。AI はこれを読んで要約・タグ付けし、後から検索したり Work で再利用したりできます。

### 3-2. 素材パネル（MaterialPanel）の使い方

各 App の左パネル（Source）には **素材パネル** があります。同じインターフェースが Writer / Video / Design 共通で使えます。

#### 取り込み（基本はドラッグ & ドロップ）

1. Finder / Explorer から素材ファイルを選択
2. 素材パネルにそのままドラッグ & ドロップ
3. Pool に追加されます

ファイル以外も取り込めます：

- **Web ページ** — URL を貼り付け
- **YouTube** — URL を貼り付け（字幕 / 文字起こし自動取得）
- **テキスト** — テキスト直接貼り付け

#### scope tab（この Work / 全 Pool）

素材パネルの上部に **scope filter** タブがあります。

- **「この Work」** — 現在開いている Work に紐づく素材だけ表示
- **「全 Pool」** — その Pool 全体（過去の Work で取り込んだ素材も含む）から検索

「過去に取り込んだあの動画を再利用したい」というときは「全 Pool」に切り替えて検索します。

### 3-3. StorageMode（参照 / コピー / 自動）の選び方

素材を取り込むときに、ファイルを **どう保存するか** を選びます。Pool 設定モーダル（PoolSettingsView）の radio で既定値を変更できます。

| モード | 動作 | こんなときに |
|---|---|---|
| 📎 **参照（Reference）** | Pool は path だけを記録。元ファイルはそのまま | 大容量ファイル（動画 / 高解像度画像）。容量を消費したくない |
| 📦 **コピー（Copy）** | Pool 内に複製を取る | 小〜中サイズ。元ファイル削除に対する保険を取りたい（**現状の既定**） |
| ✨ **自動（Auto）** | 10 MB 未満は Copy、以上は Reference | 混在ライブラリで運用したい |

> 詳しい挙動・dangling（元ファイルが消えた状態）の対応は Shell リポ（[`Akari-OS/shell`](https://github.com/Akari-OS)）の `docs/pool-howto.md` を参照。

### 3-4. AI に分析させる

取り込んだ直後、素材はファイル名と種別だけしか持ちません。**「AI 分析」** ボタンを押すと、AI が中身を読み、要約とタグを生成します。

- 個別 → アイテムを選んで「AI 分析」
- 一括 → 複数選択 → 「N 件を分析」

分析が終わると、Inspector（右ペイン）に AI Summary / Tags / 字幕（動画の場合）が表示されます。

---

## Step 4: AI に依頼する（5 分）

### 4-1. パートナー（Partner）はどこにいるか

下部または右下の **Chat パネル** がパートナーの常駐場所です。Chat パネルが閉じている場合は左上のハンバーガーメニューから「Chat」を選びます。

パートナーは **あなたの窓口**。指示を受けて、必要に応じて他のエージェント（スタジオ担当 / リサーチャー / メモリスト等）に作業を振ります。

### 4-2. 最初の頼み方（ふつうに日本語で OK）

慣れないうちは難しいことを考えず、こんな感じで頼んでみてください：

- **素材を読んでほしい**
  - 「Pool に入れた動画 3 本を読んで、共通テーマを教えて」
- **最初のドラフトを作ってほしい**
  - 「読んでくれた内容をもとに、ブログ記事の下書きを 800 字くらいで書いて」
- **方向を指示する（3P Loop の "Point"）**
  - 「2 段落目をもう少しカジュアルに」「最後に CTA を足して」

ポイントは、**意図を伝える**こと。文字を 1 つずつ直すのは AI に任せて、あなたは「どういう仕上がりにしたいか」を伝えます。

### 4-3. Skill / Workflow が出てくる場面

会話していると、ときどきパートナーが **Skill**（特定の機能）や **Workflow**（多段の処理）を提案してきます。

- `/skill-name` を直接打つこともできます（例: Writer なら `/enhance` で選択範囲を AI 強化、`/translate <lang>` で翻訳）
- Workflow は、例えば「素材を読んで → 下書きを作って → SNS 用に各 platform に最適化」のような多段の流れを自動化します

> 各 App の Skill / コマンド一覧:
> - Writer: [`Akari-OS/writer`](https://github.com/Akari-OS) の `docs/howto.md`
> - Video: [`Akari-OS/video`](https://github.com/Akari-OS) の `docs/howto.md`（Director Skill は Phase 2 で追加予定）

### 4-4. 「画面で示す」も一級市民

チャットだけがすべてではありません。AKARI では **画面上で視覚的に指示する** のも基本操作です：

- **選択 + 右クリック** — 「ここを直して」
- **ドラッグ & ドロップ** — 素材を編集領域に運ぶ
- **範囲選択 → SelectionBubbleMenu** — 選択範囲だけ AI に送る（Writer）

「テキストで全部説明する」より、**指せるものは指す** のが速いです。

---

## Step 5: 仕上げて出力する（5 分）

### 5-1. Variant とは

**Variant** は「同じ素材から作った別バージョン」です。例えば：

- 同じ動画素材から → YouTube 用 (16:9) / IG Reels 用 (9:16) / TikTok 用 (9:16) の 3 つの Variant
- 同じ記事マスターから → X 投稿 / Note 記事 / Blog 記事の 3 つの Variant

Variant ごとに独立して編集・出力できます。素材自体は変更されません（Asset は immutable、修正は Variant 単位で保存されます）。

### 5-2. Inspector / Preview で確認する

右パネルの **Inspector** で、選択中のオブジェクト（clip / 段落 / レイヤー等）の詳細を見ながら微調整します。

- **Writer の DeviceFrame** — 各 SNS platform で実際にどう見えるか（phone / pc、dark / light）
- **Video の Preview** — タイムラインの再生プレビュー（重い素材なら画質プリセットで軽量化、Video リポの `docs/howto.md` §8 参照）
- **Design の Canvas** — 直接プレビュー兼編集領域

### 5-3. 出力する

App ごとに出力形式が異なります：

| App | 出力 | 場所 |
|---|---|---|
| Writer | Markdown / SNS publish | WorkBar の「Publish」ボタン |
| Video | mp4（SNS 比率別） | WorkBar の「Export」ボタン（`Cmd+E`） |
| Design | png / svg | エクスポート UI |

詳しい操作は各リポの howto を参照：
- Writer Publish: [`Akari-OS/writer`](https://github.com/Akari-OS) の `docs/howto.md`
- Video Export: [`Akari-OS/video`](https://github.com/Akari-OS) の `docs/howto.md`

### 5-4. Pool 出力ボタンの意味（明示的・自動ではない）

完成した Variant を **Pool に保存** したい場合は、**明示的に「Pool に出力」ボタンを押す** 必要があります。

- これは **自動ではありません**。あなたが「これは保存しておく価値がある」と判断したものだけ Pool に残します
- Pool に出力したものは、後の Work で素材として再利用できます（例: 過去の動画を新しい記事の参考に引っ張る）
- 全部自動保存ではなく **「あなたが残したいものを、あなたが選んで残す」** 設計です

---

## 困ったら

- **逆引き FAQ** → [`./faq.md`](./faq.md)
- **各 App のトラブルシュート**
  - Shell: [`Akari-OS/shell`](https://github.com/Akari-OS) の `docs/howto.md`
  - Writer: [`Akari-OS/writer`](https://github.com/Akari-OS) の `docs/howto.md`
  - Video: [`Akari-OS/video`](https://github.com/Akari-OS) の `docs/howto.md`
- **Pool 関連**: [`Akari-OS/shell`](https://github.com/Akari-OS) の `docs/pool-howto.md`
- **キーボードショートカット早見表** → 各 App の howto に集約

---

## 次のステップ

### もう少し概念を深く知りたい

- [`./concept-map.md`](./concept-map.md) — Work / Pool / Asset / Variant の関係図
- [`./glossary.md`](./glossary.md) — 用語集
- [`../../VISION.md`](../../VISION.md) — AKARI の哲学（3P Loop / CAA / Edge-Native）

### 各 App をもっと使いこなしたい

- Writer: [`Akari-OS/writer`](https://github.com/Akari-OS) — マルチ platform 並列エンハンス、Style 2 層、Template 8 種
- Video: [`Akari-OS/video`](https://github.com/Akari-OS) — タイムライン編集、字幕、4K 対応
- Pool: [`Akari-OS/shell`](https://github.com/Akari-OS) の `docs/pool-howto.md` — ナレッジストアの使いこなし、StorageMode、検索

### app を作りたい / Claude Code から AKARI を使いたい

- [`./quickstart-developer.md`](./quickstart-developer.md) — 開発者向け quickstart（AKARI App SDK / MCP 経由の Pool 利用）

---

> **AKARI は「ツール」ではなく「OS」です。**
> ここで覚えた「Work / Pool / 素材 / Variant / パートナー」の 5 つは、Writer でも Video でも Design でも、これから増えるアプリでも同じ意味で使われます。
> 一度覚えれば、新しい app を覚え直す必要はありません。
