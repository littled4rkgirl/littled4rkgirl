# Development Agents & Workflow

このドキュメントは、このリポジトリでの開発フローと運用ルールを記録します。

## ブランチ戦略

このリポジトリは **Git Flow** ブランチ戦略を採用しています。

### ブランチ構成

- **master**: 本番環境用ブランチ（リリース済みコード）
- **develop**: 開発統合ブランチ（次回リリース予定の機能を統合）
- **feature/\***: 機能開発用ブランチ
- **release/\***: リリース準備用ブランチ
- **hotfix/\***: 緊急修正用ブランチ

### ワークフロー

#### 新機能開発
```bash
# developブランチから機能ブランチを作成
git checkout develop
git checkout -b feature/機能名

# 開発完了後、developにマージ
git checkout develop
git merge feature/機能名
```

#### リリース準備
```bash
# developブランチからリリースブランチを作成
git checkout develop
git checkout -b release/x.x.x

# リリース準備完了後、masterとdevelopにマージ
git checkout master
git merge release/x.x.x
git checkout develop
git merge release/x.x.x
```

#### 緊急修正（Hotfix）
```bash
# masterブランチから修正ブランチを作成
git checkout master
git checkout -b hotfix/修正内容

# 修正完了後、masterとdevelopにマージ
git checkout master
git merge hotfix/修正内容
git checkout develop
git merge hotfix/修正内容
```

## 履歴

- 2025-11-27: Git Flowブランチ戦略を採用、developブランチを作成
