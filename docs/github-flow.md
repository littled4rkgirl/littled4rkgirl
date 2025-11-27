# GitHub Flow ブランチ戦略詳細

## 概要

このリポジトリでは、シンプルで迅速な開発を実現する「GitHub Flow」ブランチ戦略を採用しています。

スペシャルリポジトリ（GitHubプロフィール用リポジトリ）として、複雑なブランチ管理やリリースサイクルは不要なため、Git Flowから移行しました。

## GitHub Flowの特徴

- **シンプル**: masterブランチとfeatureブランチのみ
- **迅速**: 変更をすぐにmasterにマージ可能
- **継続的デプロイ**: masterは常にデプロイ可能な状態
- **レビュー重視**: Pull Requestによるコードレビュー

## ブランチの役割

### master
- **役割**: 常にデプロイ可能な状態を維持するメインブランチ
- **保護**: 直接コミット禁止、PRを通じてのみマージ
- **状態**: 常に安定した状態を保つ

### feature/*
- **役割**: 新機能開発や修正のための一時的なブランチ
- **命名規則**: `feature/機能名` (例: `feature/update-readme`)
- **ライフサイクル**: masterから分岐 → 開発 → PRでマージ → 削除
- **原則**: 短命であること（数時間〜数日）

## 運用フロー

### 1. 新機能開発・修正フロー

```bash
# 1. 最新のmasterを取得
git checkout master
git pull origin master

# 2. featureブランチを作成
git checkout -b feature/機能名

# 3. 開発作業を実施
# ファイルを編集...

# 4. 変更をコミット
git add .
git commit -m "feat: 機能の説明"

# 5. リモートにプッシュ
git push -u origin feature/機能名

# 6. GitHub上でPull Requestを作成
gh pr create --base master --head feature/機能名 \
  --title "タイトル" \
  --body "説明"

# 7. レビュー後、PRをマージ（GitHub上で実施）

# 8. ローカルのmasterを更新
git checkout master
git pull origin master

# 9. featureブランチを削除
git branch -d feature/機能名
git push origin --delete feature/機能名
```

### 2. 緊急修正フロー

緊急修正も通常の機能開発と同じフローです。GitHub Flowでは全ての変更が同じプロセスを経由します。

```bash
# masterから修正ブランチを作成
git checkout master
git pull origin master
git checkout -b feature/fix-urgent-bug

# 修正作業を実施
git add .
git commit -m "fix: 緊急バグ修正"

# PRを作成してマージ
git push -u origin feature/fix-urgent-bug
gh pr create --base master --head feature/fix-urgent-bug
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

## Pull Request運用ルール

1. **タイトル**: 変更内容を簡潔に記述
2. **説明**: 変更の背景、内容、影響範囲を記載
3. **レビュアー**: 必ず指定する
4. **レビュー**: 承認後にマージ
5. **CI/CD**: 全てのチェックがパスしてからマージ

## 注意事項

1. **masterへの直接コミット禁止** - 必ずPRを経由
2. **featureブランチは短命に** - 長期間のブランチは避ける
3. **こまめなコミット** - 小さな変更単位でコミット
4. **定期的なmaster同期** - 長期作業中も定期的にmasterの変更を取り込む

## Git FlowからGitHub Flowへの移行理由

- **スペシャルリポジトリの性質**: プロフィール表示用で、複雑なリリース管理不要
- **迅速な更新**: 変更をすぐに反映できる
- **シンプルさ**: developブランチやreleaseブランチの管理が不要
- **個人開発に最適**: 1人での開発に適したシンプルなフロー

## 参考資料

- [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow)
- [Understanding the GitHub flow](https://guides.github.com/introduction/flow/)
