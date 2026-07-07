---
name: rebaser
description: 'git rebaseでブランチを最新化する'
allowed-tools: Read Write Edit Grep
argument-hint: '<base-branch> <topic-branch>'
disable-model-invocation: true
---

以下の手順でgit rebaseを行います。

1. 引数から対象のブランチを確認し、git switchで切り替える
  - 省略したら、ベースブランチを origin/main, 対象ブランチを現在のブランチとする
  - unstagedな変更がある場合は、作業を止めてユーザーに確認します
2. ブランチがまだpushされていないことを確認する
  - rebase対象のブランチがpushされている場合は、作業を止めてユーザーに確認します
3. git rebaseを行う
4. conflictが発生した場合は、解消する
  - 現在のブランチの変更内容を確認してから、conflict解消の作業を行う
  - conflictが単純な操作で解消できない場合は、作業を止めてユーザーに確認します。
