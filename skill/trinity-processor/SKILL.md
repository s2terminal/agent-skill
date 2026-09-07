---
name: trinity-processor
description: 'ChatGPT、Gemini、Claudeに対して、同じ内容を調査させる'
disable-model-invocation: true
argument-hint: '質問内容'
---

質問を入力します。
ユーザーから入力された質問に対して、複数のAIを使って指示を出して調べさせてください。

以下すべてのAIに対して同じクエリを実行してください。Deep Researchに相当する機能があれば、それを使ってください。

- ChatGPT https://chatgpt.com/
- Gemini https://gemini.google.com/
- Claude https://claude.ai/

調べた結果を取得できたら、内容をまとめてください。Webのテキスト取得ツールで簡単に取得できなかった場合は、参照用のURLだけを返してください。
