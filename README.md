# CV_generated

Generate a PDF CV from `cv.md` (Markdown) using Pandoc + a custom LaTeX template, and publish the resulting `cv.pdf` to a GitHub Release.

## Download the latest PDF

- Go to **Releases** and download `cv.pdf`:  
  https://github.com/painhardcore/CV_generated/releases/tag/latest-cv

## How it works (GitHub Actions)

On every push to the `master` branch:

1. **Build job**
   - Runs in `pandoc/extra:latest` container (includes LaTeX tooling).
   - Generates `cv.pdf`:
     ```bash
     pandoc cv.md -o cv.pdf --pdf-engine=pdflatex --template=template.latex
     ```
   - Uploads `cv.pdf` as a workflow artifact.

2. **Release job**
   - Downloads the artifact.
   - Uses GitHub CLI (`gh`) with `GITHUB_TOKEN`.
   - Uploads `cv.pdf` to the release tag `latest-cv` (creates it if missing).

## Repository layout

- `cv.md` — the source CV in Markdown (Pandoc input)
- `template.latex` — LaTeX template used by Pandoc
- `.github/workflows/*.yml` — CI workflow that builds and publishes the PDF

## Local build

### Option A — using Docker (recommended)
Requires Docker.

```bash
docker run --rm -v "$PWD":/data pandoc/extra:latest   cv.md -o cv.pdf --pdf-engine=pdflatex --template=template.latex
```

## Editing / customization

- Edit content in `cv.md`.
- Adjust formatting in `template.latex`.

The template uses Pandoc variables like:
- `$title$` (used as the big name/header)
- `$contact-info$` (if present)
- `$body$` (the rendered Markdown)

So if you keep YAML metadata at the top of `cv.md`, you can control those fields.

## Notes

This was intended for personal usage only.
