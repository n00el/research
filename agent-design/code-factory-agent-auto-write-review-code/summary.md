# Code Factory: How to setup your repo so your agent can auto write and review 100% of your code

**Author:** Ryan Carson (@ryancarson)
**Date:** Feb 16, 2026
**Source:** https://x.com/ryancarson/status/2023452909883609111

## Key Takeaways

- Define a single machine-readable contract (JSON) that maps file paths to risk tiers and specifies required checks per tier, eliminating ambiguity between scripts, workflows, and docs.
- Run a lightweight risk-policy gate before expensive CI fanout to avoid wasting compute on PRs that are already blocked by policy or unresolved review findings.
- Enforce strict current-head SHA matching for all review state: never trust stale "clean" evidence from older commits, and require reruns after every push.
- Use exactly one canonical workflow to request review reruns, deduplicating by SHA marker to prevent duplicate bot comments and race conditions.
- Wire an automated remediation loop where a coding agent reads review findings, patches code, and pushes fix commits to the same branch — but never bypasses policy gates.
- Treat browser evidence (screenshots, flow captures) as machine-verifiable CI artifacts with assertions, not just images pasted into PR descriptions.
- Convert production regressions into harness-gap issues with SLA tracking so fixes grow long-term coverage instead of becoming one-off patches.

## One-Line Summary

To let agents write and review all your code, build a deterministic control plane around risk-tiered policy gates, current-head SHA discipline, and automated remediation loops that never bypass guardrails.

## Topics

agent-driven development, ci/cd automation, code review agents, risk-based merge policy, automated remediation, harness engineering
