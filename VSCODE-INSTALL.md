# NewCodexer — VS Code install path (no terminal required)

**For people whose work computer blocks the terminal, but allows VS Code extensions.**

This is the honest answer to: *"My IT team locked down PowerShell / Terminal.app / WSL, but I can still install VS Code extensions. Can I use Codex (and this tour) anyway?"*

Short version: **probably yes, via the official OpenAI Codex IDE extension** — but you need to understand what it actually does on your machine before you install it, and you may still hit your company's controls in a different spot. Read the whole page before you click anything.

---

## TL;DR

1. Install the **Codex** extension from the VS Code marketplace (publisher: OpenAI).
2. Open the Codex side panel, sign in with your ChatGPT account (Plus / Pro / Business / Edu / Enterprise — see [README §5](./README.md#5-cost-codex-cli-requires-a-paid-plan-from-day-one)) or an API key.
3. Open a project folder in VS Code (`File → Open Folder…`).
4. In the Codex chat panel, paste:

   > *I'm new to Codex, walk me through it. Start with the NewCodexer tour.*

5. If the tour doesn't auto-load, see [§4 below](#4-installing-the-newcodexer-tour-without-a-terminal) for the no-terminal install path for this plugin specifically.

That's the whole flow. The rest of this page is the "why" and the "before you click" — which matters more than the steps on a managed machine.

---

## 1. Is this actually for you?

Use this page if **all** of these are true:

- [ ] Your work computer blocks Terminal.app / PowerShell / WSL — or your IT policy says "no shells."
- [ ] You **can** install VS Code extensions (either freely, or via your company's allowed-extension list).
- [ ] You have a paid ChatGPT plan or OpenAI API key (Codex isn't free — same as the CLI; see [README §5](./README.md#5-cost-codex-cli-requires-a-paid-plan-from-day-one)).
- [ ] You've read [README §2 — Before you install](./README.md#2-before-you-install) and either gotten permission or confirmed your company has approved OpenAI Codex.

If your terminal works fine, just use the [main README](./README.md). This page exists only because the terminal-blocked case is the single most common blocker I hear from InfoSec / GRC / helpdesk folks at locked-down shops.

---

## 2. Before you install — the part most "VS Code install" guides skip

**The Codex VS Code extension is not a lightweight chat client.** It is a coding agent that, by default, can read files in your open project folder and execute commands on your machine. Installing it from the VS Code marketplace does not change that — it is the same agent the CLI uses, just driven from a side panel instead of a terminal prompt.

What this means concretely:

- **It will read source code** in whatever folder you open in VS Code. If you open a repo that contains customer data, secrets, or anything regulated, Codex can see it.
- **It runs commands.** "Agent mode" is the default — Codex can execute shell commands inside VS Code's integrated process. If your company blocked your terminal because of DLP / EDR policy, this extension may trip the same policy. Sometimes the block is at the terminal *application* level (Codex routes around it); sometimes the block is at the *kernel / process* level (Codex hits the same wall).
- **It calls OpenAI.** Every prompt, every file Codex chooses to include as context, leaves your machine for OpenAI's servers. Same as the CLI.
- **MDM / EDR will see it.** Your IT team's tooling will log the extension install and likely the network calls. This is normal and fine — but if you didn't ask first, expect a conversation.

**The honest call:** if your terminal was blocked because IT/Security said *"no agentic AI tools on this machine,"* then installing the VS Code extension to get around it is the wrong move. Go ask first. If your terminal was blocked as a generic "lock down shells for non-developer roles" policy and AI tools are separately approved, you're probably fine — but still ask.

> **Quick check before you continue:** does your company have a vendor-approval entry for OpenAI Codex? If yes, you're cleared. If no or unclear, stop here and email security. A 20-minute approval is cheaper than an incident report.

---

## 3. Installing the Codex VS Code extension

Assuming you got the green light from §2:

### Step 3a — Install the extension

In VS Code:

1. Open the Extensions side panel (`Ctrl+Shift+X` on Windows/Linux, `Cmd+Shift+X` on macOS — or click the squares icon in the activity bar).
2. Search for **Codex** (publisher: **OpenAI**).
3. Click **Install**.

The extension is also available in Cursor, Windsurf, and VS Code Insiders. Same install path.

### Step 3b — Sign in

1. Open the Codex side panel (look for the Codex icon in the activity bar after install — restart VS Code if it doesn't appear).
2. Click **Sign in with ChatGPT** (recommended if you have a paid ChatGPT plan) or **Use API key** (if your company gave you an OpenAI API key).
3. The sign-in flow opens a browser tab. Approve, return to VS Code.

### Step 3c — Open a project folder

`File → Open Folder…` and pick any folder you want Codex to work in. Codex's view of your machine is scoped to whatever folder you have open — so for your first session, pick a throwaway folder (e.g. `Documents/codex-sandbox`), not your work repo.

### Step 3d — Sanity check

In the Codex chat panel, type:

> *Say hello and tell me one thing you can do.*

If you get a reply, the extension is working. If you don't, jump to [§6 troubleshooting](#6-troubleshooting).

---

## 4. Installing the NewCodexer tour without a terminal

The tour is a Codex **plugin** (a skill / slash-command bundle). On the CLI you'd install it with `codex plugin marketplace add joshbotz/NewCodexer` + `/plugins` — but neither of those steps requires a terminal *if* the Codex VS Code extension exposes the same commands. Two paths, try them in order:

### Path A — Use slash commands from inside the Codex panel (try this first)

In the Codex chat panel, type:

```
/plugins
```

If a plugin browser opens, search for **new-codexer**, install it, and skip to [§5](#5-start-the-tour). Done.

If `/plugins` does nothing, try:

```
/help
```

…and look for any "plugin" / "marketplace" / "extension" command the panel exposes. The VS Code extension's command set evolves; whatever it calls plugin install, use that.

### Path B — Just paste the tour trigger and let Codex find it

If your version of the extension doesn't have a plugin browser yet, the tour still works as a prompt — the skill auto-loads when Codex sees a tour-style request. In the Codex panel, paste:

> *I'm new to Codex CLI, walk me through it. Use the NewCodexer tour from github.com/joshbotz/NewCodexer.*

If Codex doesn't have the tour skill installed, it will fall back to a generic onboarding chat — still useful, but not the curated NewCodexer flow. To get the real tour without a terminal, see Path C.

### Path C — Drop the skill files into your project (advanced, no terminal)

This is the "bring your own plugin" workaround when the extension can't install plugins itself and you have no terminal.

1. In a browser, go to [github.com/joshbotz/NewCodexer](https://github.com/joshbotz/NewCodexer) and click the green **Code → Download ZIP** button.
2. Unzip it anywhere — say `Documents/NewCodexer-main/`.
3. In VS Code, open that folder (`File → Open Folder…`).
4. In the Codex chat panel, paste:

   > *The folder you're in contains the NewCodexer tour skill at `skills/new-codexer/`. Load that skill and run the tour for me.*

   Codex will read the skill files directly from the open folder and run the tour against them. This is not a "permanent install" — it works only while that folder is open — but for a first session it's enough to get the full guided experience.

If even ZIP downloads from GitHub are blocked at your company, see the [OFFLINE-INSTALL.md Drive-share path](./OFFLINE-INSTALL.md) — you can ask your trainer to send you the same folder over an approved channel and use it the same way.

---

## 5. Start the tour

In the Codex panel, type **any one** of these:

- *I'm new to Codex, walk me through it*
- *I'm a helpdesk guy trying to break into security — help me get started with Codex*
- *I work in compliance and my boss told me to look at Codex. Is this for me?*

The tour asks two questions (your day job, your terminal comfort), gives you the safety story, then walks you through **one real thing** chosen for your role. By the end you'll have a file you made and a starter-prompts cheat sheet saved to your `Documents` folder.

> **Magic phrase:** type *"slow down and explain like I'm new"* any time you're overwhelmed.

> **Extra-safe first run:** before starting the tour, type *"switch to read-only mode for this session"* — Codex can look at files and propose actions, but can't change anything. Flip back later when you're comfortable.

---

## 6. Troubleshooting

**"The Codex panel doesn't appear after install."**
Restart VS Code fully (quit and reopen — `Cmd+Q` on macOS, not just close the window). On Windows, if the panel loads but is unresponsive, install **Visual Studio Build Tools (C++ workload)** and the **Microsoft Visual C++ Redistributable (x64)**, then restart VS Code. This is a known dependency on Windows.

**"Sign in opens the browser but nothing comes back to VS Code."**
Your company's SSO or browser policy may be eating the callback. Try the API key sign-in path instead (you can get a key at [platform.openai.com/api-keys](https://platform.openai.com/api-keys) on a paid account).

**"Commands fail with permission errors when Codex tries to run them."**
This is your company's EDR / DLP policy blocking the underlying process — same root cause as your blocked terminal. The extension is not a workaround for kernel-level execution controls. Talk to security; you need an exception, not a different install path.

**"I'm on a Mac and `code .` doesn't work."**
Irrelevant for this guide — you're not using the terminal. Just use `File → Open Folder…` from inside VS Code.

**"I'm on Windows and the extension wants WSL."**
You have two choices: install WSL2 (which requires the terminal you don't have — dead end) or use the extension's native-Windows sandbox mode if your version supports it. Check the Codex panel's settings for a "sandbox" or "execution mode" option. If only WSL is offered, you're stuck on this machine and need IT to either unblock WSL or approve a different path.

**Anything else.**
Copy the exact error, paste it into the Codex chat panel with *"what does this mean and what should I try?"* — Codex is good at decoding its own errors. If it can't help, open an issue at [github.com/joshbotz/NewCodexer/issues](https://github.com/joshbotz/NewCodexer/issues) with the error text and your VS Code + extension versions.

---

## 7. What this page is *not*

- **Not a way to evade IT policy.** If your company hasn't approved Codex, none of this is for you. Ask first.
- **Not a permanent substitute for the CLI.** The terminal CLI gets new features first, has a richer plugin ecosystem, and is the supported path for serious work. This page exists so a blocked-terminal coworker can get a first useful session without waiting six months for IT to unblock the shell.
- **Not Claude.** This repo is for the OpenAI **Codex** CLI / IDE extension. The Claude Code sibling project is [NewClauder](https://github.com/joshbotz/NewClauder) — different tool, different install path. If you actually meant Claude, you want NewClauder.

---

## See also

- [README.md](./README.md) — main install path (terminal-based)
- [OFFLINE-INSTALL.md](./OFFLINE-INSTALL.md) — for machines with no GitHub access
- [TRAINING.md](./TRAINING.md) — 1-hour runbook for training a team
