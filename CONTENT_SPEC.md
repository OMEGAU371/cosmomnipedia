# Cosmomnipedia Content Specification

Version: 0.1 draft
Date: 2026-04-28

This specification defines the first stable content format for Cosmomnipedia.

Cosmomnipedia is a visual-first wiki. The format must support polished presentation, but article files should describe knowledge structure rather than arbitrary styling.

## 1. Scope

This document defines:

- Article source format: CosmoDoc v0.1.
- Site data boundaries.
- Core article blocks.
- Inline syntax.
- Migration behavior from the current HTML fragment.
- Explicitly unsupported or delayed features.

This document does not define:

- Final frontend implementation.
- Login/authentication.
- Comment backend implementation.
- Payment or sponsorship logic.
- Build tooling.

## 2. Format Choice

CosmoDoc v0.1 uses:

```text
Markdown body
+ YAML frontmatter
+ Cosmo block containers
```

Rationale:

- Markdown is readable and editable.
- Frontmatter handles article metadata cleanly.
- Cosmo blocks provide structured wiki/visual components without forcing every article into verbose JSON.
- Existing HTML can be preserved during migration through a transitional field.

## 3. File Layout

Recommended structure:

```text
data/
  site.json
  home.json
  categories.json
  authors.json
  articles/
    index.json
    anatolia-hieroglyphs.md
    anatolia-hieroglyphs.assets.json
```

### 3.1 Article Files

Article source files should live in:

```text
data/articles/{slug}.md
```

Example:

```text
data/articles/anatolia-hieroglyphs.md
```

### 3.2 Article Index

`data/articles/index.json` stores lightweight routing and discovery data.

It should not duplicate full article bodies.

Minimum shape:

```json
{
  "version": "0.1",
  "articles": [
    {
      "slug": "anatolia-hieroglyphs",
      "title": "安纳托利亚象形文字",
      "summary": "一种源于安纳托利亚中部的古代书写系统。",
      "category": "writing-systems",
      "tags": ["卢维语", "象形文字", "青铜时代"],
      "status": "draft",
      "updatedAt": "2026-04-28T00:00:00+08:00"
    }
  ],
  "redirects": {
    "luwian-hieroglyphs": "anatolia-hieroglyphs"
  }
}
```

## 4. Article Frontmatter

Every article must start with YAML frontmatter.

Required fields:

```yaml
---
cosmodoc: 0.1
slug: anatolia-hieroglyphs
title: 安纳托利亚象形文字
subtitle: 探索源自安纳托利亚心脏地带的古代书写系统
summary: 一种源于安纳托利亚中部的本土语素文字。
category: writing-systems
tags:
  - 卢维语
  - 象形文字
  - 青铜时代
aliases:
  - 卢维象形文字
  - 赫梯象形文字
status: draft
theme: bronze
cover:
  src: https://upload.wikimedia.org/wikipedia/commons/thumb/7/75/Hamath_inscription.jpg/500px-Hamath_inscription.jpg
  alt: 哈马铭文
  credit: Wikimedia Commons
  license: Public domain
author: omega
contributors:
  - omega
createdAt: 2026-02-22T14:50:00+08:00
updatedAt: 2026-04-28T00:00:00+08:00
publishedAt: null
comments: true
license: CC BY-SA 4.0
sourcePolicy: cited
---
```

### 4.1 Required Field Rules

| Field | Type | Rule |
| --- | --- | --- |
| `cosmodoc` | string/number | Required. Current value: `0.1`. |
| `slug` | string | Required. Lowercase URL-safe id. |
| `title` | string | Required. Human-facing title. |
| `subtitle` | string | Required. Used by article hero. |
| `summary` | string | Required. Used by cards/search. |
| `category` | string | Required. Must match `categories.json`. |
| `tags` | string[] | Required. Can be empty only for system pages. |
| `aliases` | string[] | Required. Can be empty. |
| `status` | enum | Required. See section 4.2. |
| `theme` | string | Required. Named visual theme token. |
| `cover` | object | Required for normal articles. |
| `author` | string | Required. Must match `authors.json`. |
| `contributors` | string[] | Required. |
| `createdAt` | ISO date | Required. |
| `updatedAt` | ISO date | Required. |
| `publishedAt` | ISO date/null | Required. |
| `comments` | boolean | Required. |
| `license` | string | Required. |
| `sourcePolicy` | enum | Required. |

