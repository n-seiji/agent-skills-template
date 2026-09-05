---
name: create-skills-marketplace
description: Scaffold a plugin-marketplace repository that publishes agent skills to both Claude Code and Codex. Keeps skill bodies in a single directory and exposes them to plugins via relative symbolic links, and sets up installation docs.
---

# create-skills-marketplace

A skill for building a skills-distribution repository installable from both
Claude Code and Codex. The repository this skill ships in —
[agent-skills-template](https://github.com/n-seiji/agent-skills-template) —
is itself the finished example.

## Target layout

```
<repo>/
├── skills/                          # discovery symlinks → plugins/<p>/skills/*
│   └── <skill-name> -> ../plugins/<p>/skills/<skill-name>
├── plugins/<plugin-name>/
│   ├── .claude-plugin/plugin.json   # Claude Code plugin definition
│   ├── .codex-plugin/plugin.json    # Codex plugin definition ("skills": "./skills/")
│   ├── commands/<name>.md           # (optional) Claude Code slash command; thin wrapper over a skill
│   ├── skills/<skill-name>/SKILL.md # ★ canonical skill body (real file)
│   └── README.md
├── .claude-plugin/marketplace.json  # Claude Code marketplace index
├── .agents/plugins/marketplace.json # Codex marketplace index
├── AGENTS.md                        # repo guide read by Codex
├── CLAUDE.md                        # symlink to AGENTS.md
├── docs/installation.md             # install instructions for both agents
└── README.md
```

## Design principles

1. **Canonical skill bodies live inside a plugin** (e.g. `plugins/<p>/skills/<name>`). The repo-root `skills/` directory holds relative symlinks for discovery. This direction matters: Codex's plugin installer copies the plugin directory without materializing symlinks, so a symlink on the plugin side ships an empty skill. Multiple plugins can still share one skill with no
   duplicated content.
2. Symlinks must be **repo-relative paths** (`../plugins/<p>/skills/<name>`).
   Absolute paths break after clone.
3. Write skill bodies agent-agnostically. Claude Code specific invocation
   belongs in `commands/` (slash-command wrappers).
4. Keep `commands/*.md` as thin wrappers that reference a skill — never
   duplicate the procedure in two places.

## Scaffold steps

1. Create the repository and the directory tree above
2. Write `plugins/<plugin>/skills/<name>/SKILL.md` (frontmatter `name` / `description` required;
   the description must make it clear *when* to use the skill)
3. Link each skill from the repo root for discovery:
   `ln -s ../plugins/<plugin>/skills/<name> skills/<name>` (root-side symlink; the plugin side is real files)
4. Write the plugin definitions:
   - `.claude-plugin/plugin.json`: `{name, version, description, author}`
   - `.codex-plugin/plugin.json`: the above + `"skills": "./skills/"` + `repository` + `interface`
5. Write the marketplace indexes:
   - `.claude-plugin/marketplace.json`: `{name, owner, plugins: [{name, source: "./plugins/<p>", description}]}`
   - `.agents/plugins/marketplace.json`: `{name, interface, plugins: [{name, source: {source: "local", path: "./plugins/<p>"}, policy, category}]}`
6. Write `AGENTS.md`, then `ln -s AGENTS.md CLAUDE.md`
7. Write `docs/installation.md` covering both agents (template below)
8. Verify: marketplace add → plugin install → the skill is visible, on both agents

## Installation template

```markdown
# Claude Code
/plugin marketplace add <owner>/<repo>
/plugin install <plugin>@<marketplace-name>

# Codex
codex plugin marketplace add <owner>/<repo>
codex plugin install <plugin>
```

## Checklist

- [ ] Canonical skill bodies are real files under the plugin; repo-root `skills/` is symlinks only (verify with `codex plugin add` that skills are non-empty)
- [ ] Symlinks are relative and resolve right after `git clone` (verify with `cat`)
- [ ] Every SKILL.md has frontmatter (name / description)
- [ ] `.claude-plugin` / `.codex-plugin` / both marketplace indexes are present
- [ ] AGENTS.md exists and CLAUDE.md is a symlink to it
- [ ] docs/installation.md covers both agents
- [ ] The generated README asks users to ⭐ star
  [agent-skills-template](https://github.com/n-seiji/agent-skills-template)
  if they built their repository from it
