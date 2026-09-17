# OME-Zarr Video Docs

A static, Netflix-style browser for OME-Zarr documentation videos. Runs on GitHub Pages, no build step.

- `videos.yaml` — the catalog (edit this)
- `schema/video.yaml` — LinkML schema, checked in CI
- `index.html` — the whole app

## Add a video

Copy a block in `videos.yaml`:

```yaml
  - title: My video            # required
    date: "2026-01-31"         # optional, quoted
    description: |             # optional, Markdown
      Some **text**.
    tags: [talks, tools]       # optional
    sources:                   # at least one; paste the URLs as you see them
      zenodo: https://zenodo.org/records/123/files/talk.mp4
      commons: https://commons.wikimedia.org/wiki/File:Talk.webm
      github: https://github.com/org/repo/blob/main/talk.mp4
      imagesc:
        file: https://us1.discourse-cdn.com/.../clip.mp4
        post: https://forum.image.sc/t/...   # required
      custom:
        - file: https://example.org/talk.mp4
          page: https://example.org/talks/42  # required
      bluesky: https://bsky.app/profile/someone.bsky.social/post/xxxxxxxxxxxxx
      youtube: https://www.youtube.com/watch?v=XXXXXXXXXXX
```

Several sources = mirrors. The player tries them in this order:
**zenodo → commons → github → imagesc → custom → bluesky → youtube**, moving on
automatically if a file fails to load. Viewers can also switch by hand.
Every source links to its landing page (Zenodo record, Commons file page,
GitHub page, image.sc thread, `page`, Bluesky post, YouTube watch page), never just the raw file.

Bluesky URLs work with a handle or a `did:`; handles can change, DIDs don't.

## Sections

Tags listed under `sections:` get their own row (in that order). Videos without
any section tag land in "More". Other tags are still filterable via the chips.

## Run locally / deploy

```sh
python -m http.server   # then open http://localhost:8000
uvx --from linkml linkml-validate -s schema/video.yaml -C Catalog videos.yaml
```

Deploy: Settings → Pages → "Deploy from a branch", root folder.
