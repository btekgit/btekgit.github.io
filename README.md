# btekgit.github.io

Personal site and research blog, built with [Quarto](https://quarto.org) and
published to GitHub Pages at <https://btekgit.github.io>.

This is a standalone repository — a clone of `btekgit/btekgit.github.io` with
its full history, on the `master` branch. It replaces the previous Jekyll setup:
`_config.yml` and `index.md` were removed, and the resume content that was in
`index.md` now lives in [`cv.qmd`](cv.qmd), updated.

## Preview locally

```powershell
quarto preview
```

Opens a live-reloading server. `quarto render` writes the static site to
`_site/` (gitignored).

> **Dropbox note.** This folder is under Dropbox, which holds locks on Quarto's
> scratch directories. `quarto render` writes the pages and then fails during
> cleanup with `os error 32 ... used by another process`, leaving `_site/`
> missing its `site_libs/` and figures. The pages themselves are fine; only the
> local preview is affected. Pause Dropbox syncing while rendering, or keep the
> working copy outside Dropbox. CI is unaffected.

## Publish

The site is not live from this setup yet. One repository setting has to change
first:

1. In the repository settings, under **Pages → Build and deployment → Source**,
   choose **GitHub Actions** (it is currently *Deploy from a branch*).
2. Push to `master`.

The workflow in [`.github/workflows/publish.yml`](.github/workflows/publish.yml)
renders with Quarto and deploys on every push to `master`. Nothing is committed
to a `gh-pages` branch and `_site/` stays out of version control.

> **Order matters.** Pushing before switching the Pages source will take the
> site down, because `index.md` is gone and Jekyll has nothing to build. Switch
> the setting first, or accept a few minutes of downtime.

If you would rather not use Actions, the alternative is `quarto publish gh-pages`
from your machine, which pushes rendered output to a `gh-pages` branch. Pick one;
using both causes conflicts.

## Adding a post

```
posts/
  my-new-post/
    index.qmd        # the post
    figures/         # images it references
    refs.bib         # optional, per-post bibliography
```

The listing on `blog.qmd` picks up anything under `posts/` automatically, sorted
by the `date` field. Minimum front matter:

```yaml
---
title: "..."
description: "One sentence; this is the listing subtitle."
date: 2026-09-20
categories: [tag, another tag]
image: figures/something.png    # listing thumbnail
---
```

Posts are plain Markdown by default. To run code at render time, use an executable
block and Quarto will embed the output:

````markdown
```{python}
#| label: fig-example
#| fig-cap: "Caption."
import matplotlib.pyplot as plt
...
```
````

That requires `jupyter` in the environment used to render — for the CI workflow
you would add a Python setup step before `quarto render`. The current post uses
pre-rendered PNGs, so no execution is needed.

## Structure

| Path | What it is |
|---|---|
| `_quarto.yml` | Site config: navbar, theme, footer, site URL |
| `index.qmd` | Landing page, with the recent-posts listing |
| `blog.qmd` | Full post listing |
| `cv.qmd` | CV |
| `about.qmd` | About page and external links |
| `styles.css` | Small overrides on top of the Bootstrap theme |
| `posts/` | One folder per post |

Two pre-existing files in this folder, `bisiklet_şile_fener_ortamesafe_tur` and
`malariasets_readme`, are unrelated to the site and are gitignored rather than
deleted. Remove them if they are no longer wanted.

## TODO before going live

- Switch the Pages source to GitHub Actions (see above).
- Check `cv.qmd` for anything out of date — it was assembled from the Europass
  CV and the old `index.md`.
- Add a link to the public code repository from the first post, if you release it.
