<!--
派生ドキュメント（公開・self-contained 版）。正典は akari-video リポの
docs/connect-ai-codex-claude.md。内容修正は正典で行い、ここへ反映すること。
-->

# AKARI を AI（Codex / Claude）で操作する — はじめてガイド

AKARI の各アプリ（**Video / SVG / Sheets / Stage / Design / 3D**）は、**Codex や Claude のような AI から直接操作**できます。
「この動画をカットして」「このアイコンを描いて」「この表をグラフにして」と頼むと、AI が実際に編集を実行します。

さらに AI は、**いま何が必要かを判断して、自分でアプリを開いて切り替える**こともできます
（例:「この動画から表紙アイコンを作って」→ AI が SVG アプリを開いて描く）。

このガイドは、初めての人が AI を AKARI につなぐまでを順番に説明します。

---

## 仕組み（3 層）

```
あなた → AI(Codex / Claude)
   ├─(MCP :47628) AKARI shell ──▶ アプリを開く / 切り替える（os_open_app）
   ├─(MCP :各ポート) 各アプリ  ──▶ そのアプリを操作（video_* / svg_* / sheets_* …）
   └─(MCP)         Akari Pool  ──▶ アプリ間の共有メモリ（素材・分析結果の受け渡し）
```

- 各アプリは **MCP サーバー（sidecar）** という小さな窓口を持っています。AI はそこに繋いで操作します。
- **AKARI でアプリを開くと、その窓口が自動で立ち上がります**。別途インストールや起動コマンドは不要です。
- **AKARI shell 自身も窓口を持ち**、AI が `os_open_app` で別のアプリを開けます。
- 接続先はすべて `127.0.0.1`（あなたの PC の中だけ。外部には公開されません）。

> MCP（Model Context Protocol）= AI と外部アプリをつなぐ共通規格。Codex も Claude も対応しています。

---

## ポート一覧

| 窓口 | 接続先 | できること（ツール接頭辞） |
|---|---|---|
| **AKARI shell**（OS 層） | `http://127.0.0.1:47628/mcp` | アプリを開く / 切り替える（`os_*`） |
| **Video** | `http://127.0.0.1:47616/mcp` | 動画編集（`video_*`） |
| **SVG** | `http://127.0.0.1:47618/mcp` | ベクター作図（`svg_*`） |
| **Sheets** | `http://127.0.0.1:47620/mcp` | 表・グラフ（`sheets_*`） |
| **Stage** | `http://127.0.0.1:47622/mcp` | 3D 配置・モーション（`stage_*`） |
| **Design** | `http://127.0.0.1:47624/mcp` | デザイン（`design_*`） |
| **3D** | `http://127.0.0.1:47626/mcp` | 3D モデリング（`3d_*`） |

> 全部つなぐ必要はありません。**使うアプリ + AKARI shell** をつなげば十分です。

---

## 準備（Codex / Claude 共通）

1. **AKARI Desktop を起動**する。← AKARI shell の窓口（`:47628`）が立ち上がります。
2. その中で **使いたいアプリを開く**。← そのアプリの窓口が立ち上がります。
3. 編集したいプロジェクト（Work）を開いておく。

> これをやらずに操作しようとすると「接続されていません / タイムアウト」になります。
> そのときは「AKARI でそのアプリを開く」だけで直ります。

---

## Codex につなぐ

`~/.codex/config.toml` に、使う窓口を追記します（最低限 shell + 使うアプリ）。

```toml
[mcp_servers.akari-shell]
url = "http://127.0.0.1:47628/mcp"

[mcp_servers.akari-video]
url = "http://127.0.0.1:47616/mcp"

# 必要なアプリを同様に追記:
# akari-svg=47618 / akari-sheets=47620 / akari-stage=47622 / akari-design=47624 / akari-3d=47626
```

保存して Codex を起動し直すと、`os_*` や `video_*` などのツールが使えるようになります。

---

## Claude（Claude Code）につなぐ

ターミナルでコマンドを実行するだけです。

