---
name: audit-a-bibliography
description: Check a whole reference list in one pass — which entries are real, which are retracted, which are ambiguous, and which cannot be confirmed. Use when the user pastes a bibliography, a reference list from a draft or a published paper, or a set of sources from an AI-generated answer and wants them checked before relying on them.
metadata:
  short-description: Check every reference in a list at once
---

# Audit a bibliography

Check every entry of a reference list against the published record in one call.
`audit_bibliography` takes up to 25 citations and returns a verdict for each, in
the order given, plus a tally.

An entry that cannot be confirmed is reported as unconfirmed. Never substitute
the closest real paper, and never supply a missing title, page, year, venue, or
DOI from memory — that is how fabricated references end up looking real.

## Quick start

1. Split the list into individual references, one string each. Keep each
   reference exactly as written, in whatever style it uses (author-year, numbered
   physics style, quoted titles). Abbreviated journal references such as
   `Phys. Rev. Lett. 103, 086001 (2009)`, DOIs, and arXiv or PubMed IDs are all
   supported and should not be expanded first.
2. Set `context`:
   - `draft` — the user's own manuscript, or sources from an AI answer.
   - `published` — the reference list of a paper that is already published.
3. Call `audit_bibliography` with up to 25 references. For a longer list, send
   the next 25 in a second call.
4. If any entries come back `not_checked`, call again with only those entries.

## Reading the result

Each entry in `entries` has a `verdict`:

- `verified` — the paper exists and matches. `resolved_by` says how (`doi`,
  `paper_id`, `title_search`, or `biblio` for an abbreviated reference matched on
  its journal locator). All of these are verifications. Quote the canonical title
  from `paper.title`. When `match_confidence` is below 0.8, show the canonical
  title next to the reference as written so the user can confirm it is the same
  paper. `matched_on_fields` lists what agreed with the record; `first_author`
  there means the cited author settled a choice between papers sharing a title.
- `retracted` — the paper exists and is retracted. Lead with that.
- `ambiguous` — several papers match about equally. List the `candidates` and
  ask which was meant.
- `metadata_mismatch` — a paper matches part of the citation, but a detail
  disagrees with the record. Two forms:
  - with `paper` — the paper exists and was found at the cited journal, volume
    and page, by the cited first author, but the cited title differs. `reason`
    quotes the record's title; report the published title as the fix.
  - without `paper` — the year, first author or journal disagrees. Show both,
    and the `candidates` when present.
- `not_found` — nothing in the index matches. Quote `reason`.
- `not_checked` — not a verdict. Either the call ran out of time before reaching
  this entry, or the paper index did not respond for it ("The lookup failed").
  It was not charged; send it again in a second call.

`tally` counts each verdict. `notes` carries caveats; repeat the ones that bear on
what you claim.

## Never

- Fill in or correct a reference from memory. If an entry is `not_found`, the
  answer is that it could not be confirmed.
- Treat `not_found` as proof of fabrication. It is strong evidence for a
  mainstream journal article and weak evidence for a book, chapter, thesis,
  report, or very recent preprint. Say which case the entry looks like.
- Report `not_checked` entries as missing.

## Reporting

1. A table or list in the user's order: the reference as given, the verdict, and
   for verified entries the canonical title, year, venue, and DOI.
2. The tally.
3. For `draft`: say that retracted, mismatched, and unconfirmed references need
   attention before submission. For `published`: say that unconfirmed lines could
   not be confirmed here, which is not a finding that the published bibliography
   is wrong.

## Related workflows

- To look closer at one reference, use `verify-citations`.
- To see who has built on one of the verified papers, use
  `trace-citation-impact`.
