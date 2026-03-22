# Contributing

Thanks for your interest in improving the Notetaker skill. This is a single-file product, so contributions tend to be focused and high-impact.

## License

This project uses MIT license. By submitting a pull request, you agree that your contributions are licensed under the same MIT license. No CLA required.

## What to Contribute

**High value:**
- New trigger patterns that feel natural
- Voice cleanup rule improvements (especially for non-English speakers)
- Edge cases where classification fails
- Real-world session transcripts showing unexpected behavior

**Lower priority:**
- Formatting changes to the skill file
- README copy suggestions
- Additional note types (the 4-type system is intentional - propose with strong evidence)

## How to Submit

1. Fork the repository
2. Create a branch: `improvement/short-description`
3. Make your changes
4. Test by uploading the modified `notetaker.md` to a Claude Project and running through 10+ notes
5. Submit a PR using the template

## Skill File Changes

All changes to `notetaker.md` require:
- Manual testing with at least 10 notes across all 4 types
- Testing with messy voice-dictated input (at least 3 examples)
- Boundary testing (does Claude still NOT act on note content?)
- A description of what changed and why in the PR

See `.github/SKILL_REVIEW_CHECKLIST.md` for the full review criteria.

## Reporting Issues

Use the issue templates:
- **Bug report:** Claude does something wrong during capture or processing
- **Feature request:** A new capability you want
- **Skill improvement:** A better way to phrase an existing instruction

Include a conversation transcript when reporting bugs. Copy-paste the exchange showing the problem.

## Code of Conduct

This project follows the [Contributor Covenant](CODE_OF_CONDUCT.md). Be respectful and constructive.
