# Memory Architecture — Akari-OS

> **AI OS における記憶はまだ誰も解けていない問題。**
> このドキュメントは、Akari-OS がその問題にどう向き合うかの哲学的立場表明。

---

## なぜこれを書くか

Letta / Mem0 / MemU / LangMem / Zep — 長期記憶を扱うプロダクトはたくさんある。
でも誰もまだ「記憶とは何か、どこに置くべきか、いつ引き出すか」の**明確な設計哲学**を
持っていない。みんな手探り。

Akari-OS は「AI OS」を名乗る以上、ここを避けて通れない。
本ドキュメントでは、**Pool + 4 層モデル**という Akari-OS 独自のアプローチを示す。

---

## 問題の正体

「長期記憶」という言葉には、実は**2 つの違う軸**が混在している：

```
         ┌─────────────────┬──────────────────┐
         │ 明示的（deliberate）│ 暗黙的（ambient）│
         │ "これ保存して"      │ "勝手に覚えてて"   │
┌────────┼─────────────────┼──────────────────┤
│ 生 raw  │ 動画・PDF・ノート   │ 会話ログ・行動履歴 │
├────────┼─────────────────┼──────────────────┤
│ 抽出・蒸留│ 要約・タグ・注釈    │ 好み・パターン・関係│
└────────┴─────────────────┴──────────────────┘
```

従来のプロダクトは「暗黙的 × 抽出」だけを "memory" と呼び、残りの象限を外部に
押し付けてきた。その結果：

- 会話ログが宙に浮く（どこにも置き場がない）
- Memory に raw を入れ始めると肥大化して管理不能に
- 「Vision」や「好きな色」のような固定方針まで Memory として扱われ、毎回曖昧な確率で再現される
- Memory の出所が辿れない（「なぜ AI はこれを知ってるのか」が不明）

**Akari-OS の立場**: この 4 象限すべてに居場所を与える。そのために 4 層モデルを採用する。

---

## Akari-OS の 4 層モデル

```
┌─────────────────────────────────────────────┐
│ L3: Working Memory（今の会話・短期）           │ ← セッションで消える
│     LLM のコンテキストウィンドウ内              │
├─────────────────────────────────────────────┤
│ L2: Long-term Memory（好み・パターン・関係）    │ ← AMP が扱う層
│     蒸留された小さな事実。Pool を参照するだけで   │   サイズは限定的
│     生データは持たない                          │
├─────────────────────────────────────────────┤
│ L1: Pool（基質・Source of Truth）              │ ← AkariPool 本体
│     全 raw データ + Wiki 層（整形済み markdown）│   サイズ無制限
│     動画・PDF・画像・ノート・**会話ログ** も全部  │
├─────────────────────────────────────────────┤
│ L0: Constitution（憲法・不変方針）              │ ← 最下層・絶対
│     Vision、コーディング規約、NG ワード等        │   人間が明示的に書く
└─────────────────────────────────────────────┘
```

各層は明確に役割が違う。**上位層は下位層を参照するが、置き換えない**。

---

## L0: Constitution（憲法）

### 何を置くか

**滅多に変わらない、絶対的な方針**。

- Vision（プロダクトの北極星）
- ブランドガイドライン（色、フォント、トーン）
- コーディング規約
- NG ワード / センシティブトピック
- 承認フローのデフォルト権限
- 言語設定（日本語で応答する、等）

### 特徴

- **人間が明示的に書く**（フォームや Markdown で）
- **AI による自動書き込み禁止**
- **全セッション起動時に必ず読み込まれる**（確率ではなく保証）
- **バージョン管理される**（Git or タイムスタンプ付きスナップショット）

### 物理配置

```
~/.akari/
└── constitution/
    ├── vision.md           ← プロダクトの方針
    ├── brand.yaml          ← 色・フォント・トーン
    ├── coding-style.md     ← 技術規約
    ├── privacy.yaml        ← プライバシー設定
    └── ng-words.txt        ← 禁止ワード
```

### なぜ Memory と分けるのか

Memory は「AI が観察から推論した不確実な知見」。Constitution は「人間が定めた確定事項」。
両者を同じ層に混ぜると、**確定事項が確率で揺らぐ**。これは許容できない。

