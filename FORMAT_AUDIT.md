# Cosmomnipedia Format Audit

Version: 0.1 draft
Date: 2026-04-28

This document audits candidate content, article, and site formats before writing `CONTENT_SPEC.md`.

The goal is not to implement everything now. The goal is to decide what belongs in the permanent content model, what belongs in frontend rendering, what should be delayed, and what should remain experimental.

## Principles

1. Aesthetic quality is a primary product requirement, not decoration.
2. The format must preserve future expressive power without allowing random per-article styling to damage the visual system.
3. Content data should describe meaning. The frontend should decide appearance.
4. Old articles must remain renderable after future format upgrades.
5. Dangerous or high-variance features must be isolated behind explicit advanced blocks.
6. Wiki-specific structure is more important than blog-specific convenience.

## Format Layers

### Layer 1: Core Article Metadata

These fields belong to the article file, but not to the article body.

| Item | Belongs To | v0.1 | Reason | Recommendation |
| --- | --- | --- | --- | --- |
| `cosmodoc` version | Article metadata | Required | Enables future migrations without breaking old entries. | Required field, e.g. `cosmodoc: 0.1`. |
| Slug | Article metadata | Required | Needed for routing and stable links. | Required. |
| Title | Article metadata | Required | Core identity. | Required. |
| Subtitle | Article metadata | Required | Supports visual article hero. | Required for polished article pages. |
| Summary | Article metadata | Required | Used by cards, search, previews, AI summaries later. | Required. |
| Lead / intro | Article body or metadata | Required | Wiki articles need a concise opening definition. | Keep as a first-class block, not just first paragraph. |
| Category | Article metadata | Required | Needed for wiki navigation. | One primary category. |
| Tags | Article metadata | Required | Needed for discovery. | Multiple tags allowed. |
| Aliases | Article metadata | Required | Needed for search and redirects. | Array of alternative names. |
| Status | Article metadata | Required | Wiki needs quality states. | `draft`, `stub`, `review`, `stable`, `featured`. |
| Author | Article metadata | Required | Drives author card. | Store author id, not freeform card HTML. |
| Contributors | Article metadata | Required | Wiki is collaborative. | Array of author ids. |
| Created time | Article metadata | Required | Article history. | ISO timestamp. |
| Updated time | Article metadata | Required | Article freshness. | ISO timestamp. |
| Published time | Article metadata | Required | Public release date. | ISO timestamp or null. |
| Reviewers | Article metadata | Later | Useful for mature wiki workflow. | Add in v0.2. |
| Copyright/source license | Article metadata | Required | Essential for wiki credibility. | Required minimal fields. |
| Comments enabled | Article metadata | Required | Article-level switch. | Boolean only; provider belongs to site config. |
| Access/paid state | Site/business config | No | Payment is not a core article format. | Do not add beyond optional `access: public`. |

### Layer 2: Basic Text Formatting

These should be Markdown-compatible where possible.

| Item | Belongs To | v0.1 | Reason | Recommendation |
| --- | --- | --- | --- | --- |
| Paragraph | Article body | Required | Base unit. | Markdown. |
| Heading h2-h4 | Article body | Required | Drives table of contents. | Avoid arbitrary heading sizes in content. |
| Bold | Inline formatting | Required | Common emphasis. | Markdown. |
| Italic | Inline formatting | Required | Common emphasis. | Markdown. |
| Strikethrough | Inline formatting | Required | Useful and low risk. | Markdown extension. |
| Underline | Inline formatting | Required | Requested by project goals. | Controlled inline mark, not raw CSS. |
| Superscript/subscript | Inline formatting | Required | Needed for scholarly notation. | Controlled inline mark. |
| Inline code | Inline formatting | Required | Needed for technical or symbol entries. | Markdown. |
| Text color | Inline formatting | v0.2 | Useful but can harm consistency. | Use semantic tokens, not arbitrary colors. |
| Font family | Inline formatting | Later | High aesthetic risk. | Allow only named semantic styles. |
| Font size | Inline formatting | Later | High layout risk. | Avoid in prose; allow in display blocks only. |
| Solid mask/spoiler | Inline formatting | v0.2 | Useful for hidden content. | Controlled `{mask}` syntax. |
| Blur mask/spoiler | Inline formatting | v0.2 | Useful for spoiler/uncertain text. | Controlled `{blur}` syntax. |
| Raw HTML inline | Advanced | No | Security/style risk. | Experimental only. |
| Raw CSS inline | Advanced | No | Breaks visual system. | Do not allow in normal articles. |

