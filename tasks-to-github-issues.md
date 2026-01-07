---
description: "Task Master の tasks.json を GitHub Issues として作成する（MCPサーバー経由、日本語翻訳）"
argument-hint: "[tasks.jsonのパス] [出力ファイル名]"
allowed-tools: ["read_file", "write_to_file", "execute_command"]
---

# /tasks-to-issues

Task Master の tasks.json を読み込み、MCPサーバー経由でGitHub Issueを作成します。

## 手順

1. `.taskmaster/tasks/tasks.json` ファイルを読み込む
2. 各タスクの内容を日本語に翻訳する
3. 以下のフォーマットに従って生成する

## 翻訳ルール

- title, description, details, testStrategy を日本語に翻訳
- priority: high→高, medium→中, low→低
- status: pending→未着手, in-progress→進行中, done→完了, deferred→延期

## Issue 作成の順序（重要）

1. **親タスク（メインタスク）を先に作成**
2. **サブタスクを個別 Issue として作成**
3. **親 Issue の本文を更新してサブタスク Issue へのリンクを追加**

## 親タスク Issue フォーマット

**タイトル**: `[Task {id}] {日本語タイトル}`

**本文**:
```markdown
## 📋 概要
{description の日本語訳}

## 📝 詳細
{details の日本語訳}

## ✅ テスト戦略
{testStrategy の日本語訳}

## 🔗 依存タスク
- #XX (Task {dep_id})

## 📊 メタ情報
- **優先度**: {priority の日本語}
- **ステータス**: {status の日本語}
- **Task Master ID**: {id}

## 📌 サブタスク
<!-- サブタスク Issue 作成後に自動更新 -->
- #XX サブタスク1
- #XX サブタスク2
```

**ラベル**: `task-master`, `priority:{high|medium|low}`, `type:parent`

## サブタスク Issue フォーマット

**タイトル**: `[Task {parent_id}.{subtask_id}] {日本語タイトル}`

**本文**:
```markdown
## 📋 概要
{description の日本語訳}

## 🔗 親タスク
#XX (Task {parent_id}: {parent_title})

## 📝 詳細
{details の日本語訳}

## ✅ テスト戦略
{testStrategy の日本語訳}

## 📊 メタ情報
- **優先度**: {priority の日本語}
- **ステータス**: {status の日本語}
- **Task Master ID**: {parent_id}.{subtask_id}
```

**ラベル**: `task-master`, `priority:{high|medium|low}`, `type:subtask`
