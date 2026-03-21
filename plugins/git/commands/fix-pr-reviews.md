---
allowed-tools: Bash(gh:*), Bash(git:*), Read, Edit
description: Fetch PR review comments, triage them, and apply fixes
---

## Context

- Current branch: !`git branch --show-current`
- PR info: !`gh pr view --json number,title,url,reviewDecision 2>/dev/null || echo "No PR found"`

## Your task

PRのレビューコメントを取得し、トリアージして修正を適用する。ユーザーの入力: $ARGUMENTS

### Step 1: PR特定

- ユーザーがPR番号を指定した場合（例: `#123` や `123`）、その番号を使う
- 指定がなければ、現在のブランチに紐づくPRを使う（上記Contextから取得）
- PRが見つからない場合、以下を表示して停止:
  > 対象のPRが見つかりません。PRが存在するブランチで実行するか、PR番号を指定してください。

### Step 2: レビューコメント取得

リポジトリ情報を `gh repo view --json owner,name --jq '.owner.login + "/" + .name'` で取得し、以下の2つのAPIを呼び出す:

1. `gh api repos/{owner}/{repo}/pulls/{number}/comments` — インラインコメント（ファイル・行に紐づくコメント）
2. `gh api repos/{owner}/{repo}/pulls/{number}/reviews` — レビューサマリ（全体コメント）

レビューコメントが1件もない場合、以下を表示して停止:
> レビューコメントはありません。

### Step 3: トリアージ分類

各コメントを以下の3カテゴリに分類する:

1. **Auto-fix** — `suggestion` ブロック付きの明確な修正提案、または客観的なバグ指摘（タイポ、型エラー、null未チェック等）。ただし解決済み（resolved）でないこと
2. **Needs confirmation** — 設計変更、リファクタリング提案、トレードオフのある最適化、自動判断が困難なもの
3. **Skip** — 解決済みのコメント、LGTM/称賛、質問のみで具体的な修正提案がないもの

### Step 4: サマリテーブル表示

全コメントの分類結果をテーブルで表示する:

| # | Category | Reviewer | File | Line | Summary |
|---|----------|----------|------|------|---------|
| 1 | Auto-fix | ... | ... | ... | ... |
| 2 | Needs confirmation | ... | ... | ... | ... |
| 3 | Skip | ... | ... | ... | ... |

### Step 5: 修正適用

- **Auto-fix**: 対象ファイルをReadで読み取り、Editで修正を即座に適用する
- **Needs confirmation**: ユーザーにどれを適用するか確認し、承認されたもののみ修正する
- **Skip**: 何もしない

### Step 6: レポート

修正完了後、以下のサマリを表示する:

- 自動修正した件数
- ユーザー承認で修正した件数
- スキップした件数
- 変更したファイルの一覧