例:
- ❌ Memory: "ユーザーはたぶん日本語を好む (confidence: 0.9)"
- ✅ Constitution: "応答言語: 日本語" ← 100% 保証

Claude Code の `CLAUDE.md` がまさにこの層。Akari-OS はこの考え方をエコシステム全体に拡張する。

---

## L1: Pool（基質）

### 何を置くか

**すべての raw データ**。動画・PDF・画像・ノート・データセット・コード ——
そして**会話ログも**。

Pool は「唯一の真実の源（Source of Truth）」。大きくなることを前提に設計されている。
Workspace による隔離で管理可能性を保つ。

### 会話ログの扱い

会話ログは立派な artifact。Pool に入れる：

```
生 chat.md (セッション終了時)
    │
    ▼ Pool.add(file: chat.md, modality: conversation)
    │
L1.raw: workspace/X/files/{uuid}/chat.md          ← 永続、変更しない
    │
    ▼ ConversationAnalyzer 自動実行
    │
L1.wiki: workspace/X/notes/{uuid}.md              ← 整形版
    │  - 参加者（ユーザー / Claude / 他エージェント）
    │  - トピックタグ
    │  - 決定事項のリスト
    │  - 学びの抽出
    │
    ▼ Filed back ループ
    │
L2 Memory に自動追記
    - "ユーザーは X を好むと発言" → preference
    - "決定: 来週までに Y をやる" → episodic
    - "X は Y と関連する" → relation
```

### サイズ問題への対策

Pool が肥大化することは避けられない。対策は 3 つ：

1. **Workspace による隔離** — プロジェクトごとに分離、横断は明示的にのみ
2. **Wiki 層で圧縮** — raw は残しつつ、検索は要約版で行う（10倍〜100倍効率化）
3. **Cold archive 化** — 古い workspace は `_archive/` に移動、インデックスから除外

> 💡 **重要な設計判断**: Pool の肥大化を恐れて raw を捨てることはしない。
> Raw は永久に残すのが原則。検索効率は Wiki 層で解決する。

---

## L2: Long-term Memory（長期記憶）

### 何を置くか

**蒸留された小さな事実**。好み・パターン・関係・エピソード。

Memory は Pool を**参照するだけで、内容を複製しない**。これが最も重要な設計判断。

```json
{
  "memory_id": "pref-001",
  "type": "preference",
  "fact": "明るいキッチンシーンを好む",
  "evidence": [
    "pool://cooking-2026/item/abc",
    "pool://cooking-2026/item/def"
  ],
  "confidence": 0.82,
  "workspace": "cooking-2026",
  "created_at": "2026-03-15T09:21:00Z",
  "last_reinforced": "2026-04-08T14:03:00Z",
  "reinforcement_count": 12
}
```

### Memory の型（5 種類）

| 型 | 例 |
|---|---|
| **Preference** | "明るい色味を好む" "BGM は 120 bpm 前後" |
| **Pattern** | "夜 22 時以降に動画編集することが多い" |
| **Relation** | "item A と item B は同じプロジェクト" |
| **Episodic** | "2026-04-01 にロゴのリニューアルを決定した" |
| **Semantic** | "このユーザーの文脈では『ブランド』= 森ラボを指す" |

### Write 戦略（いつ書くか）

| タイミング | 方法 | 理由 |
|---|---|:-:|
| ❌ セッション中 | 禁止 | プロンプト汚染のリスク |
| ✅ セッション終了時 | Hook がバッチ処理 | 汚染なし、まとまって処理できる |
| ✅ 明示的 signal | "これ覚えといて" 等 | ユーザー意図が明確 |
| ✅ 夜間 Consolidation | Backstage Job System | じっくり統合できる |

**重要**: セッション中に Memory を書かない。これは Claude Code のメモリ運用と同じ原則。
指示のプロンプトに `memorize` 命令が紛れ込むと、AI が自分の記憶を汚染する危険がある。

### Read 戦略（いつ読むか）

