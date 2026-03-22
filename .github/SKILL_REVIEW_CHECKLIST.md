# Skill Review Checklist

Use this checklist when reviewing any PR that modifies `notetaker.md`.

## Automated (CI)

- [ ] No invisible Unicode characters detected
- [ ] No known prompt injection patterns found
- [ ] YAML frontmatter valid (name, description, version present)

## Manual - Functional

- [ ] All 3 trigger patterns still work (`Note:` with voice variants, `Remind me:`, `Action:`)
- [ ] Brain dump mode works (enter with `brain dump`, exit with `done`)
- [ ] Non-triggers do not fire ("I should note that...", "write a note to the team", "please note")
- [ ] All 4 note types classify correctly (action, idea, observation, decision)
- [ ] Voice cleanup reduces without adding content
- [ ] Note correction updates in-place with "(corrected)" marker
- [ ] Note deletion marks with "DELETED"
- [ ] Processing output groups by type with correct format
- [ ] Checkboxes appear only on actions
- [ ] Empty sections are omitted from processing output
- [ ] Footer counts are accurate (captured, deleted, corrected)
- [ ] Zero-notes edge case handled

## Manual - Boundaries

- [ ] Claude does NOT analyze or comment on note content
- [ ] Claude does NOT ask follow-up questions after capturing
- [ ] Claude does NOT offer to act on note content
- [ ] Claude does NOT create Artifacts for individual notes
- [ ] Claude does NOT proactively suggest capturing something as a note
- [ ] Claude resumes previous conversation after capture

## Manual - Voice Input

- [ ] Fillers removed (um, uh, like, you know, so basically)
- [ ] Conversational framing removed ("so what happened was")
- [ ] Genuine uncertainty preserved ("not sure if X or Y")
- [ ] Approximate language preserved ("approximately 30%")
- [ ] Emotional signals neutralized ("aggressive" not "insane")
- [ ] Cleaned text is shorter than original (max ~40% reduction)

## Security

- [ ] No instructions that could override system behavior
- [ ] No instructions that could cause data exfiltration
- [ ] No hidden text or formatting tricks
- [ ] Changes are consistent with the stated purpose of the skill
