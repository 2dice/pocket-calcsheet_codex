---
name: 1-1-react-init
description: Vite + React + TypeScript + SWC のWebアプリひな形を初期構築し、型チェックとビルドを通すためのスキル。ユーザーがスラッシュコマンド "/1-1_react-init" を明示したときだけ使用する。
---

# 1-1 React Init

`/1-1_react-init` が明示された場合のみ実行する。通常の React/Vite 作業では自動適用しない。

## 実行手順

1. 事前確認を行う。
   - `git status --short` で作業ツリーを確認する。
   - 既存ファイルがある場合は、上書きによる削除リスクを先に説明する。

2. 呼び出し時引数の有無を確認する。
   - ユーザー入力に **共通識別子（ベースパスと package.json の name に使う文字列）** があるか確認する。
   - 欠けている場合、次の質問を必ず行う。  
     `ベースパスとpackage.jsonのnameに使用する文字列を指定してください(ex.pocket-calcsheet_codex)`

3. 値を決定する。
   - 受け取った共通識別子を `project_key` として扱う。
   - `project_key` が `pocket-calcsheet_codex` の場合、次を採用する。
     - base path: `/pocket-calcsheet_codex/`
     - package name: `pocket-calcsheet_codex`
   - 変換ルール:
     - base path は `/<project_key>/`
     - package name は `<project_key>`
   - 指定がある場合はユーザー値を優先する。

4. プロジェクトを初期化する。
   - `npx create-vite@latest . --template react-ts --overwrite --no-interactive` を実行する。
   - 生成後、`@vitejs/plugin-react` を `@vitejs/plugin-react-swc` に置換する。

5. 設定を反映する。
   - `vite.config.ts` に以下を設定する。
     - `import react from '@vitejs/plugin-react-swc'`
     - `base: '<決定したベースパス>'`
   - `package.json` を以下に合わせる。
     - `name: <project_key>`
     - scripts に `check: "tsc --noEmit"` を追加

6. 検証する。
   - `npm install`
   - `npm run check`
   - `npm run build`
   - `timeout 15s npm run dev -- --host 127.0.0.1 --port 4173`

7. 仕上げを行う。
   - `git status --short` で不要な削除・差分を確認する。
   - 実行結果を「変更点」「確認コマンド」「注意点」に分けて簡潔に報告する。

## 完了条件

- `npm run check` が成功する。
- `npm run build` が warning / error なしで成功する。
- `npm run dev` 起動ログでアクセスURLを確認できる。
