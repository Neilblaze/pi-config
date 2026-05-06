---
name: improve
description: Retrospective audit of Pi config — uses the auditor subagent to scan extensions, skills, AGENTS.md, settings, and sessions, then presents findings one at a time for accept/reject. Use when you want to clean up, optimize, or review your Pi setup.
---

# Improve — Pi Config Retrospective

Review conversation + Pi config, present improvement suggestions one at a time.

**Announce:** "Starting Pi config retrospective..."

**Arguments = targeted feedback.** Always full sweep. Args get highest priority but don't limit scope.

## Load Learnings

Before scope selection, read `~/.pi/agent/improve-learnings.md` if it exists. This file tracks patterns from prior runs.

- **Deprioritize** finding types consistently rejected across runs
- **Boost** categories consistently accepted
- **Adapt** tone based on modify signals
- If file doesn't exist, proceed normally

## Scope Selection

Before launching any agents, ask the user with `ask_user_question`:

**Question:** "What scope should this retrospective cover?"

**Options:**
1. **Full scan + session history** — Config audit plus recent session analysis (slower, more thorough)
2. **Config audit only** — Scan extensions, skills, AGENTS.md, settings (fast)
3. **Current conversation only** — Analyze only this session's patterns and feedback

## Phase 1: Discovery (Background)

### If scope = "Full scan + session history":

Launch the **auditor** subagent in background with this task:
```
Audit the Pi agent configuration at ~/.pi/agent (or the current working directory if it IS the Pi config).

Perform ALL checks including session history analysis.
Scan ~/.pi/agent/sessions/ for the 5 most recent session files.

<prior learnings if available>
```

### If scope = "Config audit only":

Launch the **auditor** subagent in background with this task:
```
Audit the Pi agent configuration at ~/.pi/agent (or the current working directory if it IS the Pi config).

Perform all checks EXCEPT session history analysis. Skip section 6.

<prior learnings if available>
```

### If scope = "Current conversation only":

Skip the auditor subagent. Analyze the current conversation directly (Phase 2 only).

Immediately proceed to Phase 2 while the auditor runs.

## Phase 2: Current Conversation Analysis (Foreground)

Analyze the conversation already in context for:

| Signal | What to Look For |
|--------|-----------------|
| **Corrections** | User corrected behavior, said "no", "don't", "stop", asked to redo |
| **Praise** | User confirmed approach, accepted without pushback |
| **Friction** | Multiple attempts, confusion, back-and-forth |
| **Capability gaps** | User did things manually, asked for something not available |
| **Behavioral patterns** | Tone issues, over/under-explaining, wrong assumptions |
| **Targeted feedback** | Arguments passed to /improve — HIGHEST PRIORITY |

**Low-signal:** If minimal feedback in current conversation, say "No significant findings from this session" and proceed.

## Phase 3: Cross-Reference & Categorize

Wait for the auditor subagent to complete (if launched).

### Agent Failure Handling

- If the auditor fails or returns empty: fall back to a direct foreground scan of known config paths (`ls`, `find`, `read` on key files)
- Announce what was skipped and why

### Merge Findings

Combine auditor findings + conversation findings. Deduplicate. Categorize:

| Category | When to Use | Priority |
|----------|-------------|----------|
| **Targeted** | From user's explicit /improve args | 1st |
| **Critical** | Errors, collisions, broken references, enforcement gaps | 2nd |
| **Improvement** | Enhancement to existing config | 3rd |
| **Maintenance** | Health, bloat, staleness, descriptions | 4th |
| **Reinforcement** | Working well — keep doing | 5th |
| **New Capability** | Repeated pattern that could become an extension or skill | 6th |

Skip findings that are already documented AND being followed.

### Confidence Scoring

| Level | Criteria |
|-------|----------|
| **High** | 3+ supporting signals, or recurrence, or direct user correction |
| **Medium** | 1-2 signals, or pattern match without direct evidence |
| **Low** | Speculative — inferred from config structure or best practices |

## Phase 4: Present Findings

**Announce:** "Found N findings across M categories. Presenting one at a time."

### For each finding, use `ask_user_question`:

**Question format:** "[Category | Confidence] — [Description]. File: [path]. Proposed: [what to change]"

**Options:**
1. Accept
2. Reject
3. Modify (opens editor for user to adjust the proposed change)

**Order:** Critical → Targeted → Improvement → Maintenance → Reinforcement → New Capability

If 8+ findings, after presenting 5, ask: "Continue with remaining findings, or apply what we have so far?"

### Proposed Change Quality

All proposed changes MUST:
- Reference specific file paths
- Include the exact text to add/modify/remove
- For AGENTS.md rules: lead with WHY, use precise language
- Keep concise — one clear sentence beats a paragraph

## Phase 5: Apply Changes

**Announce:** "Applying N approved changes across M files..."

1. Group approved changes by file
2. Edit existing files with approved modifications
3. Create new files if needed (new skills, new extension stubs)
4. For AGENTS.md changes: preserve existing content structure, insert in the right section
5. For extension changes: validate syntax won't break loading
6. Present a summary table:

```
## Changes Applied

| # | File | Change | Category |
|---|------|--------|----------|
| 1 | path/to/file | Brief description | Category |
```

7. Ask if user wants to commit changes (suggest `/yeet` if available)

## Save Learnings

After Phase 5, update `~/.pi/agent/improve-learnings.md`:

1. Read current file (or create if first run)
2. Append new entry under `## Recent Runs`:
   - Date
   - Acceptance rate by category
   - Any "Modify" choices that reveal preferences
3. If file exceeds 80 lines: summarize oldest entries into `## Patterns`, delete raw entries
4. Save

**First-run file structure:**
```markdown
# Improve Learnings

## Patterns (summarized from older runs)

## Recent Runs
### YYYY-MM-DD
- Acceptance: Critical 3/3, Improvement 2/4, Maintenance 0/1
- Modify signal: User adjusted proposed wording in AGENTS.md rule
```