| タイミング | 方法 |
|---|---|
| **セッション開始** | Hook が自動で関連 Memory を retrieve |
| **文脈トリガー** | 発話中の固有名詞・人名・プロジェクト名がキーと一致 → 自動 retrieve |
| **Agent 自発** | AI が必要と判断したら `memory_search` ツールを呼ぶ |
| **Push 型通知** | "2 週間前このトピック話したけど続き？" の積極提案 |

**Memory 検索は Pool 検索の軽量版**。L2 は L1 より圧倒的に小さいので、
同じ rmcp サーバーに `memory_search` ツールを生やすだけで済む。別インフラは要らない。

---

## L3: Working Memory（ワーキングメモリ）

現在のセッションのコンテキストウィンドウそのもの。セッションが終わると消える。
ただし**重要な学びは L2 に昇格**される（セッション終了時の Hook が判定）。

人間の脳における短期記憶と同じ役割。

---

## Consolidation（重複・矛盾の解消）

複数の Memory が発生したとき、どう統合するか。2 つのモードを提供する：

### Mode A: ルールベース

```yaml
consolidation:
  mode: rules
  rules:
    - condition: same memory_id + newer data
      action: overwrite
    - condition: same fact + higher confidence
      action: merge (higher confidence 採用)
    - condition: contradictory facts
      action: mark_as_conflicted  # 両方残して flag を立てる
    - condition: reinforcement_count > 10
      action: promote_to_canonical  # 頻度高いものは確定
```

- **メリット**: 予測可能、デバッグしやすい
- **デメリット**: 柔軟性なし、ニュアンスが拾えない

### Mode B: LLM-based

夜間 Consolidation 時に LLM が矛盾を検証する：

```yaml
consolidation:
  mode: llm
  llm:
    model: local-qwen3-8b
    actions_allowed: [merge, split, drop, mark_conflicted]
    require_rationale: true  # 判断理由を memory に記録
```

例:
```
矛盾ペア:
  M1: "ユーザーは静かな BGM を好む" (2026-02-01, confidence: 0.7)
  M2: "ユーザーはノリのいい BGM を好む" (2026-04-05, confidence: 0.8)

LLM 判断:
  action: split
  result:
    - "瞑想・作業動画では静かな BGM を好む" (M1 を文脈限定で保存)
    - "プロモ動画ではノリのいい BGM を好む" (M2 を文脈限定で保存)
  rationale: "動画ジャンルによって好みが変わることを観察した"
```

- **メリット**: 文脈を理解した統合ができる
- **デメリット**: 遅い、高コスト、再現性が低い

### Mode C: Hybrid（推奨デフォルト）

```yaml
consolidation:
  mode: hybrid
  primary: rules
  fallback_to_llm: true  # ルールで解決できない場合のみ LLM
```

ほとんどの統合はルールで瞬時に処理、微妙なケースだけ LLM に委ねる。

---

## Privacy Boundaries（プライバシー境界）

### デフォルト: strict workspace isolation

```yaml
# ~/.akari/constitution/privacy.yaml
privacy:
  memory_scope: workspace  # デフォルト
  pool_scope: workspace    # デフォルト
```

- Workspace A の Memory は Workspace B から**見えない**
- Pool のアイテムも workspace-scoped
- cross-workspace リンクは明示的に張らないと機能しない

### Opt-in 緩和

ユーザーが選べる：

```yaml
privacy:
  memory_scope: workspace
  cross_workspace_allow: true
  shareable_memories: ["preferences", "coding-style"]  # 型単位で共有許可
```

あるいは Memory 単位で：

```json
{
  "memory_id": "pref-001",
  "shareable": true,
  "visible_workspaces": ["cooking-2026", "vlog-2026", "default"]
}
```

### なぜこう設計するか

多くの AI ツールは「全データを共有」か「完全分離」の二択しかない。
Akari-OS は**グラデーションを提供する**。仕事とプライベートで記憶を混ぜたくない
ユーザーもいれば、全部混ぜたいユーザーもいる。両方サポートする。

---

## 既存システムとの違い

