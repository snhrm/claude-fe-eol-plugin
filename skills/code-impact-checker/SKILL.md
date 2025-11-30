---
name: code-impact-checker
description: ライブラリアップデートによる既存コードへの影響を分析し、修正が必要な箇所を特定する。テストコードのカバレッジを確認し、不足しているテストケースをレポートする。破壊的変更の影響調査、マイグレーション時のコード修正箇所特定に使用
allowed-tools: Read, Grep, Glob, WebFetch, WebSearch
---

# コード影響チェッカースキル

ライブラリのアップデートによる既存コードへの影響を分析し、修正が必要な箇所とテストの不足を特定します。

## Instructions

### 1. 破壊的変更の影響箇所を特定

#### 1.1 変更対象APIの使用箇所を検索

```bash
# 例: 非推奨になったAPIの使用箇所を検索
grep -r "deprecatedFunction" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" src/
```

検索対象パターン：
- 削除・変更されたAPI名
- 非推奨化された関数・コンポーネント
- 変更されたimportパス
- 削除されたprops/オプション

#### 1.2 影響度の分類

| 影響度 | 説明 | 例 |
|-------|------|-----|
| 🔴 必須修正 | コンパイルエラー、ランタイムエラー | API削除、型変更 |
| 🟠 推奨修正 | 非推奨警告、将来のエラー | deprecation warning |
| 🟡 確認必要 | 動作変更の可能性 | デフォルト値変更 |

### 2. 修正箇所のレポート作成

```markdown
## 修正が必要なコード一覧

### 🔴 必須修正 (X件)

#### ファイル: src/components/Example.tsx
- **行番号**: 15, 42, 78
- **影響を受けるAPI**: `oldFunction()` → `newFunction()`
- **修正方法**:
  ```diff
  - import { oldFunction } from 'library';
  + import { newFunction } from 'library';

  - const result = oldFunction(arg);
  + const result = newFunction(arg, { newOption: true });
  ```

### 🟠 推奨修正 (Y件)
...
```

### 3. テストコードの分析

#### 3.1 テストファイルの特定

テストファイルのパターン：
```
**/*.test.ts
**/*.test.tsx
**/*.spec.ts
**/*.spec.tsx
**/__tests__/**/*
```

#### 3.2 テストカバレッジの確認

影響を受けるコードに対応するテストを確認：

| ソースファイル | テストファイル | 対応テスト有無 |
|--------------|--------------|--------------|
| src/hooks/useData.ts | src/hooks/__tests__/useData.test.ts | ✅ |
| src/utils/format.ts | (なし) | ❌ |

#### 3.3 テストケースの網羅性チェック

既存テストが以下のパターンをカバーしているか確認：

```markdown
## テストカバレッジ分析

### src/hooks/useData.ts

#### カバーされているパターン ✅
- [ ] 正常系: データ取得成功
- [ ] 正常系: 空データ

#### 不足しているパターン ❌
- [ ] エラー系: API呼び出し失敗
- [ ] エッジケース: null/undefined入力
- [ ] 境界値: 大量データ

#### 新APIに必要なテスト 🆕
- [ ] 新オプション `newOption: true` の動作確認
- [ ] 新しいエラーハンドリングの確認
```

### 4. 最終レポート

```markdown
# ライブラリアップデート影響レポート

## サマリー
- 📁 影響ファイル数: X件
- 🔴 必須修正: X件
- 🟠 推奨修正: X件
- 🧪 テスト不足: X件

## 修正が必要なコード

### 1. src/components/Example.tsx:15
**変更内容**: `useOldHook` → `useNewHook`
**テスト状況**: ✅ テストあり（要更新）

### 2. src/utils/helper.ts:42
**変更内容**: 関数シグネチャ変更
**テスト状況**: ❌ テストなし

## テスト追加が必要な箇所

### 高優先度
| ファイル | 不足テスト | 理由 |
|---------|----------|------|
| src/utils/helper.ts | 基本テスト全般 | テストファイルが存在しない |
| src/hooks/useData.ts | エラーハンドリング | 新APIのエラー型に対応必要 |

### 中優先度
| ファイル | 不足テスト | 理由 |
|---------|----------|------|
| src/components/Form.tsx | 新propsのテスト | オプション追加による |

## 推奨アクション

1. **即時対応**: 🔴必須修正のコードを更新
2. **テスト追加**: テストなしファイルにテスト作成
3. **テスト更新**: 既存テストを新APIに対応
4. **動作確認**: 🟡確認必要の箇所を手動テスト
```

## 検索パターン例

### React関連
```
# Hooks使用箇所
useEffect|useState|useCallback|useMemo|useRef

# コンポーネントprops
<ComponentName\s+[^>]*deprecatedProp

# Context使用
useContext\(.*Context\)
```

### Next.js関連
```
# ルーティング
import.*from ['"]next/router['"]
useRouter\(\)

# Image/Link
import.*from ['"]next/image['"]
import.*from ['"]next/link['"]

# getServerSideProps等
export\s+(async\s+)?function\s+getServerSideProps
export\s+(async\s+)?function\s+getStaticProps
```

### 一般的なパターン
```
# 特定のimport
import\s*{[^}]*targetFunction[^}]*}\s*from

# 関数呼び出し
targetFunction\s*\(

# メソッドチェーン
\.deprecatedMethod\(
```

## Examples

### 例1: Next.js 14→15のアップグレード影響調査
```
ユーザー: Next.js 15にアップグレードする際の影響を調べて

→ 1. 破壊的変更リストを取得（library-release-checkerと連携）
→ 2. 各変更に対してgrepで使用箇所を検索
→ 3. テストファイルの存在・カバレッジを確認
→ 4. 修正箇所とテスト追加箇所をレポート
```

### 例2: 特定APIの影響調査
```
ユーザー: useRouterからuseNavigationへの移行で影響を受ける箇所を調べて

→ 1. useRouterの使用箇所を全検索
→ 2. 各使用パターンを分類
→ 3. 対応するテストの有無を確認
→ 4. 移行手順とテスト追加案をレポート
```

### 例3: テストカバレッジ分析
```
ユーザー: src/hooks/のテストカバレッジを確認して

→ 1. src/hooks/内の全ファイルをリスト
→ 2. 対応するテストファイルを検索
→ 3. 各hooksの使用パターンとテストケースを比較
→ 4. 不足テストをレポート
```
