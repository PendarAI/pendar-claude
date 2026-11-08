---
name: verify-citations
description: Check whether a cited paper actually exists, is described correctly, and has not been retracted. Use when the user asks whether a specific paper is real, pastes one or two references, or wants a citation from an AI-generated answer checked before relying on it. For a whole reference list, use audit-a-bibliography.
metadata:
  short-description: Confirm a paper exists, matches its citation, and is not retracted
---

# Verify citations

Confirm a cited paper exists in the published record, that the citation
describes it accurately, and that it has not been retracted.

A citation that cannot be found is the finding. Report it as unverified and stop
there — never substitute the closest real paper, and never supply a title, page,
year, venue, or DOI from memory. A confidently wrong reference is worse than an
acknowledged gap.

Check the citation even when the paper is familiar. The user is asking what the
record says, and a well-known paper can still be cited with the wrong year,
venue, or authors, or have been retracted since.

## Quick start

1. One `verify_paper` call per citation. For more than two or three, use
   `audit_bibliography` instead (see `audit-a-bibliography`).
2. Pass whatever the user gave, exactly as written: a DOI, an arXiv or PubMed ID,
   a bare title, a full citation string in any common style, or a journal-style
   abbreviated reference such as
   `E. Brown and H. M. Jaeger, Phys. Rev. Lett. 103, 086001 (2009).` Do not tidy
   it up first; a `paper_id` from an earlier Pendar result also resolves exactly.
3. Classify the result — verified, ambiguous, retracted, mismatch, or not found —
   using the rules below.
4. If the call returns an error saying the paper index did not respond, the
   citation was **not checked**. Try once more; if it fails again, say it could
   not be checked right now. Never report it as not found.

## Reading the result

- `found: true` — verified, unless `metadata_mismatch` is also true (below).
  `resolved_by` says how:
  - `doi`, `paper_id` or `pmid` — exact identifier match. An arXiv ID resolves
    as `doi`.
  - `title_search` — the title matched; check `match_confidence`.
  - `biblio` — an abbreviated reference with no title, matched on its journal
    locator. `matched_on_fields` lists what agreed (year, volume, page, first
    author or journal). This is a full verification. The canonical title is the
    one in `paper.title`; quote that, not a title recalled from memory.
- `found: true` with `metadata_mismatch: true` — the paper exists, but the cited
  title differs from the record: found at the journal, volume and page given, by
  the cited first author. `reason` quotes the record's title. Report it as a
  citation to correct, with the published title.
- `match_confidence` below about 0.8 — uncertain even though a paper came back.
  Show the resolved `title`, `year`, and `authors` next to what the user wrote.
- `candidates` non-empty — too ambiguous to resolve. List the candidates with
  year, venue, and author, and ask which one was meant.
- `found: false` — quote `reason`. Do not offer a different paper as though it
  were the one cited, and do not fill in missing details yourself.
- `retraction_warning` non-empty, or `paper.is_retracted: true` — lead with the
  retraction and advise against citing it as standing evidence.
- `paper.work_type` of `preprint` — flag that it has not been peer reviewed.
- `is_paratext: true` — the record is an erratum, correction notice, or similar,
  not the research article itself. Say so.
- `venue` — facts about the journal: whether it is open access, listed in the
  open-access journal directory, and has an ISSN and a publisher. Report these as
  facts. Do not call a journal predatory or low quality.
- `paper.impact_percentile` — where the paper's citation count sits among papers
  published the same year. Say "among papers from the same year".
- Metadata that disagrees with the citation — a different year, venue, or author
  list — report the discrepancy and give the canonical values from `paper`.

## Never

- Invent or guess a DOI, a `paper_id`, a URL, a page number, or a title.
- Claim a paper is retracted unless `is_retracted` is true.
- Treat `found: false` as proof of fabrication. It is strong evidence for a
  mainstream journal article and weak evidence for a book chapter, thesis,
  technical report, conference abstract, or very recent preprint.
- Report `citation_count` as a measure of quality.

## Reporting

One line per citation, in the user's order:

- **Verified** — canonical title, authors, year, venue, DOI.
- **Retracted** — the same, with the retraction stated first.
- **Ambiguous** — the candidates, and the question of which was meant.
- **Mismatch** — what the citation says against the canonical record.
- **Not found** — what was searched for, and why it may be missing.

If the user is checking their own draft, say that unresolved or retracted
references need attention before submission. If the citations come from a
published paper, an unresolved line means only that it could not be confirmed
here — not that the published bibliography is wrong.

## Related workflows

- To check a whole reference list in one call, use `audit-a-bibliography`.
- To measure a paper's influence rather than its existence, use
  `trace-citation-impact`.
- To find what else exists on the subject, use `survey-a-research-field`.