### Layer 3: Wiki-Specific Semantics

These distinguish Cosmomnipedia from a blog.

| Item | Belongs To | v0.1 | Reason | Recommendation |
| --- | --- | --- | --- | --- |
| Internal wiki link | Article body | Required | Core wiki navigation. | `[[Article Name]]` and `[[slug|label]]`. |
| Redirection | Article index | Required | Needed for aliases. | Store in article index. |
| Disambiguation | Article type | v0.2 | Needed as scale grows. | Special article template. |
| Infobox | Article block | Required | Existing design already depends on it. | Structured block, not hand-written HTML. |
| Notes | Article block/inline | Required | Existing article uses notes. | Separate explanatory notes from references. |
| References | Article block/inline | Required | Core credibility. | Use stable ids. |
| Sources list | Derived block | Required | Current page derives image/source list. | Prefer generated from media metadata. |
| External links | Article block | Required | Standard wiki appendix. | Structured list. |
| Navbox | Article block | Required | Current page uses it; strong wiki pattern. | Structured groups. |
| Related entries | Article metadata/derived | v0.2 | Useful for discovery. | Derived from tags/categories initially. |
| Word cloud | Derived frontend block | v0.1 visual optional | Existing visual feature. | Do not store manually unless overridden. |
| Table of contents | Derived frontend | Required | Current reading experience depends on it. | Derived from headings. |
| Article quality badge | Article metadata | v0.2 | Useful for wiki trust. | Derived from status/review fields. |
| Version history | Git/backend layer | Later | GitHub can provide it. | Do not encode in article body. |

### Layer 4: Media

Media should be first-class because Cosmomnipedia is visual-first.

| Item | Belongs To | v0.1 | Reason | Recommendation |
| --- | --- | --- | --- | --- |
| Image | Article block | Required | Core visual unit. | Structured `media` block with caption/credit/license. |
| Image card | Article block | Required | Existing article uses this. | First-class display variant. |
| Gallery | Article block | v0.2 | Needed for visual articles. | Controlled grid/carousel variants. |
| Video | Article block | v0.2 | Useful but not core. | Support local/URL embeds with caption. |
| Audio | Article block | v0.2 | Useful for language/pronunciation entries. | First-class media block. |
| SVG display | Article block | v0.2 | Important for scripts/maps/diagrams. | Allow sanitized SVG or asset reference. |
| Media credit | Media metadata | Required | Required for source quality. | Every media item should support credit/license/source. |
| Media alt text | Media metadata | Required | Accessibility and search. | Required for images. |
| Media source list | Derived frontend | Required | Can generate appendix. | Generate from media metadata. |
| Image viewer | Frontend behavior | Required | Already implemented. | Not part of article format except media grouping. |

### Layer 5: Layout and Visual Blocks

These provide the aesthetic system. They should be semantic, not arbitrary CSS.

| Item | Belongs To | v0.1 | Reason | Recommendation |
| --- | --- | --- | --- | --- |
| Hero | Article metadata/frontend | Required | Project is visual-first. | Derived from title/subtitle/cover/theme. |
| Lead panel | Article block | Required | Strong wiki opening. | First-class block. |
| Tip/info/warning note | Article block | Required | Common explanatory format. | Use `note` block with type. |
| Quote panel | Article block | Required | Useful and aesthetic. | First-class block. |
| Divider | Article body | Required | Low risk. | Markdown horizontal rule. |
| Timeline | Article block | v0.2 | Important for history topics. | Structured events. |
| Compare panel | Article block | v0.2 | Useful for concepts/scripts/species/etc. | Structured columns. |
| Tabs | Article block | v0.2 | Useful for multiple views. | Controlled block. |
| Collapse/details | Article block | Required | Needed for long appendix or optional depth. | First-class block. |
| Link card | Article block | v0.2 | Useful for external resources. | Structured card, not raw anchor card HTML. |
| Buttons/button groups | Frontend/site block | Later | More blog/product oriented. | Not core wiki; reserve for homepage or special pages. |
| Author card | Article metadata rendered by frontend | Required | Belongs to article metadata. | Do not define as arbitrary body block in v0.1. |
| Sponsorship card | Site frontend | Later | Site-level business feature. | Keep out of article format. |

