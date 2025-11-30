---
name: library-release-checker
description: フロントエンドライブラリのリリース内容をGitHubや公式ドキュメントで確認し、変更点（特に破壊的変更）を調査する。ライブラリのアップデート、マイグレーション、バージョンアップ時に使用
allowed-tools: Read, Grep, Glob, WebFetch, WebSearch
---

# ライブラリリリース確認スキル

フロントエンドライブラリのリリース内容を調査し、破壊的変更や重要な変更点を特定します。

## Instructions

### 1. 対象ライブラリの特定

1. プロジェクトの `package.json` を読み込む
2. 調査対象のライブラリ名と現在のバージョンを確認
3. ユーザーが指定したライブラリ、または主要なフロントエンドライブラリ（React, Next.js, Vue, Angular等）を優先

### 2. リリース情報の収集

以下の情報源から最新リリース情報を取得：

#### GitHub
- リリースページ: `https://github.com/{org}/{repo}/releases`
- CHANGELOG.md: `https://github.com/{org}/{repo}/blob/main/CHANGELOG.md`
- マイグレーションガイド: `https://github.com/{org}/{repo}/blob/main/MIGRATION.md`

#### 公式ドキュメント
- 各ライブラリの公式サイトのブログやリリースノート
- 例:
  - React: https://react.dev/blog
  - Next.js: https://nextjs.org/blog
  - Vue: https://blog.vuejs.org/

### 3. 変更点の分析

以下の観点で変更内容を分類：

| カテゴリ | 説明 | 重要度 |
|---------|------|--------|
| 🔴 Breaking Changes | 破壊的変更、API削除、動作変更 | 最重要 |
| 🟠 Deprecation | 非推奨化された機能 | 重要 |
| 🟡 New Features | 新機能の追加 | 中 |
| 🟢 Bug Fixes | バグ修正 | 低 |
| 🔵 Performance | パフォーマンス改善 | 参考 |

### 4. レポート作成

調査結果を以下のフォーマットで報告：

```markdown
## ライブラリ名 v{現在} → v{最新}

### 破壊的変更 🔴
- 変更内容1
  - 影響範囲
  - 対応方法

### 非推奨化 🟠
- 非推奨となった機能
  - 代替手段

### 新機能 🟡
- 新機能の概要

### マイグレーション手順
1. ステップ1
2. ステップ2
```

## 主要ライブラリのリソース

### React
- GitHub: https://github.com/facebook/react
- 公式ブログ: https://react.dev/blog
- アップグレードガイド: https://react.dev/blog/2024/04/25/react-19-upgrade-guide

### Next.js
- GitHub: https://github.com/vercel/next.js
- 公式ブログ: https://nextjs.org/blog
- Codemods: https://nextjs.org/docs/app/guides/upgrading/codemods

### Vue.js
- GitHub: https://github.com/vuejs/core
- 公式ブログ: https://blog.vuejs.org/
- マイグレーションガイド: https://v3-migration.vuejs.org/

### Angular
- GitHub: https://github.com/angular/angular
- 公式ブログ: https://blog.angular.io/
- アップデートガイド: https://angular.dev/update-guide

### TypeScript
- GitHub: https://github.com/microsoft/TypeScript
- リリースノート: https://www.typescriptlang.org/docs/handbook/release-notes/overview.html

## Examples

### 例1: 特定ライブラリの調査
```
ユーザー: Next.js 14から15への変更点を調べて
→ Next.jsのGitHubリリースと公式ブログから破壊的変更を調査
```

### 例2: package.jsonからの一括調査
```
ユーザー: このプロジェクトの依存関係の最新バージョンと破壊的変更を確認して
→ package.jsonを読み込み、主要ライブラリの変更点を一括調査
```
