# CGM Lab Meeting Reports - Workflow Instructions

Read this fully before touching any files in this repo. It explains where things
live, why, and the exact steps to add a new week.

## Purpose

Every Thursday there is a lab meeting where one or more journal/conference papers
are presented. This repo is the working area that produces one Word summary per
meeting date, and publishes the finished doc to a centralized OneDrive folder the
user submits from.

**The 2-page limit is per file, not per paper.** Whatever `CGMlabreport/<date>.docx`
ends up containing for that week, all papers presented that date combined, it must
be 2 pages or under. A 4-paper week and a 1-paper week both get the same 2-page
budget for the whole document. See "Adding a new week" below for how to size each
paper's summary against that budget, and the STYLE section in
`_workdir/_template/summary_template.md` for how the writing itself should read.

## Where things live (do not mix these up)

| Path | What it is | Synced to OneDrive? |
|---|---|---|
| `_workdir/` | Scratch/working area: downloaded PDFs, per-paper research notes, the markdown draft for each week, the shared template | No, local only |
| `_workdir/_template/summary_template.md` | The reusable per-paper section template. Copy-paste one block per paper into a week's `summary.md`. | No |
| `_workdir/_template/reference.docx` | Pandoc reference-doc controlling Word styles (fonts, headings, margins) used for **every** conversion, so all weekly docs look consistent. Already tuned for compactness (10.5pt body, 0.9in margins, 6pt paragraph spacing) after an earlier version overflowed to 3 pages, do not regenerate it from `pandoc --print-default-data-file` or loosen these settings without re-verifying page count per the check in "Adding a new week" step 5. | No |
| `_workdir/<YYYY-MM-DD>/papers/` | Downloaded source PDFs for that week's papers (one subfolder per paper, e.g. `01-urbanscene3d/source.pdf`). If a PDF isn't legally/freely obtainable, save a `links.md` with the DOI/URL instead of a PDF. | No |
| `_workdir/<YYYY-MM-DD>/summary.md` | The markdown draft for that week, combined summary covering all papers presented that date, built from the template. This is the source of truth; edit here, not in the docx. | No |
| **`CGMlabreport/`** | Junction (Windows directory junction, not a copy) pointing at `C:\Users\molion.surya.pradana\OneDrive - MiTAC\CGMlabreport`. **Keep this folder clean**, it should contain only the finished deliverables, one file per meeting date, named `YYYY-MM-DD.docx`. Nothing else goes here: no subfolders, no drafts, no PDFs, no templates. Anything written under `CGMlabreport/` in this repo physically lands in OneDrive and syncs automatically, no manual copy step needed. | **Yes** (it *is* the OneDrive folder) |

If `CGMlabreport/` is ever missing (e.g. repo cloned fresh on another machine),
recreate the junction, don't recreate it as a plain folder:

```powershell
New-Item -ItemType Junction -Path "D:\work\code\labmeetingreport\CGMlabreport" `
  -Target "C:\Users\molion.surya.pradana\OneDrive - MiTAC\CGMlabreport"
```

## Tooling

**Pandoc lives in a dedicated conda env, not system-wide.** Do not `winget install`
or otherwise install pandoc globally, the user explicitly wants it isolated in
the `skills` conda environment. Call it by full path (it will not be on PATH in a
fresh shell):

```
C:\ProgramData\miniconda3\envs\skills\Library\bin\pandoc.exe
```

To convert a week's draft to the final deliverable:

```powershell
$pandoc = "C:\ProgramData\miniconda3\envs\skills\Library\bin\pandoc.exe"
& $pandoc "_workdir\2026-08-13\summary.md" `
  --reference-doc "_workdir\_template\reference.docx" `
  -o "CGMlabreport\2026-08-13.docx"
