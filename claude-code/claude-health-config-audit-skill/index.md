# Claude Health - Audit Your Claude Code Config Health

**Author:** tw93 (@AiTw93)
**Date:** Mar 23, 2026 (v1.5.4 latest release)
**Source:** https://github.com/tw93/claude-health
**Stats:** 494 stars, 22 forks, 56 commits, 9 releases

---

![Claude Health header](image-1.png)

## Claude Health

*Audit your Claude Code configuration health across all layers.*

A Claude Code skill that systematically reviews your project's setup using the six-layer framework: `CLAUDE.md -> rules -> skills -> hooks -> subagents -> verifiers`. It detects project complexity, runs two parallel diagnostic agents, and outputs a prioritized report telling you what to fix first.

## Install

Recommended, install globally for Claude Code:

```
npx skills add tw93/claude-health -a claude-code -s health -g -y
```

Install only in the current project:

```
npx skills add tw93/claude-health -a claude-code -s health -y
```

**Claude Plugin:**

```
claude plugin marketplace add tw93/claude-health
claude plugin install health
```

## Usage

Restart Claude Code after installation. Then in any Claude Code session, run `/health` or just say:

> "Run a health check on my Claude Code config"

The skill automatically detects your project tier (Simple / Standard / Complex) and calibrates checks accordingly. It won't flag missing layers that aren't needed for your project size.

## Troubleshooting

- `Unknown skill: health`: install to Claude Code explicitly with `-a claude-code`. Use `-g` if you want the skill available in every project. Restart Claude Code after installation.
- Installer summary shows `./.agents/skills/health`: that summary is generic. For Claude Code, the installed path should end up at `./.claude/skills/health` or `~/.claude/skills/health`.
- Claude Code currently mishandles `disable-model-invocation` for plugin skills. When that flag is set, explicit user invocation can fail with `Skill health cannot be used with Skill tool due to disable-model-invocation`. `health` does not use that flag for now so `/health` and explicit requests keep working.
- Scope: this repository targets Claude Code only. Codex and OpenClaw are not supported here.

## What Gets Checked

| Layer | Checks |
|---|---|
| **CLAUDE.md** | Signal-to-noise ratio, missing Verification/Compact Instructions, prose bloat |
| **rules/** | Language-specific rules placement, coverage gaps |
| **skills/** | Description token count, trigger clarity, auto-invoke strategy, frequency-based optimization |
| **skill security** | Prompt injection, data exfiltration, destructive commands, hardcoded credentials, obfuscation, safety overrides |
| **hooks** | Pattern field presence, file-type coverage, stale entries |
| **MCP** | Server count, token cost estimation, context pressure detection, filesystem allowlist failures |
| **allowedTools** | Dangerous or stale one-time commands |
| **Prompt Cache** | Dynamic timestamps, tool reordering, mid-session model switching |
| **Three-Layer Defense** | Critical rules covered by CLAUDE.md + Skill + Hook together |
| **Behavior** | Rules violated in practice, repeated corrections, context hygiene habits |

## Output

Results are grouped into three priority levels:

- **Critical**: Fix now: rule violations, dangerous permissions, cache-breaking patterns, MCP overhead >12.5%, skill security issues (prompt injection, data exfiltration, etc.)
- **Structural**: Fix soon: misplaced content, missing hooks, single-layer critical rules, skill quality issues (bloated content, broken references)
- **Incremental**: Nice to have: context hygiene, HANDOFF.md adoption, skill frequency tuning, skill provenance (symlink sources, version tracking)

## Background

Built on the six-layer framework described in [this blog post](https://tw93.fun/en/2026-03-12/claude.html). If you've read the post and want to know how your config measures up, `/health` is the fastest way to find out.

## License

MIT License, feel free to enjoy and participate in open source.
