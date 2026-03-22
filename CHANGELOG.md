# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [3.0.0] - 2026-03-21

### Changed

- **Full UX restructure** - sections reordered by usage frequency, not implementation detail
- **Triggers grouped together** - brain dump (primary), single triggers, stop signals in one flow
- **State Machine section eliminated** - transitions now implicit in trigger and brain dump rules
- **Voice cleanup condensed** - 14 rules to 8 inline, full version in `references/voice-cleanup.md`
- **Note Router trimmed** - 362 words to ~80, defers to `references/note-router.md`
- **Boundaries + Examples merged** - wrong behavior examples are boundary enforcement
- **Edge cases consolidated** - first use, checkpointing, compression, multi-note, session-end in one section
- **Anti-triggers moved to bottom** - implementation detail, not user-facing
- **Capture Format + Note Types merged** - tightly related, no need for separate sections
- **Word count reduced 48%** - ~2,550 to ~1,330 words
- **Bare `organize` trigger** added with y/n confirmation gate
- **Description updated** - "Voice-friendly note capture for Claude"

### Added

- `references/voice-cleanup.md` - full 14-rule voice cleanup reference

## [2.2.0] - 2026-03-21

### Removed

- **55+ English triggers** reduced to 3: `Note:` (+ voice variants `.` `,` `-` `--`), `Remind me:`, `Action:`
- **Tiers 2-4** (imperative, declarative, voice dictation triggers) - all removed
- **Continuation triggers** (more notes, another note, etc.) - brain dump mode handles multi-note capture
- **10 multilingual trigger sections** (Chinese, Hindi, Spanish, Arabic, French, Portuguese, Russian, Japanese, Korean, Indonesian) - removed entirely
- **Language detection section** - removed (brain dump handles all languages naturally)
- Tier 1 triggers also stripped: `new note:`, `note to self:`, `/note`, `remember:`, `jot down:`, `capture this:`, `new idea:`, `idea:`, `had an idea:`

### Added

- **`Action:`** as new trigger (replaces "capture this", "jot down", etc.)

### Changed

- Trigger architecture: from 4 tiers (~105 patterns) to flat list of 3 triggers
- Anti-triggers simplified to match only the 3 remaining triggers
- First Use text updated to reflect simplified trigger set
- Description updated: "Say 'brain dump' to capture ideas freely, or 'Note:' for quick one-offs"
- Multi-note guidance now suggests brain dump mode instead of continuation triggers
- All `detected_language` references removed from non-brain-dump sections

## [2.1.0] - 2026-03-21

### Added

- **Note Router (Stage 2)** - clusters captured notes by semantic topic, tags complexity (quick/medium/project), saves handoff files with ready-to-use prompts
- **Router triggers**: `organize notes`, `sort notes`, `cluster notes` (work in both IDLE and ACTIVE states)
- **Unstructured input support**: `organize these notes` + raw pasted text from phone/voice memo/Apple Notes
- **Topic clustering** with natural language modification: "Drop 4", "Merge 2 and 3", "Move note #5 to topic 1", "Park 3"
- **a/b/c confirmation pattern**: a) Save all as handoff files, b) Drop or merge topics first, c) Let's discuss before deciding
- **Handoff file generation** to `.claude/handoffs/note-{YYYY-MM-DD}-{topic-slug}.md` with 4 suggested approaches per topic (quick start, plan first, research first, park it)
- **Parked ideas** stored separately in `.claude/notes/parked/`, accessible via `show parked ideas`, unpark via `unpark {topic}`
- **Session start detection** - note handoff files listed separately from task handoffs with complexity tags and dates
- **Progressive disclosure** - full router logic in `references/note-router.md` to keep main skill file under size ceiling
- **Spanish punctuation triggers**: `Nota.` / `Nota,` / `Nota -` / `Nota --` variants added

### Changed

- **Language detection** now based on note **content**, not trigger word. Foreign trigger + English content stays English (e.g., "Nota: buy milk" stays English, "Nota: comprar leche" switches to Spanish). Applies to all 11 languages
- Skill description updated to reflect two-stage capture + organization workflow
- Session-end integration synced to GitHub repo version

## [2.0.0] - 2026-03-21

### Added

- **State machine** (IDLE/ACTIVE) - continuation triggers and stop signals only fire after first note capture. Auto-expires to IDLE after 5 idle turns
- **Language detection** - detects language on first trigger, adapts non-marker responses to detected language. Markers (`[NOTE #N | type]`) always English for parseability
- **11-language trigger support**: Mandarin Chinese, Hindi/Hinglish, Spanish, Arabic (MSA + Arabizi), French, Portuguese (BR-PT focus), Russian (Cyrillic + transliterated), Japanese, Korean, Indonesian/Malay
- **Anti-trigger system** - 20 false-positive patterns checked BEFORE trigger matching ("note that", "please note", "side note", "notebook", etc.)
- **Disambiguation rules** for bare "note"/"notes" + word patterns
- **Tier 2 triggers** - imperative forms: "take this down", "write this down", "log this", "record this", "save this"
- **Tier 3 triggers** - declarative/casual: "i have notes", "quick thought", "random thought", "fyi", "here's a note"
- **Tier 4 triggers** - voice dictation variants: "i wanna take these notes", "let me note", "let me jot"
- **"New idea" / "Had an idea" / "Idea:"** triggers
- **Continuation triggers** (ACTIVE only): "more notes", "another note", "one more", "also note", "add to notes"
- **Stop signals** (ACTIVE only): "end notes", "/done", "done with notes", "no more notes", "stop noting"
- **Accent-insensitive matching** for French and Spanish triggers
- **Full-width colon matching** for Japanese and Korean triggers
- **Arabizi support** for Arabic (fikra:, fekra:, mulahaza:, not:)
- **Hinglish hybrid triggers** (Note kar lo:, Note karo:)
- **Russian transliterated triggers** (Zametka:, Ideya:, Mysl:, Napomni:)

### Changed

- Trigger count expanded from 8 patterns to ~55 English + ~50 multilingual
- Trigger matching now uses tiered priority (Tier 1-4) instead of flat list
- Processing section headers adapt to detected language (e.g., Japanese: "アクション", "アイデア")
- First-use onboarding now mentions 11-language support
- All user-facing messages (checkpoints, stop confirmations, zero-notes) now respond in detected language

## [1.0.0] - 2026-03-21

### Added

- Core notetaker skill file (`notetaker.md`)
- 8 trigger patterns: `Note:`, `note:`, `Note to self:`, `Remind me:`, `Remind me to:`, `/note`, `Remember this:`, `Jot down:`, `Capture this:`
- Fuzzy matching for voice dictation (accepts `Note,` `Note.` `Note -` and bare `Note` from dictation tools)
- 4 note types: action, idea, observation, decision
- 14 voice cleanup rules including dictation command stripping, self-correction handling, homophone fixing, and code-switching preservation
- Flexible correction syntax: by number, by keyword, or "fix last note"
- Quick undo: "scratch that", "undo last note", "never mind that last one"
- Structured processing output with Artifact format (markdown with checkboxes on actions)
- Plain text processing variant for Apple Notes and Google Docs
- First-use onboarding for new users
- Proactive checkpointing at 12 and 20 notes
- Multi-note message detection with user guidance
- Compression protocol for long conversations
- Negative examples as anti-drift defense
- Disambiguation rules for trigger patterns and note types
- Example session transcript demonstrating all features
- CI workflow for prompt injection scanning and invisible Unicode detection
