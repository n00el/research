# How to build a simple Claude-powered AI CLI from scratch

**Author:** Morgan (@morganlinton)
**Date:** 2026-03-07
**Source:** https://x.com/morganlinton/status/2030366723711651857

## Key Takeaways

- The Claude API is completely stateless; conversation memory is just an array of messages you send with every request - there is no built-in session persistence.
- An entire streaming Claude CLI can be built in ~80 lines of Node.js using only three dependencies: dotenv, readline, and the Anthropic SDK.
- Using `client.messages.stream()` instead of `client.messages.create()` lets tokens print in real-time, giving a dramatically better UX for CLI tools.
- The system prompt is sent separately from the message history, not as a user/assistant message - this is the correct pattern for the Anthropic API.
- Long sessions accumulate tokens because the full history is sent every turn, meaning costs scale with conversation length, not just individual messages.
- Natural extensions include global CLI installation via `npm link`, session persistence via JSON file, and tool use via the `tools` array parameter.

## One-Line Summary

A practical walkthrough of building a minimal Claude-powered CLI in one file, demonstrating that the Anthropic API's stateless design means your app owns the conversation memory as a simple array.

## Topics

claude api, anthropic sdk, cli tools, node.js, streaming, ai development, learning by building
