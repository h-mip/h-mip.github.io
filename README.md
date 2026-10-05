# h-mip.com

Website of the Human-Mosquito Interaction Project (H-MIP), an ERC Starting
Grant project (No. 853271) at UPF and CEAB-CSIC, 2020–2026. It is a static
site built with [Hugo](https://gohugo.io/) and the Meghna theme, in Catalan
(the default, at `/`), Spanish (`/es/`) and English (`/en/`).

## Preview and publish

Install Hugo (`brew install hugo`; 0.128 or later should work, and 0.155.3 is
the version last tested), then from this folder:

    hugo server

and open <http://localhost:1313>. Edits show up as soon as files are saved.

Publishing is automatic: every push to `main` runs
[.github/workflows/deploy.yml](.github/workflows/deploy.yml), which builds the
site from scratch and publishes it to GitHub Pages. Progress and errors are in
the repository's **Actions** tab. Small text edits can also be made in GitHub's
web editor, which commits to `main` and so publishes them.

Because every deploy is a clean build, a page removed from `content/` also
disappears from the live site.

## Where things are

| What | Where |
| --- | --- |
| Home page sections (about, participate, explore, contact …) | `data/<ca\|es\|en>/<section>.yml` |
| Team members (all languages) | `data/team_members.yml` |
| Team photos | `assets/images/team/` |
| Blog posts, author pages, participation page | `content/<catalan\|spanish\|english>/` |
| Interface text (menus, buttons) | `i18n/<ca\|es\|en>.yaml` |
| Site settings, menu, footer text | `config.toml` |
| Images, videos, PDFs served as-is | `static/` |
| Page templates changed from the theme | `layouts/` |
| Extra styles | `assets/css/custom.css` |
| Theme | `themes/meghna-hugo/` |

`static/CNAME` and `static/.nojekyll` are only needed if the site is ever
published from a branch again instead of through Actions.

## Team members

Each person is one entry in `data/team_members.yml`; the comments at the top
of that file describe the fields. To add someone:

1. If they want a photo, convert it to black and white like the others, strip
   its metadata (`exiftool -all= photo.jpg`) and put it in
   `assets/images/team/`. Any size works: Hugo crops it square and resizes it
   when building.
2. Add an entry in the place where they should appear. Leave out `image` for
   someone who prefers not to show a photo; their initials are shown instead.

## Things to know

- **The theme is part of this repository**, copied from
  [themefisher/meghna-hugo](https://github.com/themefisher/meghna-hugo) at
  commit `63e8bcb` (July 2020) without its demo content. Don't edit it: copy
  the file you want to change into `layouts/` (or `assets/`) and edit the copy,
  as was done for `layouts/partials/navigation.html`. Upstream has since been
  rewritten and is not compatible.
- **`static/robots.txt` belongs to the Explore site** (the separate
  [h-mip/explore](https://github.com/h-mip/explore) repository, served at
  h-mip.com/explore/). Check with its maintainers before changing it.
- **Old addresses keep working** through redirects: the `aliases` in the front
  matter of some Catalan pages cover the `/ca/...` addresses used in May 2022,
  and `static/blog/botanical_garden_2021/` redirects an old blog address.
- **Photos must have their metadata removed** (camera, date, location) before
  they are added; check with `exiftool -Model -DateTimeOriginal -GPSPosition`.
- **Videos** on the home page play automatically, so keep them small. They were
  re-encoded with
  `ffmpeg -i in.mp4 -an -c:v libx264 -preset slow -crf 26 -movflags +faststart out.mp4`.
- If a future Hugo release refuses to build the site, either fix the
  deprecation it names or install the tested version (0.155.3) from
  [Hugo's releases](https://github.com/gohugoio/hugo/releases); the
  version used for publishing is set in the workflow file.
