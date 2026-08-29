# AGENTS.md

この repo は **skills 配布用 plugin marketplace のサンプル & テンプレート**。
アプリケーションコードは無い。

## 構成ルール

- skills の正本は `skills/` のみ。編集はここで行う
- `plugins/*/skills/` 配下は symbolic link。実体を置かない
- symlink は repo 内相対パス (`../../../skills/<name>`) を維持する
- skill を追加したら: `skills/<name>/SKILL.md` 作成 → plugin から symlink →
  必要なら marketplace index / plugin.json を更新
- SKILL.md の frontmatter (name / description) は必須

## この repo 自体の再現方法

`skills/create-skills-marketplace/SKILL.md` に scaffold 手順がある。
