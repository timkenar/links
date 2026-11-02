# Timothy Links

A single-page personal link tree for **Timothy (@timkenar)**, currently served
at `links.ngazi.co.ke` until the company page takes over the name.

No build step, no dependencies. Everything lives in this folder, so it can be
published as-is.

## Project layout

```text
.
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

The page is static, so any static host works. This repository already has
`index.html` at the root, ready to publish.

### GitHub Pages

The repository is currently **private**, and GitHub Free only publishes Pages
from public repositories. Either make the repository public (Settings →
General → Danger Zone → Change visibility), or use a host that supports private
repos such as Cloudflare Pages.

Once the repo is public:

1. Go to **Settings → Pages**.
2. Under **Build and deployment**, choose **Deploy from a branch**.
3. Select `main` and `/ (root)`, then save.
4. The site appears at <https://timkenar.github.io/links/>.

For the custom domain `links.ngazi.co.ke`, add it under **Custom domain**, then
in Cloudflare DNS add a `CNAME` for `links` pointing to `timkenar.github.io`
with the proxy **disabled** (DNS only) so GitHub can issue its certificate.

### Cloudflare Pages

Because `ngazi.co.ke` is already on Cloudflare, this is the shortest path to the
custom domain:

1. Create a Cloudflare Pages project and connect this repository.
2. No build command; set the output directory to `/`.
3. Add the custom domain `links.ngazi.co.ke`.
4. Cloudflare adds the DNS record automatically. If you add it manually, point
   `links` at the `*.pages.dev` hostname with the proxy enabled.

On Render, create a **Static Site** with no build command and `.` as the
publish directory, then add `links.ngazi.co.ke` as a custom domain.
