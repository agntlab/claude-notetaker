# Notetaker

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-3.0.0-blue.svg)](CHANGELOG.md)
[![CI](https://github.com/agntlab/claude-notetaker/actions/workflows/skill-scan.yml/badge.svg)](https://github.com/agntlab/claude-notetaker/actions/workflows/skill-scan.yml)

Voice-friendly note capture for Claude. Capture ideas hands-free while driving, walking, or cooking. Fillers, false starts, and dictation artifacts are cleaned up automatically. Notes are organized into handoff files you can pick up in future sessions.

Works with **Claude Code**, **Claude.ai** (Projects), and **Claude Desktop** (Cowork). Zero config. No API keys. No dependencies.

## How It Works

Say `brain dump` and talk freely. Claude captures each idea, cleans up the dictation, and classifies it. Say `done` when finished. Say `organize` to cluster notes by topic and save them as handoff files with ready-to-use prompts.

```
You:    brain dump
Claude: Go ahead - everything you say is captured until you say 'done'.

You:    the onboarding flow has too many steps,
        we should add dark mode to settings,
        and the API response time spiked after the last deploy

Claude:
[NOTE #1 | idea] Reduce steps in the onboarding flow.
[NOTE #2 | action] Add dark mode to the settings page.
[NOTE #3 | observation] API response time spiked after last deploy.

You:    done
Claude: 3 notes captured. Say 'organize' to cluster and save as handoff files.
```

For quick one-offs without entering brain dump mode:

```
Note: switched from REST to GraphQL for the dashboard
Remind me to: write tests for the payment webhook
Action: refactor the auth middleware
```

## Setup

**Claude Code** - Add as a skill at `.claude/skills/notetaker/SKILL.md`. Copy `references/` alongside it for the note router.

**Claude.ai** - Download [`notetaker.md`](notetaker.md), open a Project, click **Edit Project** > **Add content**, upload.

**Claude Desktop** - Download [`notetaker.md`](notetaker.md), start a Cowork session, upload as a skill.

## Features

| Feature | How |
|---------|-----|
| **Brain dump** | `brain dump` - talk freely, say `done` to finish |
| **Single notes** | `Note:` / `Remind me:` / `Action:` |
| **Voice cleanup** | Fillers, false starts, and dictation errors auto-cleaned |
| **4 note types** | action, idea, observation, decision - auto-classified |
| **Corrections** | `fix note 3: new text` / `scratch that` / `delete note 5` |
| **Simple list** | `process notes` - grouped by type with checkboxes on actions |
| **Organize** | `organize` - cluster by topic, save as handoff files |
| **Resume** | Next session, Claude lists your saved ideas. Pick one and go |

## The Pipeline

```
Capture             Organize              Resume
───────             ────────              ──────
brain dump    →     organize        →     Next session:
just talk           clusters by topic     "You have 3 ideas"
done                saves handoff files   Pick a number → go
```

## FAQ

<details>
<summary>What's the difference between "process notes" and "organize"?</summary>

"Process notes" gives you a simple grouped list. "Organize" clusters by topic, tags complexity (quick/medium/project), and saves handoff files you can pick up in future sessions.
</details>

<details>
<summary>Can Claude act on my notes during capture?</summary>

No. Claude records and moves on. It will not analyze, expand, or offer to help with note content. After organizing, you choose when to act.
</details>

<details>
<summary>Does it work with voice dictation?</summary>

Yes - that's the primary use case. Fillers (um, like, you know), false starts ("no wait, actually..."), and dictation command words ("period", "comma") are cleaned up automatically. Non-English words are preserved.
</details>

<details>
<summary>Does it work with other languages?</summary>

Triggers are English-only, but note content can be in any language. Brain dump mode captures everything regardless of language.
</details>

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). The most impactful contributions are voice cleanup improvements, brain dump mode testing, and real-world session transcripts.

## License

[MIT](LICENSE)
