# Installation

## Claude Code

```
/plugin marketplace add n-seiji/agent-skills-template
/plugin install example@agent-skills-template
```

Verify: the `example` plugin appears in the **Installed** tab of `/plugin`.
Skills load automatically; slash commands are available as `/example:<command>`.

## Codex

```
codex plugin marketplace add n-seiji/agent-skills-template
codex plugin install example
```

Verify: `example` appears in `codex plugin list`.

## Smoke test

On either agent, ask: "run the hello skill".
If the response follows `skills/hello/SKILL.md`, the installation works.
