# A MEDIATED Feed — Blog Homepage

A self-contained rebuild of the Penn MEDIATED blog homepage (`infodem.upenn.edu/blog/`), restyled using the design system published at [github.com/PennMEDIATED/home](https://github.com/PennMEDIATED/home). Everything — markup, CSS, and JS — lives in the single `index.html` file; there are no external dependencies besides Google Fonts.

## Design tokens

All tokens are ported from the `/home` style guide's `:root` block, so this page stays visually consistent with the rest of the Penn MEDIATED site family.

### Color

| Token | Hex | Used for |
|---|---|---|
| `--c-dark` | `#0d0d0c` | Primary text, dark surfaces (modal header background) |
| `--c-accent` | `#5533ee` | Brand purple — hero title, pull-quote border, stat numbers |
| `--c-red` | `#f03d1f` | Brand red/orange — eyebrow and metadata labels, bio labels |
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

## Typography

Sitewide convention. The `--fs-*`/`--lh-*` block at the top of the stylesheet is canonical and identical in every page repo.

**Two families, no third.** `--f-serif` (EB Garamond) for page and section titles and pull-quote copy; `--f-sans` (DM Sans) for everything else. There is no monospace face — uppercase micro-labels are DM Sans, not Courier.

**Sizes come from tokens, never raw px.**

| Token | Mobile (=<480px) | Desktop (>=1440px) | Used for |
| --- | --- | --- | --- |
| `--fs-display` | 36px | 76px | full-bleed hero |
| `--fs-h1` | 36px | 56px | page title |
| `--fs-h2` | 26px | 40px | section titles |
| `--fs-h3` | 20px | 24px | card and third-level titles |
| `--fs-lede` | 18px | 20px | intro paragraphs |
| `--fs-body` | 16px | 16px | body copy |
| `--fs-small` | 14px | 14px | captions, meta, form controls |
| `--fs-small-serif` | 15px | 15px | EB Garamond at small sizes |
| `--fs-micro` | 12px | 12px | uppercase labels, tags, counts |

The top five are `clamp()` values that interpolate across the viewport, so tablet widths need no separate `@media` override. Only add a breakpoint font-size when a specific layout actually demands it.

**12px is the floor.** Nothing ships smaller.

**Line heights are tokens too** — `--lh-display` 1.05, `--lh-heading` 1.15, `--lh-lede` 1.26, `--lh-title` 1.3, `--lh-body` 1.55. Never set a line-height in px; it breaks the fluid sizes.

**Heading gaps.** Section title to first content is `var(--space-300)` (24px); page or hero title to content is `var(--space-250)` (20px).

**Narrow viewports.** Grid tracks are `minmax(0, 1fr)` rather than `1fr`, and flex items holding text carry `min-width: 0`. Without those, a track or item is pinned to its widest child and pushes the page wider than the viewport on small screens.

This page's CSS lives in an inline `<style>` block rather than a `styles.css`, so the token block sits at the top of that block. Inline `style=""` attributes inside individual post bodies are per-post content layout and are not part of the token system.

## Components

- **Hero** — the shared page-hero treatment: left-aligned EB Garamond title at `--fs-h1`/600 in `--c-accent`, DM Sans lede at `--fs-lede`/300, `--space-600` bottom padding. Matches `events`, `team-leadership`, `data`, `grants-rfp` and `our-team-job-openings`; it used to be centred with a serif lede, which made this the one page whose top read as a different site.
- **Filter + sort bar** — pill buttons to filter by category (`All / Announcements / Research / People / Events / Grants`; categories are an editorial addition — the source content has no built-in tagging) and a "Sort by" dropdown (`Most Recent` / `Oldest First`) that reorders posts by their actual date, all handled client-side in the inline `<script>`.
- **Post cards** — one per row; clicking anywhere on a card (or its "Read more" button) opens the full article.
- **Featured posts** — a post carrying `<span class="featured-badge">Featured</span>` in its `.post-meta` is pinned to the top of the list, in both sort modes. **The badge is the only switch.** `sortItems()` reads it straight out of the DOM (`item.querySelector('.featured-badge')`), so there is no second flag to keep in sync: add the badge and the post pins, remove it and the post falls back into date order. Featured and pinned cannot disagree, which is how the Gutmann post previously ended up badged but sitting mid-list. Pin more than one and they lead as a group, ordered among themselves by the chosen sort. **No post is currently featured.**
- **Post modal** — the expanded article view. Reuses rich content blocks straight from the original blog content: pull-quotes, bio sections (photo + credentials), stat rows, a grant list, an embedded video/chart iframe, and a collapsible "Request for Proposals" accordion.
## Embedding this page

WordPress renders the real site; this repo is the source. The launch plan is direct-to-disk deployment, which needs no iframe — but iframe embedding still works and is the documented fallback, so keep this snippet accurate if you rename the repo or change its Pages URL.

Paste into a **Custom HTML block** as one line. The site runs **Twenty Twenty-Five**, a block theme, and a Custom HTML block has no width control of its own — so wrap it in a **Group block set to Full width**. This is not optional for these pages: Twenty Twenty-Five's `theme.json` sets `contentSize: 645px` (`wideSize: 1340px`), so an unwrapped embed renders in a 645px column, and every full-bleed colour band in the design collapses with it:

```html
<iframe id="pm-blog" src="https://pennmediated.github.io/blog/" title="Blog — Penn MEDIATED" loading="lazy" style="width:100%;height:3800px;border:0;display:block"></iframe><script>(function(){var f=document.getElementById('pm-blog');window.addEventListener('message',function(e){if(e.source!==f.contentWindow)return;var d=e.data||{},h=d.frameHeight||(d.type==='partners-page-resize'?d.height:0);if(h)f.style.height=h+'px';});})();</script>
```

The `height` in the snippet is only the starting value. Every Penn MEDIATED page posts its real height to the parent as `{ frameHeight: <int> }` — on load, on resize, once webfonts settle, and on any `ResizeObserver` change, so reveal animations, expanding cards and `<details>` toggles all resize the frame. The listener in the snippet applies it. `grants-rfp` also emits an older `{ type: 'partners-page-resize', height }` message; the snippet accepts both.

The page checks `window.self === window.top` before posting, so opening it directly does nothing. If you add a new page repo, copy the script from the bottom of this `index.html` so it behaves the same way.


## Images and video

This applies to every image, GIF and video added to any Penn MEDIATED repo. It is written to be followed directly — by a person or by a Claude session — without further instruction.

### The one rule that is never optional

**Every `<img>` and `<video>` carries explicit `width` and `height` attributes, holding the file's real intrinsic pixel dimensions.**

```html
<img src="assets/example.webp" width="640" height="334" alt="…">
```

They do not set the display size — CSS does. They give the browser the aspect ratio *before* the file downloads, so it reserves a correctly shaped box instead of collapsing to nothing and shoving everything below it down the page as each file lands. That shift is measured by search engines (Cumulative Layout Shift) and is worse for a reader, who loses their place or clicks a link that just moved.

Every repo has a global `img, video { max-width: 100%; height: auto; display: block; }` reset, so the CSS keeps winning and the attributes only ever contribute the ratio. **Never guess the numbers** — read them off the file.

### Pick the format by what the file is

| Content | Format | Never use |
| --- | --- | --- |
| Photo, screenshot, artwork | **WebP**, quality 88 | PNG or JPEG at full camera resolution |
| Logo, wordmark, icon | **SVG** if you have it, else WebP | — |
| Anything that moves | **MP4** (H.264) + a WebP poster | **GIF, ever** |

GIF is the big one. It has no interframe compression, so a screen recording is roughly ten times the size it needs to be: `research-compendium.gif` was 11.3MB for 290 frames; the identical recording as H.264 is 1.2MB.

### Size it to the box it displays in, not to what you were sent

Find the CSS box the image renders into, then export at **2×** that width for retina. Anything beyond that is bytes the browser downloads and immediately throws away. (`gni-membership.png` was 7992px wide, rendering into a 319px box — a 470KB file doing a 33KB job.)

In this repo:

| Where | CSS box at 1440px | Export at |
| --- | --- | --- |
| Post feature image / video in a modal (`.post-feature-img`) | 664px wide | ~1328px |
| Inline logo pair (`.modal-body img`) | 326px wide | ~660px |
| Author portrait (`.bio-photo`) | 112px circle, cropped | ~224px square |

If you are adding an image somewhere not listed, measure the box first (`getBoundingClientRect().width` in the browser, at a 1440px viewport) and double it.

### Commands

Stills — resize and convert in one pass:

```python
from PIL import Image
TARGET = 640                      # 2x the CSS box
im = Image.open('source.png')
w, h = im.size
if w > TARGET:
    im = im.resize((TARGET, round(h * TARGET / w)), Image.LANCZOS)
im.save('out.webp', quality=88, method=6)
print(im.size)                    # <- these are the width/height attributes
```

Animation — MP4 plus a poster frame:

```bash
ffmpeg -i source.gif -movflags +faststart -pix_fmt yuv420p \
       -vf "scale=1280:-2:flags=lanczos" -crf 24 out.mp4
ffmpeg -i source.gif -frames:v 1 -vf "scale=1280:-2:flags=lanczos" poster.png
python3 -c "from PIL import Image; Image.open('poster.png').convert('RGB').save('out-poster.webp', quality=80, method=6)"
ffprobe -v error -show_entries stream=width,height -of default=nw=1 out.mp4
```

`-crf 24` is a good default; raise it toward 30 for a smaller file, lower it toward 20 for a sharper one. `-pix_fmt yuv420p` is required for Safari and iOS.

### Markup for video

```html
<video src="assets/name.mp4" poster="assets/name-poster.webp" width="1280" height="622"
       autoplay muted loop playsinline preload="metadata" aria-label="…"></video>
```

Each attribute earns its place: `muted` is what permits autoplay at all, `playsinline` stops iOS opening it fullscreen, `poster` means the slot is never empty while the video loads, and `aria-label` replaces `alt` (a `<video>` has no `alt`).

CSS cannot stop autoplay, so **a page with video needs the reduced-motion script** at the end of `<body>`. If the page already has one, leave it alone; if you are adding the first video to a page, add it:

```html
<script>
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
    document.querySelectorAll('video[autoplay]').forEach(function (v) {
      v.autoplay = false; v.pause(); v.currentTime = 0; v.removeAttribute('loop');
    });
  }
</script>
```

Also check the CSS: any rule that sizes or crops an image needs to name `video` too, or the video slot will not match the image slot it replaced (`.card__image img` becomes `.card__image img, .card__image video`).

### Before you call it done

- [ ] File is WebP, SVG or MP4 — no GIF, no full-resolution PNG or JPEG
- [ ] Its width is about 2× the CSS box it renders into
- [ ] `width`/`height` attributes match the file's real dimensions
- [ ] Real `alt` text (or `aria-label` on a video) that describes the image; empty `alt=""` only if it is purely decorative
- [ ] Lives in this repo's `assets/`, not hotlinked from another site
- [ ] Page opened in a browser at 1440px and ~400px — nothing overflows, nothing jumps on load
- [ ] Originals are not committed alongside the optimised file; git history is the backup

Do not commit an unoptimised original "just in case" — the previous commit already holds it, and a duplicate in the working tree also ships to the server.

## Hyperlinks

One taxonomy, five categories, shared by every page repo. Pick the category by what the link *is*, not by which repo you happen to be editing.

**1. In-text links** — embedded mid-sentence in flowing prose.

| ground | text | underline | hover |
| --- | --- | --- | --- |
| white / light | `--c-red-dark` | none | fade to `opacity: 0.7` |
| colour / gradient | `--c-white` | `border-bottom: 1px solid rgba(255, 255, 255, 0.5)` | fade to `opacity: 0.7` |

Both grounds use `font-weight: 500` and `transition: opacity 0.15s`, and both fade rather than change hue. On a white ground **colour is the affordance** — no underline; the underline is category 2's job. On a coloured ground the red is invisible, so the link goes white and takes the hairline rule instead. Where an underline is used it is a `border-bottom`, never `text-decoration`.

#### Why interactive red is `--c-red-dark`, not `--c-red`

`--c-red-dark` (`#df3611`) is the closing stop of `--c-gradient`, promoted to a token of its own and declared in all twelve repos.

`--c-red` (`#f03d1f`) measures roughly **3.9:1** against white — under the 4.5:1 WCAG AA threshold for body text, and the same 3.9:1 applies to white text sitting on a `--c-red` fill. `--c-red-dark` measures about **4.5:1** either way and clears it. The two are near-indistinguishable at text sizes, so this is a contrast fix, not a visual change.

**The rule: anything you click is `--c-red-dark`.** Links and buttons take it wherever they would otherwise be red-orange — as text colour, as a box fill, as a hover or active state, and on the markers inside them (disclosure chevrons and their labels). It applies in every category and every state.

**`--c-red` stays the brand accent for everything you don't click**: section headings, eyebrow and metadata labels, tag and pill backgrounds, accent bars and card borders, full-width colour bands, the `.card-arrow` hover gradient, and focus rings. These are either large text, non-text UI at the 3:1 threshold, or sit on a tinted rather than white ground.

The one deliberate hold-out is red link text on a **dark** ground (`home`'s `.footer__email`), where the darker red would *reduce* contrast rather than improve it. That link has a separate outstanding issue — on a dark ground the standard is white text with an opacity fade, not red at all.

**2. Independent links** — a standalone text link that isn't inside a sentence ("Learn More About the Center", "Download the Full Schedule"). Unlike category 1 these carry the underline and are set in the body colour, so they read as a control rather than as emphasis inside a sentence:

| ground | text | underline | hover |
| --- | --- | --- | --- |
| white / light | `--c-dark`, `font-weight: 600` | `border-bottom: 1px solid rgba(13, 13, 12, 0.35)` | text and underline both turn `--c-red-dark` (`transition: color 0.15s, border-color 0.15s`) |
| colour / gradient | `--c-white`, `font-weight: 600` | `border-bottom: 1px solid rgba(255, 255, 255, 0.5)` | fade to `opacity: 0.7` |

Plus a **thin arrow** `⟶` after the text. Use `⟶` (`&#10230;`), not the `↗` badge from category 4.

**3. Document buttons** — an independent link that opens a document (a PDF, a report). A filled button box, not text:

| ground | box | text |
| --- | --- | --- |
| white / light | `--c-red-dark` | `--c-white` |
| colour / gradient | `--c-white` | `--c-dark` |

Hover is **movement, not colour** — a lift or nudge. Do not darken or recolour the box.

**4. Links to another web page** — this site or an external one. The containing box carries the shared `.card-arrow`: a 26px dark circle with a white `↗`, in the box's top corner. On hover the arrow scales slightly and its background becomes a sliding purple-to-orange gradient (`@keyframes card-arrow-slide`), and the box itself animates. No separate text button — the whole box is the link.

**Exception:** a link to a research paper is category 2, not this — thin arrow, no badge.

**5. Hyperlinked headings** — a heading that is itself a link (a post title, a card title). Sits in the body colour and shifts to `--c-red-dark` on hover (or fades, on a coloured ground), with **no arrow and no underline**.

### Dropdowns and disclosures

A dropdown, `<details>` block or expand/collapse control uses one affordance sitewide: a **chevron SVG** (`M2 5l5 5 5-5`, 13×13, `--c-red-dark` stroke, `stroke-width: 1.8`) beside a `--c-red-dark` label at `--fs-small`, rotating `180deg` on open with `transition: transform 0.25s`. See `llm-civic-discourse`'s "Full summary & details" toggle for the reference implementation.

Never leave the marker to the browser — style `<select>` with `appearance: none` and supply the chevron, and hide the native `<summary>` marker. The `↗` circle badge is category 4's language and does not belong on a disclosure control.

## Intentionally removed / changed from earlier drafts

- Top site navigation bar and global footer — removed so this can drop into a page that already has its own site chrome.
- The "Supported by" funder-logos section — removed.
- The blog's original sidebar "Center Newsletter" and "Quick Links" sections — omitted.
- Per-post "Penn MEDIATED" byline — removed from every card except the one post with a genuinely distinct byline (the Co-Directors' introduction).
- "X min read" labels — removed from cards and the modal.

## Source content

- Live page: `https://infodem.upenn.edu/blog/`
- Underlying post content: `https://penn-mediated.github.io/MEDIATED-blog/` (the current live GitHub Pages site, in the older `penn-mediated` org)
- Style guide: `https://github.com/PennMEDIATED/home`

## Usage

Drop `index.html` into any static host or CMS embed — it's self-contained apart from `assets/` (inline CSS/JS, Google Fonts CDN, and local post artwork). Copy `assets/` alongside it.
