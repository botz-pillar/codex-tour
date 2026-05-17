# Install / pre-flight reference

Only use this if the user says they have not installed the Codex CLI yet, or seems unsure whether it's running on their machine.

If they are already chatting with you in the Codex CLI, they are already inside it — say so explicitly: **"You're talking to me through the Codex CLI right now. It's already installed and running. We're good to go."** Then return to Step 1 of the tour.

---

## Pre-install checks (especially for InfoSec audiences)

Before they touch the installer:

- **Work laptop with corporate management?** Stop and ask IT/security. Most companies now have a vendor-approval process for AI tools, and MDM/DLP/EDR agents may block the install or log every file Codex reads.
- **Sensitive data on the machine** — client files, PHI, regulated material, NDA-protected docs? Same answer.
- **Personal machine + non-sensitive learning?** Continue.
- **Personal machine + work data in the folder?** Treat it like a work laptop.

---

## Heads up: the Codex CLI requires an OpenAI plan or API credits

The free ChatGPT plan does **not** include Codex CLI usage beyond brief trials. To use it consistently, the user needs one of:

- **[ChatGPT Plus](https://openai.com/chatgpt/pricing)** — about $20/month. The entry point. Plenty for learning, light/moderate daily use, most personal projects. Default recommendation.
- **[ChatGPT Pro](https://openai.com/chatgpt/pricing)** — around $200/month. For daily-driver power users with substantially higher limits. Not where beginners should start.
- **[ChatGPT Business / Enterprise](https://openai.com/chatgpt/enterprise)** — per-seat through their org. The right path if their employer is piloting Codex CLI.
- **API credits** — pay-as-you-go via [platform.openai.com](https://platform.openai.com/). Useful for people who'd rather meter by tokens than commit monthly.

Surface this *before* they create an OpenAI account, not after — saves the "wait, I have to pay?" moment.

If they're hesitant, suggest they try ChatGPT free at [chatgpt.com](https://chatgpt.com) for a few days first to see if they like how the model thinks. The free plan won't run the Codex CLI (the agent on their machine), but it's a fair preview — paid plan unlocks the hands.

---

## If they genuinely haven't installed yet

Ask which OS: **macOS, Windows, or Linux.**

### Opening a terminal first

If they've truly never opened one:
- **macOS:** Press `Cmd+Space`, type `Terminal`, press Enter.
- **Windows:** Press `Win`, type `Terminal` or `PowerShell`, press Enter.
- **Linux:** Look for *Terminal* in the apps menu, or `Ctrl+Alt+T`.

A window opens with a blinking cursor. That's the terminal. Every command below gets pasted there.

### macOS

Two ways:

1. **Homebrew** — recommended.
   ```
   brew install --cask codex
   ```
   Then run `codex` in any terminal. If they don't have Homebrew, point them to [brew.sh](https://brew.sh/) and offer to walk through it.

2. **npm** — if they already have Node.js.
   ```
   npm install -g @openai/codex
   ```
   Requires Node. Easiest install via Homebrew:
   ```
   brew install node
   ```
   Then `codex` in any terminal. If `npm install -g` prompts for `sudo`, that's touching system paths — pause and confirm before approving.

### Windows

Two ways:

1. **npm** — requires Node.js. Install via [winget](https://learn.microsoft.com/en-us/windows/package-manager/winget/) (`winget install OpenJS.NodeJS`) or download from [nodejs.org](https://nodejs.org/). Then:
   ```
   npm install -g @openai/codex
   ```
   Then run `codex` in PowerShell or Windows Terminal.

2. **WSL** — Windows Subsystem for Linux. Option for terminal die-hards but flag it as a "set aside an hour" project for newcomers.

### Linux

`npm install -g @openai/codex` after Node is installed via their distro's package manager (apt, dnf, pacman). Then `codex` in any terminal.

---

## Signing in

After install, running `codex` will prompt for sign-in. Two paths:

- **ChatGPT (recommended)** — OAuth flow that opens the browser and uses their ChatGPT Plus/Pro subscription. Easiest.
- **API key** — paste an OpenAI API key from [platform.openai.com/api-keys](https://platform.openai.com/api-keys). Use this if they chose the pay-as-you-go route.

Run `codex login` from a fresh terminal if they ever need to re-auth.

---

## Confirming it worked

After install + sign-in, they should be able to:

1. Run `codex` in a terminal.
2. See a chat prompt waiting for input.
3. Type `/help` and see a menu pop up (this is the decisive test — if `/help` shows a menu, they're in the Codex CLI).

If `/help` does nothing or just prints `/help` as text, they're probably in ChatGPT (the browser chat) — not the Codex CLI. Point them back to the terminal.

If the install prompts for admin rights and they're on a managed laptop, stop. Do not enter someone else's credentials. Send them back to the pre-install check.

---

## What "you're already inside it" looks like

If you (Codex) are reading this skill, the user is talking to you through the Codex CLI. They are inside the tool. They might not realize this. A single sentence — "you're already in the Codex CLI; this terminal IS the tool" — saves them ten minutes of confusion.
