# Cosmomnipedia Media Storage

Cosmomnipedia stores public media in Cloudflare R2.

Base URL:

```text
https://files.cosmomnipedia.wiki
```

## Directory Layout

Use top-level directories by media type:

```text
/images/
/videos/
/audio/
/docs/
/downloads/
/svg/
/data/
/temp/
```

Article-specific assets live under the article slug:

```text
/images/articles/{slug}/
/videos/articles/{slug}/
/audio/articles/{slug}/
/docs/articles/{slug}/
/svg/articles/{slug}/
```

Site-wide assets live under:

```text
/images/site/
/svg/site/
```

Temporary uploads live under:

```text
/temp/uploads/YYYY/MM/
```

Move reviewed files out of `/temp/` before referencing them from published articles.

## Naming Rules

Use lowercase ASCII names with numbers and hyphens:

```text
hamath-inscription.webp
anatolia-map-v1.webp
luwian-table-01.png
source-01.pdf
```

Avoid spaces, uppercase names, Chinese characters, parentheses, and generic camera names.

## Image Policy

Prefer optimized `.webp` for published article images.

If an original file must be preserved, put it in:

```text
/images/articles/{slug}/original/
```

Example:

```text
https://files.cosmomnipedia.wiki/images/articles/anatolia-hieroglyphs/hamath-inscription.webp
https://files.cosmomnipedia.wiki/images/articles/anatolia-hieroglyphs/original/hamath-inscription.png
```

## Article Media Metadata

Article content should reference media with structured metadata:

```json
{
  "type": "image",
  "src": "https://files.cosmomnipedia.wiki/images/articles/anatolia-hieroglyphs/hamath-inscription.webp",
  "alt": "Hamath inscription",
  "caption": "哈马铭文，安纳托利亚象形文字的重要例证。",
  "credit": "Wikimedia Commons",
  "source": "https://commons.wikimedia.org/..."
}
```

## Current Domains

```text
cosmomnipedia.wiki        Cloudflare Pages
www.cosmomnipedia.wiki    Redirects to cosmomnipedia.wiki
files.cosmomnipedia.wiki  Cloudflare R2 public media
```
