# Concept reference — plain explanations

Deeper write-ups of each concept named in `SKILL.md`. Read the one you need before teaching it. Each entry is written to be readable aloud in under a minute, with an IT/InfoSec-flavored example so the audience sees themselves in it.

If a term in here is itself unfamiliar to the user, define it inline — never assume.

---

## Sessions

A session is one conversation with the Codex CLI — this terminal session, right here. It starts when you run `codex` and ends when you exit (or close the terminal). Everything inside it — your messages, Codex's responses, files read, commands run — is the "context" of that session.

**Why it matters:** Context is finite. When it gets long, Codex either compresses older parts automatically or you start fresh (`/clear` or relaunch). A new session does not remember the previous one unless you wrote something down (a file, a memory entry, an `AGENTS.md`).

**What breaks without this:** Users get confused when "Codex forgot." It didn't forget — it's a new conversation, or older context was summarized to make room.

**InfoSec example:** A long IR session where you've been parsing logs for two hours. Eventually the chat compacts. If you want anything to survive, write it to a file: "Save what we've learned so far to `incident-notes.md`."

---

## Tools

Tools are the verbs Codex can perform. The built-in ones include:

- Read a file
- Write or edit a file
- Run a shell command (Bash/zsh on Mac/Linux, PowerShell on Windows)
- Search files (grep — find text in files; glob — find files by name pattern)
- Fetch a URL (pulls one specific page)
- Search the web (general search; may be gated behind a feature flag depending on your config)

Every tool call shows up in the terminal output. You see what Codex is about to do before it runs.

**Why it matters:** Codex without tools is a chatbot. Codex with tools changes files on your machine and runs real commands. That is the whole power — and the whole risk.

**The honest version of what tool execution means:** when you approve a Codex shell command, it runs with *your user's full permissions* on this machine — same as if you typed it yourself, but constrained by whatever sandbox mode is active. Approving `rm -rf` in `workspace-write` deletes files inside the project; approving the same thing in `danger-full-access` deletes anything you can reach. There's no magic sandbox between Codex and your filesystem beyond what you configured; the approval prompt *plus* the sandbox mode together *are* the safety layer. Read the proposed command before you say yes.

**InfoSec example:** "Run `netstat -an` and tell me which ports are listening" — Codex proposes the command, you approve, output comes back, Codex explains it. If instead it proposed something destructive based on misreading your intent, the approval gate is what catches it.

---

## Prompt injection (the new risk class)

A risk that's specific to AI agents and worth understanding before you let one near anything important.

**What it is:** Codex reads file contents and web pages as both *data* (information to think about) AND *instructions* (things to act on). A malicious file or webpage can include text that tries to redirect Codex into doing something you didn't ask for — install malware, exfiltrate data, run a destructive command, you name it. The technical name is *prompt injection*. It's the defining new threat class for agentic AI tools in 2026.

**The tell:** if Codex proposes an action that doesn't match what you originally asked for *after it just read a file or visited a URL*, that's the signature of an injection attempt. Stop and check before approving.

**Mitigations:**
- Use **read-only sandbox** (`codex --sandbox read-only` or `/approvals`) for first sessions and any session pointed at unfamiliar data — Codex can read and propose but can't act.
- Read every command before approving. Especially when Codex has just consumed untrusted input.
- Don't auto-approve everything just to move faster. `--ask-for-approval never` is a footgun on a real machine.
- For high-stakes work (production systems, sensitive data), don't run the Codex CLI there at all.

**InfoSec framing:** This maps to OWASP LLM01 (Prompt Injection) and OWASP Agentic AI Threats T1/T6 (indirect prompt injection via tool surfaces). It's not theoretical — it's been demonstrated repeatedly against production agentic systems.

---

## Approvals and sandbox modes

Codex asks before anything risky, *and* the sandbox mode caps what it could do even if you approved. Two layers — both matter.

**Sandbox modes** (set the default with `codex --sandbox <mode>` or in `~/.codex/config.toml`):

