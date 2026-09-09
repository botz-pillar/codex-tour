# NewCodexer — 1-hour team training plan

A trainer's runbook for walking a small InfoSec team (5–15 people) from "what's a Codex CLI?" to "I just produced my first real artifact with an AI agent" in about 60 minutes.

Format: live screen-share, plain-English, audience asks questions throughout, no slide-deck slop. Voice mirrors the README — define jargon inline, lead with safety, concrete > abstract.

This is a **trainer's outline**, not a deck. Read it through once before you run the session; bring it open in a second window during the demo.

---

## Pre-flight (send the day before)

A short pre-read email to attendees so they show up with the install already done:

> **Subject:** Tomorrow's Codex CLI session — 10 min of prep
>
> Hey team — for tomorrow's session we'll go hands-on with the OpenAI Codex CLI. Two things to do tonight or first thing tomorrow morning, takes about 10 minutes total:
>
> 1. **Install Codex CLI** on your laptop. Walkthroughs by OS are in the README at https://github.com/joshbotz/NewCodexer#7-install-the-codex-cli-on-your-computer.
>     - macOS: `brew install --cask codex`
>     - Linux: `npm install -g @openai/codex`
>     - **Windows: WSL2 is required** — run `wsl --install` from an admin PowerShell, reboot, then install inside Ubuntu. Side-quest is about 15 minutes including the reboot. If this is new to you, do it tonight, not in the session.
> 2. **Sign in.** Run `codex` and pick the **ChatGPT** sign-in option. You'll need a paid ChatGPT plan (Plus is $20/mo, Pro is $200/mo). If our org has an Enterprise seat assigned to you, use that. If you don't have a plan, we'll cover next steps in the session.
>
> Don't install the NewCodexer plugin yet — we'll do that together tomorrow.
>
> Bring: laptop, charger, a synthetic phishing email or a sample log file from your queue (anything non-sensitive — we'll use it as practice data).
>
> Questions: <your contact>

Adjust org-specific bits (Enterprise seat, sample-data prompt) before sending.

---

## Session shape (60 min)

| Block | Time | What happens |
|---|---|---|
| Open | 5 min | Why we're here. What this is and isn't. |
| Concept anchor | 10 min | The mental model + the safety story |
| Install the tour | 5 min | Everyone gets NewCodexer running |
| Live demo | 15 min | Trainer runs the tour on themselves, narrated |
| Hands-on | 15 min | Each person runs the tour on themselves, role-matched |
| Q&A + close | 10 min | Pitfalls, what's next, where to ask for help |

If you only have 45 minutes, drop the live demo block (it's the most cuttable — the README walks the same flow). If you have 90 minutes, double the hands-on block.

---

## Block 1 — Open (5 min)

Goal: lower the stakes, frame what's about to happen.

Beats to hit:

1. **"This is the start of a new kind of tool — not a replacement for what you do."** AI agents are not magic and they're not your replacement; they're a force multiplier when used carefully. The skill is in *how* you use them.
2. **"We're going to install one piece of software and run one onboarding tour together."** By the end of the hour you'll have a real artifact (a triaged email, a log analysis, a runbook draft — your choice) saved to your laptop.
3. **"The Codex CLI is OpenAI's agent that lives in your terminal."** Different from ChatGPT in your browser. Same account, same subscription, but a much more powerful surface — it can read files and run commands with your approval.
4. **"NewCodexer is the onboarding plugin we built to make day-one not terrible."** It asks you two questions, gives you the safety story, then walks you through one real task chosen for your role.
5. **Ground rule.** "I'd rather we get stuck together and figure it out than have everyone silently fall behind. Interrupt me."

Don't pitch features. Don't show slides. Open your terminal and they'll know what's coming.

---

## Block 2 — Concept anchor (10 min)

Goal: install three mental models *before* anything happens on their machines.

### The mental model (3 min)

> "Codex CLI is GPT with hands. You chat in your terminal, and Codex can also read your files, run commands, and edit code — only with your permission. Every action shows up in the terminal so you can see what it's doing before you say yes."

Walk through the four core concepts on a whiteboard or in a text editor everyone can see — one sentence each, no slides:

- **Session** — one conversation. Everything inside it gets remembered until you close the terminal.
- **Tools** — the verbs Codex uses: read a file, edit a file, run a shell command, search the web.
- **Approvals + sandbox** — you approve every action, and the sandbox mode caps what Codex *could* do even if you approved.
- **The folder I'm in** — Codex thinks about one folder at a time. The files in that folder are what it can see.

### The safety story (5 min — do not skip)

This is the part that matters for an InfoSec audience. Three beats:

1. **Where your data goes.** "Your messages and the files Codex reads go to OpenAI. The terms depend on how you signed in — personal ChatGPT account is ChatGPT terms; API key is API terms with the Business data policy; ChatGPT Business/Enterprise is your org's DPA. By default, API and Business/Enterprise traffic isn't used to train models. Don't paste secrets, client data, PHI, or anything classified without checking org policy first. The [OpenAI Trust Portal](https://trust.openai.com/) has the authoritative version."
2. **Prompt injection is the new threat class.** "Codex reads files and web pages as both data AND instructions. A malicious file or webpage can try to redirect the agent into doing something you didn't ask for. The tell: if Codex proposes an action that doesn't match your original ask *right after* it just read a file or visited a URL, that's the signature. Stop and check before approving."
3. **Read-only mode exists.** "Launch with `codex --sandbox read-only` for any session pointed at unfamiliar data. Codex can look and propose, but can't change anything. Recommended for everyone's first run today."

### What you do NOT need to know yet (2 min)

Liberate them up front:

> "You don't need to know the terminal cold. You don't need git. You don't need GitHub. You don't need Python or Docker or regex. You need to type, read, and look at proposed commands before approving them. Everything else we learn by doing."

---

## Block 3 — Install the tour (5 min)

Goal: everyone has NewCodexer installed locally.

Pick the install path **before** the session and tell the team which one you're using:

### Path A — Public GitHub (after the repo is public)

Live-walk this on screen, they follow along:

```bash
# 1. exit Codex if you're in it
/exit

# 2. add the marketplace
codex plugin marketplace add joshbotz/NewCodexer

# 3. launch Codex, install via TUI
codex
/plugins   # → find new-codexer, install
```

### Path B — Drive zip (internal demo)

Share the link in chat. They click, download, unzip to `~/Documents/NewCodexer`. Then:

```bash
codex plugin marketplace add file:///Users/<username>/Documents/NewCodexer
codex
/plugins   # → find new-codexer, install
```

(Reference: [OFFLINE-INSTALL.md](./OFFLINE-INSTALL.md))

**Common failures to expect** (have these ready):

- `command not found: codex` → they opened the terminal before installing. New terminal, retry.
- `marketplace add` errors with auth → either `gh auth login` (Path A only) or fall back to SSH URL form
- `/plugins` shows nothing → exit Codex, relaunch, retry. Marketplace cache refresh.

Don't move on until every laptop in the room has the plugin installed.

---

## Block 4 — Live demo (15 min)

Goal: model the tour end-to-end so they can see the shape before they try it themselves.

Pick one role to demo — **SOC phishing triage** is the safest universal demo because every InfoSec team has someone who's done it, and it shows off the full loop (paste data, propose actions, approve, get an artifact).

Have a synthetic phishing email ready in your clipboard before you start.

### Demo script

1. Launch Codex in a clean folder: `cd ~/Documents && mkdir codex-demo && cd codex-demo && codex --sandbox read-only`
2. Trigger the tour: type *"I'm new to Codex CLI, walk me through it — I'm a SOC analyst."*
3. As Codex asks the calibrating questions, answer honestly — "SOC", "I use the terminal sometimes." Narrate what's happening to the room: "*See how it's not lecturing me yet? It's calibrating.*"
4. When the safety story plays — pause and re-emphasize. "*Notice they're spending real time on data handling and prompt injection before we touch anything. That's by design.*"
5. When Codex picks the phishing-triage exercise, paste your synthetic email. Walk through what Codex proposes step by step. **Read every proposed command aloud before approving.** Model the behavior you want them to copy.
6. When the artifact file gets saved, open it on screen. "*This is what 'one concrete win' means. You have a file you made. You could send it to a colleague.*"
7. Let the tour close itself with the session-note + starter-prompts handoff.

What you're modeling, beyond the mechanics:

- **Reading proposed commands before approving.** Do it slowly. Make a point of it.
- **Asking "why?" when Codex does something unexpected.** Demonstrates the conversational nature.
- **Using `slow down and explain like I'm new`** when something gets dense — show that magic phrase exists.

---

## Block 5 — Hands-on (15 min)

Goal: each person runs the tour on themselves and produces an artifact.

Setup:

1. Everyone opens a fresh terminal, navigates to a non-sensitive folder (`cd ~/Documents && mkdir my-codex-test && cd my-codex-test`).
2. Everyone launches with read-only: `codex --sandbox read-only`.
3. Everyone triggers the tour with the phrase that matches their role:
    - SOC → *"I'm a SOC analyst trying Codex for the first time"*
    - GRC → *"I work in compliance and want to know what Codex can do for me"*
    - IT generalist → *"I'm an IT generalist, walk me through Codex"*
    - Pentest → *"I'm a pentester, what should I try first with Codex?"*
    - Helpdesk → *"I'm in helpdesk trying to break into security, help me get started"*

While they're working: circulate. Don't hover. Answer questions, fix the install issues you didn't catch in Block 3, and **resist solving problems for them** — model "let me show you how to ask Codex itself" instead.

**What to look for:**

- People who got stuck on a trigger phrase and didn't try `$new-codexer` — surface that escape hatch publicly
- Anyone whose tour skipped the safety story (could be a Codex bug, could be a wording mismatch with the description) — note for v1.0.2
- Anyone who isn't producing an artifact in the first 8 minutes — go check on them quietly, they probably hit something
- People who finish early — point them at the starter-prompts file Codex just saved them and let them try one

---

## Block 6 — Q&A and close (10 min)

Goal: land everyone with a clear "what's next."

Standard close beats:

1. **Recap the loop.** "You ask → Codex proposes → you approve → Codex acts → you check → iterate. That's the whole game. Everything else is variations on it."
2. **Name the concepts they used.** "Today you used sessions, tools, approvals, the sandbox, and a skill firing automatically. That's most of what's worth learning about Codex CLI."
3. **What's next for them tomorrow:** "Codex just saved a starter-prompts file in your Documents folder. Pick one this week and run with it. Bring back what you made — I want to see your first real artifact."
4. **Where to ask for help.** Slack channel, email, or office hours — wherever you take questions.
5. **Honest pitch on continued use.** "This isn't a magic productivity tool. The people who get value out of it are the ones who use it three or four times a week for a month, find their groove, and start writing little skills of their own. Give it that runway before you decide if it's worth keeping."

**Common Q&A you should be ready for:**

- *"Can I use this for client work?"* → Check org policy first. The default answer is "use synthetic data while you're learning; once you're comfortable and policy clears it, the path is ChatGPT Enterprise with zero-retention guarantees."
- *"Is this going to replace my job?"* → No. It's going to change what your job looks like. The people who learn how to use these tools well will be the ones their teams keep.
- *"What if it does something I didn't approve?"* → It won't, by design. Every action requires approval. The sandbox is the second layer. If you ever see something happen that you didn't say yes to, write it down and tell me — that's a real bug, not a feature.
- *"Should we be using this in production?"* → Not yet. Use it on your own machine for your own work for a few weeks. Production deployment is a separate conversation.
- *"What about <competitor tool>?"* → They're all variations on the same idea right now. Pick one, learn the patterns, the next one is much easier. Codex is the one we're standardizing on because <your reason>.

---

## After the session

Same day (you, 5 min):

- Drop a follow-up message in the team Slack with three links: the [README](./README.md), the [starter-prompts](./skills/new-codexer/references/starter-prompts.md), and your channel/email for help
- Make a note of any install issues, trigger-phrase misses, or unclear wording — patch into v1.0.2

One week later (you, 15 min):

- Skim the team Slack for what people built. Reach out to anyone who hasn't used it since the session
- If you saw 3+ people hit the same issue, that's a documentation fix worth shipping
