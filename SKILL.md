---
name: magi
description: Launches 3 parallel sub-agents (Melchior, Balthasar, Casper) inspired by NERV's MAGI system from Neon Genesis Evangelion. Each reviews code/PRs/features from a distinct perspective and returns severity-classified findings. Claude Code synthesizes all three into a single report and persists it to `.magi/<session-id>/report.md` so future runs can skip already-resolved findings. Use when the user invokes /magi, asks for a "deep code review", wants multi-perspective analysis, or says "launch the magi" on a file, diff, PR or feature.
---

# MAGI — Multi-perspective review system

Inspired by NERV's three MAGI supercomputers in Neon Genesis Evangelion (Melchior, Balthasar, Casper). Each agent represents a different facet of analysis. All three run **in parallel, without prior conversation context**, and deliver findings classified by severity. You (Claude Code) synthesize the reports, present them in a single consolidated report, and persist that report to `.magi/<session-id>/report.md` so future runs carry state forward.

---

## When to use

Invoke this skill when:
- The user types `/magi` or "launch the magi" on code, a file, a diff or a PR
- Asks for a "deep code review", "multi-perspective analysis" or "full review"
- Wants to validate architecture, security, performance and UX at once
- Needs a second (third, fourth) opinion before merging or deploying

Do NOT use for:
- Simple code questions (answer directly)
- Specific bugs with an obvious cause (normal debug)
- Implementation tasks (this is review, not build)

---

## Language

**Detect the conversation language** and use it in ALL output (agent prompts, final report, headers). If the conversation is in Spanish, everything in Spanish. If in English, everything in English. If unsure, ask once.

---

## Session isolation

Each Claude Code chat is its own session. Reports live in a per-session folder to prevent collisions between concurrent chats reviewing the same repo:

```
.magi/<session-id>/report.md
```

**Resolving `<session-id>`**:
1. Read the env var `CLAUDE_SESSION_ID` if available.
2. Otherwise, generate a short slug (e.g. `s-YYYYMMDD-HHMMSS-<random4>`) the first time you write to `.magi/` this session and keep it in memory for the rest of the chat.

Do NOT reuse folders across chats. Do NOT delete other sessions' folders. Each chat owns exactly one folder.

---

## The 3 agents

### 🧠 Melchior — the scientist (rational analysis)
Focus:
- **Logic & correctness** — bugs, edge cases, mishandled conditions, broken logic
- **Architecture & system design** — layers, separation of concerns, patterns
- **Coupling & SOLID** — dependencies, cohesion, design principles

Read: `agents/melchior.md`

### 🛡️ Balthasar — the mother (protection)
Focus:
- **Security & vulnerabilities** — injections, authentication, sensitive data, OWASP
- **Testing & edge cases** — coverage, boundary cases, failure scenarios
- **Best practices & conventions** — language/framework standards, anti-patterns

Read: `agents/balthasar.md`

### ✨ Casper — the woman (intuition / experience)
Focus:
- **Performance & scalability** — complexity, queries, resources, bottlenecks
- **UX & visual design** — usability, accessibility, visual hierarchy (if applicable)
- **Maintainability & readability (DX)** — clarity, tech debt, next-dev experience

Read: `agents/casper.md`

---

## Execution flow

### Step 1 — Identify the target
Ask the user what to review if not obvious:
- Specific file(s)
- Diff/PR (`git diff`, `git diff main`, PR number)
- Full feature (multiple related files)
- Folder or module

If the target is a remote PR, use `gh pr diff <num>` to get the diff.

### Step 2 — Launch the 3 agents IN PARALLEL
Use the Task tool (subagent_type: general-purpose) **in a single call** with 3 simultaneous invocations. Each one:

1. Reads its role file (`agents/melchior.md`, `agents/balthasar.md`, `agents/casper.md`)
2. Receives the target (absolute file paths, full diff in the prompt, or read instruction)
3. Returns a structured report in the format defined below

**Critical:** the agents are AGNOSTIC. They share no context with each other or with the main conversation, and **NEVER** receive `.magi/<session-id>/report.md` or any prior finding. Fresh review only. Pass them only:
- Their role file
- The code/diff to review (paths or content)
- The output language

### Step 3 — After agents return, load previous report (if it exists)
Only AFTER the 3 agents have returned their findings, look for `.magi/<session-id>/report.md` in the target root. If it exists:
1. Read it and build a set of already-closed findings (marked `false positive`, `by design`, or `fixed: ...`).
2. Use it in synthesis to filter duplicates from what the agents just reported.

### Step 4 — Synthesize + persist
When the 3 reports return:
1. Discard findings already closed in the previous report (match by file:line + similar title).
2. Keep `pending` findings from the previous report.
3. Write/update `.magi/<session-id>/report.md` using the format below (create `.magi/` if it doesn't exist).
4. Show the consolidated report to the user **on screen** (don't just dump the path, show content).

## Persistent report format (`.magi/<session-id>/report.md`)

```
# 🔮 MAGI Report

**Target:** <file/PR/feature>
**Last run:** <YYYY-MM-DD HH:MM>
**Total runs:** <N>

## Summary
<2-3 lines>

## 🔴 Critical

### [PENDING] <title> — <file:line>
- **Agent:** Melchior / Balthasar / Casper
- **Description:** <short>
- **Suggestion:** <what to do>
- **Status:** `pending`

### [FIXED] <title> — <file:line>
- **Agent:** ...
- **Description:** ...
- **Status:** `fixed`
- **How:** <fix summary>

### [FALSE POSITIVE] <title> — <file:line>
- **Status:** `false positive`
- **Reason:** <why it doesn't apply>

### [BY DESIGN] <title> — <file:line>
- **Status:** `by design`
- **Reason:** <requirement/decision>

## 🟡 Medium
(same format)

## 🟢 Minor
(same format)
```

Valid statuses: `pending` | `false positive` | `by design` | `fixed`.
The tag at the start of the title (in brackets) reflects the status for quick search.

### Step 5 — Wait for user decision + update the md
After showing the report, **STOP**. Do not make automatic changes. Ask which finding to tackle first. The user decides.

When the user marks a finding (`false positive` / `by design` / `fixed`), update `.magi/<session-id>/report.md` in the same moment (inline edit on the file, do not rewrite the whole thing).

---

## Non-negotiable rules

1. **Always the 3 agents in parallel.** Never sequential.
2. **Agents without prior context.** Don't pass them conversation history.
3. **Severity is mandatory** on each finding: 🔴 critical / 🟡 medium / 🟢 minor.
4. **One final report.** Do not dump the 3 raw reports.
5. **DO NOT fix anything automatically.** The user decides what to touch.
6. **Language consistent** in all output per the conversation.
7. **Cite file:line** on each finding when possible.
8. **One persistent report per session.** Always `.magi/<session-id>/report.md` at the target root. One folder per chat, never shared.
9. **Filter closed findings ONLY AFTER agents return.** Never inject the previous report into agent prompts. The orchestrator (Claude Code) is the one that filters — the agents always do a fresh review.
10. **Update the md on the fly** when the user marks status changes.
