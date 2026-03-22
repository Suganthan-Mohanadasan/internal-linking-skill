---
name: internal-linking
description: "When the user wants to add, review, or manage internal links between blog posts. Use when the user mentions 'internal links,' 'internal linking,' 'cross-linking posts,' 'link between posts,' or 'add links to blog posts.' This skill analyses blog post content for contextually relevant linking opportunities and places them using natural anchor text."
metadata:
  version: 1.0.0
---

# Internal Linking Skill for Claude Code

You are an expert at contextual internal linking for blog content. You identify opportunities where linking between posts genuinely serves the reader, not just distributes link equity.

## Setup

Before first use, you must build the Post Manifest (see section below). This is a table of all published posts on the site with their slugs, titles, tags, and thematic clusters. The skill uses this manifest to identify linking targets.

## Operating Modes

### Mode 1: New Post Linking
When the user writes or publishes a new post:
1. Read the new post's full content
2. Scan the post manifest for contextually relevant targets
3. Suggest up to 3 internal links with reasoning
4. After approval, place them using the Edit tool
5. Also check if any existing posts should link TO the new post (reverse linking)

### Mode 2: Bulk Retrofit
When the user wants to add internal links across all existing posts:
1. Process each published post one at a time
2. For each post, read the full content
3. Identify up to 3 contextually relevant link opportunities
4. Present all suggestions with reasoning before placing any
5. After approval, place links using the Edit tool
6. Move to the next post

### Mode 3: Build Manifest
When the user asks to build or update the manifest:
1. Scan the blog content directory for all published posts
2. Read each post's frontmatter (title, tags, draft status)
3. Group posts into thematic clusters based on tags and content
4. Update the Post Manifest section in this file
5. Move any draft posts to the draft list

## Rules

### Link Limits
- Maximum 3 internal links per post
- 4 only with explicit justification and user approval
- If a post already has internal links, count them first and only add up to the limit

### Contextual Relevance
- The link must serve a reader in that specific paragraph
- Ask: "Would a reader at this point naturally want to read the target post?"
- If the answer is no, skip it, even if the topics are related
- Cross-cluster links are often the most valuable when they're genuinely relevant

### Anchor Text
- Use natural phrases already present in the text
- Never use "click here," "read more," or "check out this post"
- Never use the exact full title of the target post as anchor text
- Partial title matches and descriptive phrases are preferred
- The anchor should read naturally whether the link exists or not

### Link Format
- Always use relative paths: `[anchor text](/blog/slug/)`
- Always include trailing slash
- Links open in the same tab (no target="_blank")
- Never use full URLs

### Placement
- Prefer the first third of the post, but only when natural
- Distribute links across different sections when possible
- Never cluster multiple internal links in the same paragraph

### Safe Zones (where links CAN go)
- Inside paragraph text
- Inside list items
- Inside blockquote text (if natural)

### Forbidden Zones (NEVER place links here)
- Inside code blocks
- Inside image alt text or image references
- Inside component props or attributes
- Inside frontmatter
- Inside existing links (no nested links)
- Inside headings
- Inside bold/italic that would make the link invisible

### Never Link To
- Draft posts from published posts
- The post itself (no self-links)
- External URLs (this skill handles internal links only)

### Idempotency
- Before suggesting links, check if the post already contains internal links
- Never suggest a link to a target that's already linked in the post
- If a post already has 3 or more internal links, skip it unless the user explicitly asks

## Configuration

Customise these values for your site:

| Setting | Value |
|---------|-------|
| Blog content directory | `src/content/blog/` |
| Blog URL prefix | `/blog/` |
| Post format | `.mdx` |
| Draft field | `draft: true` in frontmatter |
| Max links per post | 3 |

## Post Manifest

<!--
  Replace this section with your own posts.
  Run Mode 3 (Build Manifest) to auto-generate this from your blog content.

  Format:
  | Slug | Title | Tags | Cluster |
  |------|-------|------|---------|
  | `my-post` | My Post Title | tag1, tag2 | Cluster Name |
-->

Published posts available for linking:

| Slug | Title | Tags | Cluster |
|------|-------|------|---------|
| | | | |

Draft posts (NEVER link to these from published posts):

| Slug | Title |
|------|-------|
| | |

## Thematic Clusters

<!--
  Group your posts into clusters to help identify cross-linking opportunities.
  Contextual relevance always trumps cluster membership.
-->

| Cluster | Posts |
|---------|-------|
| | |

## Presentation Format

When suggesting links, present each one as:

```
**Link [N]: [target post title]**
- Paragraph: "[surrounding text for context]"
- Anchor: "[proposed anchor text]"
- Target: /blog/[slug]/
- Why: [one sentence on why this serves the reader here]
```

Wait for user approval before placing any links.

## Maintenance

When new posts are published:
1. Add them to the Post Manifest table above
2. Update the cluster assignments
3. Move any draft posts to published if their `draft` field changes to `false`

Or run Mode 3 (Build Manifest) to regenerate automatically.
