# Timothy Links

A single-page personal link tree for **Timothy (@timkenar)**, planned for
`links.ngazi.co.ke` until the company page takes over the name.

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

| Button               | Destination                                             | Status |
| -------------------- | ------------------------------------------------------- | ------ |
| AI Learning Platform | `https://skool.ngazi.co.ke`                             | live   |
| Discord              | `https://discord.gg/V33UuGpcu`                          | live   |
| LinkedIn             | `https://www.linkedin.com/in/timothy-chepkener/`        | live   |
| TikTok               | `https://www.tiktok.com/@timkenar`                      | live   |
| YouTube              | `https://www.youtube.com/@timkenar`                     | live   |
| Instagram            | `https://www.instagram.com/timkener/`                   | live   |
| WhatsApp Group       | `https://chat.whatsapp.com/JHP9qxwgXSfL7eOBvmMgx2`      | live   |
| WhatsApp Channel     | `https://whatsapp.com/channel/0029VbClLptF6smzr9Or6o0h` | live   |
| Email                | `mailto:timothy@ngazi.co.ke`                            | live   |

All rows point at real destinations, so the page no longer carries the
`data-placeholder` "coming soon" script. To add another button, copy an
existing `<li>`, swap the icon and text, and add a matching
`.links li:nth-child(n)` animation delay in the stylesheet.

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

## Deploy with GitHub Pages

GitHub Pages serves this repository as-is — no build step and no workflow file
needed.

> **Visibility:** the repository is currently **private**. GitHub Free only
> publishes Pages from public repositories. Make it public first (Settings →
> General → Danger Zone → Change visibility → **Make public**), or keep it
> private and use [Cloudflare Pages](#cloudflare-pages-alternative) instead.

### 1. Enable Pages

1. Open <https://github.com/timkenar/links/settings/pages>.
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Set the branch to `main` and the folder to `/ (root)`, then **Save**.
4. Watch the **pages build and deployment** run under the **Actions** tab. It
   usually finishes in under a minute.
5. Open <https://timkenar.github.io/links/>.

Every push to `main` republishes the site automatically. No Jekyll
configuration or `.nojekyll` file is required for this structure.

### 2. Point links.ngazi.co.ke at it

1. In the repo, go to **Settings → Pages → Custom domain**, enter
   `links.ngazi.co.ke`, and save. GitHub commits a `CNAME` file to the repo.
   Do not also create a `CNAME` file by hand — use one method or the other.
2. In Cloudflare DNS for `ngazi.co.ke`, add:

   | Type  | Name  | Target             | Proxy status |
   | ----- | ----- | ------------------ | ------------ |
   | CNAME | links | `timkenar.github.io` | **DNS only** |

   The grey cloud (**DNS only**) matters: the orange-cloud proxy can block
   GitHub's certificate validation. You can re-enable the proxy later if you
   configure Cloudflare SSL deliberately, but DNS-only is the reliable setup.
3. Wait for the DNS check in **Settings → Pages** to pass, then tick
   **Enforce HTTPS**. GitHub issues the certificate automatically; it usually
   takes a few minutes and can take up to 24 hours.

Verify the setup:

```bash
dig +short links.ngazi.co.ke CNAME   # should print timkenar.github.io.
curl -I https://links.ngazi.co.ke    # should return HTTP/2 200
```

### Troubleshooting

| Symptom | Fix |
| --- | --- |
| 404 at `timkenar.github.io/links/` | Check that **Settings → Pages** uses `main` / `/ (root)` and that the Pages run under **Actions** finished. |
| "Domain does not resolve" | DNS has not propagated, or the record is proxied. Set it to **DNS only** and wait. |
| No **Enforce HTTPS** option | Wait for GitHub to issue the Let's Encrypt certificate, then reload the settings page. |
| Old content after a push | Pages CDN cache; wait a minute and hard-refresh (`Ctrl+Shift+R`). |
| **Pages** option unavailable | The repo is private on a free plan — make it public or use Cloudflare Pages below. |

### Cloudflare Pages (alternative)

Because `ngazi.co.ke` is already on Cloudflare, this is the shortest path to the
custom domain:

1. Create a Cloudflare Pages project and connect this repository.
2. No build command; set the output directory to `/`.
3. Add the custom domain `links.ngazi.co.ke`.
4. Cloudflare adds the DNS record automatically. If you add it manually, point
   `links` at the `*.pages.dev` hostname with the proxy enabled.

On Render, create a **Static Site** with no build command and `.` as the
publish directory, then add `links.ngazi.co.ke` as a custom domain.
