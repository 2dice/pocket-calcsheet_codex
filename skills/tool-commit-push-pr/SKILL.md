---
name: tool-commit-push-pr
description: Git のコミット、push、Pull Request 作成時のメッセージ規約を適用するスキル。コミット作業が発生するタスクでは自動で使用し、コミットメッセージ形式とPRテンプレートを強制する。
---

# tool-commit-push-pr

コミット操作を行うタスクでは必ずこのスキルを適用する。

## コミットメッセージ規約

1. 次の形式を必ず使用する。
   - `<step>/<type>: <short summary>`

2. `<type>` は次のいずれかのみ使用する。
   - `feat`, `fix`, `docs`, `test`, `chore`, `perf`, `refactor`, `build`, `ci`, `style`, `TYP`

3. `<short summary>` の規約を守る。
   - 日本語で記述する。
   - 80文字以内にする。

4. 例を基準にする。
   - `step1-1/feat: テンプレートを使用したプロジェクトの作成(vite+react+ts+swc)`

## Pull Request メッセージ規約

1. PR本文は必ず日本語で記述する。
2. PR本文は次の見出しテンプレートを必ず使う。

```md
### 目的 / 関連ステップ
### 実装内容
### 動作確認内容
### 備考
```

3. PRタイトルはステップ番号と内容を日本語で記述する。
4. Issue対応PRでは、`### 備考` に `Close #<番号>` を必ず含める。

## 実行フロー

1. 変更内容から step と type を選定する。
2. 80文字以内の日本語 summary を作成する。
3. 規約形式で `git commit -m` を実行する。
4. PRタイトルを日本語で作成する。
5. PR本文をテンプレートどおりに作成する。
6. Issue番号がある場合、備考に `Close #<番号>` を追加する。
