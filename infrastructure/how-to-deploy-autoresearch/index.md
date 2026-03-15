# How to deploy autoresearch:

**Author:** hoeem (@hooeem)
**Date:** March 8, 2026
**Source:** https://x.com/hooeem/status/2030720614752039185
**Stats:** 3 replies, 1 retweet, 59 likes, 103 bookmarks, 3,469 views

---

![Cover image](image-cover.jpg)

You want to deploy Andrew Karpathy's autoresearch that lets you have agents run research experiments whilst you sleep with any prompt but you don't know how, read this.

So, here's the thing, Karpathy (@karpathy) released something awesome:

> [Embedded tweet from @karpathy (status/2030371219518931079)](https://x.com/karpathy/status/2030371219518931079)

Now, myself, like 18 thousand others who bookmarked this post and whilst there's instructions on the [github](https://github.com/karpathy/autoresearch) link ([here](https://github.com/karpathy/autoresearch)):

![help me](image-1.jpg)

I need that information distilled with clear step-by-step instructions to follow and also, I don't have an NVIDIA GPU, I have a Mac, I needed help, so I asked supergrok, opus 4.6, and GPT 5.4 to HELP A BROTHER OUT...

I thought (as there's 18 thousand others that bookmarked the article) that others feel the exact same (if you don't, don't read this article), so I thought I'd at least share that here:

## PART 1: What Is This and Why Should You Care?

On March 7, 2026, Andrej Karpathy one of the most famous AI researchers alive (former AI director at Tesla, founding member of OpenAI) released an update on a project called **autoresearch**.

Here's the idea in the simplest terms possible:

Imagine you have a baby AI, a tiny language model that barely knows how to string words together. To make it smarter, a human researcher would normally sit at a computer for hours: change a setting, run a test, check if it helped, and repeat. Over and over. It's tedious.

**Autoresearch replaces the human researcher with an AI agent.** You write simple instructions in plain English inside a text file. Then the AI agent does this in a loop, automatically, all night:

1. Reads your instructions
2. Changes the training code (a single file called train.py)
3. Runs a 5-minute training test on your computer's GPU (the powerful chip that handles heavy math)
4. Measures the result, a score called **val_bpb** (lower number = smarter model)
5. If the score improved, it saves the change. If not, it throws it away
6. Repeats roughly **12 experiments per hour**, about **100 experiments overnight**

You wake up to real, measurable progress without lifting a finger.

Karpathy describes it as *"part code, part sci-fi, and a pinch of psychosis."*  Every dot in his progress chart is one complete 5-minute experiment, and the model steadily gets better as the dots accumulate.

**Why this matters beyond the experiment itself:** This is a glimpse of where AI research is heading. Instead of humans manually tweaking settings, AI agents run the experiments autonomously. You're not just running a cool demo, you're experiencing what automated AI research looks like in practice.

Okay, so the rest of this article (Part 2,3,4,5,6,7,8,9,10,11,12,13) will give you everything with a lot of detail, I also wanted to give you the stripped down version which I'm going to put inside this markdown code which you can copy and paste which gets straight to the point. You can scroll down to part 2 but I wanted to give you the option here:

<details>
<summary>Quick Start Guide (collapsed markdown)</summary>

# Run Karpathy's Autoresearch Today

### An AI runs experiments on your computer all night while you sleep. Here's how to set it up.

---

## Step 0: Can Your Computer Do This?

**You need one of these:**

- **A Windows or Linux PC with an NVIDIA graphics card** (like an RTX 3060, 4070, or 4090)
- **A Mac with an M1, M2, M3, or M4 chip** (any Mac bought since late 2020)

**To check on Windows:** Press the Windows key, type "Command Prompt", open it, type `nvidia-smi`, press Enter. If you see your GPU name, you're good.

**To check on Mac:** Click the Apple menu -> About This Mac. Look for "Chip." If it says M1, M2, M3, or M4 (or any variant like M2 Pro, M3 Max), you're good. If it says Intel, this won't work.

**If you don't have either of these, stop here** -- this project requires a powerful GPU to run.

---

## Step 1: Open Your Terminal

This is a text window where you type commands. Every computer has one.

- **Mac:** Press `Cmd + Space`, type **Terminal**, press Enter
- **Windows:** Press the Windows key, type **PowerShell**, press Enter
- **Linux:** Press `Ctrl + Alt + T`

You'll use this window for every step below.

---

## Step 2: Install Two Small Tools

**Install uv** (this handles Python and all dependencies automatically):

Mac/Linux:

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Windows (PowerShell):

```
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

**Install Claude Code** (the AI agent that runs experiments for you -- requires a Claude Pro or Max subscription at $20-100/month):

Mac/Linux:

```
curl -fsSL https://claude.ai/install.sh | bash
```

Windows (PowerShell):

```
irm https://claude.ai/install.ps1 | iex
```

**Now close your Terminal and open a fresh one.** This is essential -- skip it and the next steps will fail.

> **Don't want to pay for Claude Code?** Download Cursor for free from cursor.com instead. It does the same job but with a visual interface rather than the Terminal. The rest of this guide still applies -- you'll just use Cursor's chat panel instead of Claude Code.

---

## Step 3: Install Git (If You Don't Have It)

Git tracks all the experiments. Check if you already have it:

```
git --version
```

If you see a version number, skip ahead. If not:

- **Mac:** It will prompt you to install Xcode Command Line Tools -- click Install
- **Windows:** Download from https://git-scm.com/download/win and run the installer (accept all defaults)
- **Linux:** `sudo apt install git`

---

## Step 4: Download the Project

**Mac users** -- you need the Mac-compatible version:

```
cd ~/Desktop
git clone https://github.com/miolini/autoresearch-macos.git
cd autoresearch-macos
```

**Windows/Linux users** -- you need the original:

```
cd ~/Desktop
git clone https://github.com/karpathy/autoresearch.git
cd autoresearch
```

> On Windows, replace `~/Desktop` with `%USERPROFILE%\Desktop`

---

## Step 5: Set Everything Up

Run these three commands one at a time:

```
uv sync
```

*(Installs Python and all packages. Takes a few minutes the first time.)*

```
uv run prepare.py
```

*(Downloads training data. Takes about 2 minutes. Only needed once.)*

```
uv run train.py
```

*(Runs one 5-minute test. If it finishes and shows a number next to "val_bpb" -- you're ready.)*

**If you get a red error:** Copy the entire error message, paste it into claude.ai, and ask "What does this mean and how do I fix it?" You'll get a direct answer.

---

## Step 6: Let the AI Run All Night

Make sure you're in the project folder, then launch Claude Code:

```
claude
```

It will ask you to log in the first time -- follow the browser prompt.

Once you see the Claude Code prompt, type this:

```
Hi have a look at program.md and let's kick off a new experiment! Let's do the setup first.
```

**That's it.** Claude will start reading the project, modifying the training code, running 5-minute experiments, keeping what works, discarding what doesn't, and repeating. Minimise the window and go to sleep.

You'll wake up to dozens of successful experiments and a smarter model than you started with.

> **Using Cursor instead?** Open the project folder in Cursor (File -> Open Folder), then type the same message into Cursor's AI chat panel on the right side.

---

## Quick Answers

**What is val_bpb?** A score measuring how smart the model is. Lower = better.

**What is train.py?** The single file containing all the AI training code. The AI agent modifies this file during experiments.

**What is program.md?** Your instruction file for the AI agent. This is the only file you ever need to edit -- it tells the agent what to try.

**Is the Mac version safe?** Yes. Karpathy links to it from his own project page. The developer (Artem Andreenko) has 167 public projects on GitHub and a years-long public track record. The fork announcement got 70,000+ views on X. The entire codebase is about 630 lines -- you can read the whole thing in 20 minutes.

**How many experiments will it run overnight?** About 100 (roughly 12 per hour).

**Do most experiments succeed?** No. Most fail. That's normal. The agent automatically keeps the wins and throws away the losses. Out of 100 experiments, maybe 10-20 will be improvements.

**What does it cost?** The code is free. Claude Code requires Claude Pro ($20/month) or Max ($100/month). Cursor has a free tier if you run experiments manually.

---

## If Something Goes Wrong

| Problem | Fix |
|---|---|
| `command not found: uv` | Close Terminal, open a new one |
| `command not found: git` | Install git (see Step 3) |
| CUDA / GPU error (Windows/Linux) | Search YouTube: "install CUDA toolkit [your GPU]" |
| MPS / Metal error (Mac) | Make sure you downloaded the Mac fork, not the original |
| Out of memory | Your GPU needs more VRAM. The agent usually adapts automatically |
| Claude Code won't authenticate | You need a paid Claude subscription ($20/month minimum) |

---

**Links:**
Original (Windows/Linux): https://github.com/karpathy/autoresearch
Mac version: https://github.com/miolini/autoresearch-macos
Cursor: https://cursor.com
Claude Code: https://code.claude.com

---

*Total setup time: about 30 minutes. Then it runs by itself.*

</details>

## PART 2: Do You Have the Right Computer?

**This is the most important section. If your computer doesn't meet these requirements, the project will not work.** Read this carefully before installing anything.

Windows or Linux Users

You need:

- **An NVIDIA graphics card (GPU):** examples: RTX 3060, RTX 3070, RTX 4070, RTX 4090, or any NVIDIA card from the last few years
- **At least 10-20 GB of free disk space**
- **An internet connection**
- **Windows 10 or 11** or a **Linux distribution** like Ubuntu

**How to check if you have an NVIDIA GPU (Windows):**

1. Press the **Windows key** on your keyboard
2. Type **Command Prompt** and open it (it's a black window where you type text)
3. Type exactly this and press Enter: nvidia-smi
4. If it shows your GPU name and driver version, you're good, keep going
5. If it says "command not found" or shows an error, you either don't have an NVIDIA GPU or need to install NVIDIA drivers first

**No NVIDIA GPU?** The original autoresearch project will not work for you on Windows/Linux. But if you have a Mac, read the next section.

Mac Users

Karpathy's original code only supports NVIDIA GPUs, which Macs don't have. However, a developer named Artem Andreenko created a **Mac-compatible version** that works with Apple's own chips.

You need:

- **An Apple Silicon Mac**, any Mac with an M1, M2, M3, or M4 chip
- **16 GB of memory minimum** (32 GB or more is better for larger experiments)
- **At least 10-20 GB of free disk space**
- **An internet connection**

**How to check your Mac's chip:**

1. Click the **Apple menu** (the apple icon in the top-left corner of your screen)
2. Click **About This Mac**
3. Look for **"Chip"**, it should say M1, M2, M3, M4, or a variant like M2 Pro, M3 Max, etc.
4. If it says **"Intel"** instead, unfortunately this project will not work well for you

**Every MacBook Air, MacBook Pro, Mac Mini, iMac, Mac Studio, and Mac Pro sold since late 2020 has an Apple Silicon chip.** If you bought your Mac in the last 5 years, you're almost certainly fine.

**Important:** On Mac, you download from a different link (the macOS fork) instead of Karpathy's original. The setup commands are nearly identical, only the download source changes.

Quick Reference Table

![Quick Reference Table](image-2.jpg)

## PART 3: "Is the Mac Version Safe?"

Smart question. You should always think twice before running code from the internet. Here's what I checked:

**Karpathy himself links to it.** His own autoresearch README page directly mentions the macOS fork (miolini/autoresearch-macos) in the platforms section. He invited the community to create forks for other platforms and linked this one. That's as close to an official endorsement as you'll get.

**It's a proper GitHub fork.** GitHub publicly marks it as "forked from karpathy/autoresearch." Every single change the developer made compared to the original is visible and trackable. Nothing is hidden.

**The developer is a real, established person.** Artem Andreenko (username: miolini) has 167 public projects on GitHub, is based in San Diego, and runs a company called SentientWave. He has years of public coding history, Go tools, browser extensions, networking benchmarks. This is not an anonymous or throwaway account.

**The fork announcement was highly public.** His announcement on X (Twitter) was a direct reply to Karpathy's original post and received roughly 70,000 views, 58 retweets, and 781 likes. Thousands of developers saw and scrutinised the code.

**The changes are small and well-understood.** The fork swaps out an NVIDIA-only library (FlashAttention-3) for PyTorch's built-in equivalent, and adds Apple Metal-specific adjustments for memory and compilation. These are standard, boring adaptations, not exotic or suspicious.

**The entire codebase is tiny.** The training file is about 630 lines of Python. You can read the whole thing in 20 minutes. There is nowhere for malicious code to hide.

**Extra safety step you can take:** After downloading, open the folder in Claude Code and ask: *"Read every file in this repo. Is there anything suspicious, any hidden network calls, data collection, or code that does something other than train a language model?"* On a project this small, the audit takes about 30 seconds.

**Bottom line:** This is a legitimate, community-endorsed, Karpathy-linked fork by a real developer with a public track record. It is as safe as open-source software gets for something this new.

## PART 4: Jargon Buster (Read This)

Before installing anything, here's a plain-English dictionary so nothing catches you off guard:

**Terminal:** A text-based window where you type commands instead of clicking buttons. Every computer has one built in. On Mac it's called "Terminal." On Windows it's called "Command Prompt" or "PowerShell." Think of it like texting your computer and it texts back.

**GPU:** Graphics Processing Unit. The powerful chip in your computer that handles heavy math. AI training relies heavily on GPUs. Your regular processor (CPU) is the brain; the GPU is the muscle.

**CUDA:** NVIDIA's software that lets programs use NVIDIA GPUs for math. You need this on Windows/Linux. Think of it as the translator between the training code and your NVIDIA hardware.

**Metal / MPS:** Apple's version of CUDA. It's built into every Mac with Apple Silicon. You don't need to install anything, it just works.

**Git:** A system for tracking changes to files over time. Think of it like "save points" in a video game. Every time the AI agent finds an improvement, it creates a save point. If the next experiment fails, it can go back to the last good save.

**uv:** A small, free tool that automatically installs Python (the programming language this project uses) and all the other software the project needs. It makes setup dramatically easier. Without uv, you'd need to install several things separately.

**val_bpb:** "Validation bits per byte." The single score that measures how smart the AI model is. **Lower numbers = smarter model.** This is the number the AI agent is trying to push down overnight.

**train.py:** The single Python file that contains all the AI training code. This is the only file the AI agent modifies during experiments. It contains the model architecture, the optimiser settings, the training loop, everything.

**program.md:** The instruction file you write for the AI agent. This is the only file you (the human) ever need to edit. It tells the agent what kind of experiments to try, what to prioritise, and how to behave. Think of it as the mission briefing you hand to your tireless lab assistant before going to bed.

**Repo / Repository:** A project folder on GitHub. When people say "download the repo," they mean "download the project folder."

**Fork:** A copy of someone else's project that you can modify independently. The Mac version is a "fork" of Karpathy's original, same foundation, adapted for different hardware.

**Clone:** Downloading a copy of a project from GitHub to your computer using a Terminal command.

## PART 5: What You Need to Install

You need exactly **three free tools** plus one **AI tool** (which requires a paid subscription). Here's each one and how to get it.

**Tool 1: Terminal (You Already Have This)**

Every computer comes with a Terminal built in. You just need to open it.

- **Mac:** Press Cmd + Space (this opens Spotlight search), type **Terminal**, press Enter. A window with a blinking text cursor will appear
- **Windows:** Press the **Windows key**, type **Command Prompt** or **PowerShell**, press **Enter**. A window (usually black or blue) with a blinking text cursor will appear
- **Linux:** Press Ctrl + Alt + T. A terminal window will appear

This is where you'll type all the commands in this guide. When this guide says "open Terminal," it means open this window.

**Tool 2: Git**

Git is the save-point system that tracks every experiment. You need it installed.

**Mac:** Git usually comes pre-installed. To check, open Terminal and type:

```
git --version
```

If you see a version number like git version 2.39.0, you're done. If macOS asks you to install "Xcode Command Line Tools," click **Install** and wait a few minutes. That will install git for you.

**Windows:**

1. Go to [https://git-scm.com/download/win](https://git-scm.com/download/win) in your web browser
2. Download the installer
3. Run it and **accept all the default settings** (just click Next, Next, Next, Install)
4. When it's done, close and reopen Command Prompt
5. Type git --version to confirm it worked

**Linux (Ubuntu/Debian):** Open Terminal and type:

```
sudo apt install git
```

Enter your password when asked.

**Tool 3: uv (The Automatic Installer)**

This tiny tool will install Python and every package the project needs, automatically. You do not need to install Python separately. uv handles everything.

**Mac or Linux:** Open Terminal and paste this exact command, then press Enter:

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows:** Open PowerShell (not Command Prompt) and paste:

```
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

**Critical step after installing uv:** Close your Terminal/PowerShell window completely. Then open a brand new one. This is essential, the new window will recognise the uv command. If you skip this step, your computer won't know uv exists yet.

To verify it worked, type in the new window:

```
uv --version
```

You should see a version number.

Tool 4: Your AI Agent (The Brain That Runs the Experiments)

The AI agent is the intelligence that actually reads your instructions, modifies the code, runs experiments, and decides what to keep or discard. You need one of the following options:

Option A: Claude Code, Best for Full Autopilot

**What it is:** A command-line tool made by Anthropic (the company behind Claude). It runs directly in your Terminal, can read and write files, run commands, and use git, exactly what autoresearch needs for the fully autonomous loop.

**What it costs:** Requires a paid subscription, Claude Pro ($20/month) or Claude Max ($100 or $200/month). A free Claude account will not work with Claude Code.

**How to install (Mac or Linux):**

```
curl -fsSL https://claude.ai/install.sh | bash
```

**How to install (Windows or PowerShell):**

```
irm https://claude.ai/install.ps1 | iex
```

After installing, close and reopen your Terminal. Then type:

```
claude --version
```

If you see a version number, the installation worked.

**First time you run it:** When you type claude for the first time, it will open your web browser and ask you to log in to your Anthropic account. Follow the prompts to authenticate. This only happens once.

Option B: Cursor, Best for Visual Learners

**What it is:** A free AI-powered code editor. It looks like a normal text editor but has an AI chat panel built into the right side. Under the hood, it uses Claude or other AI models.

**What it costs:** Has a free tier with limited AI usage. Pro plan is $20/month for heavier use.

**How to install:**

1. Go to [https://cursor.com](https://cursor.com) in your web browser
2. Download the installer for your operating system
3. Install it like any other app (drag to Applications on Mac, run the installer on Windows)

Cursor is easier if you like seeing files visually and chatting with the AI in a sidebar. However, the fully autonomous overnight loop works more smoothly with Claude Code.

Option C: [Claude.ai](https://claude.ai) Chat, Manual Only (What You're Reading This In)

You can use the [Claude.ai](https://claude.ai) chat interface to troubleshoot errors, get explanations, or ask for code change suggestions. But you cannot automate the loop, you'd have to copy-paste code and results back and forth manually. This works for learning, but it's not what the project was designed for.

**Can you use Claude with autoresearch?** Yes, Karpathy says so himself. His README explicitly says: *"Simply spin up your Claude/Codex or whatever you want in this repo."*

This bit is up to you, you can use Claude Code, Codex, Cursor etc. The choice is yours here.

## PART 6: Step-by-Step Setup

Pick your operating system below and follow every command exactly. Copy-paste is your friend, don't try to type these from memory.

> Setup for Mac (Apple Silicon)

**Step 1: Open Terminal**

Press Cmd + Space, type **Terminal**, press Enter.

**Step 2: Download the Mac version of autoresearch**

Type these commands one at a time, pressing Enter after each:

```
cd ~/Desktop
git clone https://github.com/miolini/autoresearch-macos.git
cd autoresearch-macos
```

What this does: Goes to your Desktop, downloads the Mac-compatible version of the project, and enters the project folder.

**If you'd rather not use git:** Go to [https://github.com/miolini/autoresearch-macos](https://github.com/miolini/autoresearch-macos) in your web browser, click the green **Code** button, click **Download ZIP**, unzip it to your Desktop. Then in Terminal type:

```
cd ~/Desktop/autoresearch-macos-master
```

**Step 3: Install all the software the project needs**

```
uv sync
```

This downloads and installs Python, PyTorch, and everything else automatically. The first time takes a few minutes, you'll see progress bars. Be patient.

**Step 4: Download the training data**

```
uv run prepare.py
```

This downloads the text data the model will learn from and builds a tokenizer (a tool that chops text into small pieces the model can process). Takes about 2 minutes. You only ever need to do this once.

**Step 5: Run one test training to make sure everything works**

```
uv run train.py
```

This runs a single 5-minute training session on your Mac's GPU using Metal. You should see:

- Numbers scrolling by (this is normal, it's showing training progress)
- After about 5-7 minutes (including startup time), it finishes
- It displays a val_bpb score at the end

**If it finishes without big red error messages, your setup is complete.** You're ready for the fun part.

**If you see errors:** Copy the entire red error text, paste it into [Claude.ai](https://claude.ai) (this chat), and ask: "What does this error mean and how do I fix it on a Mac?" You'll get a direct answer.

> Setup for Windows (NVIDIA GPU)

**Step 1: Open Command Prompt or PowerShell**

Press the **Windows key**, type **Command Prompt**, press Enter.

**Step 2: Download autoresearch**

```
cd %USERPROFILE%\Desktop
git clone https://github.com/karpathy/autoresearch.git
cd autoresearch
```

**If you'd rather not use git:** Go to [https://github.com/karpathy/autoresearch](https://github.com/karpathy/autoresearch) in your browser, click the green **Code** button, click **Download ZIP**, unzip to your Desktop, then:

```
cd %USERPROFILE%\Desktop\autoresearch-master
```

**Step 3: Install all the software the project needs**

```
uv sync
```

**Step 4: Download the training data**

```
uv run prepare.py
```

**Step 5: Run one test training**

```
uv run train.py
```

If it finishes after about 5 minutes and shows a val_bpb score, you're good.

**If you get a CUDA error or GPU error:** This means your NVIDIA drivers or CUDA toolkit aren't set up correctly. Search YouTube for *"install NVIDIA CUDA toolkit [your GPU model] Windows"* and follow a video tutorial. This is a one-time setup, and there are excellent step-by-step videos available.

> Setup for Linux (NVIDIA GPU)

**Step 1: Open Terminal**

Press Ctrl + Alt + T.

**Step 2: Download autoresearch**

```
cd ~/Desktop
git clone https://github.com/karpathy/autoresearch.git
cd autoresearch
```

**Step 3-5: Same as Windows**

```
uv sync
uv run prepare.py
uv run train.py
```

If it finishes and shows a val_bpb score, your setup is complete.

## PART 7: Let the AI Do the Research

You've confirmed the test training works. Now it's time to hand the keys to the AI agent and let it run all night.

Full Autopilot with Claude Code (Recommended)

**Step 1:** Open Terminal and go to your project folder:

Mac:

```
cd ~/Desktop/autoresearch-macos
```

Windows:

```
cd %USERPROFILE%\Desktop\autoresearch
```

Linux:

```
cd ~/Desktop/autoresearch
```

**Step 2:** Launch Claude Code:

The first time, it will ask you to authenticate in your browser. Follow the prompts and log in to your Anthropic account. Once you see the Claude Code prompt (a cursor waiting for your input), type this exact message:

```
Hi have a look at program.md and let's kick off a new experiment! Let's do the setup first.
```

**Step 3:** Claude Code will now automatically:

- Read the program.md instruction file
- Read all the project files to understand the codebase
- Create a git branch (a separate save-point track) for the experiment run
- Start modifying train.py with its first experimental idea
- Run the 5-minute training
- Check the val_bpb score
- If the score improved, save the change with git. If not, discard it
- Move on to the next experiment and repeat

**Step 4:** Minimise the window and go to bed. Or go cook dinner. Or watch a film. The AI agent will keep running experiments all night without you.

**Pro tip:** To make it fully autonomous without pausing to ask you questions, you can tell it at the start: *"Run fully autonomously. Don't ask for confirmation between experiments. Keep going until I come back."*

Semi-Manual with Cursor (Good for Learning)

**Step 1:** Open Cursor. Click **File -> Open Folder** and navigate to your autoresearch folder on the Desktop.

**Step 2:** In the file list on the left, click on program.md to open it. Read through it, it's written in plain English.

**Step 3:** In Cursor's AI chat panel (usually on the right side), type:

```
Hi have a look at program.md and let's kick off a new experiment! Let's do the setup first.
```

**Step 4:** The AI will suggest changes to train.py. Click the accept/apply button to make the changes.

**Step 5:** Open Terminal, navigate to your project folder, and run:

```
uv run train.py
```

**Step 6:** When it finishes (about 5 minutes), tell the AI in Cursor's chat what the val_bpb score was. It will decide whether to keep or discard the change, then suggest the next experiment.

**Step 7:** Repeat. As you get comfortable, Cursor's AI can handle more of the loop automatically.

Fully Manual with [Claude.ai](https://claude.ai) Chat

If you don't have Claude Code or Cursor:

1. Run uv run train.py in your Terminal
2. Copy the output (especially the val_bpb score) and paste it into [Claude.ai](https://claude.ai)
3. Ask Claude to suggest the next experiment, what to change in train.py and why
4. Make the change in a text editor (any text editor works, TextEdit on Mac, Notepad on Windows)
5. Run uv run train.py again
6. Repeat

This is the slowest option but works perfectly for understanding how the process works.

## PART 8: What You'll See When You Wake Up

After a night of autonomous experiments, here's what you'll find in your project folder:

**A git history full of commits.** Each commit is one successful experiment where the agent found an improvement. View it by opening Terminal, navigating to your project folder, and typing:

```
git log --oneline
```

**A lower val_bpb score.** The model has genuinely gotten smarter compared to where it started. The starting baseline is around 0.9979, anything lower means progress.

**A heavily modified train.py.** The agent will have changed the training code in ways that improve performance, architecture tweaks, different optimiser settings, adjusted hyperparameters, modified batch sizes, and more.

**A results.tsv file.** This is a spreadsheet-style log of every experiment with its score, memory usage, and whether it was kept or discarded.

**An analysis.ipynb notebook.** You can open this in Cursor or Jupyter Notebook to see graphs showing how the score improved over time, each dot is one experiment.

## Part 9: Understanding What's Happening (Optional but Interesting)

You don't need to understand any of this to use autoresearch. But if you're curious:

**What does the agent actually change?** Everything inside train.py is fair game. The agent might change the model's architecture (how the neural network is shaped), the optimiser (how the model learns from mistakes), the learning rate (how big the adjustments are), the batch size (how much data it looks at per step), and more.

**Why 5 minutes?** A fixed time budget means every experiment is directly comparable. Whether the agent tries a huge model or a tiny one, it always gets exactly 5 minutes. This forces the agent to find the most efficient configuration for your specific hardware.

**Why is val_bpb the metric?** Bits per byte is a vocabulary-independent measure of how well the model predicts text. Unlike some other metrics, it works fairly even when the agent changes the tokeniser vocabulary size, making it ideal for comparing wildly different configurations.

**What's program.md doing?** It's the "meta-prompt", the instructions that tell the AI agent how to behave. Improving this file is your job as the human. Better instructions lead to faster research progress. Karpathy's insight is that the human is now programming the research organisation, not running individual experiments.

## PART 10: Tips for Getting the Best Results

**Start simple.** Get one manual run working first (uv run train.py). If that doesn't work, the autonomous loop won't either. Fix the basics before going autopilot.

**Your one job is to improve program.md.** This is the leverage point. Add instructions like: *"Try small improvements first. Focus on making val_bpb go down. Think step by step and explain every change before making it. If an experiment direction hasn't worked after 3 attempts, try something completely different."*

**Don't panic when experiments fail.** Most experiments will not improve the score. Out of 100 overnight experiments, maybe 10-20 will be keepers. This is completely normal, it's how research works. The agent discards failures and keeps successes automatically.

**Check in periodically at first.** Before trusting the full overnight run, watch the first 3-4 experiments to make sure the loop is working. If the agent is stuck or confused, adjust your program.md instructions and restart.

**More memory helps.** If you have a Mac with 32 GB or 64 GB of unified memory, or a GPU with lots of VRAM (like the RTX 4090 with 24 GB), the agent can explore larger models and more complex architectures within each 5-minute window.

## PART 11: Troubleshooting

**"command not found: uv"** Close your Terminal window completely and open a fresh one. After installing uv, you must start a new Terminal session for it to be recognised.

**"command not found: git"** You need to install git first. See Part 5, Tool 2.

**"CUDA error" or "no CUDA-capable device" (Windows/Linux)** Your NVIDIA drivers or CUDA toolkit aren't installed or configured. Search YouTube for *"install CUDA toolkit [your GPU model]"* and follow a step-by-step video. This is a one-time setup.

**"MPS error" or Metal-related error (Mac)** Make sure you downloaded the **macOS fork** (miolini/autoresearch-macos), not Karpathy's original repo. The original does not support Mac.

**"Out of memory" or "OOM"** Your GPU doesn't have enough memory for the model size the agent is trying. The agent should handle this automatically by trying smaller configurations, but if it keeps happening, you may need a GPU with more VRAM or more unified memory on Mac.

**"uv sync" is very slow or seems stuck** This is normal on the first run. It's downloading PyTorch, which is a large package (several gigabytes). Make sure your internet connection is stable and just wait.

**Claude Code says "authentication required" or won't start** You need a paid Claude subscription. Claude Pro is $20/month and is the minimum required. Free accounts do not work with Claude Code.

**The test training works but Claude Code doesn't start the experiment loop** Make sure you're in the right folder when you launch claude. Use cd to navigate to your autoresearch folder first. If Claude Code seems confused, try being more explicit: *"Read the file program.md in this directory, then follow its instructions to set up and run autonomous experiments on train.py."*

**"Permission denied" errors** On Mac/Linux, you might need to make files executable. Try: chmod +x train.py prepare.py. On Windows, make sure you're running PowerShell or Command Prompt normally (not as Administrator unless specifically needed).

## PART 12: What Everything Costs

![Cost table](image-3.png)

**Minimum cost to run full autopilot:** $20/month for a Claude Pro subscription (for Claude Code).

**Minimum cost to run manually:** $0 (use Cursor's free tier and run experiments by hand).

## PART 13: Links and Resources

Original repo:

(Windows/Linux) [https://github.com/karpathy/autoresearch](https://github.com/karpathy/autoresearch)

Mac fork (Apple Silicon) [https://github.com/miolini/autoresearch-macos](https://github.com/miolini/autoresearch-macos)

Karpathy's announcement on X [https://x.com/karpathy/status/2030371219518931079](https://x.com/karpathy/status/2030371219518931079)

Mac fork announcement on X [https://x.com/miolini/status/2030402705374728218](https://x.com/miolini/status/2030402705374728218)

Cursor (AI code editor) [https://cursor.com](https://cursor.com)

Claude Code documentation [https://code.claude.com](https://code.claude.com/)

uv (Python package manager) [https://astral.sh/uv](https://astral.sh/uv)

Git for Windows [https://git-scm.com/download/win](https://git-scm.com/download/win)

[Claude.ai](https://claude.ai) (chat interface) [https://claude.ai](https://claude.ai/)

## OKAY I HOPE THAT HELPED! THANK YOU!
