<div align="center">
  <img src="./banner.svg" alt="Akari-OS — 個人のための AI OS" width="100%">
</div>

# Akari-OS

> **個人のための AI OS** — あなた専属の AI チームが、目に見える形で働いてくれる OS。

ローカルファーストで動く、AI ネイティブなアプリ・コンステレーション。
1 つの巨大アプリではなく、**軽い「器」(Shell) の中に Module が灯り、
重い仕事は独立アプリが担う**エコシステム。すべてがローカルで繋がり合っている。

---

## 解きたい問題

> ひとりのクリエイターが、チームを雇わずに、
> アイデアからお金に変わるまでの全工程を回せたら？

動画を作る。ブログに載せる。SNS で告知する。分析して改善する。
今これをやるには、4 人以上のチームか、10 個のバラバラなツールが要る。

Akari-OS は、**あなた専属の AI チーム**がこれを全部やる。
しかもローカルで。速くて、安くて、あなたのデータはあなたのもの。

---

## 解決策: あなた専属の AI チーム

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

チャットログを読み返す必要はない。**全員の状態が一目でわかる**。
承認が必要なものだけタップする。

---

## 3 つの原則

### 1. 決め打ち
エージェントの構成は**最初から決まっている**。ユーザーが設定する必要はない。
開いたら、もう全員いる。もう動いてる。あなたは「これやって」と言うだけ。

### 2. 見える
エージェントが何をしているかが**視覚的にわかる**。
動いてるのが画面の上で見える。ログを遡らなくていい。

### 3. 寄り添う
完全自動ではない。**あなたの感性を最大化するツール。**
AI が得意なこと（大量処理、24 時間稼働）は AI。
人間が得意なこと（「これが好き」「ここをこう変えたい」）は視覚的に指示する。

---

## 3 層構造の AI スタック

「AI 搭載ツール」ではなく「AI OS」である理由。

```
┌─────────────────────────────────────────┐
│ ハーネス    ─ AI をどう走らせるか         │
│ Job System / 承認フロー / 自律レベル      │
├─────────────────────────────────────────┤
│ コンテキスト ─ AI に何を見せるか          │
│ M2C / Pool / Memory (AMP)              │
├─────────────────────────────────────────┤
│ プロンプト   ─ AI への指示を最適化        │
│ Skill Manifest / エージェント定義        │
└─────────────────────────────────────────┘

Adobe / Canva  = 第1層（プロンプト）だけ → 「AI 搭載」
Akari-OS       = 3 層全部              → 「AI OS」
```

---

## 星座のように繋がるアプリ群

Akari-OS は 1 つの巨大アプリではない。
**軽い「器」の中に Module が灯り、重い仕事は独立アプリが担う。**
**そのすべてがローカルで繋がり合っている。**

```
┌─────────────────────────────────────────┐
│         Akari-OS（Shell + Module）       │
│   軽量な器。開いたら、もう全員いる。         │
│  ┌──────┐┌───────┐┌──────┐┌─────────┐  │
│  │ Home ││Writer ││ Pool ││ Memory  │  │
│  │      ││       ││Browser│ Viewer │  │
│  └──────┘└───────┘└──────┘└─────────┘  │
│       ↑ Module が灯るように切り替わる       │
└─────────────────┬───────────────────────┘
                  │
                  │   ╔═══════════════╗
                  ├──►║  Akari Video  ║  独立アプリ
                  │   ║ (ネイティブ)    ║  （重い / 専門）
                  │   ╚═══════════════╝
──────────────────┴──────────────────────
         共有層（ローカル・常駐）
┌──────────────┐ ┌──────┐ ┌──────┐ ┌─────┐
│ Agent daemon │ │ Pool │ │ AMP  │ │ M2C │
│  （7 人固定）  │ │ 素材  │ │ 記憶 │ │意味 │
└──────────────┘ └──────┘ └──────┘ └─────┘
```

- **器は 1 つ**: Shell を開けば、そこに 7 エージェントと全 Module が揃っている
- **Module は星座**: Home / Writer / Pool Browser / Memory Viewer … 必要なものが灯る
- **独立アプリは必要な時だけ**: Akari Video のようにネイティブ性能が要るものだけ別プロセス
- **共有層は常駐**: Agent daemon・Pool・AMP・M2C はどの Module / アプリからも同じものが見える

動画を作ったら、**アップロードなし**で記事 Module に反映。
書くときに選んだデザインの好みが SNS 投稿にも自動で反映（Memory 共有）。
全メディアは Pool 登録時に AI が意味を理解（M2C コンテキスト化）。

Module や独立アプリが増えるほど、同じ共有層を通じてエコシステム全体が強くなる。

---

## リポジトリ一覧

### 🧬 Core 層 — プロトコルと共有データ