### Layer 6: Data, Code, and Technical Blocks

Needed for advanced wiki entries, but risk is higher.

| Item | Belongs To | v0.1 | Reason | Recommendation |
| --- | --- | --- | --- | --- |
| Code block | Article body | Required | Common. | Markdown fenced code. |
| Copyable text block | Article block | Required | Useful for scripts/names/transliterations. | First-class `copy` block. |
| Table | Article body/block | Required | Current article uses tables. | Markdown table for simple; structured table for complex. |
| Collapsible table | Frontend behavior | Required | Existing feature. | Table block can be collapsible. |
| Data chart | Article block | v0.2 | Useful but needs renderer decisions. | Structured data + chart type. |
| Mermaid diagram | Article block | v0.2 | Useful for relations/timelines. | Controlled renderer. |
| HTML preview | Advanced block | Experimental | Powerful but dangerous. | Sandbox only, disabled by default for public submissions. |
| Interactive demo | Advanced block | Later | High power, high risk. | Plugin layer, not core. |
| Raw script | Advanced block | No | Security risk. | Do not allow in article format. |

### Layer 7: Site and Navigation Formats

These are not article body format, but should be specified early.

| Item | Belongs To | v0.1 | Reason | Recommendation |
| --- | --- | --- | --- | --- |
| Site identity | `site.json` | Required | Name, logo, default theme. | First-class site config. |
| Theme presets | `site.json` | Required | Aesthetic priority. | Named themes, not per-article CSS. |
| Navigation | `site.json` | Required | Header/footer routes. | Structured nav items. |
| Home layout | `home.json` | v0.2 | Homepage should be designed, not hard-coded forever. | Define sections and featured entries. |
| Categories | `categories.json` | Required | Core wiki navigation. | Tree or flat list with parent ids. |
| Article index | `articles/index.json` | Required | Search, routes, cards. | Generated or manually maintained initially. |
| Search index | Derived | v0.2 | Needed soon. | Generate from article index/content. |
| Sponsor links | `site.json` | Later | Site-level feature. | Keep out of article format. |
| Friendly links | `site.json` | Later | Site-level feature. | Separate from article external links. |
| Login/auth config | `site.json` / editor | Later | Editing feature, not article format. | Keep separate. |
| Comments provider | `site.json` | Later | Provider-level config. | Article only has `comments: true/false`. |

### Layer 8: Comments and Discussion

Comments should not determine the article format.

| Item | Belongs To | v0.1 | Reason | Recommendation |
| --- | --- | --- | --- | --- |
| Article comments switch | Article metadata | Required | Some entries should disable comments. | `comments: true/false`. |
| Comment provider | Site config | Later | Could be giscus, utterances, custom backend later. | Do not hard-code into article body. |
| Discussion link | Article metadata | v0.2 | Useful if using GitHub Discussions. | Optional id/url. |
| Self-hosted comments | Backend | Later | Adds security/ops burden. | Delay until the site has real user demand. |
| Moderation status | Backend | Later | Not relevant without backend. | Do not design now. |

## Current Site Capability Inventory

Observed in current `index.html`:

- Single article shell with dynamic content injection into `#article-content-container`.
- Hero section with title, subtitle, animated background, scroll indicator.
- Home page with featured article and sidebar cards.
- Dark/light/auto theme toggle.
- Font size toggle.
- Width toggle.
- Mobile menu.
- Loading overlay and transition system.
- Smooth scrolling article wrapper.
- Interactive side table of contents.
- Chapter summary derived from headings and `data-summary`.
- Image cards and infobox images.
- Image viewer with zoom, rotate, flip, pagination, panning.
- Generic tooltips via `data-title`.
- Footnote tooltip on desktop and mobile.
- Footnote/reference index highlighting.
- Collapsible tables.
- Word cloud generated from article text.
- Custom right-click menus for text/page/image.
- External link confirmation modal.
- In-page iframe modal.
- Wiki hover card for internal links.
- Sources list population from article images.
- Accessibility helpers for keyboard activation and Escape behavior.
- Mobile swipe gesture for table of contents.

Existing format assumptions:

