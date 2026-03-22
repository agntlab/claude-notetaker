# Maintainer Guide

How this project is maintained, so contributors know what to expect.

## Contribution Flow

1. **Issue first** - All work starts as an issue. Bug reports, feature requests, and improvements go through issue templates
2. **Triage** - Issues are reviewed within 48 hours and labelled: `confirmed`, `help-wanted`, `cannot-reproduce`, or closed with explanation
3. **PR** - Once an issue has `confirmed` or `help-wanted`, contributors can fork, branch, and submit a PR referencing the issue
4. **Review** - PRs are reviewed within 72 hours. One round of revision if needed
5. **Merge** - Squash merge to keep history clean. CHANGELOG updated with each release

## Bug Severity

| Label | Definition | Response Time |
|-------|-----------|---------------|
| `critical` | Skill breaks Claude's behaviour (e.g., acts on note content, ignores triggers entirely) | Fixed within 1 week by maintainer |
| `minor` | Edge case, cosmetic, or non-blocking issue | Labelled `help-wanted` for community |

## Versioning

This project uses [Semantic Versioning](https://semver.org/):

- **Patch** (3.0.x) - Bug fixes, typo corrections
- **Minor** (3.x.0) - New triggers, new reference files, non-breaking improvements
- **Major** (x.0.0) - Breaking changes to skill structure, trigger removals

## Release Process

1. Changes merged to `main` via squash merge
2. CHANGELOG.md updated
3. Git tag created (e.g., `v3.0.1`)
4. GitHub Release published with CHANGELOG entry as notes

## Branch Protection

- All changes to `main` go through PRs
- CI must pass before merge
- Maintainer approval required
- Squash merge only

## Stale Issues

- Issues with no activity for 30 days receive a `stale` label
- 14 more days of silence and the issue is closed
- Stale issues can be reopened with new information

## If a Bad Merge Gets Through

1. Revert via `git revert` (not force push)
2. Comment on the original PR explaining what broke
3. Open a new issue for the proper fix

## Decision Log

Non-obvious design decisions are recorded in `docs/decisions/`. Format: `YYYY-MM-DD-decision-title.md` with context, options considered, and reasoning. This prevents re-litigating settled decisions.
