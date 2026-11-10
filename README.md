自分用Agent Skills置き場

## スキルのインストール方法

### Claude Codeのプラグインマーケットプレイスを使う場合

```
/plugin marketplace add s2terminal/agent-skill
/plugin install markdown-writer@s2terminal
```

Claude Code 2.1.275以降では、マーケットプレイスの追加とインストールを1ステップで実行できます。

```
/plugin install markdown-writer --marketplace s2terminal/agent-skill
```

### 手動でコピーする場合

```
$ mkdir -p ~/.claude/skills
$ cp -r skill/xxxxxxxx ~/.claude/skills
```

[vercel-labs/skills](https://github.com/vercel-labs/skills)を使う場合

```
$ npx skills add https://github.com/s2terminal/agent-skill
$ npx skills add https://github.com/s2terminal/agent-skill/tree/main/skill/xxxxxxxx
```

[skills.sh/s2terminal/agent-skill](https://www.skills.sh/s2terminal/agent-skill)
