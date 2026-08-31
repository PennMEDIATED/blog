# A MEDIATED Feed — Blog Homepage

A self-contained rebuild of the Penn MEDIATED blog homepage (`infodem.upenn.edu/blog/`), restyled using the design system published at [github.com/PennMEDIATED/home](https://github.com/PennMEDIATED/home). Everything — markup, CSS, and JS — lives in the single `index.html` file; there are no external dependencies besides Google Fonts.

## Design tokens

All tokens are ported from the `/home` style guide's `:root` block, so this page stays visually consistent with the rest of the Penn MEDIATED site family.

### Color

| Token | Hex | Used for |
|---|---|---|
| `--c-dark` | `#0d0d0c` | Primary text, dark surfaces (modal header background) |
| `--c-accent` | `#5533ee` | Brand purple — hero title, pull-quote border, stat numbers |
| `--c-red` | `#f03d1f` | Brand red/orange — active filter pill, "Featured" accents, bio labels |
| `--c-gray` | `#888680` | Secondary/quiet text (filter labels, sort label) |
| `--c-gray-dark` | `#54534f` | Reserved for muted UI chrome (not used for reading copy — see note below) |
| `--c-light-bg` | `#f8f7f4` | Pull-quote background, stat boxes, RFP toggle background |
| `--c-white` | `#ffffff` | Page background |
| `--c-gradient` | `linear-gradient(150deg, #5533ee 0%, #df3611 81%)` | Modal header background only |

**Note:** the source style guide uses `--c-gray-dark` for most body copy. Per request, all reading copy (excerpts, full post text, bios, grant descriptions, RFP details) was switched to `--c-dark` (near-black) for stronger contrast — only small UI labels still use the grays above.

### Typography

| Token | Stack | Used for |
|---|---|---|
| `--f-serif` | `'EB Garamond', Georgia, 'Times New Roman', serif` | Hero title, post headlines, modal headings, pull-quotes |
| `--f-sans` | `'DM Sans', system-ui, -apple-system, sans-serif` | Body copy, buttons, filter pills |
| `--f-mono` | `'Courier New', Courier, monospace` | Dates, "sort by" label, bio labels, stat labels — small uppercase meta text |

Loaded via Google Fonts (`DM Sans` + `EB Garamond`) with `preconnect` hints in `<head>`.

### Spacing

An 8px base scale, identical to `/home`'s Atlassian-style tokens: `--space-025` (2px) through `--space-1000` (80px). Page-level side padding (`--pad-x`) starts at 80px and steps down to 32px and 20px at the 900px/480px breakpoints.

### Layout

- `--max-w: 1440px` — the page's outer content width (matches `/home`'s container)
- The post feed itself now spans that full width rather than a narrow reading column, laid out as a wide row: a fixed 150px date rail on the left, headline/excerpt in the middle, and the "Read more" action resolving at the far right — a common wide news-feed pattern.

## Components

- **Hero** — a plain centered header (bold purple serif title + one-line serif subtitle) rather than the source style guide's dark gradient splash hero, per request.
- **Filter + sort bar** — pill buttons to filter by category (`All / Announcements / Research / People / Events / Grants`; categories are an editorial addition — the source content has no built-in tagging) and a "Sort by" dropdown (`Most Recent` / `Oldest First`) that reorders posts by their actual date, all handled client-side in the inline `<script>`.
- **Post cards** — one per row; clicking anywhere on a card (or its "Read more" button) opens the full article.
- **Post modal** — the expanded article view. Reuses rich content blocks straight from the original blog content: pull-quotes, bio sections (photo + credentials), stat rows, a grant list, an embedded video/chart iframe, and a collapsible "Request for Proposals" accordion.
- **Hyperlinks in body copy** (`.post-excerpt a`, `.modal-body a`) — inline links inside flowing prose use one shared treatment sitewide: `color: var(--c-red)`, `font-weight: 500`, no underline, `opacity: 0.7` on hover, no color change. Same rule as `.event-card__caption a` (`events`), `.intro__body a` (`data`/`grants`), and the bio/card-desc link rules in `our-team-faculty`. This page originally used `--c-accent` (purple) with an underline instead — recolored 2026-08-29 to match the rest of the design system rather than inventing a page-specific treatment. All `target="_blank"` links also carry `rel="noopener"`, added the same day (they were previously missing it everywhere).

## Intentionally removed / changed from earlier drafts

- Top site navigation bar and global footer — removed so this can drop into a page that already has its own site chrome.
- The "Supported by" funder-logos section — removed.
- The blog's original sidebar "Center Newsletter" and "Quick Links" sections — omitted.
- Per-post "Penn MEDIATED" byline — removed from every card except the one post with a genuinely distinct byline (the Co-Directors' introduction).
- "X min read" labels — removed from cards and the modal.

## Source content

- Live page: `https://infodem.upenn.edu/blog/`
- Underlying post content: `https://penn-mediated.github.io/MEDIATED-blog/`
- Style guide: `https://github.com/PennMEDIATED/home`

## Usage

Drop `index.html` into any static host or CMS embed — it's fully self-contained (inline CSS/JS, Google Fonts CDN, remote image URLs for post artwork/logos already used by the source blog).
