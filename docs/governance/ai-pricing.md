---
title: "AKARI Cloud AI Pricing — 透明料金体系"
status: published
last_synced_from: akari-cloud/docs/governance/ai-pricing.md
last_synced_date: 2026-05-08
---

# AKARI Cloud AI Pricing

> **このドキュメントの位置付け**: AKARI Cloud のクレジット課金は **OpenRouter 実コスト + 10% margin** という透明な料金体系で運営されます。本ドキュメントは Hub 側の同名ファイルをフィルタ適用した**公開版コピー**です。
> **更新**: Hub 側を更新後、公開ドキュメント管理ルール に従って本ファイルを再生成します。
> 関連仕様: [AKARI-HUB-059（AI クレジット統合ガイド）](https://akari-os.app)

---

## 1. この文書について

AKARI Cloud は **透明性ポリシー** に基づき、AI クレジット課金を OpenRouter の実コストに直結させ、margin を明示する。

- **実コスト連動**: OpenRouter が請求する USD → AC への変換は公式レート（1 AC = 0.0001 USD）に従う
- **Margin 公開**: クラウドプロバイダとしてのコスト（基盤維持・API サーバ・オートスケール）は margin **10%** を乗せて、公開・固定
- **Two-phase 課金**: pre-flight estimate でロック → settle で実費精算し、ユーザーの予測可能性を保つ

---

## 2. 料金体系の原則

### 2.1 通貨と単価定義

| 項目 | 値 | 説明 |
|---|---|---|
| **1 AC** | **0.0001 USD** | 小数 AC を許容（表示は 0.01 AC 単位で四捨五入） |
| **1 AC** | **≒ ¥0.015** | 参考値。`¥1 / 67` に近似（実際のレート変動は考慮しない）|
| **Margin** | **10%** | OpenRouter 実費の 110% を消費として課金 |
| **合計上乗せ** | **~15%** | OpenRouter margin 5% + AKARI margin 10% = 合計 ~15% |

**レート設定のポイント**:
- AC を小さな単位にすることで小額利用（数円）も表現可能
- OpenRouter の価格カタログ（`GET /api/v1/models`）が USD 表記のため、USD → AC の変換は固定レートで提供
- 円表示は参考値のみ（為替変動時の説明コストを避けるため）

### 2.2 課金フロー（Two-phase 方式）

AI リクエストは以下の 2 段階で処理される。

#### Phase 1: Pre-flight Estimate & Lock

1. モデル許可リスト確認（未許可なら拒否）
2. 推定コストを計算
3. 安全マージン 50% を含めた pending credit をロック
4. spending cap チェック（月額上限・Burst 上限超過で拒否）
5. OpenRouter API 呼び出し開始

#### Phase 2: Settle（実費精算）

1. OpenRouter から実際のトークン数を取得
2. 実コストを計算し margin 10% を適用
3. 実費に基づいた AC を決定
4. 差分がある場合：
   - 予測 > 実費 → 差分を返金
   - 予測 < 実費 → 追加消費＋ログ記録

**誤差許容範囲**: ±5%（pre-flight マージンで大半は収まる）

---

## 3. レート定義と換算

### 3.1 AC ↔ USD の固定レート

```
1 AC = 0.0001 USD（固定）

例）
- $0.00123 → ceil(12.3) = 13 AC
- $0.10 → ceil(1000) = 1000 AC  
- $1.00 → ceil(10000) = 10000 AC
```

### 3.2 参考：JPY 換算（表示用）

決済画面で JPY を表示する場合のみ使用。**ユーザーへの説明は AC ベース**（為替による混乱回避）。

```
参考レート: 1 USD ≒ 150 JPY

例）
- 1000 AC = $0.10 ≒ ¥15
- 10000 AC = $1.00 ≒ ¥150
- 300000 AC = $30 ≒ ¥4,500
```

---

## 4. モデル別単価表

OpenRouter が提供するすべてのモデルについて、AKARI Cloud は適切な価格設定を行います。

### 4.1 初期値（2026-05-08 時点）

| モデル | Provider | Input<br/>（$/1M tok） | Output<br/>（$/1M tok） | Tier | Apps |
|---|---|---|---|---|---|
| Claude Opus | Anthropic | 15.00 | 75.00 | premium | writer / design / video |
| Claude Sonnet | Anthropic | 3.00 | 15.00 | premium | writer / video |
| Claude Haiku | Anthropic | 0.80 | 4.00 | standard | all |
| Gemini Flash | Google | 0.10 | 0.40 | standard | all |
| GPT-4o | OpenAI | 2.50 | 10.00 | premium | all |
| GPT-4o Mini | OpenAI | 0.15 | 0.60 | standard | all |
| Llama 3.1 8B | Meta | 0.05 | 0.10 | standard | all |
| Llama 3.1 70B | Meta | 0.35 | 0.70 | standard | all |

### 4.2 単価更新ポリシー

OpenRouter の価格が変動した場合、AKARI Cloud はマージン 10% を維持しながら単価を更新します。基本的にはマージンは変更せず、OpenRouter の価格変更を反映します。

---

## 5. 経路別課金ポリシー

AKARI エコシステムは 3 つの AI 経路を提供する。各経路の margin 設定は異なる。

### 5.1 AKARI Cloud Credit（`cloud` 経路）

| 項目 | 値 |
|---|---|
| **Margin** | 10% |
| **課金対象** | 認証ユーザー |
| **対応** | 全 app（Writer / Video / Design / Shell / Chat Panel 等） |

**実例**:
- モデル: Claude Sonnet
- 入力: 1000 token → $3.00 / 1M = $0.003
- 出力: 500 token → $15.00 / 1M = $0.0075
- 実費: $0.0105
- **Margin 適用**: $0.0105 × 1.1 = $0.01155
- **課金**: ceil($0.01155 / 0.0001) = **116 AC** (≒ ¥1.74)

### 5.2 BYOK（Bring Your Own Key、`byok` 経路）

| 項目 | 値 |
|---|---|
| **Margin** | 0%（実費のみ） |
| **課金対象** | なし（ユーザーが OpenRouter キー負担） |
| **対応** | advanced user（要設定） |

**メリット**: AKARI margin が不要なため、OpenRouter 実費のみで利用可能。

### 5.3 Local LLM（`local` 経路）

| 項目 | 値 |
|---|---|
| **Margin** | N/A |
| **課金対象** | なし |
| **対応** | 軽量・オフライン要件 |

GPU 電気代はユーザーの infrastructure cost です。

---

## 6. Spending Cap（利用上限）

### 6.1 デフォルト値

| プラン | 月額上限<br/>（JPY） | Burst 上限<br/>（JPY/h） |
|---|---|---|
| **Free** | 無料枠に従う | - |
| **Paid** | ¥3,000 | ¥500 |

**Burst guard の意図**: API キー流出・bot 暴走時に時間帯で抑止する。

### 6.2 超過時の動作

**月額 cap 超過**:
- 402 Exceeded Limit レスポンス
- ユーザー account page に「超過」表示
- メール通知
- Admin dashboard に alert 表示

**Burst guard 超過**（1時間で ¥500 超過）:
- 自動 freeze（一時停止）
- メール通知
- ユーザーが自分で解除申請可能 or 48h 自動解除

### 6.3 Cap 設定の変更

ユーザーが Settings > Credits で cap を任意に下げることは可能。

---

## 7. 透明性コミットメント

### 7.1 ユーザーへの情報提供

**Cost Breakdown**: API レスポンスに以下を含める
```json
{
  "openrouter_usd": 0.0105,
  "margin_yen": 0.16,
  "total_ac": 116,
  "timestamp": "2026-05-08T12:34:56Z"
}
```

**月次レポート**: opt-in で「あなたの今月の AI 利用」を email 配信
- 利用したモデル・回数・合計金額
- Spending cap までの余額
- 前月比

**Dashboard**: ユーザーの Settings > Credits で過去 30 日の AC 消費グラフを表示。

### 7.2 公開ドキュメント更新スケジュール

本ドキュメント（公開版）は以下の trigger で更新されます：

- Margin 変更時
- モデル単価大幅変更（5% 超）
- 年 1 回以上のレビュー確認

---

## 8. 利用規約上の位置付け

### 8.1 Cloud Credit の利用対象

**対象**: 公式 AKARI アプリ（Writer / Video / Design / Shell / Chat Panel 等）を通常の目的で使用するユーザー

**除外**:
- Bot / 自動化スクリプトによる大量リクエスト
- API リミット違反（60 req/min/user 超過）

### 8.2 OSS fork の扱い

AKARI を fork / 自作する場合、公式 Cloud Credit は利用不可：

- **BYOK 経由なら許可**（ユーザーの OpenRouter キー負担）
- **Local LLM 限定も許可**（Ollama 等）
- 非公式ビルド版は AKARI Cloud API にアクセス不可（auth fail）

---

## 9. Q&A

### Q. なぜ margin 10% なのか？

**A.** クラウドプロバイダの標準慣例：
- API サーバ・データベース・監視・async job などの基盤維持コスト
- AWS Lambda / Azure OpenAI と同等の水準（業界標準 15〜25%）
- 透明性ポリシーに基づき固定・公開

### Q. Margin は時間帯別・モデル別で変動するか？

**A.** v1 では固定です。需要があれば将来のバージョンで検討。ただし透明性ポリシーを維持します。

### Q. Local LLM の GPU 電気代は誰が払う？

**A.** ユーザーです。AKARI は関与しません。Shell Settings で「月額予測」を表示する際は「Local-only: ¥0（GPU電気代別）」と明示します。

### Q. BYOK 経由で同じモデルを使う場合、Cloud と同じ単価か？

**A.** いいえ。BYOK は AKARI margin 0% で OpenRouter 実費のみ。ユーザー負担は Cloud Credit より軽いです。

---

## 10. 関連ドキュメント

| ドキュメント | 内容 |
|---|---|
| [AKARI Vision](../../VISION.md) | 公開版ビジョン |
| [AKARI Roadmap](../../ROADMAP.md) | 公開ロードマップ |
| [Akari-OS/.github](https://github.com/Akari-OS) | AKARI 公開ハブ |

---

## 11. 更新履歴

| 日付 | 変更内容 |
|---|---|
| 2026-05-08 | 初版作成。Hub 側をフィルタ適用して公開版化 |