- **`read-only`** — Codex can read files in the project folder and run read-only commands. It cannot edit anything or run state-changing commands. Recommended for your first session and any session pointed at unfamiliar data.
- **`workspace-write`** — Codex can read and write files inside the project folder, and run commands. Network access defaults off; turn it on per-project with a `[sandbox_workspace_write]` block in `config.toml`. This is the daily-driver default.
- **`danger-full-access`** — No sandbox. Codex can do anything your user can. Only ever use this in a throwaway environment (a fresh VM you can wipe). Never on a real work machine. Removes the prompt-injection mitigation entirely.

**Approval policy** (orthogonal to the sandbox — controls *when* Codex pauses to ask):

- **`on-request`** — Codex asks before each risky action. Use this until you have a feel for the tool.
- **`untrusted`** — Codex auto-runs commands it considers trusted/low-risk, asks for the rest. (The config-file value is the bare word `untrusted`, even though people informally call this "unless-trusted".)
- **`on-failure`** — Codex auto-runs, but pauses to ask if a command fails.
- **`never` (`--ask-for-approval never`)** — Codex never asks; only ever pair this with `--sandbox read-only` if you must use it at all. Pairing `never` with `workspace-write` or `danger-full-access` is how people accidentally destroy their work.

Set or change at runtime with `/approvals`. Permanent defaults live in `~/.codex/config.toml`.

**InfoSec example:** Working in a folder of real production logs? Launch with `codex --sandbox read-only`. Codex can read and analyze but can't accidentally modify or delete anything. Switch out only when you're ready to act.

**Read-only is not a sandbox in the kernel sense.** It's a review gate enforced by the agent. It stops Codex from acting, but if you switch out of read-only and approve a bad plan, the bad plan runs. The control is the human reading the plan before approving — *you*.

---

## Folders and projects

The Codex CLI thinks about ONE folder at a time. That folder — and everything inside it — is what Codex can see and touch.

**To switch projects:** exit Codex, `cd` to the other folder, run `codex` again. Or pass `-C /path/to/folder` when launching.

There's no `cd` mid-session that flips Codex's whole world. One folder per session.

**Why it matters:** when you ask Codex to "read all files in this folder," it really means the folder you launched in. Files outside that folder are invisible to it unless you give an explicit absolute path. This is the safety boundary — Codex can't accidentally rummage through your whole disk.

**Pro tip:** put an `AGENTS.md` in each project folder. It teaches Codex about that specific project — conventions, gotchas, what not to touch. Codex reads it automatically at session start.

---

## Slash commands

Anything starting with `/` is a slash command — a shortcut that runs inside this Codex session.

Useful ones:
- `/help` — list everything
- `/clear` — wipe the conversation and start fresh
- `/approvals` — change approval policy and sandbox mode (including read-only)
- `/model` — switch which OpenAI model is doing the work
- `/login` — sign in when prompted
- `/exit` — quit

Slash commands are NOT shell commands. `/ls` does not list files. To run a shell command, just ask Codex ("list the files in this folder") and it'll propose `ls` for you to approve.

Slash commands work in the Codex CLI (terminal, IDE extension). They do **not** work in ChatGPT (the browser chat at chatgpt.com).

---

## Skills

A skill is a folder of instructions (and sometimes scripts) that teaches Codex how to handle a specific kind of task well. Every skill has a `SKILL.md` with a `name` and a `description`.

**How they trigger:** When you start a session, Codex reads all skill descriptions. When your prompt matches what a skill says it covers, Codex loads the full skill and follows it. You usually don't call skills by name — they fire from intent. If you want to force-trigger one, prefix your message with `$<skill-name>`.

**Why they matter:** Specialization without bloat. A "phishing-triage" skill exists dormant until you paste an email header. A "soc2-evidence" skill stays out of the way until you mention an audit. You can install many; the right one shows up when needed.

**InfoSec example:** A "log-triage" skill that knows your SIEM's quirks, your IR runbook format, and your ticket template — fires whenever you start an investigation, stays silent otherwise.

**Trust note:** skills can include scripts that execute on your machine. Read every skill before installing it — the SKILL.md and any scripts under `scripts/`. Codex Tour is just markdown, no scripts; not every skill is.

---

## Subagents (agents)

