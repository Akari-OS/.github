---
title: "AKARI Cloud AI Pricing — 透明料金体系"
status: published
last_synced_from: akari-cloud/docs/governance/ai-pricing.md
last_synced_date: 2026-06-13
---

# AKARI Cloud AI Pricing

> **このドキュメントの位置づけ**: AKARI Cloud の AI クレジット課金は **USD ベース単価 + 10% margin** という透明な料金体系で運営されます。本ドキュメントは Hub 側の同名ファイルをフィルタ適用した**公開版**です（戦略根拠・admin 詳細は除外）。
> **最新版**: v0.2.0（2026-05-09 改訂）— USD 統一 / クーポンコード制 / SLA 観測公開
> **Hub 正典**: `akari-cloud/docs/governance/ai-pricing.md`
> 関連仕様: AKARI-HUB-059 AI クレジット統合ガイド

---

## 1. この文書について

AKARI Cloud は **透明性ポリシー** に基づき、AI クレジット課金を OpenRouter の実コストに直結させ、margin を明示する。

- **USD 統一**: 課金単位は USD（米ドル）に統一（v0.2.0）。OpenRouter 実費を USD で表現
- **Margin 公開**: クラウドプロバイダとしてのコスト（基盤維持・API サーバ・監視）は margin **10%** で固定・公開
- **Two-phase 課金**: pre-flight estimate でロック → settle で実費精算し、ユーザーの予測可能性を保つ
- **SLA 観測**: uptime / latency を公開 status page で継続報告（透明性強化 — v0.2.0 新規）

---

## 2. 料金体系の原則

### 2.1 通貨定義（USD 統一 — v0.2.0）

| 項目 | 値 | 説明 |
|---|---|---|
| **課金単位** | **USD（米ドル）** | OpenRouter と同じ通貨で直結（AC 概念は廃止）|
| **表示単価** | **$X.XX** | ドル表示が primary。JPY は参考表示のみ |
| **Margin** | **10%** | OpenRouter 実費 × 1.1 = 課金額 |
| **参考 JPY** | **当日為替** | Settings で更新（例: 1 USD = ¥155）|

**v0.2.0 変更の背景**:
- OpenRouter が USD ベース価格表示 → 直結させるのが透明性最大化
- 小数点以下コイン（AC）を廃止し、実通貨に統一
- Stripe 決済: 顧客通貨で請求（日本ユーザーは JPY）、レシートに USD 内訳明示

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
3. 実費に基づいた USD を決定
4. 差分がある場合：
   - 予測 > 実費 → 差分を返金
   - 予測 < 実費 → 追加消費＋ログ記録

**誤差許容範囲**: ±5%（pre-flight マージンで大半は収まる）

---

## 3. 通貨換算（USD 主軸、JPY 参考表示）

### 3.1 主単位：USD

課金・請求は **USD（米ドル）** 統一。小数点以下 2 桁で表示（例: $12.34）。

```
USD での計算例
- OpenRouter 実費 $0.0105 × 1.1 margin = $0.01155 → $0.01 or $0.02（端数処理）
- OpenRouter 実費 $0.50 × 1.1 margin = $0.55
- OpenRouter 実費 $10.00 × 1.1 margin = $11.00
```

### 3.2 参考：JPY 換算（UI 表示用のみ）

決済画面で JPY を表示する場合のみ使用。**公式金額は USD**（為替変動時の説明コストを避けるため）。

```
参考レート: 当日為替（環境変数で日次更新、デフォルト 155）

表示例）
- $0.01155 → $0.01 or $0.02 → "¥1.55" or "¥3.10"（当日為替で参考表示）
- $10.00 → "$10.00 (≒¥1,550)"
- $100.00 → "$100.00 (≒¥15,500)"
```

---

## 4. モデル別単価表

OpenRouter が提供するモデルについて、AKARI Cloud の価格設定を示します。

### 4.1 初期値（2026-05-09 時点）

