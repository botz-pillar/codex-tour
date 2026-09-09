# Changelog

## 1.0.1 — 2026-05-17

Rebrand + Windows accuracy pass. This version was tested with a small group before the public launch.

- **Renamed** from `codex-tour` to **NewCodexer** (skill slug `new-codexer`) to match the NewClauder naming family.
- **Windows support corrected.** Codex CLI requires **WSL2** on Windows — the previous README/install.md implied native PowerShell worked, which is false per OpenAI's official requirements. Updated §7a (terminal-opening), §7b (install path), and `references/install.md` Windows section to walk through `wsl --install` first, then install Node + Codex *inside* the Ubuntu/WSL2 shell.
- **File-path guidance for WSL2 users.** SKILL.md "open this file" snippet now uses `explorer.exe ~/path` (from WSL2) and includes the `\\wsl$\Ubuntu\home\<user>\…` path so users can find their files from Windows Explorer.
- **Concept fix:** the "tools" entry in `concepts.md` no longer claims Codex runs PowerShell on Windows — clarified that on Windows, Codex runs inside WSL2 so shell tool calls are bash.
- **OG URL fix** in `docs/index.html` (case-corrected to `/NewCodexer/`).
- No tour-flow content changed; this is a name + accuracy patch.

## 1.0.0 — 2026-05-17

Initial release (shipped as `codex-tour`; renamed in 1.0.1).

- Role-aware onboarding tour for IT/InfoSec users brand-new to the OpenAI Codex CLI.
- Single skill (`new-codexer`) with `SKILL.md` plus four references files: concepts, install, where-am-i, starter-prompts.
- README walks a beginner from "no Codex CLI installed" to "first real artifact saved to disk":
  - Section 2: work-laptop / sensitive-data pre-install check.
  - Section 4: honest version of what tool execution and prompt injection mean on an agentic CLI.
  - Section 5: cost honesty before signup (ChatGPT Plus / Pro or API credits).
  - Section 6–7: account + install via `brew install --cask codex` or `npm i -g @openai/codex`.
  - Section 8: read-it-before-you-install discipline as a standing rule for any third-party Codex plugin.
- Tour itself recommends `--sandbox read-only` mode (Codex's plan-mode equivalent) for the first session and any unfamiliar data.
- Session note at end of tour — role, terminal comfort, first artifact, next step. No conversation content. Saved to `~/Documents/new-codexer-session-<date>.md` (or optionally into the project's `AGENTS.md` so Codex auto-loads it next session).
- Six role-conditioned example banks: SOC, GRC, IT generalist, pentest, helpdesk/career-changer, dev, plus a "just trying it out" lane.
- Two rounds of `battle-test` review (3-persona iterate tier) ran before tagging v1.0.0.
