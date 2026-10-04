# corpus-fda-food-code

Mirror of the **FDA Food Code 2022** (full document) as plain text, split per
provision for retrieval / corpus use.

## What this is

- **Edition:** Food Code 2022, U.S. Department of Health and Human Services,
  Public Health Service, FDA, College Park, MD 20740.
- **Source (page checked 2026-10-04):**
  https://www.fda.gov/food/fda-food-code/food-code-2022
  (the URL in the task brief, `/food/fda-food-code/fda-food-code-2022`,
  now 404s — FDA reorganized the site; the page above is the current home.)
- **Source PDF ("Food Code 2022", full document):**
  https://www.fda.gov/media/164194/download?attachment
  ("FDA Food Code 2022: Full Document", 668 pages, 4,990,051 bytes,
  sha256 `aa523bbee8e4fd90a2b6eb9394943c724b74949c0046fa6e0f147a5348f6d93f`)
- **Downloaded:** 2026-10-04 (plain curl, no auth).

### Scope notes (honest boundaries)

- This mirror contains the **2022 Food Code as published** — Chapters 1-8,
  Annexes 1-7, front matter. Two later publications exist and are **not**
  included: the *Supplement to the 2022 Food Code*
  (https://www.fda.gov/media/183271/download?attachment) and the **Food Code
  2026** (https://www.fda.gov/food/fda-food-code/food-code-2026), which now
  supersedes 2022 as the current edition. This repo is a 2022-edition mirror
  by design; do not treat it as the current code.
- Also on the source page but excluded (not the Food Code itself): the print
  version PDF (/media/184685), two factsheets, a poster, and the supplement.

## Extraction lane

- `pypdf` 6.19.0, page-by-page extraction, then structural reassembly.
- **Extraction quality:** born-digital Word/PDFMaker PDF; clean text
  throughout (~2,300-3,100 chars on sampled text pages; median provision
  file 401 bytes). No glyph soup; tables extract as line sequences with cell
  alignment not reconstructed. Temperature notation follows the source
  text layer (`63oC (145oF)` — the PDF encodes the degree sign as the
  masculine ordinal `o`); kept as extracted.
- **Page classification:** every body page carries a running footer
  (`Chapter N - X` / `Annex N - X`); pages were assigned to their chapter or
  annex by that footer. Annex 4 uses a different footer form (its running
  header occupies that line) and is classified by header instead.
- **Provision split:** provisions number as `N-NNN.NN Title` (e.g.
  `2-201.11 Responsibility`), which is regular; 358 provision files were
  emitted, one per provision, in document order. Headings were accepted only
  in monotonic per-chapter order to reject wrapped cross-references; one
  provision number that wraps mid-number in the text layer (`7-301.` /
  `11 Separation.`) is re-joined before matching.
- **Cleaning:** running headers (`FDA Food Code 2022` + chapter/annex title)
  and running footers stripped; front-matter folio lines (Preface/TOC page
  numbers) stripped. Chapter openings (`Parts:` listings) are carried at the
  top of each chapter's first provision file marked `[chapter opening]`;
  mid-chapter `Part N-M` / `Subpart` headings are attached to the provision
  that follows them, marked `[part heading]`.

## Layout

```
provisions/<ch>-<part>-<sec>.txt   358 files (e.g. 3-401-11.txt = provision 3-401.11)
annexes/annex-<n>.txt              7 files (Annex 1 .. Annex 7)
front-matter/front-matter.txt      cover, preface, chapter/annex TOCs
downloads/fda-food-code-2022.pdf   the source PDF, as fetched
MANIFEST.tsv                       path -> source URL (+PDF page)
```

Notes on content:
- Annex 1 contains the model enforcement ordinance whose own provisions are
  numbered `8-6xx`-`8-9xx`; those exist only inside Annex 1 and are kept
  there as content (they are not Chapter 8 body provisions).
- Chapter 7's pages carry Chapter 8's running header in the source PDF (a
  printing defect in the FDA file); the correct chapter 7 title
  ("Poisonous or Toxic Materials") was taken from its title page.

## Licence

US Government work — public domain (17 U.S.C. § 105). No copyright notice
appears anywhere in the document (0 hits for "copyright" across all
extracted text). FDA's Food Code is a model code intended for state/local
adoption.

## Provenance / reproducibility

- Fetched 2026-10-04 from the URL above with `curl` (no auth, no cookies).
- `MANIFEST.tsv` maps every file to the source PDF URL with a `#page=`
  deep link and the PDF page it starts on.
- Regenerate with pypdf 6.19.0 against the sha256 above.
