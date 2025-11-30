---
name: dependency-analyzer
description: フロントエンドプロジェクトの依存関係を分析し、Node.js、React、Next.js等のバージョン互換性とEOL状況を調査する。依存関係の確認、互換性チェック、EOL調査時に使用
allowed-tools: Read, Grep, Glob, WebFetch, WebSearch, Bash
---

# 依存関係分析スキル

フロントエンドプロジェクトの依存関係を分析し、バージョン互換性とEOL（End of Life）状況を調査します。

## Instructions

### 1. プロジェクト情報の収集

以下のファイルから依存関係情報を収集：

```bash
# 必須ファイル
package.json          # 依存関係の定義
package-lock.json     # 正確なバージョン情報
# または
yarn.lock
pnpm-lock.yaml

# オプション
.nvmrc               # Node.jsバージョン指定
.node-version        # Node.jsバージョン指定
.tool-versions       # asdf用バージョン指定
engines              # package.json内のエンジン指定
```

### 2. ランタイム・フレームワークのEOL確認

#### Node.js
- EOL情報: https://nodejs.org/en/about/previous-releases
- LTSスケジュール: https://github.com/nodejs/release#release-schedule

| バージョン | ステータス | 備考 |
|-----------|----------|------|
| 22.x | Current | 2024年10月からLTS |
| 20.x | Active LTS | 2026年4月までメンテナンス |
| 18.x | Maintenance LTS | 2025年4月まで |
| 16.x以下 | EOL | サポート終了 |

#### React
- サポートポリシー: https://react.dev/community/versioning-policy
- 現在のサポート状況：
  - React 19: 最新
  - React 18: サポート中
  - React 17以下: セキュリティ修正のみ

#### Next.js
- リリースサイクル: https://nextjs.org/docs/pages/building-your-application/upgrading
- 一般的に最新2メジャーバージョンがアクティブサポート

### 3. 依存関係の互換性マトリクス

主要な互換性要件を確認：

```
Node.js ← React ← Next.js
          ↓
        TypeScript
```

#### 互換性チェックポイント

| 組み合わせ | 確認事項 |
|-----------|---------|
| Node.js + Next.js | Next.js の engines 要件を満たすか |
| React + Next.js | Next.js が要求する React バージョンか |
| TypeScript + React | @types/react のバージョン互換性 |
| ESLint + TypeScript | parser バージョンの互換性 |

### 4. 分析レポートの作成

```markdown
## 依存関係分析レポート

### 環境情報
- Node.js: v{version} ({status})
- npm/yarn/pnpm: v{version}

### コアライブラリ
| ライブラリ | 現在 | 最新 | EOL状況 | 対応優先度 |
|-----------|------|------|---------|-----------|
| react | 18.2.0 | 19.0.0 | サポート中 | 中 |
| next | 14.0.0 | 15.0.0 | サポート中 | 中 |

### EOL警告 🔴
- Node.js 16.x は 2023年9月にEOL
  - 推奨: Node.js 20.x LTSへアップグレード

### 互換性の問題 🟠
- 検出された互換性の問題

### 推奨アクション
1. 優先度高: ...
2. 優先度中: ...
3. 優先度低: ...
```

### 5. EOL情報の参照先

#### 公式リソース
- Node.js: https://endoflife.date/nodejs
- React: https://endoflife.date/react
- Vue.js: https://endoflife.date/vue
- Angular: https://endoflife.date/angular
- TypeScript: https://endoflife.date/typescript

#### 統合EOLデータベース
- https://endoflife.date/ - 複数技術のEOL情報を統合

## Bashコマンド

依存関係の確認に使用できるコマンド：

```bash
# 現在のNode.jsバージョン
node --version

# npmパッケージの最新バージョン確認
npm view {package} version

# 依存関係のアウトデート確認
npm outdated

# セキュリティ脆弱性チェック
npm audit

# 依存関係ツリーの表示
npm ls --depth=0
```

## Examples

### 例1: プロジェクト全体の依存関係分析
```
ユーザー: このプロジェクトの依存関係とEOL状況を確認して
→ package.json と lockファイルを読み込み、全体的な分析レポートを作成
```

### 例2: 特定ランタイムの互換性確認
```
ユーザー: Node.js 20にアップグレードできるか確認して
→ 依存関係のNode.js要件を確認し、互換性をレポート
```

### 例3: アップグレードパスの提案
```
ユーザー: React 19にアップグレードするために必要な変更は？
→ 現在の依存関係を分析し、必要なアップグレードパスを提案
```
