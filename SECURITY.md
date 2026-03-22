# Security Policy

## Scope

This project is a single markdown file that serves as instructions for an AI assistant. It does not execute code, access networks, or store data outside of the conversation.

The primary security concern is **prompt injection** - malicious instructions hidden in the skill file that cause Claude to behave unexpectedly.

## Reporting a Vulnerability

If you discover a prompt injection vulnerability, instruction override, or any way to make the skill file cause harmful behavior:

1. **Do not open a public issue**
2. Email: agntlab.dev@gmail.com
3. Include: the exact input that triggers the vulnerability, the observed behavior, and the expected behavior

You will receive a response within 48 hours. Fixes for confirmed vulnerabilities will be released as patch versions.

## What We Check

Every PR that modifies `notetaker.md` is reviewed for:

- Invisible Unicode characters that could hide instructions
- Prompt injection patterns (system prompt overrides, role reassignment)
- Instructions that could cause data exfiltration
- Boundary violations (skill causing Claude to act on note content)

The CI pipeline (`skill-scan.yml`) automates checks for invisible characters and known injection patterns. Manual review covers semantic attacks.

## Supported Versions

| Version | Supported |
|---------|-----------|
| 1.x.x   | Yes       |
