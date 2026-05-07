---
name: feature
description: 実装を、plan -> verifier -> 人間チェック -> 実装 -> verifier のゲート付きワークフローで進める。feature 実装、機能追加、/feature の依頼で使う。
disable-model-invocation: true
---

# feature

機能追加をゲート付きで進める。`plan` スキルでPlanを作り、`verifier` と人間チェックが PASS するまで計画を直してから実装する。実装後も `verifier` が PASS するまで修正を繰り返す。

## ワークフロー

1. **plan**
   - `skills/plan/SKILL.md` を読み、Planを作る。
   - コード変更はまだ行わない。
2. **verifier（計画検証）**
   - `agents/verifier.md` の出力契約に従って、Planを検証する。
   - 判定が `PASS` 以外なら、指摘を反映して `plan` に戻る。
3. **人間チェック**
   - verifier が PASS したPlanをユーザーに提示し、明示承認を得る。
   - ユーザーが修正を求めた場合は `plan` に戻る。
4. **実装**
   - 承認済み Planの範囲だけ実装する。
   - 変更後、Verification Plan に書いた lint / format / typecheck / test を実行する。
5. **verifier（実装検証）**
   - 実装差分、実行した検証結果、承認済みPlanを渡して検証する。
   - 判定が `PASS` 以外なら、指摘を反映して実装修正に戻る。

## ループ条件

- 計画検証の `verifier` は `PASS` するまで `plan -> verifier` を繰り返す。
- 人間チェックはユーザーが明示承認するまで `plan -> verifier -> 人間チェック` を繰り返す。
- 実装検証の `verifier` は `PASS` するまで `実装修正 -> verifier` を繰り返す。
- 同じ指摘で 10 回以上進展しない場合は停止し、ブロッカーをユーザーに報告する。

## verifier への渡し方

`agents/verifier.md` を読み、次を含めて依頼する。

- 検証対象: Planまたは実装差分。
- 検証観点: 受け入れ条件、ルール、既存コードとの整合性、検証結果。
- 必須判定: `PASS` / `FAIL` / `UNCLEAR` のいずれか。

`UNCLEAR` は PASS 扱いしない。不足証拠を解消してから次へ進む。

## 実装ルール

- 承認済み Sprint Contract にない追加機能は実装しない。
- 既存コードのパターンを優先する。
- 変更後は実行した検証コマンドと結果を最終報告に含める。
- verifier が未完了、または PASS していない状態で完了報告しない。
