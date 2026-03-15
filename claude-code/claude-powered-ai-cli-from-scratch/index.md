# How to build a simple Claude-powered AI CLI from scratch. No framework. One file.

**Author:** Morgan (@morganlinton)
**Date:** 2026-03-07
**Source:** https://x.com/morganlinton/status/2030366723711651857
**Stats:** 4 replies, 6 retweets, 109 likes, 217 bookmarks, 8,719 views

---

![Cover image](image-1.jpg)

I think we're moving into a pretty neat time in history, where how people learn shifts from reading books and watching YT videos, to actually building the thing. This also means that anyone can learn how to build with AI by actually building with AI.

This article walks through building an 80-line Node.js CLI using the Anthropic SDK - a bare-bones, AI-native command line chat interface powered by Claude. One file, no framework, streams tokens as they arrive.

## The Core Concept

The Claude API is completely stateless - it remembers nothing between calls. The app keeps a `history` array of every message and sends the whole thing on every request. That array is the memory.

## The Nine Components

The entire app lives in `index.js`. Here are the nine building blocks:

### 1. Imports

```javascript
import "dotenv/config";
import readline from "readline";
import Anthropic from "@anthropic-ai/sdk";
```

dotenv reads your .env file and puts values onto process.env. readline is Node's built-in module for reading line-by-line from stdin. The official Anthropic SDK handles auth, retries, and the streaming protocol.

### 2. Client Setup

```javascript
const client = new Anthropic();
```

Instantiating with no args: SDK auto-reads ANTHROPIC_API_KEY from process.env.

### 3. Conversation History

```javascript
const history = [];
```

The Claude API is stateless - it has no memory between calls. We manually track the full conversation and send it every time. Each message is `{ role: "user" | "assistant", content: string }`.

### 4. System Prompt

```javascript
const SYSTEM = "You are a concise CLI assistant. Be direct and accurate. No fluff, tell it like it is.";
```

This shapes Claude's personality and constraints for every turn. It's sent separately from the history, not as a message.

### 5. Readline Interface

```javascript
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});
```

rl wraps stdin/stdout so we can prompt the user and read their input.

### 6. Colors

```javascript
const dim   = (s) => `\x1b[2m${s}\x1b[0m`;
const cyan  = (s) => `\x1b[36m${s}\x1b[0m`;
const gray  = (s) => `\x1b[90m${s}\x1b[0m`;
```

ANSI escape codes. `\x1b[Xm` sets color, `\x1b[0m` resets it.

### 7. Core: Send Message + Stream Response

```javascript
async function chat(userInput) {
  history.push({ role: "user", content: userInput });
  process.stdout.write(cyan("Claude: "));

  const stream = await client.messages.stream({
    model: "claude-sonnet-4-6",
    max_tokens: 1024,
    system: SYSTEM,
    messages: history,
  });

  let fullText = "";

  stream.on("text", (chunk) => {
    process.stdout.write(chunk);
    fullText += chunk;
  });

  await stream.finalMessage();
  process.stdout.write("\n\n");

  history.push({ role: "assistant", content: fullText });
}
```

`stream()` returns an async iterable of server-sent events. We use it instead of `create()` so tokens print as they arrive - no waiting for the full response. `stream.on("text")` fires for every text delta chunk. `finalMessage()` resolves when the stream is complete.

### 8. Input Loop

```javascript
function loop() {
  rl.question(cyan("You: "), async (input) => {
    const trimmed = input.trim();

    if (!trimmed || trimmed === "/quit" || trimmed === "/exit") {
      console.log(gray("\nBye.\n"));
      rl.close();
      process.exit(0);
    }

    if (trimmed === "/history") {
      console.log(dim(JSON.stringify(history, null, 2)));
      loop();
      return;
    }

    await chat(trimmed);
    loop();
  });
}
```

Recursive async function: prompt, chat, repeat. Recursion here is clean because each call awaits the previous one - the stack doesn't grow unbounded.

### 9. Entry Point

```javascript
console.log(gray("\nAI CLI  |  /quit to exit  |  /history to inspect context\n"));
loop();
```

## Built-in Commands

| Command | What it does |
|---------|-------------|
| `/quit` or `/exit` | Exit the CLI |
| `/history` | Print the full conversation context as JSON |

## Project Structure

```
ai-cli/
├── index.js        # The entire app
├── package.json    # Dependencies and scripts
├── .env            # Your API key (never commit this)
├── .env.example    # Safe template to copy from
└── .gitignore      # Excludes node_modules and .env
```

## Extending It

A few natural next steps if you want to go further:

**Make it a global command** - add this to `package.json`, then run `npm link`:

```json
"bin": { "ai": "index.js" }
```

Now you can type `ai` from any directory.

**Pipe input** - already works out of the box:

```bash
echo "summarize this in one sentence: $(cat somefile.txt)" | node index.js
```

**Persist sessions** - write `history` to a JSON file on exit, load it on startup to continue where you left off.

**Clear context** - add a `/clear` command that resets the `history` array to `[]`.

**Switch models** - change the `model` field in the `chat()` function. Available options: `claude-opus-4-6`, `claude-sonnet-4-6`, `claude-haiku-4-5-20251001`.

**Add tools** - pass a `tools` array to the API call to give Claude the ability to run functions (web search, code execution, file access, etc.).

## Cost

Claude Sonnet charges per token. A typical back-and-forth message costs a fraction of a cent. Long sessions with large history accumulate tokens faster since the full history is sent every turn. Use `/history` to see how large your context has grown.

## Source Code

Full code available at: https://github.com/Zen-Open-Source/SimpleCLI
