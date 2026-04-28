# Codex Cloud の「バイナリファイル非サポート」調査メモ

## 結論

- 本リポジトリで PR 作成失敗の原因になり得るバイナリは `src/assets/hero.png`。
- テキストベースの `svg` へ置換すれば、同等の見た目を維持しつつ Codex Cloud の制約回避が可能。

## 調査方法

- `git ls-files` の対象を走査し、NULL バイトを含むファイルを検出。
- 検出結果: `src/assets/hero.png` のみ。

## 回避案と影響

| 対応案 | 回避可否 | 影響 / 懸念 |
|---|---|---|
| `hero.png` を削除のみ（参照コードはそのまま） | 不可 | `App.tsx` の import 解決エラーで build が失敗する。 |
| `hero.png` を削除し、`hero.svg`（テキスト）へ置換 | 可 | 画像表現を維持しつつバイナリ排除できる。 |
| バイナリを `public/` に移動のみ | 不可の可能性あり | 依然としてバイナリを含むため、同制約下では根本解決にならない。 |
| Git LFS で管理 | 環境依存 | Codex Cloud 側のサポート状況に依存し、即効性が低い。 |

## 今回の対応

- `src/assets/hero.png` を削除。
- `src/assets/hero.svg` を追加。
- `src/App.tsx` の import を `./assets/hero.svg` に変更。

