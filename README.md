# Wannalead plugin

One portable plugin package for ChatGPT, Codex, Claude, Cowork, and Claude
Code. It connects to the hosted Wannalead MCP server over OAuth and bundles the
Wannalead workflow skills. It does not install or execute the Wannalead CLI.

## Components

- `plugin.json` and `mcp.json`: portable Agent Plugins package for ChatGPT and
  Codex.
- `.codex-plugin/plugin.json`: Codex compatibility metadata.
- `.claude-plugin/plugin.json` and `.mcp.json`: Claude and Claude Code
  compatibility metadata.
- `skills/`: MCP-native Wannalead workflows shared by every host.

## Local validation

```bash
python3 ~/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py plugins/wannalead
claude plugin validate plugins/wannalead --strict
```

## Install from the public marketplace repository

Codex:

```bash
codex plugin marketplace add ThomasMarcelin754/wannalead-plugin
codex plugin add wannalead@wannalead
```

Claude Code:

```bash
claude plugin marketplace add ThomasMarcelin754/wannalead-plugin
claude plugin install wannalead@wannalead
```

## Local development install

```text
/plugin marketplace add ThomasMarcelin754/wannalead
/plugin install wannalead@wannalead
```

The public ChatGPT/Codex and Claude/Cowork listings require approval in their
respective plugin directories. Every surface uses the same hosted MCP endpoint:
`https://wannalead.co/mcp`.
