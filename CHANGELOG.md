# Changelog

## 1.0.0 — 2026-05-17

Initial public release.

- Role-aware onboarding tour for IT/InfoSec users brand-new to the OpenAI Codex CLI.
- Single skill (`codex-tour`) with `SKILL.md` plus four references files: concepts, install, where-am-i, starter-prompts.
- README walks a beginner from "no Codex CLI installed" to "first real artifact saved to disk":
  - Section 2: work-laptop / sensitive-data pre-install check.
  - Section 4: honest version of what tool execution and prompt injection mean on an agentic CLI.
  - Section 5: cost honesty before signup (ChatGPT Plus / Pro or API credits).
  - Section 6–7: account + install via `brew install --cask codex` or `npm i -g @openai/codex`.
  - Section 8: read-it-before-you-install discipline as a standing rule for any third-party Codex plugin.
- Tour itself recommends `--sandbox read-only` mode (Codex's plan-mode equivalent) for the first session and any unfamiliar data.
- Memory write at end of tour — role, terminal comfort, first artifact, next step. No conversation content. Stored at `${CODEX_HOME:-$HOME/.codex}/memory/user_codex_cli_onboarding.md`.
- Six role-conditioned example banks: SOC, GRC, IT generalist, pentest, helpdesk/career-changer, dev, plus a "just trying it out" lane.
- Two rounds of `battle-test` review (3-persona iterate tier) ran before tagging v1.0.0.
