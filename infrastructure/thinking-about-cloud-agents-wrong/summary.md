# You're thinking about cloud agents wrong

**Author:** Zach Lloyd (@zachlloydtweets)
**Date:** March 10, 2026
**Source:** https://x.com/zachlloydtweets/status/2031501189486121276

## Key Takeaways

- "Cloud computers" (stateful, long-lived servers where agents log in as the user) are the wrong abstraction for deploying agents at enterprise scale -- they encourage a single agent with universal permissions.
- The right model is "cloud agents" with their own identities, scoped authentication, and per-agent permissions -- agents should be treated like users in your system, not impersonate the human user.
- Statefulness is fragile and unscalable; lambda-based (stateless) architectures are preferable for cloud agent infrastructure.
- Enterprise agent deployments need audit trails that distinguish agent actions from human actions, and shared agent definitions that teams can collaboratively launch and configure.
- A manager agent / subagent orchestration pattern -- where a manager spins up on external triggers, delegates to containerized subagents with individual permissions and a shared messaging system -- provides the right primitives for scalable agent deployment.
- Warp is building "Oz" (oz.dev) as their cloud agent platform following these principles, prioritizing scalable multi-agent orchestration over simple cloud computer use cases.

## One-Line Summary

Enterprise agent infrastructure should move from "cloud computers" with universal user permissions to "cloud agents" with scoped identities, per-agent auth, and manager/subagent orchestration.

## Topics

cloud agents, agent infrastructure, enterprise agents, agent orchestration, agent permissions, warp