### 4.2 Article Status

Allowed status values:

```text
draft
stub
review
stable
featured
archived
```

Meaning:

- `draft`: visible only if deliberately published in draft mode.
- `stub`: short or incomplete article.
- `review`: content exists but requires verification.
- `stable`: normal trusted article.
- `featured`: high-quality curated article.
- `archived`: kept for history, not actively maintained.

### 4.3 Source Policy

Allowed values:

```text
cited
translated
original
mixed
unclear
```

`unclear` should be used only during migration.

## 5. Markdown Baseline

CosmoDoc v0.1 supports these Markdown features:

- Paragraphs.
- `##`, `###`, `####` headings.
- Bold.
- Italic.
- Strikethrough.
- Inline code.
- Fenced code blocks.
- Blockquotes.
- Ordered and unordered lists.
- Tables.
- Horizontal rules.
- Links.
- Images.
- Footnotes, if renderer supports standard Markdown footnote syntax.

Article body should not use `#` headings. The article title comes from frontmatter.

## 6. Inline Extensions

### 6.1 Wiki Links

Syntax:

```md
[[安纳托利亚]]
[[anatolia|安纳托利亚]]
```

Rules:

- If the target contains spaces or non-Latin characters, renderer resolves it through aliases and article index.
- If the target is missing, render as a missing-entry link.

### 6.2 Underline

Syntax:

```md
{u}underlined text{/u}
```

Renderer output:

- Semantic underline styling.
- No inline CSS.

### 6.3 Superscript and Subscript

Syntax:

```md
H{sub}2{/sub}O
x{sup}2{/sup}
```

### 6.4 Semantic Mark

v0.2 candidate.

Reserved syntax:

```md
{mark tone=important}text{/mark}
```

`tone` must be a named token, not arbitrary color.

### 6.5 Masked Text

v0.2 candidate.

Reserved syntax:

```md
{mask}hidden text{/mask}
{blur}blurred text{/blur}
```

## 7. Cosmo Blocks

Cosmo blocks use container syntax:

```md
:::blockName key="value" key2="value"
content
:::
```

Rules:

- Block names are lowercase kebab-case or lowercase single words.
- Attributes are key/value pairs.
- Unknown blocks should render a clear fallback rather than break the page.
- Raw scripts are never allowed.

## 8. Required v0.1 Blocks

### 8.1 Lead

Purpose: concise article opening/definition.

Syntax:

```md
:::lead
安纳托利亚象形文字是一种源于安纳托利亚中部的古代书写系统。
:::
```

Rules:

- One lead block per article.
- Should appear before the first `##` heading.

### 8.2 Infobox

Purpose: structured wiki summary panel.

Syntax:

```md
:::infobox title="安纳托利亚象形文字"
image:
  src: https://example.com/image.jpg
  alt: 哈马铭文
  caption: 哈马的一处安纳托利亚象形文字铭文
rows:
  - label: 文字类型
    value: 语素文字
  - label: 使用时期
    value: 公元前14-7世纪
  - label: ISO 15924
    value: Hluw (080)
:::
```

Implementation note:

- The final syntax can be YAML-like inside the block.
- Renderer must output the current `.wiki-infobox` DOM shape during migration.

### 8.3 Note

Purpose: visually distinct information/tip/warning panels.

Syntax:

```md
:::note type="tip" title="阅读提示"
本文包含 IPA 转写和古文字编号。
:::
```

Allowed types:

```text
info
tip
warning
danger
quote
```

### 8.4 Media

Purpose: image/video/audio display with caption and source.

Syntax:

```md
:::media type="image" variant="card" align="right"
src: https://example.com/hamath.jpg
alt: 哈马铭文
caption: 哈马的一处安纳托利亚象形文字铭文
credit: Wikimedia Commons
license: Public domain
source: https://commons.wikimedia.org/
:::
```

Rules:

- `alt` is required for images.
- `caption` is required for visual media.
- `credit` and `source` should be provided when known.
- Media source list can be generated from these fields.

Allowed media types in v0.1:

```text
image
```

Reserved for v0.2:

```text
video
audio
svg
gallery
```

### 8.5 Collapse

Purpose: optional detail without overwhelming the page.