| Repository | 内容 |
|---|---|
| **[pool](https://github.com/Akari-OS/pool)** | Universal Knowledge Store。SQLite + FTS5 + Analyzer + MCP サーバー (11 ツール) + Obsidian 同期 |
| **[m2c](https://github.com/Akari-OS/m2c)** | Media-to-Context プロトコル。メディアを構造化された AI コンテキストに変換 |
| **[amp](https://github.com/Akari-OS/amp)** | Agent Memory Protocol。記憶の保存・検索・減衰を標準化 |
| **[ace](https://github.com/Akari-OS/ace)** | Agent Context Engineering Framework。エージェントに渡すコンテキストの組み立て方と品質 Lint を標準化 |

#### 📡 AKARI Protocol Suite

Core 層は **4 つのプロトコル**で AI エージェント向け OS レベル標準を構成する。

| プロトコル | 標準化対象 | リポ |
|---|---|---|
| **MCP**  | ツール呼び出し | (external) [modelcontextprotocol.io](https://modelcontextprotocol.io) |
| **M2C**  | メディア → コンテキスト変換 | [m2c](https://github.com/Akari-OS/m2c) |
| **AMP**  | エージェント記憶 | [amp](https://github.com/Akari-OS/amp) |
| **ACE**  | エージェントコンテキスト + Lint | [ace](https://github.com/Akari-OS/ace) |

> **AKARI Protocol Suite — The OS-level standards for AI agents.**
> Tools / Memory / Media / Context — all standardized.

### 📱 Apps 層 — ユーザーが触る場所

| Repository | 内容 |
|---|---|
| **[video](https://github.com/Akari-OS/video)** | Akari Video — デスクトップ動画編集（Tauri + Rust + React） |
| **[cloud](https://github.com/Akari-OS/cloud)** | Akari Cloud — 認証・クレジット・マーケットのバックエンド |
| **[voice](https://github.com/Akari-OS/voice)** | コミュニティフィードバック + チェンジログ |
| **[lp](https://github.com/Akari-OS/lp)** | ランディングページ (akari-oss.app) |

---

## なぜローカルか

```
クラウド依存:
  動画アップロード（5 分待ち）→ AI 処理（課金）→ ダウンロード（3 分待ち）
  = 遅い、高い、オフラインで使えない

Akari-OS:
  Pool に既にある → AI は既に理解してる → 結果は即反映
  = 速い、安い、オフラインでも全機能動作
```

### コスト革命

```
従来（月額サブスク地獄）:
  Premiere + Canva + Buffer + ConvertKit + ... = 月 5〜30 万円

Akari-OS:
  ソフト: 無料（OSS）
  AI:    ローカル推論（Qwen / Llama）でほぼ無料
  重い処理だけ: Akari Cloud（月数千円）
```

---

## 誰のためか

### GTM エンジニア — 1 人が 3 チーム分

```
従来:      クリエイター + マーケター + エンジニア + オペレーター = 4 人以上
Akari-OS:  あなた + AI チーム                                 = 1 人で全部
```

- **プロダクトが決まっている人**: やりたいことはある → AI チームが動く
- **これから始めたい人**: 何から始めればいいかわからない → パートナーが「まずこれ」と提案
- **ソロクリエイター**: 全工程を 1 人で回したい → 寝てる間も進む

---

## ステータス

🚧 **Early development** — 主要リポは Phase 開発中。
Star / Issue / PR はどのリポにも大歓迎。

- **pool**: Phase 6 完了（MCP 11 ツール + Obsidian 同期 + プリセット）— 最も進んでる旗艦リポ。次は Phase 7（Shell 統合）
- **video**: Step 2 完了（CLI + GUI + MCP サーバー）
- **m2c / amp**: v0.1〜0.2 Draft
- **ace**: v0.1-draft 公開（2026-04-14）— Phase C 素振り中
- **cloud**: Phase 4 完了（認証 / クレジット / マーケット）

---

## ライセンス

リポジトリごとに個別設定：

- **pool**: AGPL-3.0
- **m2c / amp**: Apache-2.0
- **video / cloud / voice / lp**: 各リポジトリを参照

---

## 📚 ドキュメント

- 📖 **[Vision](https://github.com/Akari-OS/.github/blob/main/VISION.md)** — 構想・原則・7 エージェントの詳細
- 🗺 **[Roadmap](https://github.com/Akari-OS/.github/blob/main/ROADMAP.md)** — 段階的ロードマップ
- 🧠 **[Memory Architecture](https://github.com/Akari-OS/.github/blob/main/docs/memory.md)** — 記憶の 4 層モデル（Constitution / Pool / Memory / Working）
- 🤝 **[Contributing](https://github.com/Akari-OS/.github/blob/main/CONTRIBUTING.md)** — 貢献方法・コーディング規約（Issue / PR Template が整備されています）
- 📜 **[Code of Conduct](https://github.com/Akari-OS/.github/blob/main/CODE_OF_CONDUCT.md)** — 行動規範
- 🔒 **[Security Policy](https://github.com/Akari-OS/.github/blob/main/SECURITY.md)** — 脆弱性報告

---

## ☕ プロジェクトを応援する

Akari-OS は個人開発の OSS プロジェクト。応援してくれたら泣いて喜びます。

- ⭐ **各リポに Star** を付ける（タダで一番嬉しい）
- 🐛 **Issue で使い方・バグ・感想**を教えてくれる
- 💬 **[Discussions](https://github.com/Akari-OS/.github/discussions)** で議論に参加する
- ☕ **[Buy Me a Coffee](https://buymeacoffee.com/kyacharsx)** で 1 杯分の応援
- 🌐 プロジェクトを **SNS で広めてくれる**

ひとりで作っていると、正直しんどい時もあります。
「応援してるよ」という一言が、想像以上に力になります。

---

> **Akari-OS は「ツール」ではない。**
> **あなた専属の AI チームが住んでいる、個人のための OS。**
> **開いたら、もう全員いる。もう動いてる。あなたは「これやって」と言うだけ。**

---

## 💡 名前の由来

**Akari**（あかり / 明かり）は日本語で「光・明るさ」を意味する。
そして "**A**kar**i**" の最初と最後の文字を取ると "**AI**"、そこに "**OS**" を加えると **"AI OS"** になる。

🇯🇵 **日本発の AI OS** — クラウド依存ではなく、ローカルで動く、個人のための AI OS。
その先駆けになりたいという願いを込めて。

詳しい背景は [Vision](https://github.com/Akari-OS/.github/blob/main/VISION.md#なぜ-akari-os-なのか--名前の由来) を参照。

---

*Built by [@ryoma-nakajima](https://github.com/ryoma-nakajima) — **GTM パイオニア**、そして成長中のコミュニティ。*
