# Step1-1 Issue ブラッシュアップメモ

Issue #3 の実装時に発生した問題と、実際に行った対処を一覧化する。

## 問題と対処一覧

| # | 発生した問題 | 対処内容 | 次回 Issue に追記する指示案 |
|---|---|---|---|
| 1 | 既存ファイルがある状態で `create-vite` を実行すると初回コマンドが中断された。 | `--overwrite --no-interactive` を指定して再実行し、生成後に必要な既存ファイルを保持する運用へ切り替えた。 | `create-vite` 実行時の推奨コマンドを明記する（例: `npx create-vite@latest . --template react-ts --overwrite --no-interactive`）。 |
| 2 | `--overwrite` により `docs/` や `AGENTS.md` など既存管理ファイルが消えるリスクが発生した。 | Git 管理下の削除差分を確認し、必要ファイルを `git checkout -- <path>` で復元した。 | 「`--overwrite` 実行後に必ず `git status` で不要削除を確認する」チェック項目を追加する。 |
| 3 | `@vitejs/plugin-react-swc` のバージョンを `@vitejs/plugin-react` と同じにするとインストールエラーになった。 | `npm view @vitejs/plugin-react-swc version` で公開バージョンを確認し、利用可能な `^4.3.0` に修正した。 | 「依存追加時は `npm view <pkg> version` で実在バージョンを確認する」を明記する。 |
| 4 | Vite v8 + `plugin-react-swc` 組み合わせで build 時に非推奨警告が出た。 | Vite を v7 系へ調整し、`npm run build` で警告なしを確認した。 | 「完了条件: build ログに warning が出ないこと。出た場合は依存バージョン整合性を再確認」を追記する。 |
| 5 | `npm run dev` は常駐プロセスのため CI/自動実行環境では終了しない。 | `timeout 15s npm run dev -- --host 127.0.0.1 --port 4173` で起動確認のみ行った。 | 「dev サーバー確認は `timeout` 付きで実施し、起動ログ確認を完了条件とする」と明記する。 |

## 推奨: Issue テンプレの最小追記

- **コマンド固定化**
  - 初期化コマンド（非対話 + 上書き）の明記
  - 検証コマンド（check / build / dev起動確認）の明記
- **削除防止チェック**
  - `create-vite` 後に `git status` で不要削除がないか確認
- **依存確認ルール**
  - バージョンは npm レジストリの実在値を確認してから確定
- **警告ゼロ基準**
  - エラーだけでなく warning も解消対象にする
