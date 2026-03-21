---
allowed-tools: Bash(git fetch:*), Bash(git merge:*), Bash(git branch:*), Bash(git log:*)
description: マージ先ブランチの変更を現在のブランチに取り込んで最新化する
---

## Context

- Current branch: !`git branch --show-current`
- Available remote branches: !`git branch -r`

## Your task

マージ先（target）ブランチの最新変更を**現在のブランチに取り込む**。現在のブランチを target にマージするのではない点に注意。

方向: `origin/<target> → 現在のブランチ (!`git branch --show-current`)`

1. ユーザーのプロンプトからマージ先ブランチ名を取得する。指定がなければデフォルトで `main` を使用する。
2. **安全チェック**: 現在のブランチがマージ先ブランチと同じ場合、「現在すでに対象ブランチ上にいるため、sync は不要です」と報告して停止する。
3. `git fetch origin` でリモートの最新情報を取得する。
4. `git merge origin/<target>` を実行して、target の変更を現在のブランチに取り込む。
5. **コンフリクト発生時**: コンフリクトが発生したことをユーザーに報告し、対応はユーザーの指示に委ねる旨を伝えて停止する。自動的にマージを中断（abort）しないこと。
6. 成功時: マージ結果と、取り込まれたコミット数などの要約を報告する。

Do not use any other tools or do anything else. Do not send any other text or messages besides these tool calls and the result summary.
