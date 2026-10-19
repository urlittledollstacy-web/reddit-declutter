# reddit-declutter

A Brave / AdBlock cosmetic filter list for Reddit.

Two things in one list:

1. **OLED black** — forces pure black on Reddit's dark theme, for OLED screens.
2. **No background blur** — hides the blurred backdrop Reddit puts behind image
   previews in posts.

## Use it

Brave → Settings → Brave Shields & Privacy → Content Filtering →
Add custom filter lists → paste:

```
https://raw.githubusercontent.com/urlittledollstacy-web/reddit-declutter/main/reddit-declutter.txt
```

## Requirements

- **Reddit dark mode.** The OLED black rules are scoped to `.theme-dark`, so
  nothing fires in light mode. This is deliberate — painting black backgrounds
  under dark text would be worse than doing nothing.
- Works on `reddit.com` and its subdomains (`www`, `old`, `sh`, `new`).

## What's in it

16 rules total, all `reddit.com`-scoped.

| Part | Rules | What it does |
|---|---|---|
| OLED black | 12 | Sets three Reddit CSS variables to `black`, plus one border |
| No background blur | 4 | Hides `img.post-background-image-filter` and clears the dark backdrop behind it |

The two parts don't conflict. The first sets CSS variables
(`--color-neutral-background`); the second sets `background-color` directly
with `!important`. A direct declaration on an element beats a variable
inherited from an ancestor.

## Provenance

Both parts are conversions of other people's work, documented in the file:

- **OLED black** — converted from the Stylus userstyle
  ["Vantablack for reddit"](https://userstyles.world/style/28223) by replaceitem.
  The original is written in LESS, so the mixin was expanded by hand.
- **No background blur** — ported from the
  [RedditEnhancer](https://github.com/joelacus/RedditEnhancer) extension by
  joelacus (`hide_blurred_media_background.js`).

## Known limitations

- **`:has()` is not supported.** This engine drops `:has()` selectors at parse
  time. Two rules from RedditEnhancer use it and could not be ported; both only
  remove an extra tint or border on carousel and multi-image posts.
- **The `@media` branch is omitted.** The original userstyle had a
  `@media (prefers-color-scheme: dark)` branch. Filter syntax has no equivalent
  for media queries. It is included as a commented-out block with a warning
  rather than guessed at, because applying it unconditionally would also match
  Reddit in light mode.
- **The NSFW/spoiler thumbnail blur is a different feature** and is not covered
  here. Use Reddit's own setting: Profile → Settings → Preferences → Content →
  "Blur mature (18+) images and media".

## If the blur rules stop working

Reddit A/B tests its markup. If a class is renamed, the rule silently does
nothing. Open a post with an image, inspect the blurred backdrop element, and
look for a class containing `background` or `blur` on an `<img>`.
