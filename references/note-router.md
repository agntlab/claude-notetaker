# Note Router - Full Logic

Read this file when `organize notes`, `sort notes`, or `cluster notes` is triggered.

## Prerequisites

- At least one `[NOTE` marker must exist in the conversation
- If no markers found AND no raw text provided: "No notes captured yet. Start any message with 'Note:' to capture one."

## Flow

### Step 1: Review

Show all captured notes in a numbered list:

```
12 notes captured:

#1 [action] Set up CI pipeline with GitHub Actions
#2 [idea] Auto-generate API docs from TypeScript types
#3 [action] Run content audit on the blog before month end
...

Any corrections before I organize? (edit/delete, or 'go' to proceed)
```

User can:
- "delete #4" - removes note
- "fix #3: Run content audit on docs not blog" - corrects note
- "go" / "looks good" / "proceed" - moves to clustering

### Step 2: Cluster

Group notes by semantic topic similarity. Notes about related subjects cluster together regardless of their type (action/idea/observation/decision).

**Clustering rules:**
- Group by topic relatedness, not by note type
- A single-note topic is valid - not everything needs a group
- Aim for 2-7 topics. If more than 7, consider whether some can merge
- Each cluster gets: topic name, note references, complexity tag

**Complexity tagging:**

| Tag | Signals |
|-----|---------|
| **quick** | Single action, no dependencies, < 1 session |
| **medium** | Multi-step, some research needed, 1-2 sessions |
| **project** | Multiple components, architecture decisions, 3+ sessions |

Complexity is a suggestion. User can override.

**Output format:**

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

### Step 3: Modify (option b)

Accept natural language commands:

- "Drop 4" - removes topic 4 entirely
- "Merge 2 and 3" - combines into one topic, user names the merged topic
- "Move note #5 to topic 1" - reassigns a note between topics
- "Rename 1 to Social Media System" - changes topic name
- "Park 4" - marks topic for parked folder instead of handoffs
- "Done" - proceeds to save

After modifications, show updated cluster list and re-prompt a/b/c.

### Step 4: Save

**Active topics** save to: `.claude/handoffs/note-{YYYY-MM-DD}-{topic-slug}.md`
**Parked topics** save to: `.claude/notes/parked/note-{YYYY-MM-DD}-{topic-slug}.md`

**Handoff file format:**

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

**Prompt generation rules:**
- Quick start prompts should be specific and actionable - the user can paste them directly
- Plan first prompts should reference brainstorming or planning workflows
- Research first prompts should focus on exploration and comparison
- All prompts should include enough context from the notes that the user doesn't need to re-explain

**Confirmation output:**

```
Saved 4 topic files:

- .claude/handoffs/note-2026-03-21-auth-refactor.md
- .claude/handoffs/note-2026-03-21-api-docs-generator.md
- .claude/handoffs/note-2026-03-21-ci-pipeline-setup.md
- .claude/notes/parked/note-2026-03-21-dark-mode-idea.md

Pick any of these up in your next session.
```

## Unstructured Input

When `organize these notes`, `sort these notes`, or `cluster these notes` is triggered with raw text (no `[NOTE` markers):

1. Split raw text into individual note-like chunks
2. Use sentence boundaries as primary delimiter
3. Detect topic shifts for secondary splitting
4. Classify each chunk with a type (action/idea/observation/decision)
5. Show numbered list: "I found 8 ideas in that text:"
6. User confirms, corrects, or deletes
7. Proceed to clustering (Step 2)

**Splitting heuristics:**
- Each chunk = one coherent thought (1-3 sentences max)
- Imperative verbs signal action boundaries
- "What if" / "could we" / "maybe" signal idea boundaries
- Topic nouns changing signals a new chunk
- When ambiguous, prefer over-splitting (user can merge later)

## Session Start Detection

When Claude Code starts and finds `note-*.md` files in `.claude/handoffs/`:

```
You have 3 saved ideas from previous sessions:

1. Social Media Automation [project] (2026-03-21)
2. Competitor Research Methods [medium] (2026-03-21)
3. Blog Audit Improvements [medium] (2026-03-21)

Pick a number to start working on one, or ignore to do something else.
```

Present note handoffs separately from task handoffs:
- Task handoffs show as: "Resumable task: {slug}"
- Note handoffs show as: "Saved idea: {topic} [{complexity}] ({date})"

When user picks one:
- Read the file, present context and suggested approaches
- User picks an approach (1-4) or describes their own
- Work begins
- File deleted from handoffs after user starts working

## Parked Ideas

- Stored in `.claude/notes/parked/`
- Do NOT show at session start
- Accessible via: `show parked ideas` / `show parked notes`
- Unpark: `unpark {topic}` moves file back to `.claude/handoffs/`
- List shows: topic name, complexity, date, first line of context

## Boundaries

The note router does NOT:
- Execute tasks - it organizes and recommends only
- Prioritize or rank topics - complexity is descriptive, not prescriptive
- Act as a persistent task manager - files are temporary
- Carry notes across sessions without explicit organization
- Reject input based on content type
