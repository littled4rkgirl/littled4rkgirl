# Development Agents & Workflow

このドキュメントは、このリポジトリでの開発フローと運用ルールを記録します。

## ブランチ戦略

このリポジトリは **GitHub Flow** ブランチ戦略を採用しています。

GitHub Flowは、シンプルで迅速な開発フローを実現します。スペシャルリポジトリ（GitHubプロフィール用リポジトリ）として、複雑なブランチ管理は不要なため、この戦略を採用しています。

### ブランチ構成

- **master**: 常にデプロイ可能な状態を維持するメインブランチ
- **feature/\***: 機能開発・修正用の一時的なブランチ

### ワークフロー

#### 新機能開発・修正
```bash
# masterブランチから機能ブランチを作成
git checkout master
git pull origin master
git checkout -b feature/機能名

# 開発作業を実施
git add .
git commit -m "変更内容"

# リモートにプッシュ
git push -u origin feature/機能名

# GitHub上でPRを作成してレビュー
# PRがマージされたら、ローカルブランチを削除
git checkout master
git pull origin master
git branch -d feature/機能名
```

### 原則

1. **masterは常にデプロイ可能な状態**
2. **featureブランチは短命** - 早めにマージする
3. **PRでのレビュー必須** - 直接masterへのコミットは禁止
4. **こまめなコミット** - 小さな変更単位でコミット

## 履歴

- 2025-11-27: Git Flowブランチ戦略を採用、developブランチを作成
- 2025-11-27: GitHub Flowに移行、developブランチを削除