```

Use the `web_search` and `web-to-markdown` skills for finding/fetching papers
(preferred over the built-in WebSearch/WebFetch tools, `web-to-markdown` has
PDF-first routing for IEEE/Springer and produces clean markdown for extraction).
Use `file_to_markdown` if you need to convert a downloaded PDF to markdown for
easier reading/extraction.

## Adding a new week (step by step)

1. Confirm the date is a Thursday; create `_workdir/<YYYY-MM-DD>/papers/`.
2. For each paper presented that date:
   a. Search for it (title + venue/year is usually enough). Prefer arXiv /
      author project pages / open conference archives (ISMIR, CVF, etc.) over
      the publisher paywall.
   b. If a PDF is obtainable, save it to
      `_workdir/<date>/papers/0N-slug/source.pdf` (N = presentation order,
      slug = short kebab-case title fragment).
   c. If it's paywalled and no open copy exists, save
      `_workdir/<date>/papers/0N-slug/links.md` with the DOI/landing-page URL
      and a note that manual institutional download is needed. Still write the
      summary from the abstract + any available excerpts, don't block on this.
   d. Read/extract the content, then copy the block from
      `_workdir/_template/summary_template.md` into `_workdir/<date>/summary.md`
      and fill it in, following the word-budget and style rules written into the
      template. The whole week's file must fit 2 pages, so divide that budget
      across however many papers are on the list for that date before writing,
      not after.
3. At the top of `summary.md`, add the date header (`# Lab Meeting - <Month D, YYYY>`)
   followed by a short first-person opening paragraph (~80-100 words) before
   any paper sections, per the OPENING guidance in `summary_template.md`. This
   is a personal synthesis across that week's papers (what they have in
   common, why this set), not a per-paper summary and not a formal report
   header (no trip dates/location/attendee fields, the user explicitly does
   not want that). Save it as `_workdir/<date>/opening.md` so it's easy to
   revise separately from the per-paper sections, then prepend it when
   assembling `summary.md`.
4. Convert to docx with the pandoc command above, output straight into
   `CGMlabreport/<YYYY-MM-DD>.docx`.
5. **Verify the real page count, don't trust a word-count guess.** Word count
   is not a reliable proxy here, an earlier pass estimated ~550 words/page and
   was wrong by roughly 2x once actually measured, producing a 3-page doc. The
   current `reference.docx` (10.5pt body font, 0.9in margins, 6pt paragraph
   spacing, no horizontal rules between papers) was tightened specifically to
   fix that, and as measured, holds roughly 1400-1500 words in 2 pages. Target
   **90% of that, about 1300 words max** for the whole week's `summary.md`
   (title + all papers combined) as a starting budget, but always confirm the
   actual page count after converting, using Word COM (requires Microsoft
   Word installed, which it is on this machine):

   ```powershell
   $word = New-Object -ComObject Word.Application
   $word.Visible = $false
   $doc = $word.Documents.Open("D:\work\code\labmeetingreport\CGMlabreport\<date>.docx", $false, $true)
   Write-Output "$($doc.ComputeStatistics(2)) pages"
   $doc.Close($false)
   $word.Quit()
   ```

   If this reports more than 2 pages, cut content and reconvert, don't loosen
   `reference.docx` margins/font/spacing back up to make it "fit" differently,
   that's what caused the original overflow. If you want to double check there
   is real headroom (not just barely 2 pages), append filler text to a throwaway
   copy in a loop like above and see how many words it takes to flip to a 3rd
   page. Do not save changes from a headroom test, close with `$doc.Close($false)`.

## Conventions

- Folder/file dates are always ISO `YYYY-MM-DD`, regardless of what format the
  date was given in conversation (e.g. "20/8/2026" -> `2026-08-20`).
- One combined `summary.md` per week (not one file per paper), this was a
  deliberate choice by the user over per-paper files.
- Never write scratch/working files into `CGMlabreport/`, that folder mirrors
  straight to OneDrive and the user wants it to contain only the final,
  submittable `.docx` files.
- The user has explicitly flagged that the summaries should not read like
  AI-agent output. Concretely: no em-dashes anywhere in `summary.md` or the
  resulting docx, no bolded "Label:" sub-headers breaking a paper's summary
  into a form, no numbered `(1) (2) (3)` question lists, and never mention a
  local file path like `papers/01-slug/source.pdf` inside the summary text
  itself, that's an internal detail, not something a reader needs. One plain
  paragraph per paper, written like reading notes for a labmate. Full rules
  are in `_workdir/_template/summary_template.md`, follow them exactly.
- Never state or imply how many papers are in a given week's file, anywhere
  in the document, especially not in the opening. No "these three papers,"
  no "today's four papers," no "a smaller/bigger set than usual." The user
  corrected this explicitly after an opening paragraph said "this week is a
  smaller, more varied set, three papers that..." Just refer to them as
  "these papers" or "today's papers" and let the content speak for itself.
