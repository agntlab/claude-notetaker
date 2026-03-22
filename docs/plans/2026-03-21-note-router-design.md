# Note Router (Stage 2) - Design Document

**Date:** 2026-03-21
**Version target:** v2.1.0
**Status:** Approved

## Problem

Users have scattered ideas for Claude tasks throughout their day. They brain dump into conversations but lose context, forget ideas, or spend 20+ minutes organizing and writing up tasks manually. There's no way to go from messy input to organized, actionable briefs without significant manual effort.

## Solution

Add a Note Router to the existing notetaker skill. It clusters captured notes by topic, tags complexity, and saves each topic as a handoff file with ready-to-use prompts. Users walk away from a session with a folder of pick-up-and-go files.

## Jobs-to-be-Done

*When I* have scattered ideas for things I want Claude to do,
*I want to* dump them quickly and have them organized automatically,
*So I can* pick up any idea in a future session without re-explaining context.

## User Journey

```
Capture (2 min)          Organize (30 sec)           Resume (10 sec)
──────────────          ─────────────────           ────────────────
Note: try X             "organize notes"            Session start:
Note: what if Y         → shows notes for review    "You have 4 ideas"
Note: build Z           → clusters by topic         Pick a number → go
Remind me: test W       → a/b/c confirmation
                        → saves handoff files
```

**Time saved:** ~30 minutes per brain dump session vs manual organizing and task writing.

## Triggers

### Stage 2 Triggers (added to existing skill)

- `organize notes` / `sort notes` / `cluster notes`
- Works regardless of IDLE/ACTIVE state
- Requires at least one `[NOTE` marker in conversation
- If no markers found: "No notes captured yet. Start any message with 'Note:' to capture one."

### Unstructured Input Triggers

For raw text pasted from phone/voice memo/Apple Notes:

- `organize these notes` / `sort these notes` / `cluster these notes`
- Must include the word "notes" to avoid false positives
- Followed by or preceded by a block of raw text

### Triggers Explicitly Rejected

These were considered and dropped due to high false-positive risk:

- ~~`organize this`~~ - conflicts with file-organizer, too generic
- ~~`sort this out`~~ - colloquial for "fix this problem"
- ~~`make sense of this`~~ - used when asking Claude to explain content

## Flow

### Step 1: Review

When `organize notes` is triggered, the skill first shows all captured notes:

```
12 notes captured:

#1 [action] Set up CI pipeline with GitHub Actions
#2 [idea] Auto-generate API docs from TypeScript types
#3 [action] Run content audit on the blog before month end
#4 [idea] What if we added live reload to the dev server
#5 [observation] SQLite handles concurrent reads better than expected for small apps
...

Any corrections before I organize? (edit/delete, or 'go' to proceed)
```

User can correct, delete, or say "go" to proceed to clustering.

### Step 2: Cluster

Skill groups notes by semantic topic similarity:

```
Found 4 topics in 12 notes:

1. Social Media Automation (3 notes: #1, #2, #8) [project]
2. Competitor Research Methods (2 notes: #5, #11) [medium]
3. Blog Audit Improvements (5 notes: #3, #4, #6, #7, #12) [medium]
4. Schema Validation Idea (1 note: #9) [quick]

How would you like to proceed?
a) Save all as handoff files
b) Drop or merge topics first
c) Let's discuss before deciding
```

### Step 3: Modify (if option b)

Natural language commands for modifying clusters:

- "Drop 4" - removes topic 4 entirely
- "Merge 2 and 3" - combines topics into one
- "Move note #5 to topic 1" - reassigns a note
- "Rename 1 to Social Media System" - changes topic name
- "Done" - proceeds to save

After modifications, shows updated cluster list and re-prompts a/b/c.

### Step 4: Save

On "a" (save all) or "done" after modifications:

**Active topics** save to: `.claude/handoffs/note-{YYYY-MM-DD}-{topic-slug}.md`
**Parked topics** save to: `.claude/notes/parked/note-{YYYY-MM-DD}-{topic-slug}.md`

Confirmation:

```
Saved 4 topic files:

- .claude/handoffs/note-2026-03-21-auth-refactor.md
- .claude/handoffs/note-2026-03-21-api-docs-generator.md
- .claude/handoffs/note-2026-03-21-ci-pipeline-setup.md
- .claude/notes/parked/note-2026-03-21-dark-mode-idea.md

Pick any of these up in your next session.
```

