# Give your Agent Maximum Freedom

**Author:** Gregor Zunic (@gregpr07)
**Date:** Mar 13, 2026
**Source:** https://x.com/gregpr07/status/2032539581359546757

## Key Takeaways

- Training always beats prompting: when a system prompt instruction conflicts with what the model learned during training (e.g., stateless Python calls, sequential loops, raw API calls), the training prior wins every time.
- Runtime guards are far more effective than prompt rules -- instead of telling the model "don't do X," let it try, catch the behavior with a runtime check, and return a clear error message. The model reliably fixes errors because error->fix loops are deeply embedded in training data.
- Not all unexpected agent behavior is bad -- some of it (self-healing crashed browsers, verifying output visually, preferring raw APIs) reveals genuine model intelligence. The correct response is to enable it safely rather than block it harder.
- The recommended method is: give maximum freedom in a sandbox, observe what the model reaches for, then decide whether to catch (harmful) or enable (useful unexpected behavior).
- Simplicity wins: a few robust runtime guards plus model intelligence outperform elaborate prompt engineering or complex heuristic systems every time.
- Build the harness around observed behavior, not assumed behavior -- stop predicting what the model will do and start observing what it actually does.
- From tens of thousands of Browser Use agent sessions: the less you assume about model behavior, the better it works.

## One-Line Summary

Runtime guards that leverage the model's trained error->fix loop are far more effective than prompt instructions for controlling agent behavior -- and unexpected behaviors should be observed and selectively enabled rather than blocked.

## Topics

AI agents, harness engineering, runtime guards, training priors, prompt engineering limits, Browser Use, agent autonomy, sandbox design
