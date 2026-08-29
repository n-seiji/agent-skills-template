# agent-skills-template

Claude Code / Codex の**両方**に skills を公開する plugin marketplace リポジトリの
サンプル & テンプレート。

English: [README.md](README.md)

## ポイント

- **skills 正本は `skills/` 一箇所** — 各 plugin へは相対 symbolic link で公開
  (二重管理しない)
- `.claude-plugin/` + `.agents/plugins/` の 2 つの marketplace index で
  Claude Code / Codex 両対応
- 「このようなリポジトリを作る」ための skill
  ([skills/create-skills-marketplace](skills/create-skills-marketplace/SKILL.md))
  を同梱。agent にこの skill を読ませればこの構成を再現できる

## 構成

```
├── skills/                          # ★ skills の正本 (single source of truth)
│   ├── hello/                       #   install 確認用の最小 skill
│   └── create-skills-marketplace/   #   この構成の repo を作るための skill
├── plugins/example/
│   ├── .claude-plugin/plugin.json   # Claude Code 用
│   ├── .codex-plugin/plugin.json    # Codex 用
│   ├── commands/                    # (任意) Claude Code の slash command wrapper
│   └── skills/*                     # → ../../skills/* への symlink
├── .claude-plugin/marketplace.json  # Claude Code marketplace index
├── .agents/plugins/marketplace.json # Codex marketplace index
├── AGENTS.md / CLAUDE.md (symlink)
└── docs/installation.md
```

## インストール

[docs/installation.md](docs/installation.md) を参照。

```
# Claude Code
/plugin marketplace add n-seiji/agent-skills-template
/plugin install example@agent-skills-template

# Codex
codex plugin marketplace add n-seiji/agent-skills-template
codex plugin install example
```

## このテンプレートから新しいリポジトリを作る

GitHub の "Use this template" を使うか、agent に
[skills/create-skills-marketplace/SKILL.md](skills/create-skills-marketplace/SKILL.md)
を読ませて scaffold させる。

⭐ **このテンプレートを参考にリポジトリを作ったら、この repo に star (favorite) をお願いします!**

## License

MIT
