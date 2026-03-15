# How We Built Secure, Scalable Agent Sandbox Infrastructure

**Author:** Larsen Cundric (@larsencc)
**Date:** Feb 27, 2026
**Stats:** 558K views | 1.8K likes | 248 reposts | 5K bookmarks
**URL:** https://x.com/larsencc/status/2027225210412470668

---

## Context

Browser Use runs millions of web agents. They started with browser-only agents on AWS Lambda (isolated, instant scaling, no secrets). Then they added code execution — agents could write/run Python, execute shell commands, create files. But the agent loop still ran on the same backend as the REST API. Redeploys killed running agents. Memory-hungry agents slowed the API.

## The Two Patterns

When an agent can run arbitrary code, it can access anything on the machine (env vars, API keys, DB credentials, internal services). It needs isolation.

### Pattern 1: Isolate the Tool
- Agent runs on your infrastructure
- Dangerous operations (code execution, terminal) run in a separate sandbox
- Agent calls sandbox via HTTP
- Code runs somewhere with nothing to leak

### Pattern 2: Isolate the Agent
- The **entire agent** runs in a sandbox with zero secrets
- Talks to outside world through a **control plane** that holds all credentials
- Agent becomes disposable — no secrets to steal, no state to preserve
- Can kill/restart/scale independently
- Control plane holds the truth

**Browser Use started with Pattern 1 and moved to Pattern 2.**

## The Sandbox

Same container image runs everywhere:
- **Production:** Unikraft micro-VM
- **Local dev & evals:** Docker container
- Single config switch: `sandbox_mode: 'docker' | 'ukc'`

### Unikraft in Production
- Each agent gets its own micro-VM, booting in <1 second
- Provisioned via Unikraft Cloud's REST API on dedicated bare metal in AWS
- Sandbox receives only 3 env variables: `SESSION_TOKEN`, `CONTROL_PLANE_URL`, `SESSION_ID`
- No AWS keys, no DB credentials, no API tokens
- Scale-to-zero: idle VMs suspend, resume instantly on next request
- Distributed across multiple Unikraft metros

### Docker in Development/Evals
- Same image, same entrypoint, same control plane protocol
- Can run on dev laptop or spin up hundreds in parallel for evals

### Hardening

1. **Bytecode-only execution:** Compile all Python to `.pyc` bytecode during Docker build, delete every `.py` file. Source is gone before agent runs.
2. **Privilege drop:** Entrypoint starts as root (to read root-owned bytecode), immediately drops to `sandbox` user via `setuid`/`setgid`.
3. **Environment stripping:** After reading `SESSION_TOKEN`, `CONTROL_PLANE_URL`, `SESSION_ID` into Python variables, delete them from `os.environ`. VM sits in private VPC with no permissions other than talking to control plane.

## Control Plane Architecture

A stateless FastAPI service acting as a proxy. The sandbox has no direct access to the outside world — every request hops through the control plane.

Every request carries a `Bearer: {session_token}` header. Control plane looks up session, validates it's active, executes operation with real credentials.

### LLM Proxying
- Sandbox sends only new messages
- Control plane owns full conversation history in database
- Reconstructs context on each call, forwards to LLM provider
- Keeps sandbox stateless — kill and respawn, conversation picks up where it left off
- Also enforces cost caps and handles billing

### File Sync via Presigned URLs
- Sandbox has `/workspace` directory for agent files
- File sync service watches for changes, periodically syncs to S3
- Sandbox never sees AWS credentials
- Flow: Sandbox detects changes → calls `POST /presigned-urls` → control plane generates scoped S3 upload URLs → sandbox uploads directly to S3
- Downloads work the same in reverse

### Gateway Protocol

```python
class AgentGateway(Protocol):
    async def invoke_llm(self, new_messages, tools, tool_choice) -> LLMResponse: ...
    async def persist_messages(self, messages) -> None: ...
```

- Production: `ControlPlaneGateway` sends HTTP to control plane
- Dev/evals: `DirectGateway` calls LLM directly, keeps history in memory
- Agent code doesn't know which one it's using — same interface, different backend

## Scaling

- Control plane is stateless: validate token, do work, return result
- Backend on ECS Fargate in private subnets behind ALB
- Control plane auto-scales on CPU utilization
- Sandboxes scale independently through Unikraft
- Each session gets its own VM

## Key Takeaway

> "Your agent should have nothing worth stealing and nothing worth preserving."

The tradeoff is an extra network hop on every operation and three services to deploy instead of one. In practice, latency is noise compared to LLM response times, and the operational complexity is familiar to ops teams.
