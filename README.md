# Internal Linking Skill for Claude Code

A Claude Code skill that adds contextual internal links between blog posts. It reads your content, identifies natural linking opportunities, and places links using anchor text that reads like a human wrote it.

No "click here." No "read more." No stuffing links into headings. Just links that serve the reader.

## What it does

- Scans blog posts for contextually relevant linking opportunities
- Suggests up to 3 internal links per post with reasoning
- Uses natural phrases already in the text as anchor text
- Handles both new post linking and bulk retrofit across all posts
- Tracks a post manifest to avoid duplicate links and respect limits
- Never links to draft posts, never self-links, never nests links

## Install

Copy the skill into your Claude Code project:

```bash
mkdir -p .claude/skills/internal-linking
cp SKILL.md .claude/skills/internal-linking/SKILL.md
```

Or clone directly:

```bash
git clone https://github.com/Suganthan-Mohanadasan/internal-linking-skill.git
cp internal-linking-skill/SKILL.md .claude/skills/internal-linking/SKILL.md
```

## Setup

### 1. Update the configuration

Open `.claude/skills/internal-linking/SKILL.md` and update the Configuration table to match your site:

| Setting | Default |
|---------|---------|
| Blog content directory | `src/content/blog/` |
| Blog URL prefix | `/blog/` |
| Post format | `.mdx` |
| Draft field | `draft: true` |
| Max links per post | 3 |

### 2. Build your post manifest

Ask Claude Code:

```
Build the internal linking manifest
```

This scans your blog directory, reads frontmatter from every post, and populates the Post Manifest table in the skill file. It also groups posts into thematic clusters.

## Usage

### Link a new post

After publishing a new post:

```
Add internal links to my new post about [topic]
```

Claude will read the post, check the manifest for relevant targets, and suggest links with reasoning. It also checks if existing posts should link back to the new one.

### Bulk retrofit

To add internal links across all existing posts:

```
Add internal links across all my blog posts
```

Claude processes each post one at a time, presents suggestions, and waits for approval before placing any links.

### Update the manifest

When you've added or removed posts:

```
Update the internal linking manifest
```

## Rules it follows

**Link limits:** Maximum 3 per post. 4 only with explicit justification.

**Contextual relevance:** Every link must serve the reader at that specific point. "Would someone reading this paragraph naturally want to read the target?" If no, the link gets skipped.

**Anchor text:** Natural phrases from the existing text. Never the full title of the target post. Never generic phrases.

**Placement:** Prefers the first third of the post. Distributes across sections. Never clusters multiple links in the same paragraph.

**Forbidden zones:** No links inside code blocks, headings, image alt text, frontmatter, component props, or existing links.

**Idempotency:** Checks for existing internal links before suggesting. Skips posts that already have 3 or more unless explicitly asked.

## Example output

```
**Link 1: SEO Competitor Analysis**
- Paragraph: "Understanding who you're competing against in search results is half the battle."
- Anchor: "competing against in search results"
- Target: /blog/seo-competitor-analysis/
- Why: Reader discussing competitive positioning would benefit from the detailed rival identification methodology

**Link 2: Logfile Analysis for SEO**
- Paragraph: "If you're not looking at how search engines actually crawl your site, you're guessing."
- Anchor: "how search engines actually crawl your site"
- Target: /blog/logfile-analysis-seo/
- Why: Direct relevance to crawl behaviour analysis, extends the reader's understanding of technical auditing
```

## License

MIT
