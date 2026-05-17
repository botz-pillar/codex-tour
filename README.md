# NewCodexer

**Get from install to your first useful OpenAI Codex CLI session in about 15 minutes — even if you've never spent time in a terminal.**

If you work in IT or security, just heard about the Codex CLI (or are setting it up for the first time), and you're not sure where to start — this is for you. The tour shows you the shape of the tool, gets you to your first real win, and leaves you with a cheat-sheet of things to try the next day.

**No prior coding experience required.** Not git. Not GitHub. Not Python. Not AI tools. If you can type and read, you can do this. We'll walk you through every step below — start to finish — including signing up for the things you might not have yet.

> **A note about safety first:** because the Codex CLI can run commands and edit files on your machine, "no prior experience required" applies to *getting set up* — not to *what you let it do*. Sections 2, 3, and 4 below cover the honest version of what that means before you install anything.

This is the second one I built. NewClauder came first — same idea for [Claude Code](https://github.com/botz-pillar/NewClauder).

---

## Table of contents

1. [Is this for you?](#1-is-this-for-you)
2. [Before you install (work-laptop / sensitive-data check)](#2-before-you-install)
3. [What you'll actually be able to do](#3-what-youll-actually-be-able-to-do)
4. [What this means for your machine (the honest safety version)](#4-what-this-means-for-your-machine)
5. [Cost: Codex CLI requires a paid plan from day one](#5-cost-codex-cli-requires-a-paid-plan-from-day-one)
6. [Set up your OpenAI account](#6-set-up-your-openai-account)
7. [Install the Codex CLI on your computer](#7-install-the-codex-cli-on-your-computer)
8. [Install this tour](#8-install-this-tour)
9. [Start the tour](#9-start-the-tour)
10. ["Wait, where am I?" reference](#10-wait-where-am-i)
11. [What you'll learn (in plain English)](#11-what-youll-learn)
12. [What you do NOT need to learn first](#12-what-you-do-not-need-to-learn-first)
13. [Helpful links for beginners](#13-helpful-links-for-beginners)
14. [Troubleshooting](#troubleshooting)
15. [Privacy](#privacy)
16. [Feedback & contact](#feedback--contact)

---

## 1. Is this for you?

Check any of these:

- [ ] You just installed the Codex CLI (or are about to) and don't know where to start
- [ ] You work in IT, security, compliance, or somewhere adjacent
- [ ] You've heard "AI agents are a big deal" but every tutorial assumes you're already a developer
- [ ] You're worried about pasting the wrong thing into the wrong tool
- [ ] You don't have a GitHub account or anything special installed and that has been a blocker

Any of those hit? Keep reading.

Already a developer comfortable with the terminal, MCP servers, and Codex sandbox modes? Skip this. Read the [official Codex CLI docs](https://github.com/openai/codex) instead.

---

## 2. Before you install

**Getting permission once is cheaper than getting caught.** If you're on a work laptop, check with IT/security before installing — most companies now have a vendor-approval process for AI tools, and many run MDM (mobile-device management), DLP (data-loss prevention), or EDR (endpoint-detection) agents that will either block the install or quietly log every file Codex reads.

A 30-second check before you touch the installer:

- **Work laptop with corporate management?** Stop. Ask IT/security. Forwarding policy violations is harder than checking first.
- **Sensitive data on your machine** — client files, PHI, regulated material, NDA-protected docs? Same answer.
- **Personal machine + non-sensitive learning projects?** You're good. Continue.
- **Personal machine but the folder you'd open contains any work data?** Treat it like a work laptop.

When you chat with the Codex CLI, your messages go to OpenAI. The legal terms depend on how you signed in — a personal ChatGPT account follows the [ChatGPT Terms of Use](https://openai.com/policies/row-terms-of-use/); an OpenAI API key follows the [API terms and Business data policy](https://openai.com/policies/business-terms/); a ChatGPT Business / Enterprise account follows whatever DPA your org signed. By default, Codex CLI traffic through the API and ChatGPT Business / Enterprise is **not** used to train OpenAI's models — see the [OpenAI Trust Portal](https://trust.openai.com/) for the authoritative details and any zero-retention options.

---

## 3. What you'll actually be able to do

By tomorrow morning you could be doing things like:

- **Pasting a suspicious email or alert into the terminal** — Codex reads the headers and body, pulls out the indicators, drafts a verdict paragraph you could send back to the reporter, and saves the analysis to a file.
- **Pointing Codex at a folder of logs** — it ranks the noisy IPs, finds the talkers, and outputs a CSV you can hand to the next analyst.
- **Asking it to draft a Sigma rule, Splunk SPL, or KQL query for a CVE you just read** — and lint it against your existing rule pack before you commit.
- **Turning a regulator's findings PDF into a tracker** — control, owner, severity, due date, evidence needed.
- **Walking it through a codebase you inherited** — and getting back an `AGENTS.md` future-you can re-use.

It's a chat in your terminal where Codex can also touch your files and run commands on your machine — with your approval each time. The tour in section 9 will pick one of these (matched to your role) and do it with you live.

> **Heads up for the tour exercise:** the examples above are *capabilities*, not what you should paste in during your first session. Use **synthetic, sample, or non-sensitive content** for the tour — practice the workflow without putting real client data, PHI, or regulated material through it the first time. The tour will pick low-stakes example data so you can focus on learning the tool.

---

## 4. What this means for your machine

The honest version, in three plain-English sentences:

- **When you approve a Codex CLI command, it runs with your user's full permissions** — same as if you typed it yourself in a terminal, capped by whichever sandbox mode is active. Approving `rm -rf` in a sandbox that allows it deletes files. Approving "curl this URL and pipe it to bash" runs whatever that script says. Read the proposed command before you say yes.
- **Codex reads file content and web pages as both data AND instructions.** *Prompt injection* — a malicious file or webpage tricking the AI into doing something you didn't ask for — is the defining new risk class of agentic AI tools. **The tell:** if Codex proposes an action that doesn't match what you originally asked for *after* it just read a file or visited a URL, that's the signature of an injection attempt — stop and check before approving.
- **There's a read-only mode for when you're nervous.** Launch with `codex --sandbox read-only` — Codex can look at things and propose ideas but can't change anything until you flip it back. Recommended for your first session and any session pointed at unfamiliar data.

You don't need to memorize this. The tour will hand it back to you at the moment it matters. Sandbox modes are also covered in the concept list in section 11.

---

## 5. Cost: Codex CLI requires a paid plan from day one

Disclosing this **before** you create an account, because it matters: **there is no free tier for the Codex CLI**. The free ChatGPT chat at chatgpt.com is a different product. To use the Codex CLI you'll need one of:

- **[ChatGPT Plus](https://openai.com/chatgpt/pricing)** — about **$20/month**. **Start here.** Enough for learning, light/moderate daily use, and most personal projects. You can upgrade later.
- **[ChatGPT Pro](https://openai.com/chatgpt/pricing)** — around **$200/month**. For daily-driver power users with much higher usage limits. Not where beginners should start.
- **[ChatGPT Business / Enterprise](https://openai.com/chatgpt/enterprise)** — per-seat through your org, with admin controls and a stronger default data-handling posture. Right path if your employer is piloting Codex CLI.
- **[OpenAI API credits](https://platform.openai.com/)** — pay-as-you-go. Useful if you want to meter usage by tokens rather than commit to a subscription. Skip unless you already know you prefer this model. Pricing changes; check the [pricing page](https://openai.com/api/pricing) before you commit.

**Whether it's worth the cost is a personal call.** Some folks save five-plus hours a week and the math is obvious; others try it for a month and decide it's not for them yet. Both are valid.

> **Want to feel out the model before paying?** Try ChatGPT free at [chatgpt.com](https://chatgpt.com) for a few days — ask it actual questions from your job and see how it thinks. The free plan won't run the Codex CLI (the agent on your machine), but it's a fair preview of the model's reasoning. If you like the conversation, the paid plan unlocks the hands.

The rest of this README assumes you've decided to try it.

---

## 6. Set up your OpenAI account

1. Go to **[chatgpt.com](https://chatgpt.com)** (or [auth.openai.com/sign-up](https://auth.openai.com/log-in/identifier))
2. Sign up with email, Google, Microsoft, or Apple
3. Subscribe to **[ChatGPT Plus](https://openai.com/chatgpt/pricing)** (per section 5) — or generate an API key at [platform.openai.com](https://platform.openai.com/api-keys) if you chose the API path

> **Sidebar — "what's the difference between ChatGPT and the Codex CLI?"**
> *ChatGPT* is the AI chat at chatgpt.com. *The Codex CLI* is an OpenAI agent that runs in your terminal with the ability to read files, run commands, and edit things on your computer — only after each action you approve. Same account, same subscription. This tour is for the Codex CLI.

---

## 7. Install the Codex CLI on your computer

The Codex CLI is a terminal tool. There's no desktop app — you'll be running it in a terminal window. If you've never used a terminal, that's fine; you just need to open one and paste a command.

### Step 7a — Open a terminal

- **macOS:** Press `Cmd+Space`, type `Terminal`, press Enter. (Or open `/Applications/Utilities/Terminal.app`.)
- **Windows:** Codex CLI requires **WSL2** (Windows Subsystem for Linux 2) — it does not run natively in PowerShell or `cmd.exe`. Open PowerShell as administrator and run `wsl --install` (this installs Ubuntu by default; reboot when prompted). After reboot, launch *Ubuntu* from the Start menu — that opens your WSL2 terminal. Every Codex command from here on gets typed inside the WSL2 / Ubuntu window, not PowerShell. Full guide: [learn.microsoft.com/windows/wsl/install](https://learn.microsoft.com/en-us/windows/wsl/install).
- **Linux:** Look for *Terminal* or *Console* in your applications menu. Or `Ctrl+Alt+T` on most distros.

A new window opens with a blinking cursor. That's the terminal. Every command in this section gets pasted there, followed by Enter.

### Step 7b — Install Codex CLI

Pick the path that matches your OS. **Don't run this on a managed work laptop** without going back to section 2 first.

**macOS (recommended path — Homebrew):**

```bash
brew install --cask codex
```

If you don't have Homebrew, install it from [brew.sh](https://brew.sh/) first, or use the npm path below.

**Any OS (npm — requires Node.js):**

```bash
npm install -g @openai/codex
```

- **macOS:** install Node first with `brew install node` (or download from [nodejs.org](https://nodejs.org/)). On some macOS setups `npm install -g` wants `sudo` — that's a yellow flag, not a red one, but it means the install is touching system paths. If that prompt appears and you're not sure, stop and ask before approving.
- **Windows (inside WSL2 / Ubuntu):** install Node inside your WSL2 shell with `sudo apt update && sudo apt install -y nodejs npm` (or use [nvm](https://github.com/nvm-sh/nvm) for newer Node versions). Then `npm install -g @openai/codex` and run `codex` — all inside the Ubuntu / WSL2 terminal, not PowerShell. **Homebrew on Windows is not supported by Codex CLI** — the Brew path is macOS-only.
- **Linux:** install Node from your distro's package manager (`apt`, `dnf`, `pacman`). Then run `codex` in any terminal.

> **Heads up on managed machines:** Homebrew or npm-global may need admin rights. If the install halts with a credentials prompt, don't enter someone else's credentials — go back to section 2 and check policy first.

### Step 7c — Sign in

In the same terminal, run:

```bash
codex
```

It'll prompt you to sign in. Pick **ChatGPT** (recommended — your Plus/Pro subscription covers it; a browser will open so you can sign in, then close itself). Pick **API key** if you went the pay-as-you-go route.

### Step 7d — Confirm it's working

After signing in, you should:

1. See a chat prompt waiting for input in your terminal — *inside the `codex` command, not ChatGPT in a browser*
2. Be signed in
3. Be able to type `/help` and see a Codex menu pop up

If `/help` does nothing or just prints `/help` as text, you're probably in the wrong place — chatgpt.com in a browser doesn't support Codex slash commands. Make sure you ran `codex` in a terminal and your terminal is the active window. See section 10 for the full "which surface am I in?" reference if you're still confused.

---

## 8. Install this tour

Before you install: this plugin is a folder of plain markdown instructions plus a couple of supporting reference files. It makes no network calls, adds no tools to the Codex CLI, and requests no additional permissions. The source is at [github.com/botz-pillar/NewCodexer](https://github.com/botz-pillar/NewCodexer) — every file is human-readable.

> **Standing rule for any Codex CLI plugin: read it before you install it.** This plugin's source is public and every file is plain markdown. The same discipline applies to *every* plugin you'll consider going forward — third-party plugins can install tools, request permissions, or include scripts that run on your machine. Trust nothing you can't read. This is the single most valuable security habit for working with an agentic AI tool.

The Codex CLI has two steps for installing a plugin from GitHub:

### Step 8a — Add the marketplace (from your shell)

**Exit Codex first** (type `/exit` if you're inside it). Then in your terminal, run:

```bash
codex plugin marketplace add botz-pillar/NewCodexer
```

You should see a confirmation that the marketplace was added. If you see an error, copy it, launch `codex` again, paste it back with *"what does this mean?"* — Codex is good at decoding its own errors.

### Step 8b — Install the plugin (from inside Codex)

Now launch Codex:

```bash
codex
```

Inside the Codex TUI, type:

```
/plugins
```

This opens the plugin browser. Find **new-codexer** in the list, highlight it, and confirm install. The browser shows you everything the plugin will install before you say yes. Approve it.

> If the `/plugins` browser doesn't show up, you're either on an older Codex CLI version (run `brew upgrade --cask codex` or `npm update -g @openai/codex`) or you're not inside the Codex CLI. Use the `/help` test from section 10.

The tour is now installed.

---

## 9. Start the tour

In the same session (or a new one — run `codex` again), type **any one** of these:

- `I'm new to Codex CLI, walk me through it`
- `I'm a helpdesk guy trying to break into security — help me get started with Codex`
- `I work in compliance and my boss told me to look at the Codex CLI. Is this for me?`

The tour will ask you two questions (your day job, your terminal comfort), give you the safety story, then walk you through **one real thing** chosen for your role. By the end of the chat you'll have a file you made, a starter-prompts cheat sheet saved to your Documents folder, and a sense of what to try next.

Default tour length: **5–10 minutes**, depending on how deep your role-matched exercise goes. Ask for the *"full tour"* if you want all the concepts walked through explicitly.

> **Magic phrase for any moment you're overwhelmed:**
> Type *"slow down and explain like I'm new"* — the tour will reset its pace and walk you through whatever confused you. Use it freely; it's literally what the phrase is for.

> **Want to be extra-safe on your first run?** Relaunch with `codex --sandbox read-only` before starting the tour. The agent can look at your files and propose actions, but can't change anything. Switch to `workspace-write` (the default) once you're comfortable.

> If the tour doesn't auto-trigger from those phrases, force-invoke it by typing `$new-codexer` at the start of your message — that explicitly tells Codex to load this skill.

---

## 10. "Wait, where am I?"

Reference for surface confusion. Newcomers get tangled up between the different places OpenAI's products live:

| If you're here… | What you can do | Does this tour work here? |
|---|---|---|
| **Codex CLI — terminal** (`codex` command) | Chat, read your files, edit files, run commands. Slash commands work. | Yes |
| **Codex CLI — IDE extension (VS Code, JetBrains)** | Same as terminal, embedded in your editor. | Yes |
| **chatgpt.com in a web browser** | Chat only. No file access. No slash commands. | No — different product |
| **ChatGPT iPhone/Android app** | Chat only. | No |
| **OpenAI Playground (platform.openai.com)** | API-style prompt testing. Different surface entirely. | No |

**The decisive test:** type `/help`. If you see a Codex-style menu, you're in the Codex CLI. If `/help` does nothing or just shows up as text, you're somewhere else.

---

## 11. What you'll learn

By the end of the tour you'll understand — *in plain English*:

- **Sessions** — your conversation; what's remembered between turns and what gets summarized
- **Tools** — the verbs Codex can use on your machine (read, write, run, search)
- **Approvals + sandbox modes** — the approve-before-acting system, plus the three sandbox levels (`read-only`, `workspace-write`, `danger-full-access`)
- **Folders & projects** — Codex works on one folder at a time; opening another folder is how you switch projects
- **Slash commands** — typed shortcuts like `/help`, `/approvals`, `/model`, `/plugins`, `/clear`
- **Skills** — packaged know-how that activates automatically based on what you ask (like this tour)
- **MCP servers** — external systems Codex can plug into (GitHub, Notion, your SIEM, your ticketing system) — each one is its own trust decision, the same way every plugin is
- **Subagents** — focused helpers for big jobs that would clog the main chat
- **`AGENTS.md`** — a file in your project that teaches Codex its conventions and gotchas

You'll learn the names *while you use them*, not as a lecture upfront.

---

## 12. What you do NOT need to learn first

Things people *think* they need before starting — they don't:

- The terminal / command line in depth (you just need to be able to launch `codex`)
- Git or GitHub
- Python or any programming language
- Docker
- Regular expressions
- "AI prompt engineering"

Pick those up later, on demand, when a real task forces you into them. Day one, you just need to type and read — *and* read the proposed commands before you approve them (see section 4).

---

## 13. Helpful links for beginners

You don't need any of this for the tour. Bookmark this section for the *day after*, when you start wondering "what's next?"

**Official Codex CLI:**
- [github.com/openai/codex](https://github.com/openai/codex) — the official Codex CLI repo and docs
- [OpenAI Trust Portal](https://trust.openai.com/) — data handling, retention, compliance answers

**A nicer way to read `.md` files** (the tour will save Markdown files to your Documents folder):
- [Obsidian](https://obsidian.md/) — free, most popular Markdown reader
- [VS Code](https://code.visualstudio.com/) — also free, with a built-in Markdown preview (Ctrl/Cmd + Shift + V)

**When you're ready for GitHub** (no rush — needed for the day you want to publish or fork things):
- [github.com/join](https://github.com/join) — free, two minutes
- [GitHub Hello World](https://docs.github.com/en/get-started/start-your-journey/hello-world) — their official first-repo walkthrough

**When you're ready for the terminal in depth** (no rush):
- [The Missing Semester of Your CS Education](https://missing.csail.mit.edu/) — free MIT class on the terminal, shell, and version control. Best resource on the internet for this.

**Free security learning that pairs well with the Codex CLI:**
- [TryHackMe](https://tryhackme.com/) — hands-on cybersecurity learning; pair it with Codex CLI for writing up what you solved
- [Microsoft Learn — Security paths](https://learn.microsoft.com/en-us/training/browse/?roles=security-engineer) — free, well-structured

**Stuck on anything else?** Just ask Codex directly: *"I'm stuck on X — explain like I'm new."* Don't grind silently.

---

## Troubleshooting

**"`codex plugin marketplace add` says it doesn't know that command."**
→ You're on an older version of the Codex CLI. Update it: `brew upgrade --cask codex` (macOS) or `npm update -g @openai/codex`, then try again.

**"I typed `/plugins` and nothing happened."**
→ Check that you're in the Codex CLI, not chatgpt.com in a browser. Use the `/help` test in [section 10](#10-wait-where-am-i).

**"`codex` says command not found."**
→ The install didn't put `codex` on your `$PATH`, or the terminal window predates the install. Open a new terminal and try again. If it still fails, re-run the install step from [section 7](#7-install-the-codex-cli-on-your-computer).

**"It installed but the tour won't start."**
→ Try one of the trigger phrases in [section 9](#9-start-the-tour) verbatim — exact wording like *"I'm new to Codex CLI, walk me through it"* works best. If still nothing, prefix your message with `$new-codexer` to force-invoke it.

**"The installer asked for admin rights and I can't proceed."**
→ Your laptop is managed by IT. Go back to [section 2](#2-before-you-install) and check policy. Don't enter someone else's credentials.

**"Codex won't let me edit a file."**
→ You're in `read-only` sandbox. Type `/approvals` and switch to `workspace-write`, or relaunch with `codex --sandbox workspace-write`.

**"Some other weird error I can't decode."**
→ Copy the message, paste it into the Codex CLI session, ask *"what does this mean and how do I fix it?"* Codex is good at this. Genuinely.

**"How do I uninstall the tour?"**
→ Open `/plugins` inside Codex, highlight new-codexer, and choose Uninstall. (Or edit `~/.codex/config.toml` and set `enabled = false` under the new-codexer plugin block.) Nothing else stays on your machine.

**"Something is broken and the above didn't help."**
→ [Open a GitHub issue](https://github.com/botz-pillar/NewCodexer/issues) (need a free GitHub account — see [section 13](#13-helpful-links-for-beginners) if you don't have one yet), or **email josh@pillarsecurity.io** — you don't need GitHub.

---

## Privacy

- **This plugin makes no network calls of its own.** It's a folder of markdown instructions that Codex reads when you start the tour. No analytics, no telemetry, no third-party services. The source at [github.com/botz-pillar/NewCodexer](https://github.com/botz-pillar/NewCodexer) is plain markdown — every file is readable before you install.
- **Your conversation with Codex is governed by OpenAI's terms.** The plugin doesn't change that — see the [OpenAI Trust Portal](https://trust.openai.com/) for data handling, retention, and zero-retention enterprise options.
- **The tour writes one note at the end** to `~/Documents/new-codexer-session-<date>.md` so the next time you come back, you can paste it in and Codex picks up where you left off. Here's exactly what gets written, structurally:

```markdown
# NewCodexer — session note (YYYY-MM-DD)

- **Role:** <SOC / GRC / IT / pentest / helpdesk-to-security / dev / just trying it>
- **Terminal comfort:** <never opened it / sometimes / lives in it>
- **First artifact produced:** <file path + one-line description>
- **Next step picked:** <one-line summary>

Greet me warm next time; reference what I built; offer to keep going on the next step.
```

No conversation content. Delete the file any time. The tour will offer to add the same content to your project's `AGENTS.md` if you'd rather Codex pick it up automatically next time you launch from that folder.

---

## Feedback & contact

- **Found a bug or have an idea?** [Open a GitHub issue](https://github.com/botz-pillar/NewCodexer/issues).
- **Never used GitHub before?** Email **josh@pillarsecurity.io** — happy to hear it. I read every message and try to reply within a week. If something is broken for you, you're not the only one — write me and I'll fix it.
- **Want to share what you built during the tour?** Same email. I love seeing first-day artifacts.

---

## License

MIT — see [LICENSE](./LICENSE). Use it, fork it, share it, remix it for your org.

## Who made this

[Josh Botz](https://github.com/botz-pillar) — cloud security practitioner, builder of [AI Cloud Security Lab](https://www.skool.com/cloud-security-lab), occasional writer of skills like this one. [NewClauder](https://github.com/botz-pillar/NewClauder) is the Claude Code sibling of this tour.

If this helped you, the kindest thing you can do is tell one other person in IT or security who's struggling to get started with AI tools.
