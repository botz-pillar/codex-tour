# Tomorrow-morning test walkthrough

Personal pre-flight before the team demo. Goal: install NewCodexer from your other Mac, run the tour end-to-end on yourself, find any rough edges, then flip the repo public.

The repo is **private** right now. You can install it from any machine where your GitHub identity is wired up (either SSH key in 1Password agent, or `gh auth login` configured). It does NOT need to be public for tonight or tomorrow's solo test.

---

## 0. Before you start (2 min)

On the other Mac, confirm:

```bash
# GitHub auth works (any one of these is enough)
gh auth status                        # if you use HTTPS + gh
ssh -T git@github.com                 # if you use SSH (1Password agent)

# Node ≥ 18 if you'll npm-install Codex
node --version

# Homebrew if you'll brew-install Codex
brew --version
```

If `gh auth status` fails, run `gh auth login` → choose GitHub.com → HTTPS → "Login with a web browser." You're done in 30 seconds.

---

## 1. Install Codex CLI (5 min)

Pick one — **Homebrew is the easier path** on macOS:

```bash
brew install --cask codex
```

OR npm (if Homebrew is annoying for any reason):

```bash
npm install -g @openai/codex
```

Then sign in:

```bash
codex
# It'll prompt for ChatGPT or API key. Pick ChatGPT — browser opens, you sign in, browser closes.
```

Confirm it works: inside the `codex` session, type `/help` — you should see a slash-command menu. If you don't, you're not actually in Codex (probably still in plain terminal). Type `codex` again.

---

## 2. Install NewCodexer from the (still-private) repo (1 min)

**Exit Codex first** if you're in it (`/exit`). Then from your shell:

```bash
codex plugin marketplace add botz-pillar/NewCodexer
```

If the shorthand resolution fails because of how `gh` is set up, fall back to the SSH form:

```bash
codex plugin marketplace add git@github.com:botz-pillar/NewCodexer.git
```

You should see a confirmation that the marketplace was added.

Then launch Codex and install:

```bash
codex
# inside Codex:
/plugins
# find new-codexer in the browser, highlight it, confirm install.
```

The browser shows everything the plugin will install before you say yes. Approve. Plugin installed.

---

## 3. Trigger the tour (the actual test — 10–15 min)

In the same Codex session (or a new one — `codex` again), type one of:

```
I'm new to Codex CLI, walk me through it
```

```
I'm a SOC analyst trying Codex for the first time — what should I know?
```

If the tour doesn't fire from natural language, force-invoke it:

```
$new-codexer
```

**What you're looking for as you go through it:**

- [ ] Step 0–1 actually ask the calibrating questions (role + terminal comfort)
- [ ] Step 2 delivers the safety story (data handling, approvals, read-only sandbox)
- [ ] Step 3 picks a role-matched exercise and lets you do it
- [ ] A file gets saved to `~/Documents/` and you get the full path
- [ ] Tour ends with starter-prompts saved to `~/Documents/new-codexer-starter-prompts.md`
- [ ] Session note saved to `~/Documents/new-codexer-session-<date>.md`
- [ ] Voice stays plain English — no "transform/unlock/leverage" or slop
- [ ] At any point, *"slow down and explain like I'm new"* resets the pace

**Try at least one InfoSec-role pitch** end-to-end (recommend SOC phishing-triage with a synthetic email — fastest to verify quality).

---

## 4. Things to specifically look for during the demo dry-run

These are the bugs most likely to surface on first contact:

1. **Trigger phrase doesn't auto-fire.** Codex's skill-matching is description-based. If natural language doesn't trip it, the `$new-codexer` prefix always works. Note any phrases that *should* have triggered but didn't — those become description-tuning fixes for v1.0.2.
2. **First file write goes somewhere unexpected.** The skill says save to `~/Documents/`. Verify the file actually lands there and the path Codex announces matches.
3. **Step 2 safety story feels too long.** The intent is ~90 seconds of safety talk before the work starts. If it feels longer than that, flag it.
4. **A starter prompt references something that doesn't apply to you.** Read all 10 in your role's bank — anything weird, note it.
5. **A concept gets defined twice or never.** Concepts are supposed to land in-flight; if Codex front-loads them or skips one entirely, note which.
6. **A Codex-specific behavior differs from the README claim.** Examples: `/approvals` UI, sandbox mode names, `/plugins` browser layout. If anything looks different, capture a screenshot.

Keep a scratch file open for notes — `~/Documents/newcodexer-test-notes.md` works.

---

## 5. If something is broken

| Symptom | Likely fix |
|---|---|
| `codex plugin marketplace add` says "unknown command" | You're on an older Codex CLI. Update: `brew upgrade --cask codex` |
| `marketplace add` errors with permission/auth | Run `gh auth login` first, or switch to the SSH URL form |
| `/plugins` shows nothing | Marketplace add succeeded but the registry hasn't refreshed — quit Codex (`/exit`), relaunch, try `/plugins` again |
| Tour doesn't trigger | Use `$new-codexer` as the message prefix to force-invoke |
| Tour writes file to a weird path | Note the path it announced vs. where the file actually landed — that's a SKILL.md fix |
| Anything else | Open `~/Documents/NewCodexer/skills/new-codexer/SKILL.md` and read the offending section directly — you wrote it, you can hot-fix it on disk and re-trigger |

For any non-trivial bug: jot it in your scratch notes, finish the test (don't get tunnel-vision on one issue), then patch in batch before flipping to public.

---

## 6. After the test passes — flip to public (1 min)

```bash
gh repo edit botz-pillar/NewCodexer --visibility public --accept-visibility-change-consequences
```

GitHub Pages will resume serving at https://botz-pillar.github.io/NewCodexer/ within a minute or two. Confirm by curling:

```bash
curl -sI https://botz-pillar.github.io/NewCodexer/ | head -5
```

You should see `HTTP/2 200`.

---

## 7. Team demo (after public)

The team will run the exact same flow from §1–§3 above on their own machines. Give them:

- This repo: https://github.com/botz-pillar/NewCodexer
- The landing page: https://botz-pillar.github.io/NewCodexer/
- The README — that's the entire onboarding doc, designed to walk a complete beginner from install to first artifact

**Windows folks on the team:** flag in advance that they'll need WSL2 (Codex CLI does not run natively on Windows). Setting up WSL2 is a 15-minute side-quest — better they do it before the demo than during. Point them at README §7a.

**Linux folks:** standard `npm install -g @openai/codex` after Node is in place. README §7b covers it.

---

## 8. Tomorrow's success criteria

- [ ] Installed from `botz-pillar/NewCodexer` on the other Mac, end-to-end, in under 10 min
- [ ] Tour triggered from one of the documented natural-language phrases (no `$` force-invoke needed)
- [ ] Produced a real artifact saved to `~/Documents/`
- [ ] Session note + starter-prompts also saved as documented
- [ ] No copy in the tour felt off, slow, or slop-y
- [ ] Flipped to public, Pages live, team install path verified

If all six hit, you're cleared for the team demo. If two or more miss, batch a v1.0.2 patch before going public.
