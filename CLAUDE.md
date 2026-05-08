# Workflow notes for Claude

This repo holds the publication list of the CV/ML group at HTW Berlin (`paper.bib`).
Style rules live in `README.md` — read it before editing entries. This file
documents the recurring workflow for **adding new publications**.

## Sources for new entries

When the user asks to add Erik Rodner's recent publications, pull from:

- HTW catalog: <https://www.htw-berlin.de/forschung/online-forschungskatalog/publikationen/person/?eid=12811>
- Google Scholar (sort by date): <https://scholar.google.com/citations?user=2ItLnFgAAAAJ&hl=en&sortby=pubdate>
- dblp: <https://dblp.org/pid/90/5428.html>

The HTW catalog is most authoritative for venue/year/pages; Google Scholar
catches arXiv preprints the catalog misses; dblp is best for full author
names + DOIs of GI/INFORMATIK papers.

Cross-check what's already in `paper.bib` before adding — `grep` for a
distinctive title fragment first.

## BibTeX entry conventions

The README covers the basics. Additional conventions seen in the file:

- **Key naming** (recent style): `LastnameYYYY` or `LastnameYYYYShortname`
  (e.g. `Reiss2025`, `Knauer2024PMLBmini`, `Westerhoff2025RWI`). Older
  entries use lowercase `lastnameYYYYword` — keep those alone but use the
  capitalized form for new ones.
- **Indentation**: tabs for new entries (matches Reiss2025, Knauer2025KDD).
- **Insertion point**: prepend new entries near the top (file is roughly
  reverse-chronological, but not strictly).
- **Special characters**:
  - Umlauts: `{\"u}`, `{\"a}`, `{\"o}`, `ß` → `{\ss}`
  - En/em dashes in titles: `--` / `---` (avoid Unicode `–` `—`)
  - `&` is forbidden — rephrase or use `{\&}`
  - In abstracts: escape `%` as `\%`, `&` as `\&`. Curly quotes → `` `` `` ``
  - URLs/DOIs: leave `_` and `&` raw inside `url = {...}` / `doi = {...}`
- **Optional fields** seen in use: `code`, `extpdf`, `pdf`, `keywords`,
  `series`, `isbn`, `issn`, `note` (for "accepted for publication", "preprint").
- **arXiv-only preprints**: use `@article` with
  `journal = {arXiv preprint arXiv:NNNN.NNNNN}` and `note = {preprint}`.

### Abstracts

Add `abstract = {...}` only when you have the **verbatim** text. Sources:

- arXiv abstract page (always reliable)
- Frontiers / PLOS / TMLR HTML article pages
- For paywalled Springer chapters, skip — paraphrased descriptions
  shouldn't be passed off as the abstract.

Apply the escaping rules above. Don't fabricate or paraphrase.

## Teaser figures

Teasers live at `/Users/rodner/dev/webpage/teaser/` (sibling repo) named
exactly `<BibKey>.teaser.png`. They're not referenced from `paper.bib` —
the webpage matches by filename.

Existing teasers run ~500–800 px wide and are cropped Figure 1's, not
full pages. Workflow for new ones:

1. Download the PDF (arXiv: `https://arxiv.org/pdf/<id>`; GI:
   `dl.gi.de/bitstreams/<uuid>/download`).
2. `pdfimages -png -f 1 -l 4 paper.pdf out/prefix` — extracts embedded
   raster images. Sort by file size; the largest 1–2 are usually Figure 1.
3. If the paper uses vector figures (pdfimages yields tiny logos only),
   fall back to `pdftoppm -png -f N -l M -r 110 paper.pdf out/prefix` to
   render pages, then `magick page.png -crop WxH+X+Y +repage out.png` to
   isolate the figure region. Use `Read` to verify the crop visually
   before saving.
4. For Frontiers / PLOS articles, fetch the figure directly:
   - Frontiers: `https://www.frontiersin.org/files/Articles/<id>/xml-images/<slug>-g001.webp`
   - PLOS: `https://journals.plos.org/<journal>/article/file?id=<doi>.g001&type=large`
5. Resize to ~700–800 px wide with `magick in.png -resize 800x out.png`.
6. Save as `<BibKey>.teaser.png` in the teaser dir.

Skip teasers for paywalled Springer chapters where neither PDF nor
open-access HTML is reachable.

## Git setup

Two remotes:

- `github` → `git@github.com:erodner/publications.git` (branch `main`)
- `origin` → `git@gitlab.rz.htw-berlin.de:rodner/publications.git`
  (default branch was renamed `master` → `main` via the GitLab UI in
  May 2026; old `master` history diverged ~503 commits and is abandoned)

Push to both: `git push github main && git push origin main`.

If local tracking still points at `origin/master`, retarget once:
`git branch --set-upstream-to=origin/main main`.