```bash
# OS 層（アプリを開く/切替）
claude mcp add --transport http akari-shell  http://127.0.0.1:47628/mcp
# 使うアプリ
claude mcp add --transport http akari-video   http://127.0.0.1:47616/mcp
claude mcp add --transport http akari-svg     http://127.0.0.1:47618/mcp
claude mcp add --transport http akari-sheets  http://127.0.0.1:47620/mcp
claude mcp add --transport http akari-stage   http://127.0.0.1:47622/mcp
claude mcp add --transport http akari-design  http://127.0.0.1:47624/mcp
claude mcp add --transport http akari-3d      http://127.0.0.1:47626/mcp
```

どのプロジェクトでも使いたい場合は `-s user` を付けてユーザー全体に登録します。
登録できたか確認: `claude mcp list`

> まだアプリを開いていないと `Failed to connect` と表示されますが、これは正常です。
> AKARI でそのアプリを開けば自動でつながります。

---

## AI に「アプリを開かせる」

AKARI shell の窓口（`:47628`）をつないでおくと、AI が状況に応じてアプリを開けます。

例:
- 「この動画から表紙アイコンを作って」→ AI が `os_open_app` で **SVG** を開いて作図
- 「Stage に切り替えて、さっきの動画を画面に配置して」→ AI が **Stage** を開いて配置
- 「いま開いてるアプリを教えて」→ `os_current_app` で確認

`os_open_app` の直後はアプリの画面が立ち上がるのに少し時間がかかります。AI は接続できるまで少し待ってから操作します。

---

## 使ってみる

AI に普通の言葉で頼むだけです。**最初に「状態を見て」と頼むのがコツ**（AI が現状を把握してから安全に編集します）。

例:
- 「いま開いてるアプリの状態を見て」
- **Video**: 「最初の無音部分をカットして」「字幕を付けて」
- **SVG**: 「角丸の四角に星を重ねたアイコンを描いて」
- **Sheets**: 「この表を棒グラフにして、色を青系に」
- **Stage**: 「iPhone を中央に置いて、ゆっくり回して」
- **Design**: 「このデザインを Instagram 投稿サイズに展開して」
- **3D**: 「角丸の箱を作って、上に球を融合させて」

AI が行った操作は AKARI の画面にも追従表示され、`元に戻す（undo）` で取り消せます。

---

## スキルで“賢く”使う（任意）

**スキル**とは、AI に「各アプリの使い方・作法」を教える説明書（Markdown 1 ファイル）です。
入れておくと、AI が毎回正しい手順（**まず状態を読む → 提案 → 適用**）で動いてくれます。

### スキルの置き場（Claude Code の場合）
- 自分のどのプロジェクトでも効かせる: `~/.claude/skills/<スキル名>/SKILL.md`
- そのプロジェクトだけ: `<プロジェクト>/.claude/skills/<スキル名>/SKILL.md`

### URL からスキルを入れて発動する流れ
1. スキルの **URL（SKILL.md へのリンク）を AI に渡す**。
2. 「このスキルを `~/.claude/skills/` に保存して、有効化して」と頼む。
3. AI がファイルを保存 → スキルが有効化されます。
4. あとは `/<スキル名>` と打つか、関連する依頼をすると**自動で発動**します。

---

## うまく動かないとき

| 症状 | 対処 |
|---|---|
| 「接続されていません」「タイムアウト」「Failed to connect」 | AKARI で**そのアプリを開いているか**確認（窓口が動いているか）。`os_*` 系は AKARI を起動していれば OK。 |
| ツールが見つからない / 古い | アプリを更新・リロードしてから、AI 側で MCP を**つなぎ直す**。 |
| どんな操作ができるか分からない | AI に「使えるツールを一覧して」と聞く（常に最新が返ります）。 |
| アプリ名と窓口がわからない | 上の「ポート一覧」を参照。`os_list_apps` でも一覧できます。 |

---

AKARI Desktop の入手は [akari-oss.app](https://akari-oss.app) から。
