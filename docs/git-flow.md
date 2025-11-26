# Git Flow ブランチ戦略詳細

## 概要

このリポジトリでは、Vincent Driessen氏が提唱した「Git Flow」ブランチ戦略を採用しています。

## ブランチの役割

### 永続ブランチ

#### master
- 本番環境にデプロイ可能な状態を常に維持
- リリース済みのコードのみを含む
- タグ付けによるバージョン管理

#### develop
- 次回リリース向けの開発統合ブランチ
- 新機能はこのブランチに統合される
- 常にビルド可能な状態を維持

### 一時的ブランチ

#### feature/*
- **目的**: 新機能の開発
- **派生元**: develop
- **マージ先**: develop
- **命名規則**: `feature/機能名` (例: `feature/user-authentication`)
- **削除**: developへのマージ後

#### release/*
- **目的**: リリース準備（バージョン番号更新、最終テストなど）
- **派生元**: develop
- **マージ先**: master と develop
- **命名規則**: `release/バージョン` (例: `release/1.0.0`)
- **削除**: masterとdevelopへのマージ後

#### hotfix/*
- **目的**: 本番環境の緊急バグ修正
- **派生元**: master
- **マージ先**: master と develop
- **命名規則**: `hotfix/修正内容` (例: `hotfix/login-bug`)
- **削除**: masterとdevelopへのマージ後

## 運用フロー

### 1. 新機能開発フロー

```bash
# 1. 最新のdevelopを取得
git checkout develop
git pull origin develop

# 2. featureブランチを作成
git checkout -b feature/機能名

# 3. 開発作業を実施
git add .
git commit -m "feat: 機能の説明"

# 4. developに戻ってマージ
git checkout develop
git merge --no-ff feature/機能名

# 5. リモートにプッシュ
git push origin develop

# 6. featureブランチを削除
git branch -d feature/機能名
```

### 2. リリースフロー

```bash
# 1. developからreleaseブランチを作成
git checkout develop
git checkout -b release/1.0.0

# 2. リリース準備（バージョン更新など）
# package.json, version files などを更新
git commit -m "chore: bump version to 1.0.0"

# 3. masterにマージ＆タグ付け
git checkout master
git merge --no-ff release/1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"

# 4. developにもマージ（リリース準備の変更を反映）
git checkout develop
git merge --no-ff release/1.0.0

# 5. リモートにプッシュ
git push origin master
git push origin develop
git push origin v1.0.0

# 6. releaseブランチを削除
git branch -d release/1.0.0
```

### 3. Hotfixフロー

```bash
# 1. masterからhotfixブランチを作成
git checkout master
git checkout -b hotfix/緊急修正内容

# 2. 修正作業
git add .
git commit -m "fix: 緊急バグ修正"

# 3. masterにマージ＆タグ付け
git checkout master
git merge --no-ff hotfix/緊急修正内容
git tag -a v1.0.1 -m "Hotfix version 1.0.1"

# 4. developにもマージ
git checkout develop
git merge --no-ff hotfix/緊急修正内容

# 5. リモートにプッシュ
git push origin master
git push origin develop
git push origin v1.0.1

# 6. hotfixブランチを削除
git branch -d hotfix/緊急修正内容
```

## コミットメッセージ規約

Conventional Commits形式を推奨：

- `feat:` 新機能
- `fix:` バグ修正
- `docs:` ドキュメント変更
- `style:` コードスタイル変更（機能に影響なし）
- `refactor:` リファクタリング
- `test:` テスト追加・修正
- `chore:` ビルドプロセスや補助ツールの変更

## 注意事項

1. **直接のmasterへのコミットは禁止**
2. **developへの直接コミットは最小限に**（小さな修正のみ）
3. **featureブランチは定期的にdevelopとリベース**（コンフリクト防止）
4. **マージは `--no-ff` オプションを使用**（マージ履歴を明確に）
5. **リリース後は必ずタグを付ける**

## 参考資料

- [A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/) - Vincent Driessen
