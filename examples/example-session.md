# Example Session

A real conversation showing the Notetaker skill in action. The user is working on a side project, using Claude for both note-taking and regular questions. Demonstrates triggers, voice cleanup, corrections, quick undo, and processing.

---

**User:** Hello, what can this project do?

**Claude:**

> Notetaker is active. Three ways to capture:
>
> - **Note:** Quick one-off capture
> - **Remind me:** / **Action:** Also capture as notes
> - **Brain dump** - say it once, then talk freely until you say 'done'
>
> Say 'process notes' to see them organized, or 'organize notes' to cluster and save as handoff files.

---

**User:** What's the difference between SQLite and Postgres for a small app?

**Claude:** SQLite is great for single-user apps, prototypes, and embedded systems - zero config, just a file. Postgres is better once you need concurrent writes, full-text search, or plan to scale. For a side project that might grow, Postgres is usually worth the setup cost.

---

**User:** Note: going with Postgres for the recipe app backend

**Claude:**

[NOTE #1 | decision] Going with Postgres for the recipe app backend.

---

**User:** Note: um so like the image upload is taking forever, you know, around 12 seconds for a single photo

**Claude:**

[NOTE #2 | observation] Image upload takes approximately 12 seconds per photo.

---

**User:** Can you explain what WebP is and whether it's worth converting images?

**Claude:** WebP is a modern image format by Google that typically reduces file size by 25-35% compared to JPEG with similar quality. Most browsers support it now. For an app with image uploads, converting to WebP server-side can significantly reduce storage costs and load times.

---

**User:** Remind me to: look into sharp for server-side image compression

**Claude:**

[NOTE #3 | action] Look into sharp for server-side image compression.

---

**User:** Note: what if we let users tag recipes by cuisine and dietary restrictions

**Claude:**

[NOTE #4 | idea] Let users tag recipes by cuisine and dietary restrictions.

---

**User:** Note: we decided to use Stripe for the premium tier, not Paddle

**Claude:**

[NOTE #5 | decision] Using Stripe for the premium tier, not Paddle.

---

**User:** Note: competitor app just added AI-generated meal plans - saw it on Product Hunt

**Claude:**

[NOTE #6 | observation] Competitor launched AI-generated meal plans on Product Hunt.

---

**User:** Action: set up GitHub Actions for CI before merging the PR

**Claude:**

[NOTE #7 | action] Set up GitHub Actions for CI before merging the PR.

---

**User:** fix the note about image upload: it's 8 seconds, not 12

**Claude:**

[NOTE #2 | observation] (corrected) Image upload takes approximately 8 seconds per photo.

---

**User:** scratch that

*(Deletes note #7, the most recently captured note)*

**Claude:**

[NOTE #7] DELETED

---

**User:** Note: the search function should handle typos, maybe use fuzzy matching

**Claude:**

[NOTE #8 | idea] Search function should handle typos - consider fuzzy matching.

---

**User:** Note: the auth token -- no wait, the refresh token expires too quickly

**Claude:**

[NOTE #9 | observation] The refresh token expires too quickly.

---

**User:** Note: need to add rate limiting comma especially on the public API period should cap at 100 requests per minute

**Claude:**

[NOTE #10 | action] Add rate limiting, especially on the public API. Cap at 100 requests per minute.

---

**User:** Note: maybe we could add a sharing feature where users generate a link to their recipe collection

**Claude:**

[NOTE #11 | idea] Sharing feature where users generate a link to their recipe collection.

---

**User:** process notes

**Claude:**

*Creates an Artifact:*

# Notes - 2026-03-21

## Actions
- [ ] Look into sharp for server-side image compression (#3)
- [ ] Add rate limiting, especially on the public API. Cap at 100 requests per minute (#10)

## Decisions
- Going with Postgres for the recipe app backend (#1)
- Using Stripe for the premium tier, not Paddle (#5)

## Ideas
- Let users tag recipes by cuisine and dietary restrictions (#4)
- Search function should handle typos - consider fuzzy matching (#8)
- Sharing feature where users generate a link to their recipe collection (#11)

## Observations
- Image upload takes approximately 8 seconds per photo (#2)
- Competitor launched AI-generated meal plans on Product Hunt (#6)
- The refresh token expires too quickly (#9)

---
*11 captured | 1 deleted | 1 corrected | 2026-03-21*
