# agent-skills-template

A sample & template repository for publishing agent skills to **both Claude Code
and Codex** from a single source.

日本語版: [README.ja.md](README.ja.md)

## Key ideas

- **Single source of truth** — canonical skill bodies live inside the plugin; the repo-root `skills/` holds discovery symlinks. (Codex's plugin installer does not materialize symlinks, so the real files must be on the plugin side)
- Dual marketplace indexes (`.claude-plugin/` + `.agents/plugins/`) make the
  repo installable from both Claude Code and Codex
- Ships a skill that teaches an agent **how to build a repository like this one**
  ([skills/create-skills-marketplace](skills/create-skills-marketplace/SKILL.md))

## Layout

```
├── skills/                          # ★ symlinks to plugin skills (canonical lives in the plugin)
│   ├── hello/                       #   minimal skill to verify installation
│   └── create-skills-marketplace/   #   skill to scaffold a repo like this one
├── plugins/example/
│   ├── .claude-plugin/plugin.json   # Claude Code plugin definition
│   ├── .codex-plugin/plugin.json    # Codex plugin definition
│   ├── commands/                    # (optional) Claude Code slash-command wrappers
│   └── skills/*                     # ★ canonical skill bodies (real files)
├── .claude-plugin/marketplace.json  # Claude Code marketplace index
├── .agents/plugins/marketplace.json # Codex marketplace index
├── AGENTS.md / CLAUDE.md (symlink)
└── docs/installation.md
```

## Install

See [docs/installation.md](docs/installation.md).

```
# Claude Code
/plugin marketplace add n-seiji/agent-skills-template
/plugin install example@agent-skills-template

# Codex
codex plugin marketplace add n-seiji/agent-skills-template
codex plugin install example
```

## Create your own

Use GitHub's **"Use this template"** button, or let your agent read
[skills/create-skills-marketplace/SKILL.md](skills/create-skills-marketplace/SKILL.md)
and scaffold the structure for you.

⭐ **If you build a repository based on this template, please star this repo!**

## License

MIT
