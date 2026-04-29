---
name: wechat-post-skill
description: End-to-end WeChat Official Account (微信公众号) publishing workflow for high-traffic posts: topic selection, article drafting, humanization, AI cover/body images, self-review, draft publishing, and cleanup. Use when the user asks to 发公众号, 写公众号文章, 做公众号推送, 发布草稿箱, or package/operate a complete WeChat article workflow.
---

# WeChat Post Skill

Use this skill to run a complete WeChat Official Account article pipeline, not just publish Markdown. The goal is a polished draft in the WeChat Official Account draft box.

## Core workflow

1. **Pick topic**
   - If the user already gives a topic: use it directly.
   - If not: search current hot topics and offer 3 options.
   - Prefer topics with local relevance, clear public interest, conflict, usefulness, or share value.

2. **Write article**
   - Create an article folder under the project/workspace, e.g. `articles/YYYY-MM-DD-short-title/`.
   - Write `article.md` at roughly 900–1300 Chinese characters unless the user asks otherwise.
   - Include image placeholders in Markdown syntax only: `![](img0.png)`, `![](img1.png)`, etc.
   - Use a strong opening, varied paragraph rhythm, useful subheads, and a hard ending with a comment/share prompt.

3. **Humanize/self-edit**
   - Remove generic AI phrases, over-formal transitions, excessive bold, and excessive em dashes.
   - Keep the article concrete, local, useful, and conversational.
   - If an AI-humanizer skill/tool exists, use it for analysis and targeted revision.

4. **Generate images**
   - Generate/copy into the article folder:
     - `cover.png` — WeChat cover, 2.35:1 preferred.
     - `img0.png` … — body images, usually 16:9.
   - Use the existing image-generation capability available in the environment. If a Gemini browser image skill exists, use it for browser-based Gemini generation.
   - For fragile browser image workflows, read `references/gemini-browser-notes.md`.

5. **Self-review before publishing**
   - Verify image placeholders use `![](imgX.png)`.
   - Verify all referenced image files exist.
   - Verify no secrets or local-only credentials are embedded in `article.md`.
   - Choose theme by topic:
     - tech/military/hard analysis: `modern`
     - finance/business/career: `grace`
     - emotion/life/soft topics: `simple`
     - controversy/hot news: `default`
     - unsure: `grace`

6. **Publish to draft box**
   - Use the existing WeChat publishing skill/tool if available, especially `baoyu-post-to-wechat`.
   - Preferred command pattern:

```powershell
$env:WECHAT_APP_ID="<from-env-or-local-config>"
$env:WECHAT_APP_SECRET="<from-env-or-local-config>"
cd "<article-folder>"
bun "<baoyu-post-to-wechat>/scripts/wechat-api.ts" article.md --theme <theme> --submit --cover "cover.png"
```

   - Do not hardcode credentials into the skill or repo.
   - If API publishing fails because of WeChat IP whitelist, check current outbound IP and ask the user to whitelist it.

7. **Clean up**
   - Remove temporary generated-image downloads when safe.
   - Keep the article folder and source assets unless the user asks to delete them.

## Quality bar

- The article must have a clear traffic goal: clicks, saves, comments, shares, or follower growth.
- The first 100–300 characters must create tension, curiosity, usefulness, or emotional resonance.
- Every image must serve the article: cover for click-through, body images for pacing/comprehension.
- Never publish before self-review.

## Safety and secrets

- Never commit or publish `WECHAT_APP_SECRET`, tokens, cookies, real account secrets, or `.env` files.
- Keep account-specific settings in local config, environment variables, or ignored files.
- For public GitHub repos, include placeholders and setup notes only.

## Optional references

- `references/content-playbook.md` — article structure, title/opening/ending patterns.
- `references/gemini-browser-notes.md` — operational notes for browser-based Gemini image generation.
- `references/publishing-notes.md` — WeChat draft publishing checks and common failures.