- Headings use real `h2`/`h3` nodes and sometimes `data-summary`.
- Images must appear under `.image-card` or `.wiki-infobox` to receive viewer behavior.
- Notes/references use `sup a[href="#note-*"]` and `sup a[href="#ref-*"]`.
- Appendix sections use fixed ids such as `notes-list`, `references-list`, `sources-list`, `links-list`.
- Navbox expects `.navbox`, `.navbox-group`, `.navbox-label`, `.navbox-list`.
- Word cloud expects readable text in `article`, especially anchors and bold elements.
- Current article data is a broken JSON file containing a large HTML fragment. This is not acceptable as the permanent format.

## Reference Site Capability Inventory

The Anheyu example page presents a blog/editor-oriented feature set:

- Markdown basics.
- Text color, size, alignment, bold, underline, italic, strikethrough, superscript, subscript.
- Images, tables, formulas, charts, Mermaid diagrams.
- Code examples and component examples.
- Fold/collapse blocks.
- Prompt/tip/info blocks.
- Link cards.
- Paid content blocks.
- Article metadata: created/updated time, author, tags, categories, cover, primary color, related/previous/next article.
- Site interaction concepts: comments, recent comments, search, random access, theme toggle, console panel, shortcut keys.

Items to absorb:

- Markdown plus custom block architecture.
- Rich but controlled visual blocks.
- Article metadata model.
- Link cards and prompt blocks.
- Chart/diagram support as later enhanced blocks.

Items to avoid or delay:

- Paid content as article format.
- Arbitrary per-article style controls as normal content.
- Blog-first previous/next article as core wiki structure.
- Excessive homepage card logic before the wiki taxonomy is defined.

## Recommended v0.1 Scope

### Required in v0.1

- Versioned article files.
- Legal JSON/Markdown source format.
- Article metadata: slug, title, subtitle, summary, category, tags, aliases, author, contributors, created/updated/published time, status, license/source fields, comments switch.
- Markdown basics.
- Wiki links.
- Lead block.
- Infobox block.
- Note block.
- Media/image block with caption/credit/license/alt.
- Table support.
- Collapse block.
- Footnotes and references.
- External links.
- Navbox.
- Derived table of contents.
- Derived sources list.
- Derived or optional word cloud.

### v0.2 Candidates

- Gallery.
- Timeline.
- Tabs.
- Link cards.
- Audio/video.
- Data charts.
- Mermaid diagrams.
- Text masks/blur.
- Semantic color marks.
- Search index.
- Category pages.
- Home page configuration.
- Giscus/Discussion integration.

### Experimental / Later

- HTML preview.
- SVG raw embed.
- Interactive demos.
- Custom per-article CSS.
- Self-hosted comments.
- Login system.
- Payment/sponsorship gating.

### Explicitly Not Core Format

- Payment logic.
- Login/auth flows.
- Comment provider implementation.
- Sponsorship cards.
- Global homepage layout implementation.
- Arbitrary raw scripts.
- Arbitrary inline CSS.

## Proposed File Boundaries

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

`CONTENT_SPEC.md` should decide whether article bodies are stored as Markdown files with frontmatter or JSON files with Markdown/HTML fields. The audit recommendation is Markdown with frontmatter plus Cosmo blocks.

## Migration Notes

The current article should not be converted directly to fully structured blocks in one step.

Recommended migration sequence:

1. Preserve current visual behavior.
2. Create legal article source file with `cosmodoc: 0.1`.
3. Keep `contentHtml` temporarily for the first article.
4. Add article metadata around it.
5. Move to Markdown plus blocks section by section.
6. Remove the broken `data/anatolia.json` fallback only after a clean article source exists.

## Open Decisions Before `CONTENT_SPEC.md`

1. Article body source: Markdown with frontmatter vs JSON with `blocks`.
2. Custom block syntax: container syntax, shortcode syntax, or JSON block arrays.
3. Whether arbitrary HTML is allowed only for trusted local author mode.
4. Whether v0.1 article source should support existing HTML fragments as a transitional field.
5. Whether references should use Markdown footnote syntax or explicit structured bibliography objects.

## Recommendation

Use Markdown with frontmatter and Cosmo blocks.

Reason:

- It is author-friendly.
- It maps well to existing rich text and wiki needs.
- It can still render aesthetic controlled components.
- It avoids locking the project into a verbose JSON authoring experience.
- It allows gradual migration from the current HTML fragment without a full rewrite.

