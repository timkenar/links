# Timothy Links

A single-page personal link tree for **Timothy (@timkenar)**, currently served
at `links.ngazi.co.ke` until the company page takes over the name.

No build step, no dependencies. Everything lives in this folder, so it can be
published as-is.

## Project layout

```
links/
├── index.html                 the page (styles are inline)
└── assets/
    ├── timothy.jpg            profile photo (from the YouTube channel)
    ├── favicon.ico            browser tab icon
    ├── apple-touch-icon.png   iOS home-screen icon
    ├── og-image.png           1200×630 preview for WhatsApp/Discord/Google
    └── ngazi-logo.png         kept for when the company page comes back
```

## Links

| Button           | Destination                                    | Status      |
| ---------------- | ---------------------------------------------- | ----------- |
| TikTok           | `https://www.tiktok.com/@timkenar`             | live        |
| YouTube          | `https://www.youtube.com/@timkenar`            | live        |
| Instagram        | `https://www.instagram.com/timkener/`          | live        |
| WhatsApp Channel | `https://whatsapp.com/channel/<channel-id>`    | placeholder |

To finish the WhatsApp row, replace its `href="#"` in `index.html`, then remove
the `data-placeholder` and `title` attributes from that link. The small script
at the bottom only intercepts clicks on links that still have
`data-placeholder`, showing a "coming soon" note instead of jumping to the top
of the page.

> **Handle check:** Instagram is set to `timkener`, as provided, while TikTok
> and YouTube use `timkenar`. If the Instagram handle is also `timkenar`,
> update both the `href` and the visible `@` text.

## Branding

The page uses Timothy's YouTube avatar, name, and `@timkenar` handle. To swap
the photo, replace `assets/timothy.jpg` with another square image (480×480 is
plenty) and regenerate the icons:

```bash
magick assets/timothy.jpg -resize 180x180 -strip assets/apple-touch-icon.png
magick assets/timothy.jpg -resize 256x256 -define icon:auto-resize=64,48,32,16 assets/favicon.ico
```

When the company page returns, restore the Ngazi logo and copy from
`assets/ngazi-logo.png` (kept in this folder). The page metadata
(`canonical`, `og:url`, and image URLs) still points at
`https://links.ngazi.co.ke/`; update those if it moves to a personal domain.

## Preview locally

From this folder:

```bash
python3 -m http.server 8080
```

Then open <http://localhost:8080>.

## Deploy

The page is static, so any static host works. Because `ngazi.co.ke` is already
on Cloudflare, Cloudflare Pages is the shortest path:

1. Push this folder to a Git repo, or drag the `links` folder into the Pages
   dashboard.
2. Create a Cloudflare Pages project with no build command and `/` as the
   output directory (publish the contents of `links/`, not its parent).
3. Add the custom domain `links.ngazi.co.ke`.
4. Cloudflare adds the DNS record automatically. If you add it manually, point
   `links` at the `*.pages.dev` hostname with the proxy enabled.

Other hosts: on Render, create a **Static Site** with no build command and `.`
as the publish directory, then add `links.ngazi.co.ke` as a custom domain. On
GitHub Pages, add a file named `CNAME` containing `links.ngazi.co.ke` and set
it as the custom domain in the repo settings.
