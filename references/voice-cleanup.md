# Voice Cleanup Rules (Full Reference)

The main skill file has 8 consolidated rules. This is the full 14-rule version for edge cases.

1. Remove fillers: um, uh, like (filler usage), you know, so basically, I mean
2. Remove conversational framing: "so what happened was", "the thing is"
3. Convert hedging to direct ONLY when the user clearly has a position. When in doubt, preserve the hedge
4. Preserve genuine uncertainty: "not sure if X or Y" stays as-is
5. Convert reported dialogue to reported speech
6. Preserve approximate language: "like 30%" becomes "approximately 30%"
7. Preserve emotional signals as neutral assessments: "insane deadline" becomes "aggressive deadline", "terrible UX" becomes "poor UX", "amazing results" becomes "strong results"
8. Do NOT add words, conclusions, or context the user did not provide
9. Maximum approximately 40% word reduction. If your cleanup is longer than the original, you have added content - fix it
10. Preserve code snippets, URLs, file paths, and technical identifiers verbatim
11. Strip dictation command words ("period", "comma", "new line", "new paragraph", "exclamation point", "question mark") and convert to actual punctuation when clearly formatting instructions, not content
12. When self-correction markers appear ("no wait", "actually", "I mean", "scratch that", "let me start over"), capture only the corrected version
13. Fix obvious dictation transcription errors (homophones, phonetic misspellings) when intended word is clear from context. When ambiguous, preserve original
14. Preserve non-English words and code-switching. If the user said it, it stays. Do not translate or strip
