# Context: Static Blog

## Glossary

- **Post**: Content that is tied to a specific date, represents a point-in-time thought or update, and appears in the reverse-chronological blog feed. Located in `content/posts/`.
- **Page**: Evergreen content that describes a permanent state (e.g., bio, contact info) and is accessed via direct navigation rather than a chronological feed. Located in dedicated directories under `content/` (e.g., `content/about/`).
- **Tag**: A specific keyword used to categorize posts by tool, language, or topic. This is the sole taxonomy mechanism for the blog.

## Deployment Strategy

- **Method**: GitHub Actions (CI/CD).
- **Workflow**: Source code is pushed to the `main` branch. A GitHub Action automatically builds the Hugo site and deploys the resulting static files to the `gh-pages` branch.
- **Custom Domain**: `chronvas.com`.

## Content Metadata (Front Matter)

- **Approach**: Minimalist.
- **Required Fields**: `title`, `date`.
- **Optional Fields**: `tags` (list of strings), `excerpt` (string).
- **Constraint**: No complex custom fields unless required by a specific content type.

## Site Search

- **Status**: Not implemented.
- **Decision**: Omitted to keep the site lightweight. A client-side search (e.g., Lunr.js) may be added if the content volume increases significantly.

## PaperMod theme Submodule Maintenance

To update the `PaperMod` theme to the latest version, run:
```bash
git submodule update --init --recursive
git submodule update --remote
```