| モデル | Provider | Input<br/>（$/1M tok） | Output<br/>（$/1M tok） | Tier | Apps |
|---|---|---|---|---|---|
| `anthropic/claude-opus-4` | Anthropic | 15.00 | 75.00 | premium | writer / design / video |
| `anthropic/claude-sonnet-4-6` | Anthropic | 3.00 | 15.00 | premium | writer / video |
| `anthropic/claude-haiku-4-5` | Anthropic | 0.80 | 4.00 | standard | all |
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
| **通貨** | **USD 統一** |

**実例**:
- モデル: `anthropic/claude-sonnet-4-6`
- 入力: 1000 token → $3.00 / 1M = $0.003
- 出力: 500 token → $15.00 / 1M = $0.0075
- 実費: $0.0105
- **Margin 適用**: $0.0105 × 1.1 = **$0.01155** → 端数処理で $0.01 or $0.02
- **参考 JPY**: 当日為替で ¥1.55 or ¥3.10（参考表示のみ）

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

## 6. クーポンコード制と利用上限（v0.2.0 新規）

### 6.1 Free プランの新仕様

v0.1.1 の「月 50 AC 無料枠」を廃止し、**クーポンコード制** に移行：

- **登録は無料**: アカウント作成・プロフィール設定は費用なし
- **利用には課金またはクーポン redeem**: AI 機能を使う際は、クレジットカード登録 OR クーポンコード入力
- **クーポン配布**: 招待施策（ベータユーザー / イベント / パートナー）で配布
- **1 ユーザー 1 コード制限**: 複数 redeem は不可
- **残高区分**: 購入残高（$額面）と無料残高（クーポン額）を分離表示

### 6.2 Spending Cap（利用上限）

| プラン | 月額上限<br/>（USD）| Burst 上限<br/>（USD/h） |
|---|---|---|
| **Free（coupon redeem）** | Coupon 額面 | Coupon 額面 × 0.2 |
| **Paid** | $20 | $5 |

**Burst guard の意図**: API キー流出・bot 暴走時に時間帯で抑止。

### 6.3 超過時の動作

**月額 cap 超過**:
- 402 Exceeded Limit レスポンス
- ユーザー account page に「超過」表示
- メール通知
- Admin dashboard に alert 表示

**Burst guard 超過**（1時間で cap 超過）:
- 自動 freeze（一時停止）
- メール通知
- ユーザーが自分で解除申請可能 or 48h 自動解除

### 6.4 Cap 設定の変更

ユーザーが Settings > Credits で cap を任意に下げることは可能。

---

## 7. 透明性コミットメント

### 7.1 SLA 観測と公開（v0.2.0 新規）

AKARI Cloud は SLA メトリクスを公開 status page で継続報告します。

**公開ページ**: `cloud.akari-oss.app/status`

**観測内容**:
- **Uptime**: 月次 / 四半期 / 年 (目標 99.9%)
- **Latency**: p50 / p95 / p99 (単位: ms)
- **エラー率**: API 5xx / 429 throttle / timeout 発生率 (月次 累計)

**公開スケジュール**:
- リアルタイム: 現在の uptime / latency（API endpoint で確認可能）
- 日次: メトリクス更新（UTC 00:00）
- 月次: レポート PDF 発行

### 7.2 ユーザーへの情報提供

**Cost Breakdown**: API レスポンスに cost 詳細を含める
```json
{
  "openrouter_cost_usd": 0.0105,
  "akari_margin_usd": 0.00105,
  "total_charged_usd": 0.01155,
  "timestamp": "2026-05-09T12:34:56Z"
}
```

**月次レポート**: opt-in で「あなたの今月の AI 利用」を email 配信
- 利用したモデル・回数・合計金額（USD）
- Spending cap までの残額
- 前月比

**Dashboard**: ユーザーの Settings > Credits で過去 30 日の USD 消費グラフを表示。

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

**A.** ユーザーです。AKARI は関与しません。Shell Settings で「月額予測」を表示する際は「Local-only: $0（GPU電気代別）」と明示します。

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

| 日付 | 変更内容 | 版番 |
|---|---|---|
| 2026-05-08 | 初版作成。Hub 側をフィルタ適用して公開版化 | v0.1.1 |
| 2026-05-09 | **v0.2.0 改訂**: USD 統一 / AC 廃止 / クーポンコード制 / SLA 観測公開 / キャンペーン透明性 | v0.2.0 |
