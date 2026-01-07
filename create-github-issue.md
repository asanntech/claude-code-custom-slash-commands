---
description: "GitHub Issueを作成する（MCPサーバー経由）"
argument-hint: "[file_path='/path/to/issue.md']"
allowed-tools: ["Bash", "Read", "LS", "Grep", "Glob", "Task"]
---

# GitHub Issue作成コマンド

## 概要
指定されたMarkdownファイルの内容を読み込み、MCPサーバー経由でGitHub Issueを作成します。

## 使用方法
```
/create-github-issue file_path="/path/to/issue.md"
```

## Markdownファイルフォーマット
ファイルの先頭にFrontmatter形式でメタデータを記述できます：

```markdown
---
title: Issue Title
labels: ["bug", "enhancement"]
assignees: ["username"]
milestone: 1
---

# Issue内容

ここにIssueの本文を記述します。
```

Frontmatterがない場合は、以下のルールで処理されます：
- title: ファイルの最初の見出し（#）または最初の行
- labels: ファイル名やパスから自動推定
- assignees: 指定なし
- milestone: 指定なし

## 処理フロー

1. 指定されたMarkdownファイルを読み込む
2. Frontmatterとコンテンツを解析
3. リポジトリ情報を取得（git remote）
4. MCPサーバーのmcp__github__create_issueを使用してIssue作成
5. 作成されたIssueのURLを返す

## エラーハンドリング
- ファイルが存在しない場合
- git リポジトリでない場合
- GitHub認証エラー
- ネットワークエラー

これらのエラーは適切にキャッチし、ユーザーにフィードバックします。