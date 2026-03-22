# Contributing

Thanks for your interest in improving the Notetaker skill. This is a single-file product, so contributions tend to be focused and high-impact.

## Before You Start

**Open an issue first.** All contributions start as an issue - bug report, feature request, or skill improvement. Wait for a `confirmed` or `help-wanted` label before starting work. This prevents wasted effort on changes that don't align with the project direction.

PRs without a linked issue will be closed with a note to open an issue first.

## License

MIT license. By submitting a pull request, you agree that your contributions are licensed under the same terms. No CLA required.

## Design Principles

These guide every decision. PRs that conflict with these principles will be rejected:

1. **Minimal friction** - The skill exists to capture ideas before they're forgotten. Anything that adds steps or cognitive load works against this
2. **Small by design** - The skill file stays under ~1,500 words. Complexity lives in `references/`, not in the main file
3. **Capture, don't act** - Claude records and moves on. It never analyzes, expands, or offers to help with note content during capture
4. **Voice-first** - Primary use case is hands-free dictation. Triggers must work when spoken, not just typed

## What to Contribute

**High value:**
- Voice cleanup rule improvements (especially for non-English speakers)
- Edge cases where classification fails
- Real-world session transcripts showing unexpected behaviour
- Bug fixes with clear reproduction steps

**Lower priority:**
- Formatting changes to the skill file
- README copy suggestions
- Additional note types (the 4-type system is intentional - propose with strong evidence)

## How to Submit

1. Open an issue and wait for `confirmed` or `help-wanted` label
2. Fork the repository
3. Create a branch: `improvement/short-description`
4. Make your changes
5. Test by uploading the modified `notetaker.md` to a Claude Project and running through 10+ notes
6. Submit a PR referencing the issue number

## Skill File Changes

All changes to `notetaker.md` require:
- Manual testing with at least 10 notes across all 4 types
- Testing with messy voice-dictated input (at least 3 examples)
- Boundary testing (does Claude still NOT act on note content?)
- A description of what changed and why in the PR

See `.github/SKILL_REVIEW_CHECKLIST.md` for the full review criteria.

## Review Process

PRs are reviewed within 72 hours. Three outcomes:

- **Approve** - Merged via squash commit
- **Request changes** - One round of revision. Clear feedback on what needs to change
- **Close** - With an explanation. Common reasons:
  - Out of scope for the skill's purpose
  - Adds complexity that outweighs the benefit
  - Conflicts with design principles above
  - No linked issue or issue wasn't confirmed

Rejections are not personal. If you disagree, discuss in the issue - not the PR.

## Reporting Issues

Use the issue templates:
- **Bug report** - Claude does something wrong during capture or processing. Include a conversation transcript
- **Feature request** - A new capability you want. Explain the use case, not just the feature
- **Skill improvement** - A better way to phrase an existing instruction

## Code of Conduct

This project follows the [Contributor Covenant](CODE_OF_CONDUCT.md). Be respectful and constructive.
