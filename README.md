# dataado.com

The public marketing site, kept here so it versions alongside the product it describes. It is
published separately to the **`dataado-website`** repo, which GitHub Pages serves.

## Publishing

**This whole folder is the site root.** Copy its contents — not the folder itself — into the
`dataado-website` repo, so `index.html` lands at that repo's root. Everything here ships; nothing
outside here does.

```
index.html              the page (fonts and the interactive demo are inlined)
404.html                served by GitHub Pages for any unmatched path
robots.txt              points crawlers at the sitemap
sitemap.xml             one real URL — this is a single-page site
dataado-overview.mp4    the 30s overview video (~6.9 MB)
dataado-overview.jpg    its poster, and the og:image for social previews
InstrumentSans.woff2    used by 404.html only
SpaceGrotesk.woff2      used by 404.html only
```

Copy all of them. The page is not broken-but-usable without the video and poster — the `#watch`
section renders an empty player — and `404.html` falls back to a system font stack without the two
`.woff2` files, which is exactly the page-to-page type drift they exist to prevent.

## Things worth knowing before editing

**Every path is relative and bare** (`dataado-overview.mp4`, `InstrumentSans.woff2`), so the files
only work while they sit beside each other. Moving one into a subfolder breaks it silently — the
page still renders, just without a video or with the wrong font.

**`index.html` inlines almost everything on purpose**: both fonts as base64 data URIs, the favicon as
a data URI, and the entire interactive demo. That is why it is ~230 KB and why it makes no external
requests for its own chrome. The video and poster are the deliberate exceptions — a base64 poster
would have added ~126 KB to a payload every visitor parses, and the video cannot be inlined at all.

**The video is `preload="none"`.** Without that, every visitor downloads 6.9 MB whether or not they
press play. If you ever swap the video, keep that attribute.

**Absolute URLs are baked into the head** — `canonical`, `og:url` and `og:image` all point at
`https://dataado.com/`. If the domain ever changes, those four tags change with it.

**The claims on this page are checked against the code**, not aspirational. Four SQL engines, eight
transports, the import and export formats, and the AI behaviour all reflect what is actually shipped.
InterSystems Caché is deliberately absent: it is read-only and has not been run against a real
instance. Keep that discipline when editing — an overclaim here is the one thing a prospect can
verify against the product and catch.

## Checking a change before publishing

```bash
cd dataado-website
python -m http.server 8080
```

Then open `http://127.0.0.1:8080/` — serving the folder is the only way to see it as the site,
because opening `index.html` from disk gives it a `file://` origin where the video and the theme's
`localStorage` behave differently.
