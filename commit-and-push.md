---
description: "ブランチ作成からプッシュまでの完全なgitワークフローの自動化コマンド"
argument-hint: "[コミットメッセージ]"
allowed-tools: ["Bash", "Read", "LS", "Grep", "Glob", "Task"]
---

# Git Commit and Push Workflow

このコマンドは、ブランチ作成からプッシュまでの完全なgitワークフローを自動化します。

## 実行内容

1. 現在のgit状態を確認
2. 必要に応じて新しいfeatureブランチを作成
3. すべての変更をステージング
4. 変更内容を分析して英語のコミットメッセージを自動生成
5. コミットを実行
6. リモートにプッシュ

## 引数

- `$ARGUMENTS`: オプション。コミットメッセージを明示的に指定する場合に使用

## 実行手順

### ステップ1: 現在の状態を確認

```bash
# 現在のブランチを確認
git branch --show-current

# 変更状況を確認
git status
```

### ステップ2: ブランチの処理

**⚠️ 重要な制限事項**:
以下のブランチには直接コミット・マージしないでください:
- `main`
- `master`
- `release/*`（releaseプレフィックスのブランチ）

これらのブランチへの変更は、必ずPull Request経由で行ってください。

現在のブランチが `main` または `master` の場合:
- 変更内容を分析
- 適切なブランチ名を生成（例: `feature/add-user-auth`, `fix/login-validation`）
- 新しいブランチを作成して切り替え

```bash
# mainブランチにいる場合、新しいブランチを作成
git checkout -b [自動生成されたブランチ名]
```

それ以外のブランチの場合:
- 現在のブランチで作業を続行

### ステップ3: 変更をステージング

```bash
# すべての変更をステージング
git add .

# ステージングされた内容を確認
git diff --cached --stat
```

### ステップ4: コミットメッセージの生成

変更内容を分析して、以下の形式で英語のコミットメッセージを生成:

**フォーマット**: `[種類]: [英語の簡潔な説明]`

**長さの制限**:
- 最大50文字を目安に端的に表現
- 不要な冠詞（a, the）は省略可
- 動詞の原形で始める（add, fix, update など）
- 詳細な説明は避け、何を変更したかのみを記載

**種類の選択基準**:
- `feat`: 新機能の追加
- `fix`: バグ修正
- `docs`: ドキュメントのみの変更
- `style`: コードの意味に影響しない変更（フォーマット、セミコロンなど）
- `refactor`: バグ修正や機能追加を伴わないコードの改善
- `perf`: パフォーマンス改善
- `test`: テストの追加や修正
- `chore`: ビルドプロセスやツールの変更

**メッセージ例**:
- `feat: add user authentication`
- `fix: resolve login validation error`
- `docs: add setup instructions`
- `refactor: clean up user service`
- `style: format code consistently`

### ステップ5: コミット実行

```bash
# $ARGUMENTSが指定されている場合はそれを使用、なければ自動生成
git commit -m "[生成されたメッセージ]"

# コミットが成功したことを確認
git log -1 --oneline
```

### ステップ6: リモートにプッシュ

```bash
# 現在のブランチをリモートにプッシュ
# 新しいブランチの場合は -u オプションを使用
git push -u origin $(git branch --show-current)

# プッシュが成功したことをユーザーに通知
echo "✅ 変更を $(git branch --show-current) ブランチにプッシュしました"

# GitHubリポジトリの場合、PR作成用のURLを表示
```

## 使用例

### 基本的な使用（メッセージ自動生成）
```
/commit-and-push
```

### 明示的なコミットメッセージを指定
```
/commit-and-push feat: add dashboard graphs
```

## エラーハンドリング

- コミットする変更がない場合: エラーメッセージを表示して終了
- リモートへの接続に失敗した場合: エラー内容を表示して対処方法を提案
- コンフリクトが発生した場合: ユーザーに手動解決を促す
