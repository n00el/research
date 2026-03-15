# Codex Security: Now in Research Preview

**Author:** OpenAI
**Date:** March 6, 2026
**Source:** https://openai.com/index/codex-security-now-in-research-preview/

---

OpenAI announced **Codex Security**, an AI-powered application security agent that builds deep context about a software project to identify complex vulnerabilities, validate them, and propose fixes. It is the evolution of **Aardvark**, OpenAI's private-beta security research agent unveiled in October 2025, now integrated into the broader Codex platform.

## Availability and Pricing

- Rolling out in research preview to **ChatGPT Pro, Enterprise, Business, and Edu** customers via Codex web.
- **Free usage for the first month** of the research preview.
- Works with connected **GitHub repositories** through Codex cloud.

## How It Works — Three-Stage Pipeline

1. **Context Analysis:** Analyzes the repository to understand the security-relevant structure of the system. Generates a **project-specific threat model** that captures what the system does, what it trusts, and where it is most exposed. This threat model is editable by the team to keep the agent aligned.

2. **Vulnerability Identification:** Uses the threat model as context to search for vulnerabilities and categorize findings based on **expected real-world impact** in the specific system (not generic severity).

3. **Validation and Fix Generation:** Tests flagged issues in **sandboxed environments**. When configured with project-specific environments, it can validate issues directly in running systems, reducing false positives further and enabling proof-of-concept creation. Proposed fixes are aligned with system behavior to reduce regressions.

## Beta Results and Statistics

- Over a recent **30-day period**, scanned more than **1.2 million commits** across external repositories.
- Identified **792 critical** and **10,561 high-severity** findings.
- Noise reduction of **84%** in one case since initial rollout.
- Over-reported severity findings reduced by more than **90%**.
- False positive rates on detections fell by more than **50%** across all repositories.

## Real-World Vulnerabilities Found

Codex Security discovered vulnerabilities in notable open-source projects including:
- GnuPG, GnuTLS, GOGS, Thorium, libssh, PHP, Chromium, OpenSSH
- Specific CVEs assigned include: CVE-2026-24881, CVE-2026-24882, CVE-2025-32988, CVE-2025-32989, CVE-2025-64175, CVE-2026-25242, CVE-2025-35430 through CVE-2025-35436.

## Key Differentiators

- **Context-aware detection:** Understands how code functions within its specific application context rather than applying generic scanning rules -- distinguishing genuine risks from noise.
- **Agentic reasoning from frontier models** combined with automated validation, delivering high-confidence findings and actionable fixes.
- The system "grounds vulnerability discovery in system context and validates findings before surfacing them to users."

> "It builds deep context about your project to identify complex vulnerabilities that other agentic tools miss, surfacing higher-confidence findings with fixes that meaningfully improve the security."

## Limitations

- Currently in **research preview** status -- still being evaluated and refined.
- Free usage is limited to the first month only.
- Requires configuration and tailoring for optimal project-specific validation.
- Threat models may need manual editing to stay aligned with a team's understanding of their system.
