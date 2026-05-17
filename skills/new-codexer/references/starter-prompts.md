# Starter prompts — by role

A handout for users finishing the tour. When the user reaches the 5-minute exit ritual, extract the 10 prompts for their role from this file, write them to `~/Documents/new-codexer-starter-prompts.md`, tell them the full path, and offer to open it.

These are written in the user's voice — what they would type to the Codex CLI — not in Codex's voice. The point is to remove "what do I even ask for?" friction the day after the tour.

**Safety-conscious tip baked into several prompts:** when working with potentially untrusted content (logs from unknown sources, files received via email, web pages), suggest the user launch with `codex --sandbox read-only` so nothing executes automatically.

---

## SOC / security ops

1. "Here's a phishing email — full headers and body pasted below. Triage it: verdict, IOCs, why it's suspicious, what to tell the reporter. Run me in read-only sandbox."
2. "Decode this base64 PowerShell blob without running it (read-only sandbox). Walk me through what it would do, flag the malicious bits, save your analysis to a file."
3. "Read every Zeek log in this folder and rank source IPs by suspicion. Output a CSV I can hand to triage."
4. "Draft a Sigma rule, a Splunk SPL, and a KQL version for this CVE. Lint them against any existing rules in `./rules/`."
5. "Turn my IR runbook into a checklist for a specific scenario: ransomware on one endpoint, after-hours, single analyst on shift."
6. "Tail this log file and tell me what looks weird. Don't just describe it — call out specific lines."
7. "Convert this messy Excel of alert exports into a clean markdown table grouped by severity."
8. "Read these three threat-intel PDFs and produce one consolidated brief: campaign, TTPs mapped to MITRE, IOCs as a table."
9. "Write me a triage rubric for benign-looking outbound DNS to newly-registered domains. What's the deciding question?"
10. "Build me an `AGENTS.md` for this folder that says 'this contains synthetic data only, default to read-only sandbox.'"

---

## GRC / compliance

1. "Here's an auditor's findings PDF. Turn it into a remediation tracker: control, owner, severity, due date, evidence needed."
2. "Map our policy set against NIST 800-53. Surface every control with no coverage. Output a gap matrix."
3. "Score this vendor SIG questionnaire. Flag every weak answer with no detail. Draft my follow-up questions."
4. "Write a CC6.1 control narrative using these policy excerpts and these evidence references."
5. "Sanitize this incident write-up for an SOC 2 disclosure. Strip identifiers, preserve the timeline."
6. "Read these three SOC 2 reports and compare their control coverage side by side."
7. "Take this risk register and re-sort it by residual risk, not inherent risk. Same columns, different order."
8. "Draft a one-page exec summary of our quarterly compliance posture from this evidence folder."
9. "Convert this regulatory text into a checklist of obligations we'd need to meet. Plain English."
10. "Help me write an `AGENTS.md` for our compliance folder so future sessions know our naming conventions and what frameworks we care about."

---

## IT generalist / sysadmin

1. "Explain this scheduled task line by line. Is it safe to disable?"
2. "Write a PowerShell script that audits local admin accounts and outputs CSV ranked by risk."
3. "Write a nightly backup script that snapshots D:\\Shares to the NAS, retains 30 days, emails on failure. Dry-run it first."
4. "Generate a Mermaid network diagram from these Cisco config exports."
5. "Inventory every service running on this host and tell me which ones are weird."
6. "Convert this 800-line bash script into something readable with sections and comments."
7. "Help me write a runbook for the thing my team keeps Slack-asking me to do."
8. "Read this folder of GPO exports and tell me what's enforced that probably shouldn't be."
9. "Build a one-pager: 'how to onboard a new IT hire to this environment.' Edit the parts you guess at."
10. "Set up an `AGENTS.md` in this homelab folder so I don't have to re-explain my setup every session."

---

## Pentest / red team

1. "Parse this nmap XML. Group hosts by attack surface. Draft a 'next steps' list ranked by interest."
2. "Convert this curl command (from Burp) into a clean Python requests script with auth handling and a TODO for the param I want to fuzz."
3. "Read this CVE PoC on GitHub. Explain how the bug works in 5 sentences. Sketch a safe lab repro using Docker."
4. "Rewrite the exec summary of this pentest report in client-readable English. Don't lose the technical accuracy."
5. "Take this messy notes file and turn it into a proper findings doc: title, CVSS, evidence, remediation."
6. "Generate me a wordlist tuned to this target's tech stack. Save to a file."
7. "Read this engagement scope doc and tell me what's *out* of scope I might miss in the rush."
8. "Help me write a retesting checklist for the findings we shipped last quarter."
9. "Set up an `AGENTS.md` in this engagement folder that says 'never touch live targets, lab IPs only.'"
10. "Draft a Slack message to the client explaining the critical finding. Plain English. No FUD."

---

## Helpdesk / career-changer

1. "Turn my CTF notes into a portfolio-worthy markdown writeup. Then walk me through `git init`, commit, and push to GitHub."
2. "Inventory my homelab into a `homelab.md` from this folder. VMs, containers, services, IPs."
3. "Pick a recent CVE and write me a Sigma detection rule. Push it to a 'detections' repo on my GitHub."
4. "Quiz me Socratically on a Security+ topic. Save the gaps to a study file so I can review."
5. "Read this job description. Gap-check against my resume. Build me a 3-task plan that closes the biggest gap."
6. "Walk me through setting up an `AGENTS.md` for my study folder so future sessions know I'm prepping for Sec+."
7. "Help me write a LinkedIn post about a security thing I just learned, in my voice (read my last three posts first)."
8. "Take this raw incident report I'm studying and turn it into flashcards I could use for review."
9. "Set me up a 'first 30 days' learning plan: 10 minutes a day, security-focused, ending with a public artifact."
10. "Help me build a 'now-next-later' personal roadmap for getting into security from helpdesk."

---

## Dev / engineer

1. "Tour this codebase: entry point, main logic, data layer, config, tests. Build an `AGENTS.md` as you go."
2. "Read-only sandbox: I think the bug is somewhere in `auth/`. Form a hypothesis, find suspects, propose a failing test before fixing."
3. "Refactor `<file>` to extract this 200-line function. Migrate one caller. Run tests."
4. "Read this README and tell me what's outdated or wrong relative to the actual code."
5. "Generate a migration script from this old schema to this new schema."
6. "Convert these inline SQL queries into prepared statements. Show me the diff."
7. "Help me write integration tests for `<module>`. Start with the unhappy paths."
8. "Document this API. Output OpenAPI 3.1 yaml."
9. "Stage these changes in a way that makes a sensible PR — multiple commits, each one a coherent story."
10. "Write an `AGENTS.md` for this repo that captures the gotchas a new contributor would trip over."

---

## "Just trying it out"

1. "Read the files in this folder and tell me what this thing is."
2. "Pick the most interesting file you can find on my Desktop and summarize it."
3. "Take this PDF and turn it into a markdown outline I can scan in a minute."
4. "Read this conversation we just had and save the key points to a file."
5. "Help me figure out what to use you for. Ask me five questions and propose three ideas."
6. "Find the largest files in my Downloads folder and tell me what they are."
7. "Read this screenshot. What is it, and is there anything I should worry about?"
8. "Help me write a one-page personal cheat sheet of things I want to remember about my job. We'll iterate."
9. "Read-only sandbox: explore my Documents folder and propose three things you could help me with."
10. "Just describe what *you* would do if you were me looking at this folder for the first time."