Syntax:

```md
:::collapse title="更多细节" open=false
这里是可折叠内容。
:::
```

Rules:

- Must be accessible by keyboard.
- Should not hide core article facts by default.

### 8.6 Copy Block

Purpose: copyable names, transliterations, commands, or snippets.

Syntax:

````md
:::copy title="转写示例"
```text
ta-/da-
la-
```
:::
````

### 8.7 Navbox

Purpose: wiki navigation template.

Syntax:

```md
:::navbox title="安纳托利亚语族"
groups:
  - label: 赫梯-卢维语支
    links:
      - title: 赫梯语
        target: hittite
      - title: 卢维语
        target: luwian
  - label: 相关文字
    links:
      - title: 赫梯楔形文字
        target: hittite-cuneiform
:::
```

Renderer must output a visual navbox equivalent to the current design.

### 8.8 References

Purpose: bibliography and citations.

Preferred v0.1 syntax:

```md
## 参考文献

[^ref-payne-2004]: Payne, A. (2004). *Hieroglyphic Luwian*. Wiesbaden: Harrassowitz.
```

Citation syntax:

```md
安纳托利亚象形文字由约 500 个符号组成。[^ref-payne-2004]
```

Rules:

- Reference ids should be stable and descriptive.
- Numeric display can be generated by renderer.

### 8.9 External Links

Syntax:

```md
:::links title="外部链接"
- [Wikipedia: Anatolian hieroglyphs](https://en.wikipedia.org/wiki/Anatolian_hieroglyphs)
- [Omniglot: Anatolian Hieroglyphs](https://omniglot.com/writing/anatolian.htm)
:::
```

External links should open through the site's existing confirmation behavior where applicable.

## 9. Derived Blocks

These should be generated by the renderer, not written manually unless overridden.

### 9.1 Table of Contents

Generated from `##` and `###` headings.

Optional heading summary:

```md
## 简介 {summary="源自安纳托利亚中部的古代书写系统。"}
```

### 9.2 Sources List

Generated from media blocks with `source`, `credit`, and `license`.

### 9.3 Word Cloud

Generated from article text and wiki links.

Article may disable it:

```yaml
wordCloud: false
```

### 9.4 Related Articles

Generated from tags/category initially.

Manual override can be added later:

```yaml
related:
  - luwian
  - hittite
```

## 10. v0.2 Reserved Blocks

These names are reserved and should not be used for other meanings:

```text
gallery
timeline
tabs
link-card
chart
mermaid
audio
video
svg
map
compare
```

## 11. Experimental Blocks

Experimental blocks are trusted-author features.

They must not be enabled for public submissions without review.

Reserved names:

```text
html-preview
svg-raw
interactive-demo
custom-style
```

Rules:

- No raw scripts in normal rendering.
- HTML preview must be sandboxed.
- Custom style must be scoped.
- Renderer must show an explicit experimental badge.

## 12. Site Data Specifications

### 12.1 `site.json`

Minimum shape:

```json
{
  "version": "0.1",
  "name": "Cosmomnipedia",
  "displayName": "寰普百科",
  "description": "审美驱动的视觉百科。",
  "basePath": "/cosmomnipedia",
  "logo": "/cosmomnipedia/cosmomnipedia-logo.png",
  "defaultTheme": "cosmic",
  "themes": ["cosmic", "bronze", "nocturne"],
  "navigation": [
    { "label": "首页", "href": "/" },
    { "label": "分类", "href": "/categories" },
    { "label": "随机", "action": "random" }
  ],
  "comments": {
    "provider": "none",
    "enabled": false
  }
}
```

### 12.2 `authors.json`

Minimum shape:

```json
{
  "version": "0.1",
  "authors": [
    {
      "id": "omega",
      "name": "OMEGAU371",
      "avatar": "/assets/authors/omega.png",
      "bio": "Cosmomnipedia creator",
      "links": []
    }
  ]
}
```

### 12.3 `categories.json`

Minimum shape:

```json
{
  "version": "0.1",
  "categories": [
    {
      "id": "writing-systems",
      "title": "文字系统",
      "description": "书写系统、符号体系与文字史。",
      "parent": null,
      "theme": "bronze"
    }
  ]
}
```

### 12.4 `home.json`

`home.json` is v0.2, not required for the first migration.

