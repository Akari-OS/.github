# AKARI Reusable Workflows

> AKARI エコシステム共通の GitHub Actions reusable workflows。
> 各実装リポから `uses: Akari-OS/.github/.github/workflows/<file>.yml@main` で参照する。

## 提供するワークフロー

| Workflow | 役割 | 必要シークレット |
|---|---|---|
| [`spec-check.yml`](./spec-check.yml) | spec / ADR の frontmatter 検証 | なし |
| [`markdown-lint.yml`](./markdown-lint.yml) | markdownlint + リンク切れチェック | なし |
| [`claude-review.yml`](./claude-review.yml) | Claude Code Action による AI PR レビュー | `ANTHROPIC_API_KEY` |

## 使い方

各実装リポの `.github/workflows/ci.yml` に以下を追加：

```yaml
name: CI
on: [push, pull_request]

jobs:
  spec-check:
    uses: Akari-OS/.github/.github/workflows/spec-check.yml@main
    with:
      spec_path: docs/specs

  markdown:
    uses: Akari-OS/.github/.github/workflows/markdown-lint.yml@main

  claude-review:
    if: github.event_name == 'pull_request'
    uses: Akari-OS/.github/.github/workflows/claude-review.yml@main
    secrets:
      ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

これだけで全 SDD ガードレールが有効化される。

## 段階的導入（推奨順）

1. **Phase 2-A**: `spec-check.yml` + `markdown-lint.yml`（型チェックのみ、無料）
2. **Phase 2-B**: `claude-review.yml` 追加（API 課金発生）
3. **Phase 2-C**: Dangerfile 追加（spec 未更新警告） — TODO
4. **Phase 2-D**: branch protection + merge queue
5. **Phase 2-E**: release-please による SemVer 自動化

## 設計

詳細は [`akari-os/docs/sdd/guardrails.md`](https://github.com/Akari-OS) を参照（非公開ハブ）。

## バージョニング

- `@main` で参照すると常に最新
- 安定版が必要な場合は `@v1` のような tag を切ること（将来）
