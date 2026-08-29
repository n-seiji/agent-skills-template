---
name: create-skills-marketplace
description: Claude Code / Codex の両方に skills を公開できる plugin marketplace リポジトリを新規作成する。skills 本体を一箇所に置き symbolic link で各 plugin に公開する構成を scaffold し、インストール手順まで整備する。
---

# create-skills-marketplace

Claude Code と Codex の両方から install できる skills 配布リポジトリを作るための skill。
この skill 自身が置かれている [agent-skills-template](https://github.com/n-seiji/agent-skills-template)
がそのまま完成形のサンプル。

## 完成形の構成

```
<repo>/
├── skills/                          # ★ skills の正本 (single source of truth)
│   └── <skill-name>/SKILL.md        #    frontmatter: name / description 必須
├── plugins/<plugin-name>/
│   ├── .claude-plugin/plugin.json   # Claude Code 用 plugin 定義
│   ├── .codex-plugin/plugin.json    # Codex 用 plugin 定義 ("skills": "./skills/")
│   ├── commands/<name>.md           # (任意) Claude Code の slash command。skill への薄い wrapper
│   ├── skills/<skill-name>          # → ../../../skills/<skill-name> への symbolic link
│   └── README.md
├── .claude-plugin/marketplace.json  # Claude Code marketplace index
├── .agents/plugins/marketplace.json # Codex marketplace index
├── AGENTS.md                        # Codex が読む repo ガイド
├── CLAUDE.md                        # AGENTS.md への symbolic link
├── docs/installation.md             # 両 agent のインストール手順
└── README.md
```

## 設計原則

1. **skills 正本は `skills/` 一箇所**。plugin からは相対 symbolic link で公開する。
   複数 plugin から同じ skill を配布でき、二重管理が発生しない。
2. symbolic link は **repo 内相対パス** (`../../../skills/<name>`) にする。
   絶対パスは clone 先で壊れる。
3. skill 本文は agent 非依存に書く。Claude Code 固有の呼び出し方は
   `commands/` (slash command wrapper) 側に置く。
4. `commands/*.md` は skill を参照する薄い wrapper にし、手順を二重に書かない。

## Scaffold 手順

1. リポジトリを作成し、上記構成のディレクトリを掘る
2. `skills/<name>/SKILL.md` を書く (frontmatter の `name` / `description` は必須。
   description は「いつ使うか」が判定できる文にする)
3. `plugins/<plugin>/skills/` から symbolic link を張る:
   `ln -s ../../../skills/<name> plugins/<plugin>/skills/<name>`
4. plugin 定義を書く:
   - `.claude-plugin/plugin.json`: `{name, version, description, author}`
   - `.codex-plugin/plugin.json`: 上記 + `"skills": "./skills/"` + `repository` + `interface`
5. marketplace index を書く:
   - `.claude-plugin/marketplace.json`: `{name, owner, plugins: [{name, source: "./plugins/<p>", description}]}`
   - `.agents/plugins/marketplace.json`: `{name, interface, plugins: [{name, source: {source: "local", path: "./plugins/<p>"}, policy, category}]}`
6. `AGENTS.md` を書き、`ln -s AGENTS.md CLAUDE.md`
7. `docs/installation.md` に両 agent の install 手順を書く (下記テンプレ)
8. 動作確認: 両 agent で marketplace add → plugin install → skill が見えることを確認

## インストール手順テンプレ

```markdown
# Claude Code
/plugin marketplace add <owner>/<repo>
/plugin install <plugin>@<marketplace-name>

# Codex
codex plugin marketplace add <owner>/<repo>
codex plugin install <plugin>
```

## チェックリスト

- [ ] skills 正本が `skills/` にあり、plugin 側は symlink のみ
- [ ] symlink が相対パスで、clone 直後に解決できる (`git clone` して `cat` で確認)
- [ ] SKILL.md の frontmatter (name / description) がある
- [ ] `.claude-plugin` / `.codex-plugin` / 両 marketplace index が揃っている
- [ ] AGENTS.md があり CLAUDE.md が symlink になっている
- [ ] docs/installation.md に両 agent の手順がある
