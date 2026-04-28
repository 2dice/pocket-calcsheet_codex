# AGENTS.md

このファイルは、このリポジトリ内のコードを操作するガイダンスを提供する。

## コメント(Issue / Pull Request / Response / Code comment)

- 全てのコメントは日本語であること。
- 簡潔で過不足なく、わかりやすい説明をすること。
- Issue / Pull Request コメントには箇条書きを効果的に使い、視覚的に見やすいフォーマットとすること。

## あなたの属性

- あなたは以下のことに重点を置くプログラミングアシスタントのエキスパートである。
  - Vite, TypeScript,React, Node.js, Shadcn UI, Tailwind CSS, Playwright, Vitest
  - ベストプラクティスをもとに実装する
  - テストはt_wadaのポリシーに従う
  - 明確で読みやすく、保守しやすいコード要件を慎重かつ正確に遵守する

## プロジェクト概要

### ディレクトリ構造

- 最終的なディレクトリ構造は `@docs/design_directory_data.md`を参照

#### 現状のディレクトリ構造

```
pocket-calcsheet_codex/
├── docs/                             # 開発用ドキュメント
├── .github/
│   └── workflows/
│       └── manual_deploy.yml         # 各ブランチでのテストデプロイ用
└── AGENTS.md                         # AIコーディングエージェント用
```

## Project Overview

ぽけっと計算表 (Pocket Calcsheet) - PWA対応のスマートフォン専用計算シートアプリ

- 計算式を保存できる計算シートアプリ
- 保存した名前付き変数を参照し、関数を組み合わせて結果を算出
- 計算結果を自然な数式(LaTeX形式)で表示

## Technology Stack

### Core

- Vite + React + TypeScript + SWC
- Tailwind CSS (v4) + shadcn/ui
- PWA対応 (vite-plugin-pwa)

### State Management & Storage

- Zustand (+ middleware: persist, immer)
- localStorage for data persistence

### Specialized Libraries

- math.js: 数式処理・計算エンジン
- KaTeX: LaTeX数式レンダリング
- dnd-kit: ドラッグ&ドロップ (リスト並び替え)
- React Router + HashRouter: ルーティング

### Testing & Quality

- Vitest + React Testing Library (jest-dom): ユニットテスト
- Playwright: E2Eテスト (モバイルブラウザ特化)
- ESLint + Prettier: コード品質管理

## Architecture Overview

### Data Flow

- **ルートモデル**: アプリ全体データを1つのオブジェクトで管理
- **Zustand Store**: 永続化スライス (localStorage) + UI一時状態スライス (非永続)
- **計算エンジン**: math.js ベースのカスタム実装で循環参照対応

### Key Components Structure

- **pages/**: 各画面のルートコンポーネント (Top, Overview, Variables, Formula タブ)
- **components/sheets/**: シート一覧・編集機能
- **components/keyboard/**: カスタムキーボード実装 (ネイティブキーボード無効)
- **components/calculator/**: 変数スロット・数式入力・結果表示
- **utils/calculation/**: 数式パース・LaTeX変換・数値フォーマット

### Mobile-First Design

- カスタムキーボード: 数値・演算子・関数・変数選択
- SafeArea対応: iPhone X系のnotch考慮
- タッチ操作最適化: 長押しドラッグ、競合回避

### Data Persistence Strategy

- **保存キー**: `pocket-calcsheet/〈スキーマ世代〉`
- **スキーママイグレーション**: schemaVersion による自動変換
- **プリセットデータ**: 初回起動時のみ自動ロード

### Specialized Calculation Features

- **変数参照**: `[var1]` 形式での相互参照
- **循環参照対応**: 2回再計算で打ち切り
- **SI接頭語表示**: 10の3の倍数乗での数値表示
- **LaTeX変換**: 関数名変換 (atan → tan^{-1}) とカスタムTeX生成

## Development Guidelines

### Testing Philosophy

- TDD: t_wadaのポリシーに従う
- モバイルブラウザ特化: iPhone Safari + Android Chrome
- コンソールエラー検知: vitest-fail-on-console + Playwright Console監視

### Code Organization

- **型定義**: types/ に集約 (sheet.ts, calculation.ts など)
- **フック分離**: 機能別カスタムフック (useCalculation.ts, useCustomKeyboard.ts など)
- **ユーティリティ分離**: 計算・バリデーション・ストレージを utils/ で分離

### PWA Requirements(vite-plugin-pwa/pwa-assets-generator)

- オフライン動作: Service Worker + Cache First
- ホーム画面追加: vite.config.ts(manifest) + index.html tag
- アイコン: 複数サイズ対応

## Important Notes

### Deploy Target

- GitHub Pages (ベースパス: '/pocket-calcsheet_cca/')
- 手動デプロイ + PR時自動デプロイ

### Performance Considerations

- dnd-kit最適化: シート一覧メタ情報の分離保持
- 計算最適化: 依存関係なし・上から順に2回再計算方式
- localStorage最適化: 編集完了時のみ保存

### Mobile Browser Compatibility

- PointerSensor設定: delay={300}, touchAction="none" (長押し競合回避)
- キーボード制御: ネイティブキーボード完全無効化
- スクロール制御: 入力時のキーボード上部へのスクロール

### 重要な注意事項

- 全ての工程で全力を尽くし最善の結果を残すこと。遠慮は要らない。
- 結果だけでなく、作業行程も、claudeとgeminiがレビューする
- サブエージェントを最大展開し、メインエージェントはオーケストレーションに徹する。
