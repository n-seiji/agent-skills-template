# インストール手順

## Claude Code

```
/plugin marketplace add n-seiji/agent-skills-template
/plugin install example@agent-skills-template
```

確認: `/plugin` の Installed タブに example が出る。skill は自動で読み込まれ、
slash command があれば `/example:<command>` で呼べる。

## Codex

```
codex plugin marketplace add n-seiji/agent-skills-template
codex plugin install example
```

確認: `codex plugin list` に example が出る。

## 動作確認

どちらの agent でも「hello skill を実行して」と依頼し、
`skills/hello/SKILL.md` の応答が返れば install 成功。
