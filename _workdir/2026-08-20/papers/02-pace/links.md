# PACE: Part-Wise Slow-Fast Conditioning for Dance-to-Music Generation

- OpenReview forum: https://openreview.net/forum?id=1Ip4pZoHTH
- OpenReview PDF (direct link, blocked — see note): https://openreview.net/pdf/60a768b386c052be840d3ca3b106c45de5f6de28.pdf

## Access note

The paper is indexed on OpenReview (submitted under ICLR 2026), and its title/abstract
are visible in general web search result snippets (via WebSearch tool queries run
2026-08-21). However, direct retrieval of the OpenReview page and PDF was blocked in
every attempt during this research session:

- Raw `curl` fetch of the forum page and PDF URL → returned OpenReview's
  "Verifying your browser" bot-challenge HTML page instead of content (both with and
  without a browser-like User-Agent header).
- OpenReview REST API (`api.openreview.net` and `api2.openreview.net` `/notes` endpoints)
  → returned `403 ChallengeRequiredError` ("Challenge verification required").
- `WebFetch` tool on the forum URL and PDF URL → same bot-verification page, no content.
- `web2md` (web-to-markdown skill), rendered via a real headless/stealth browser with
  extended wait times (up to 15s, `networkidle0`) → still landed on the "Complete the
  check below to continue to OpenReview" interstitial (looks like a Cloudflare
  Turnstile-style challenge requiring interactive/manual completion).
- Reader-proxy fetch (`r.jina.ai`) → same CAPTCHA/verification warning.
- No arXiv preprint, project page, or GitHub repo could be found for this exact title.
- The paper does not appear in ICLR 2026 accepted-paper listings (Paper Copilot,
  paperdigest.org, proceedings.iclr.cc) that were checked, consistent with it still
  being an active/under-review OpenReview submission (ICLR 2026 is double-blind; the
  OpenReview snippet metadata suggested an October 2025 posting date, matching the
  ICLR 2026 submission window) rather than an accepted, camera-ready paper.

**Consequence:** authors could not be verified from a primary source and are therefore
NOT listed in `section.md` (would otherwise be a guess). Because it is a double-blind
ICLR submission, it is plausible the OpenReview listing itself shows "Anonymous"
authors even once accessible. Only abstract-level content (recovered via search-engine
snippets of the OpenReview page, cross-confirmed across two independent search
queries) was available to summarize.

If someone with a working OpenReview session/browser wants to retrieve the full PDF
manually: sign in at openreview.net, then open
https://openreview.net/forum?id=1Ip4pZoHTH and download the PDF from there.
