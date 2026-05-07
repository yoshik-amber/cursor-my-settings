---
name: plan
description: feature スキル内部で使う実装前プラン作成。要件整理、影響範囲、システム設計、実装ステップ、受け入れ条件、検証方法をPlanとしてまとめる。
disable-model-invocation: true
---

# plan

要件整理とシステム設計を担うスキル。コード変更は行わず、実装前に合意できるPlanを作る。
Planモードが使える環境であればPlanモードに移行して実行すること。

## 手順

1. ユーザー要件についてわからないことはできる限り質問する。
2. 既存コードをresercherエージェントで調査し、影響範囲と既存パターンを確認する。
3. 次の形式で Plan を作成する。
4. Contract はユーザー承認前の草案として扱い、コード変更に進まない。

## Plan 形式

```markdown
## Goals
- <達成すること>

## Background
- <対応の背景・意図など>

## Out of Scopes
- <今回やらないこと>

## Design
- <採用した設計についてわかりやすく簡単に説明する>

## Scope
- <変更対象ファイル・領域>

## Implementation Plan
- <実装ステップ>

## Acceptance Criteria
- <満たすべき条件>

## Verification Plan
- <lint / format / typecheck / test / 手動確認など>

## Risks / Open Questions
- <リスクまたは未確定事項。なければ「なし」>
```

## ルール

- 実装コードを書かない。
- 推測は推測として明記する。
- Acceptance Criteria は verifier が PASS / FAIL 判定できる粒度にする。
- Verification Plan には、このリポジトリで実行可能な確認を優先して書く。