It should eventually define homepage sections, featured entries, topic cards, and sponsor/friendly links.

## 13. Comments

Article format only controls whether comments are enabled:

```yaml
comments: true
```

Comment provider belongs to `site.json`.

Recommended future providers:

- `giscus` for GitHub Discussions.
- `utterances` for GitHub Issues.
- `custom` for a future backend.

Self-hosted comments are explicitly out of v0.1 scope.

## 14. Security and Style Constraints

Normal articles must not contain:

- Raw `<script>`.
- Inline event handlers.
- Arbitrary inline CSS.
- External iframe embeds without an approved block.
- Unscoped custom styles.

Renderer should sanitize or reject unsafe content.

Content should use semantic blocks and tokens instead of arbitrary styling.

Examples:

Use:

```md
:::note type="warning"
...
:::
```

Avoid:

```html
<div style="color:red;font-size:48px">
```

## 15. Transitional HTML Support

During migration, an article may use:

```yaml
legacyContent: true
```

And include:

```md
:::legacy-html
<!-- existing trusted article HTML fragment -->
:::
```

Rules:

- Only trusted local authors may create or edit `legacy-html`.
- This block exists to preserve the current article while migrating section by section.
- New articles should not use it.
- The renderer must mark it internally as deprecated.

## 16. Example Article

```md
---
cosmodoc: 0.1
slug: anatolia-hieroglyphs
title: 安纳托利亚象形文字
subtitle: 探索源自安纳托利亚心脏地带的古代书写系统
summary: 一种源于安纳托利亚中部的本土语素文字。
category: writing-systems
tags: [卢维语, 象形文字, 青铜时代]
aliases: [卢维象形文字, 赫梯象形文字]
status: draft
theme: bronze
cover:
  src: https://upload.wikimedia.org/wikipedia/commons/thumb/7/75/Hamath_inscription.jpg/500px-Hamath_inscription.jpg
  alt: 哈马铭文
  credit: Wikimedia Commons
  license: Public domain
author: omega
contributors: [omega]
createdAt: 2026-02-22T14:50:00+08:00
updatedAt: 2026-04-28T00:00:00+08:00
publishedAt: null
comments: true
license: CC BY-SA 4.0
sourcePolicy: unclear
---

:::lead
安纳托利亚象形文字是一种源于安纳托利亚中部的古代书写系统。
:::

:::infobox title="安纳托利亚象形文字"
image:
  src: https://upload.wikimedia.org/wikipedia/commons/thumb/7/75/Hamath_inscription.jpg/250px-Hamath_inscription.jpg
  alt: 哈马铭文
  caption: 哈马的一处安纳托利亚象形文字铭文
rows:
  - label: 文字类型
    value: 语素文字
  - label: 使用时期
    value: 公元前14-7世纪
  - label: ISO 15924
    value: Hluw (080)
:::

## 简介 {summary="源自安纳托利亚中部的古代书写系统。"}

**安纳托利亚象形文字**是一种源于[[安纳托利亚]]中部的本土语素文字。[^ref-payne-2004]

:::note type="tip" title="阅读提示"
本文包含 IPA 转写和古文字编号。
:::

:::media type="image" variant="card" align="right"
src: https://example.com/map.png
alt: 安纳托利亚象形文字分布图
caption: 安纳托利亚象形文字的地理分布。
credit: Wikimedia Commons
license: Public domain
source: https://commons.wikimedia.org/
:::

## 参考文献

[^ref-payne-2004]: Payne, A. (2004). *Hieroglyphic Luwian*. Wiesbaden: Harrassowitz.
```

## 17. Migration Plan

1. Keep current `index.html` running.
2. Add `data/articles/index.json`.
3. Add `data/articles/anatolia-hieroglyphs.md` with frontmatter and temporary `legacy-html`.
4. Change loader to read article by slug from `data/articles`.
5. Preserve fallback to current `data/anatolia.json` during transition.
6. Convert legacy HTML into Cosmo blocks section by section.
7. Remove fallback after the first article is clean.

## 18. Non-Goals for v0.1

- Full WYSIWYG editor.
- Public user login.
- Custom backend.
- Self-hosted comments.
- Payment gating.
- Arbitrary visual scripting.
- Full homepage redesign.
- Full migration to React/Vite.

