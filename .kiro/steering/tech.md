# 技術スタック

## アーキテクチャ

- 実行時のサーバー/アプリを持たない「コンテンツ（Markdown）中心」のリポジトリ
- `zenn-cli` を通して、Zenn 仕様のファイル配置・プレビュー・生成を行う
- 品質担保は「プレビュー」と「文章/フォーマットの静的チェック」で行う

## コア技術

- **Runtime**: Node.js（`aqua.yaml` でバージョン固定）
- **Package manager**: `pnpm`（`aqua.yaml` でバージョン固定）
- **Zenn**: `zenn-cli`（記事/本の生成・プレビュー）
- **文章校正**: `textlint`（`preset-japanese` + AI writing preset など）
- **フォーマット**: Prettier（Markdown/JSON 等の整形）

## 開発（執筆）標準

### 文章品質

- 公開前に `textlint` を実行して、表記・文体・文長などのルール違反を検出する
- 自動修正（`--fix`）は便利だが、意味が変わらないかを必ず確認する

### フォーマット

- Prettier で Markdown/設定ファイルを整形し、差分を安定させる

### テスト

- 自動テストは基本的に持たない（`pnpm test` はプレースホルダー）
- 代替として、`zenn preview` と `textlint` を「動作確認・品質確認」の基準にする

## 開発環境

### 必要ツール

- Node.js: `aqua.yaml` の pin を基準（例: `nodejs/node@v24.12.0`）
- pnpm: `aqua.yaml` の pin を基準（例: `pnpm/pnpm@v10.26.0`）

### よく使うコマンド

```bash
pnpm install
pnpm exec zenn preview
pnpm exec zenn new:article
pnpm exec zenn new:book
pnpm exec textlint "articles/**/*.md" "books/**/*.md"
pnpm exec textlint --fix "articles/**/*.md" "books/**/*.md"
pnpm run format
```

## 重要な技術的判断

- **ツールチェーン固定**: `aqua.yaml` による Node/pnpm の pin で、執筆環境の再現性を上げる
- **依存を増やしすぎない**: 執筆体験に直結するツール（Zenn/文章校正/整形）に寄せる
- **Zenn 仕様を優先**: 画像参照（`/images/...`）やフロントマター等、Zenn の流儀に合わせる

---
_依存の羅列ではなく、開発（執筆）判断に効く「標準」と「選択理由」を残します_

updated_at: 2025-12-18
