# "Where am I running this?" — surface-by-surface reference

Most newcomer confusion isn't about *what Codex does*, it's about *which OpenAI surface they're in*. Use this reference any time the user gets tangled up between the Codex CLI (this terminal, where NewCodexer lives), ChatGPT (the website and app), and the OpenAI Playground.

Read the section that matches what the user reports seeing.

---

## The decisive test

If the user can type `/help` and a Codex menu pops up — they're in the **Codex CLI**. Everything in this tour applies.

If `/help` does nothing (or just prints `/help` as text), they're in **ChatGPT** at chatgpt.com (or the mobile app). Different product. Slash commands don't work there, file edits don't work there, this tour doesn't apply there.

That one keystroke answers 90% of "where am I?" questions.

---

## The surfaces, plain English

### Codex CLI — terminal

A program you run by typing `codex` in a terminal window (the black-or-white window where you type commands). The current folder you're in becomes the project folder. **There is no desktop app — terminal is the only place this lives.**

- Slash commands: ✅
- Local file access: ✅ (the folder you launched from)
- Terminal commands: ✅ (Codex runs them after you approve)
- Sandbox modes: ✅ (read-only / workspace-write / danger-full-access)
- Setup: install via `brew install --cask codex` or `npm install -g @openai/codex`

Requires a ChatGPT Plus/Pro plan or OpenAI API credits.

### Codex CLI — inside your editor (VS Code, JetBrains, Cursor)

An extension that puts the Codex CLI into the editor you already use. Skip this until you've used it from a terminal once and want it closer to your code.

### ChatGPT (chatgpt.com)

The chat window in your web browser at `chatgpt.com`. This is normal ChatGPT — great for asking questions and getting written work — but it does **not** run on your computer, does **not** edit files on your disk, and does **not** support Codex slash commands or skills.

The free ChatGPT plan lives here. NewCodexer and the Codex CLI are not on the free plan.

If you opened this tour expecting it to work in chatgpt.com: it won't. You need the Codex CLI.

### ChatGPT in the iPhone or Android app

Same as chatgpt.com — a chat, no file access, no Codex slash commands. Useful for quick questions on the go. Not where NewCodexer lives.

### OpenAI Playground (platform.openai.com)

A developer-focused API testing surface. Different product entirely — for trying out model API calls and tuning parameters. NewCodexer doesn't apply here.

---

## "Why doesn't this slash command work?"

- You're in ChatGPT instead of the Codex CLI → switch to the Codex CLI (run `codex` in a terminal).
- You typed the command but didn't press Enter → press Enter.
- You're in the Codex CLI but the command isn't installed → type `/help` and see what's available. If the command you wanted isn't there, it's a plugin or skill you haven't installed yet.

---

## "Where do shell commands run?"

When Codex runs a shell command (like `ls`, `git status`, `python script.py`), it runs **on your computer**, in **the folder you launched `codex` from**, with **your user's permissions** capped by the active sandbox mode.

- Terminal → in the folder you launched `codex` from
- Editor extension → in the project root of the editor

If you want Codex to work in a different folder, exit this session, `cd` to the other folder, and run `codex` again. (Or launch with `codex -C /path/to/folder`.) There's no `cd` mid-session that flips Codex's whole world.

---

## "How do I work on different projects?"

One folder per session. To switch:

- Terminal: exit, `cd /path/to/other/project`, run `codex` again
- Editor: open the other project in your editor; Codex follows
- Either: launch with `codex -C /path/to/other/project`

Each project can have its own `AGENTS.md` — a little file in the folder that tells Codex about that specific project ("this is a Python codebase, never touch the `secrets/` directory, run tests with `make test`"). Newcomers don't need to write one on day one, but it's where the leverage shows up later.

---

## "How do I find the files Codex makes?"

Codex tells you the full path every time it writes a file. The path looks like:

- macOS: `/Users/yourname/Documents/something.md` or `~/Documents/something.md`
- Windows: `C:\Users\yourname\Documents\something.md` or `%USERPROFILE%\Documents\something.md`
- Linux: `/home/yourname/Documents/something.md` or `~/Documents/something.md`

To open it:

- **macOS:** in Terminal or in the Codex session, run `open /the/full/path` — it opens in your default editor.
- **Windows:** press `Win+E` (opens File Explorer), paste the path into the address bar, press Enter.
- **Linux:** `xdg-open /the/full/path`.

To read `.md` files comfortably (they're plain text but look prettier in a real reader): install one of these free apps:

- **Obsidian** — free, cross-platform, what most people use: [obsidian.md](https://obsidian.md/)
- **VS Code** — free, has a built-in Markdown preview (Ctrl/Cmd + Shift + V): [code.visualstudio.com](https://code.visualstudio.com/)

You can also drag any `.md` file into a browser tab and it'll display as plain text — readable, just not pretty.