| 観点 | Mem0 / MemU / Letta / LangMem | **Akari-OS** |
|---|---|---|
| **Raw データの置き場** | 外部システム or なし | **Pool が Raw 基質として存在** |
| **Memory の出所証跡** | 多くの場合 None / opaque | **evidence フィールドで必ず Pool を参照** |
| **Constitution 層** | 存在しない（通常 System Prompt に混ぜる） | **独立層として明示** |
| **会話ログの扱い** | 別管理 or Memory に埋め込み | **Pool の1モダリティとして統合** |
| **プライバシー境界** | 通常 single scope | **Workspace 隔離 + opt-in 緩和** |
| **Consolidation** | LLM または単純ルール | **Rule + LLM の hybrid 選択可** |
| **検証可能性** | Memory が何を根拠にしているか不明 | **`pool://` リンクで常に検証可能** |

### 決定的な差別化: Verifiability（検証可能性）

既存の Memory システムは「AI がこう覚えている」という状態だけを持つ。
ユーザーは「なぜ AI はそう思うのか」を確かめられない。

Akari-OS の Memory は**必ず Pool のアイテムを evidence として参照する**。
ユーザーが「なぜ明るいキッチンが好きだと思ったの？」と聞けば、
AI は「この 3 つの動画で明るいキッチンの場面を長時間編集していたから」と
**具体的な Pool アイテムを提示**できる。

これが Akari-OS が「AI が勝手に決めつける不気味な体験」を避ける方法。

---

## 未解決の問題（正直に言う）

以下は本ドキュメントでも解決していない。今後の研究課題：

### 1. Memory の良し悪しを測る指標

「いい Memory」とは何か？ 評価関数がない。
- ユーザーが修正した頻度？
- 再利用された回数？
- エージェントの出力精度への寄与度？

### 2. Consolidation の最適アルゴリズム

Rule と LLM の hybrid は理論上いいが、どこで線を引くかは試行錯誤。

### 3. Memory の「忘れ方」

人間の記憶は時間で薄れる（decay）。Akari-OS の Memory もそうすべきか？
完全保存すべきか？ 忘却を制御するのは難しい。

### 4. マルチユーザー環境での境界

個人ツールとしては workspace 隔離で十分。でもチーム利用時は？
誰の Memory が誰に見えるか、承認プロセスが要る。

これらは Akari-OS が成長する過程で実装しながら解いていく。

---

## まとめ

Akari-OS の記憶観は以下の原則に集約される：

1. **4 層を明確に分ける** — Constitution / Pool / Memory / Working
2. **Constitution は人間が書き、AI は書き換えない** — 確定事項は確率で揺らさない
3. **Pool が唯一の Source of Truth** — Memory は Pool を参照するだけ
4. **会話ログも Pool に入れる** — Analyzer で前処理、Wiki 層で圧縮
5. **Memory は小さく保つ** — 蒸留された事実のみ、raw は持たない
6. **Write は session 外、Read は文脈駆動** — 汚染を避け、精度を保つ
7. **Consolidation は選べる** — Rule / LLM / Hybrid
8. **Privacy はグラデーション** — strict default + opt-in 緩和
9. **Memory は検証可能であるべき** — evidence フィールドで常に出所を示す

この設計が「AI が自分をちゃんと理解してくれている、でも不気味じゃない」
体験の基盤になる。

---

## 関連ドキュメント

- 📖 **[Vision](../VISION.md)** — Akari-OS 全体の構想
- 🗺 **[Roadmap](../ROADMAP.md)** — 実装の段階
- 🏗 **[AkariPool](https://github.com/Akari-OS/pool)** — Pool の実装リポジトリ
- 🧠 **[AMP](https://github.com/Akari-OS/amp)** — Agent Memory Protocol 仕様

## 貢献・議論歓迎

本ドキュメントは**ドラフト**。完璧な答えではない。
この設計に賛同する方、異論がある方、どちらも [GitHub Discussions](https://github.com/Akari-OS/.github/discussions) で議論を歓迎する。

特に歓迎する意見:
- 既存プロダクト（Mem0 / Letta / Zep 等）との具体的な比較
- 実装時の落とし穴
- プライバシー境界のエッジケース
- Consolidation ルールの改善案

---

*"記憶は AI OS の魂である。設計を間違えれば、どんなに優秀なエージェントも信頼されない。"*
