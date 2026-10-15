# portfolio-assets

Content and media for <https://dellanuel-portfolio.web.app>. The site reads
these files directly from GitHub, so **editing them here updates the live site
within about 5 minutes**, no rebuild or deploy needed.

| File | What it controls |
| --- | --- |
| `content.json` | Every piece of text: profile, hero, stats, experience, projects, services, blog list, contact |
| `blogs/*.md` | Blog post bodies (Markdown) |
| `profile.jpg` | Portrait used across the site |
| `cv.pdf` | The "Download CV" file |
| `*.png` | Project and blog images |

## Editing

1. Open `content.json`, click the pencil icon, make your change, then **Commit changes**.
2. Refresh the site after a few minutes.

- Highlight words in a headline with `*asterisks*`.
- Image and file paths are relative to this repo (for example `"photo": "profile.jpg"`).
- Mark a current job with `"end": "Present"`; the timeline shows a "Current" badge.
- `"featured": true` on a project shows it on the home page (the first three are used).

## Adding a blog post

1. Add `blogs/my-post.md` with the article in Markdown.
2. Add an entry to `blogs` in `content.json`:

```json
{
  "id": "my-post",
  "category": "Flutter",
  "readTime": "5 min read",
  "title": "My post title",
  "description": "One or two sentences for the card.",
  "image": "my-cover.png",
  "file": "blogs/my-post.md"
}
```

The post appears at `/insights/my-post`. Newest posts go first in the list.

## If something looks wrong

A JSON mistake (such as a missing comma or quote) makes the site fall back to
the copy bundled with the last deploy, so the site never breaks. Paste the file
into any JSON validator to find the error, fix it, and commit again.
