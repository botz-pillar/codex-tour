# Offline / Google Drive install (alternative to GitHub)

Use this if you don't want to walk your team through GitHub auth, or you're handing the plugin to someone who doesn't have a GitHub account at all. The GitHub-marketplace path in the [README](./README.md#8-install-this-tour) is still the primary recommendation when the repo is public — this is the **alternative**.

The plugin is plain markdown, so it works just as well from a local folder as from a git clone. Codex's `marketplace add` accepts a `file://` URL pointing at a local directory, so the offline flow is essentially "give them the folder, they point Codex at it."

---

## What you (Josh) do once

From your machine, with the repo cloned at `~/Documents/NewCodexer`:

```bash
cd ~/Documents
zip -r NewCodexer.zip NewCodexer \
  -x "NewCodexer/.git/objects/pack/*" \
  -x "NewCodexer/node_modules/*"
```

Then upload `NewCodexer.zip` to a shared Google Drive folder. Share with your team (view + download access is enough — no need for edit).

> **Important:** keep `.git/` in the zip. Codex's `marketplace add file://…` treats the folder as a git source and needs the repo metadata. The exclusion above only strips the heavyweight pack files inside `.git/objects/`, not the `.git/` directory itself. If your zip is over ~5 MB you probably included more than you need — re-check the excludes.

A simple alternative if zipping irritates you: share the unpacked `NewCodexer/` folder via Drive's "Share a folder" feature. Smaller download size, slightly more clicks for the recipient.

---

## What each teammate does (5 min)

### 1. Install Codex CLI

Same as the README §7. Short version:

- **macOS:** `brew install --cask codex` then `codex` to sign in
- **Linux:** `npm install -g @openai/codex` then `codex`
- **Windows:** `wsl --install` first, then run the npm install inside the Ubuntu/WSL2 shell

### 2. Download and unzip

From the Drive link you sent them:

1. Click **Download**
2. Unzip to `~/Documents/NewCodexer` (or any folder they prefer)

On macOS the default Finder unzip works. On Windows (inside WSL2): `unzip ~/Downloads/NewCodexer.zip -d ~/Documents/`.

### 3. Install the plugin from the local folder

In a terminal — **outside** Codex:

```bash
codex plugin marketplace add file:///Users/<their-username>/Documents/NewCodexer
```

(Replace `<their-username>` with whatever `whoami` prints. On Windows/WSL2 the path looks like `file:///home/<linux-username>/Documents/NewCodexer`.)

Then launch Codex and finish the install in the TUI:

```bash
codex
# inside Codex:
/plugins
# find new-codexer, highlight it, confirm install.
```

### 4. Trigger the tour

Inside Codex, type:

```
I'm new to Codex CLI, walk me through it
```

If it doesn't auto-fire, prefix with `$new-codexer`. Same behavior as the GitHub-installed version.

---

## Caveats vs. the GitHub install

- **No auto-updates.** If you ship a v1.0.2, teammates with the offline copy stay on v1.0.1 until you re-share. The GitHub-marketplace install gets `codex plugin marketplace update` for free. Mitigation: include the version number in your Drive filename (`NewCodexer-v1.0.1.zip`) so it's obvious when it's stale.
- **No GitHub Issues handoff.** Bug reports come back to you in Slack/email instead of as filed issues. Fine for an internal team; less ideal for broad community release.
- **Path of least resistance is still GitHub once public.** If your goal is "team installs in 2 minutes from a public repo," flip the repo to public after your dry-run and use the README's standard flow. Reserve the Drive path for genuinely offline scenarios (air-gapped lab, GitHub-blocked corporate networks, demos to non-technical audiences who shouldn't have to think about git accounts).

---

## When to use which

| Situation | Use |
|---|---|
| Internal team demo, repo still private | **Drive zip** — no GitHub auth headache |
| Public launch, posted to LinkedIn or social | **GitHub marketplace** (after flipping public) |
| Locked-down corporate laptop with no GitHub access | **Drive zip** — works wherever Codex CLI installs |
| Air-gapped or offline environment | **Drive zip** (or USB stick) |
| Anyone you want to be able to upgrade themselves later | **GitHub marketplace** |

The two paths can coexist — share the Drive zip with the team for the demo, then point them at the public GitHub repo for ongoing updates after.
