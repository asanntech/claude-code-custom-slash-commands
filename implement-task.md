---
description: "Task Master のタスク/サブタスクを実行する"
argument-hint: "<task_id> (例: 2, 2.1, 3.2)"
allowed-tools: ["Bash", "Read", "Write", "Edit", "Grep", "Glob", "Task"]
---

# Task Master タスク実行コマンド

## 概要
指定された Task Master のタスクまたはサブタスクの詳細を取得し、その内容に基づいて実装を行います。

## 使用方法
```
/implement-task 2      # タスク 2 を実行
/implement-task 2.1    # サブタスク 2.1 を実行
```

## 処理フロー

### Step 1: タスク詳細を取得

```bash
# タスク/サブタスクの詳細を取得
task-master show <task_id>
```

このコマンドで以下の情報を取得します：
- タイトル
- 説明（description）
- 詳細（details）
- テスト戦略（testStrategy）
- 依存関係（dependencies）
- ステータス（status）
- 優先度（priority）

### Step 2: 依存タスクの確認

依存関係がある場合、依存タスクが完了しているか確認します：

```bash
# 依存タスクのステータスを確認
task-master show <dependency_id>
```

依存タスクが未完了の場合：
- ユーザーに警告を表示
- 先に依存タスクを完了するか確認

### Step 3: ステータスを「進行中」に更新

```bash
task-master set-status --id=<task_id> --status=in-progress
```

### Step 4: 実装を実行

タスクの `details` と `testStrategy` に基づいて実装を行います。

**実装時の注意点：**
- `details` に記載された手順に従う
- 既存のコードベースとの整合性を保つ
- `testStrategy` に基づいてテストも実装する
- 必要に応じてコメントを追加

### Step 5: テストの実行

`testStrategy` に基づいてテストを実行

### Step 6: 実装完了後の処理

テストが成功したら：

```bash
# ステータスを完了に更新
task-master set-status --id=<task_id> --status=done

# 実装メモを追記（オプション）
task-master update-subtask --id=<task_id> --prompt="実装完了: <実装内容の概要>"
```

### Step 7: 次のタスクを提案

```bash
# 次に取り組むべきタスクを表示
task-master next
```

## 出力形式

実装完了後、以下の形式で報告：

```
✅ タスク <task_id> の実装が完了しました

📝 実装内容:
- <実装した内容1>
- <実装した内容2>

🧪 テスト結果:
- <テスト結果の概要>

📌 変更ファイル:
- <変更したファイル1>
- <変更したファイル2>

➡️ 次のタスク:
<task-master next の結果>
```

## エラーハンドリング

### タスクが見つからない場合
```
❌ エラー: タスク <task_id> が見つかりません
利用可能なタスク一覧を確認してください: task-master list
```

### 依存タスクが未完了の場合
```
⚠️ 警告: 依存タスク <dep_id> が未完了です
先に依存タスクを完了することを推奨します。
続行しますか？ (y/n)
```

### テストが失敗した場合
```
❌ テストが失敗しました
失敗したテスト: <テスト名>
エラー内容: <エラーメッセージ>

修正して再実行しますか？
```

## オプション

### --skip-tests
テストをスキップして実装のみ行う
```
/implement-task 2.1 --skip-tests
```

### --dry-run
実際の実装は行わず、実装計画のみ表示
```
/implement-task 2.1 --dry-run
```

## 注意事項

- 複雑なタスクは、サブタスク単位で実行することを推奨
- 実装前に必ず `task-master show <id>` で詳細を確認
- 大きな変更を行う場合は、事前にブランチを作成
- 実装中に問題が発生した場合は `task-master update-subtask` でメモを残す
- 実装後に必ず CLAUDE.md のルールに従っているかを確認

$ARGUMENTS