A subagent is a focused Codex instance that gets spawned inside your session to handle one job, with its own conversation. Examples: a code-reviewer agent, a research agent, a debug agent.

**Why they matter:** Some jobs would otherwise eat the whole context (read 50 files, run 20 searches in parallel, scan a giant repo). A subagent does that work in isolation and reports back with a summary. Your main chat stays clean.

**When to use:** Long research, parallel independent tasks, anything where the raw output is huge but the answer is small.

**InfoSec example:** "Spawn an agent that scans this folder of YARA rules for ones that overlap and report the duplicates." You get the report; the noise stays in the subagent.

---

## MCP servers

MCP (Model Context Protocol) is how the Codex CLI plugs into outside systems. An MCP server is a small program that exposes a set of tools to Codex — Notion, GitHub, Gmail, a database, a SIEM, a ticketing system.

Configure them in `~/.codex/config.toml`. Once configured, those tools show up alongside the built-in ones. Codex can `github-create-issue` the same way it can read a file.

**Why they matter:** This is how the Codex CLI stops being just "a CLI tool on your laptop" and becomes connected to your real workflow.

**InfoSec examples:**
- GitHub MCP → read repos, search code, open issues, manage PRs
- Splunk / Elastic MCP → run searches from the terminal
- Notion / Confluence MCP → pull runbooks into context
- Jira / ServiceNow MCP → file and update tickets

**Each MCP server is its own trust decision.** Same way every plugin is. They add tool surface area to your Codex session — meaning more things prompt injection can try to hijack. Pick servers from trustworthy sources, audit their permissions, and don't connect everything just because you can.

---

## Memory and `AGENTS.md`

Two related but distinct things:

- **`AGENTS.md`** — a file in a project directory that teaches Codex about *that project*. Conventions, gotchas, what not to touch, where things live. Loaded automatically when a session starts in that folder. (This is the Codex equivalent of Claude Code's `CLAUDE.md`.) A global `~/.codex/AGENTS.md` also works for personal preferences that apply across all your projects.
- **Persistent context** — Codex does not have a Claude-Code-style `memory/` directory. The way you "remember" things across sessions is by writing to an `AGENTS.md` (project-local or global) or by saving notes in your own filesystem and pasting them back in. Plain markdown, you own it, you can read it.

**Why they matter:** Without these, every session starts from zero. With them, Codex shows up already knowing the project and the person.

**InfoSec example:** An `AGENTS.md` in your IR notebook folder that says: "This folder contains incident data. Default to read-only sandbox. Never `rm` anything. All findings get appended to `findings.md`, not overwritten." That preamble travels with the folder.

**Codex Tour writes one session note at the end of the tour** — your role, your terminal comfort, the first artifact you produced, and your chosen next step. It saves to `~/Documents/codex-tour-session-<date>.md` so you can paste it back in the next time you launch Codex (or drop it into your project's `AGENTS.md`). No conversation content. Delete the file any time.

---

## Configuration: `~/.codex/config.toml`

The single config file that controls Codex CLI defaults. Worth knowing it exists so you can find it later. Common things people set:

```toml
approval_policy = "on-request"
sandbox_mode = "workspace-write"

[sandbox_workspace_write]
network_access = false
```

You don't usually edit this on day one — defaults are fine. But when you want to make read-only the default for a project, or wire up an MCP server permanently, this is where it lives.

---

## Bonus: "where does my data go?"

The most common silent question from an InfoSec audience. Brief answer:

- Your messages and the files Codex reads go to OpenAI for the model to process.
- The terms depend on how you signed in: personal ChatGPT account → ChatGPT terms; OpenAI API key → API terms with the Business data policy; ChatGPT Business / Enterprise → your org's DPA. By default, **API and ChatGPT Business / Enterprise traffic is not used to train OpenAI's models.** (Verify on the [OpenAI Trust Portal](https://trust.openai.com/) — it's the source of truth.)
- Treat this like any cloud tool: do not paste secrets, credentials, customer data, PHI, or classified material without first checking your org's policy and any data-processing agreement.
- For high-sensitivity work, ask your security team whether your org has an enterprise agreement with zero-retention guarantees.

When in doubt, redact before pasting.
