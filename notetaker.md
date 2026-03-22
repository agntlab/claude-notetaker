---
name: notetaker
description: Don't forget random thoughts. Voice-friendly note capture for Claude. Say 'brain dump' to talk freely, 'Note:' for quick one-offs. Say 'process notes' for a grouped list, or 'organize' to cluster notes/ideas and save as handoff files. Designed for voice dictation - cleans up fillers, false starts, and dictation artifacts automatically.
version: 3.0.0
---

## When to use this skill

Capture ideas during conversations without breaking flow:
- Brain dumps (voice or typed)
- Quick one-off notes
- Action items, ideas, observations, decisions
- Organizing captured notes into handoff files for future sessions

## How to use this skill

### Capture

**Brain dump (primary):** Say `brain dump`, talk freely, say `done` when finished. Every message is captured and split into individual ideas. No per-message triggers needed.

**Single notes:** Start a message with `Note:`, `Remind me:`, or `Action:` to capture one thought.

**Voice variants:** `Note.` / `Note,` / `Note -` also work for dictation.

### Output format

Each captured idea becomes:
```
[NOTE #N | type] cleaned-up text
```

Types are auto-classified: **action** (imperatives, "need to", "should"), **idea** ("what if", "maybe"), **observation** (facts, data), **decision** ("agreed", "let's"). Default: observation.

### After capture

- `process notes` - grouped list with checkboxes on actions
- `organize` - cluster by topic, save as handoff files with ready-to-use prompts
- `fix note 3: new text` - correct a note
- `scratch that` - undo last note
- `delete note 5` - remove a note

### Rules

1. **Tape recorder mode.** Capture and move on. Do not analyze, expand, question, or act on note content. No acknowledgment words before markers
2. **Voice cleanup.** Strip fillers, fix dictation errors, neutralize emotional language, preserve uncertainty. See `references/voice-cleanup.md` for full rules
3. **Splitting.** In brain dump mode, split by sentence boundaries. When in doubt, over-split
4. **Numbering.** Sequential from #1. Deleted numbers are never reused
5. **Exit signals.** Brain dump exits on whole-message `done` / `stop` / `that's all`. "The project is done" does NOT exit
6. **Anti-triggers.** "Note that", "please note", "remind me about", "action item" are NOT capture triggers. See full list at bottom of this file
7. **Multi-note messages.** Only the first trigger fires. Suggest brain dump mode for multiple notes
8. **Checkpointing.** At notes #12 and #20, suggest processing to avoid context loss
9. **Compression.** If earlier notes are lost to compression, state the gap honestly. Do not reconstruct
10. **Session-end.** Check for unprocessed notes before closing

### Note Router

Say `organize` to cluster notes into topic-based handoff files. Full logic: `references/note-router.md`

### First use

If asked what this does: "Say 'brain dump' to talk freely, 'Note:' for quick one-offs, or 'organize' to cluster and save."

## Anti-triggers

These are NOT capture triggers: "note that" / "note how" / "noted" / "please note" / "remind me of" / "remind me what" / "remind me about" / "take note of" / "side note" / "as a note" / "do you have notes" / "show my notes"

**Rule:** `note` + punctuation = trigger. `note` + "that"/"how"/"the" = not a trigger.

## Use cases

- Capture ideas on your commute before you forget them
- Voice-dictate meeting notes without switching apps
- Dump shower thoughts and sort them later
- Track decisions made during a conversation
- Build a backlog of tasks from a brainstorming session
- Save random thoughts throughout the day and pick up the best ones tomorrow
- Hands-free note-taking while cooking, walking, or driving