### Step 5: Session Start Detection

When Claude Code starts and finds `note-*.md` files in `.claude/handoffs/`:

```
You have 3 saved ideas from previous sessions:

1. Social Media Automation [project] (2026-03-21)
2. Competitor Research Methods [medium] (2026-03-21)
3. Blog Audit Improvements [medium] (2026-03-21)

Pick a number to start working on one, or ignore to do something else.
```

When user picks one:
- Reads the file, presents context and suggested approaches
- User picks an approach or describes their own
- Work begins, file deleted from handoffs

Note files are presented separately from task handoffs:
- Task handoffs: "Resumable task: {slug}"
- Note handoffs: "Saved idea: {topic} [{complexity}]"

## Handoff File Format

```markdown
# {Topic Name}

## Context
- Note content 1 (cleaned up)
- Note content 2
- Note content 3

## Complexity: {Quick | Medium | Project}
{1-2 sentence explanation of why this complexity level.}

## Suggested Approaches
1. **Quick start** - "{prompt to begin immediately}"
2. **Plan first** - "{prompt that triggers brainstorming/planning}"
3. **Research first** - "{prompt to explore before committing}"
4. **Park it** - Move to parked ideas for later
```

## Unstructured Input Flow

For raw text without `[NOTE #N]` markers:

1. User says `organize these notes` + pastes raw text
2. Skill splits text into individual note-like chunks
3. Shows them: "I found 8 ideas in that text:" + numbered list
4. User confirms, corrects, or deletes
5. Proceeds to clustering (same as structured path from Step 2)

Splitting heuristics:
- Sentence boundaries as primary delimiter
- Topic shifts detected by semantic change
- Each chunk classified with a type (action/idea/observation/decision)
- Minimum chunk: one coherent thought. Maximum: 2-3 related sentences

## Complexity Tagging

| Tag | Signals | Examples |
|-----|---------|----------|
| **quick** | Single action, no dependencies, < 1 session | "Add dark mode toggle to settings page" |
| **medium** | Multi-step, some research needed, 1-2 sessions | "Build an API docs generator from TypeScript types" |
| **project** | Multiple components, architecture decisions, 3+ sessions | "Full authentication system with OAuth and refresh tokens" |

Complexity is a suggestion, not a gate. User can override.

## Parked Ideas

- Stored in `.claude/notes/parked/` (not handoffs)
- Do NOT show at session start (reduces noise)
- Accessible via: "show parked ideas" / "show parked notes"
- Can be unparked: "unpark {topic}" moves file back to handoffs

## Scope Boundaries

The skill does NOT:
- Execute tasks - it organizes and recommends only
- Prioritize - it tags complexity but doesn't rank importance
- Act as a task manager - handoff files are temporary, deleted when actioned
- Carry notes cross-session - notes must be organized before session ends or they're lost
- Reject non-Claude content - it's optimized for Claude tasks but doesn't gatekeep

## File Architecture

```
notetaker/
  SKILL.md                    # Core skill (~3,400 words max)
                              # Stage 1 (capture) + Stage 2 summary + pointer
  references/
    note-router.md            # Full Stage 2 logic (~1,200 words)
                              # Clustering, complexity, handoff format, examples
```

Progressive disclosure keeps SKILL.md under the recommended ceiling. The `references/note-router.md` file is loaded on-demand when `organize notes` is triggered.

## Version History

- v2.0.0: Stage 1 (capture) - triggers, state machine, voice cleanup
- v2.1.0: Stage 2 (router) - clustering, handoff files, session start detection

## Changes Adopted from Reviews

### From PM Review
1. Single `organize notes` command handles review + clustering inline
2. Defined merge/drop UX with natural language commands
3. Date prefix on handoff filenames prevents collisions
4. Parked ideas in separate folder, not handoffs
5. No content gatekeeping - users can organize anything

### From Skill Reviewer
1. Progressive disclosure - detail in `references/note-router.md`
2. Dropped generic triggers - kept only note-specific ones
3. No ORGANIZING state - processing command like `process notes`
4. Sync GitHub repo before implementation
5. Distinguish note handoffs from task handoffs at session start
