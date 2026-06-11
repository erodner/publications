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

## Auditing entries for missing metadata

When asked to "check entries for proper updates" (typically after a
conference has run or a preprint has been formally published), do a
sweeping pass over the relevant year range:

1. **Inventory** which entries are missing key fields. An awk one-liner
   over `paper.bib` summarising `year/pages/doi/url/note` per entry
   surfaces gaps quickly.
2. **Stale notes to retire**: `note = {accepted for publication}` once
   the paper is out; `note = {preprint}` once it has a venue;
   `note = {in discussion}` after the discussion phase ends. Keep
   genuinely descriptive notes (e.g. "Presented at AI4EA Workshop 2024").
3. **Authoritative metadata sources**:
   - Crossref REST API: `curl -s https://api.crossref.org/works/<DOI> | python3 -m json.tool`
     — gives canonical title, page range, container, publisher,
     author order, ISBN. Use this whenever a DOI is known.
   - dblp page (`https://dblp.org/rec/<conf>/<slug>.html`) — fast
     lookup for DOIs of CVPR/ECCV/ICCV/KDD papers.
   - CVF Open Access — canonical URL pattern
     `openaccess.thecvf.com/content/<CONF><YEAR>/html/<slug>.html`.
     Note CVF papers don't carry DOIs themselves; pair with the IEEE
     DOI (`10.1109/CVPR<id>.<year>.<n>`).
   - For Springer chapters, follow the DOI to confirm author order;
     the bib's original author list may be from a draft and miss
     co-authors added at publication time.
4. **URL vs extpdf convention**: when both a canonical publisher URL
   and an arXiv mirror exist, set `url` to the publisher (CVF, ACM DL,
   Springer, OpenReview) and demote arXiv to `extpdf`. This keeps the
   webpage's primary link pointing at the official record.
5. **Author corrections**: if the published author list differs from
   what's in the bib, fix the order and add/remove names — but keep
   the existing bib key even if the first author changes, since the
   key may be cited externally. Flag the discrepancy to the user.
6. **Cross-check pages**: workshop / journal papers often get
   renumbered between arXiv and final proceedings. Always prefer the
   page range Crossref reports for the DOI.

Entries that legitimately stay sparse: ICLR/AutoML/EurIPS workshop
papers (no DOIs), German-only GI workshop notes without persistent
records, and live preprints not yet at a venue.

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
