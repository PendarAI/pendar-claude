---
name: trace-citation-impact
description: Measure a paper's scholarly impact — whether its citations are accelerating or fading, which research fields adopted it, and what kinds of institutions cite it. Use when the user asks how influential a paper is, whether it is still relevant, where a method spread, or what it was built on.
metadata:
  short-description: Measure a paper's impact and where it spread
---

# Trace citation impact

Take one paper and report how the literature has used it: the per-year citation
history, the research fields of the citing papers, the kinds of institution the
citing papers come from, and how that mix is shifting. `graph_hopper` resolves
the paper, looks one step forward to the papers citing it and one step back to
its references, and returns exact counts over the whole citing set.

The value is in the composition of the citing literature, not the total. A
citation count is available anywhere; the mix of fields and institution types is
what shows whether a method travelled beyond its home field. Describe the result
as how one paper connects to the field around it: which areas picked it up, and
which it was built on.

## Quick start

1. Call `graph_hopper` with the paper as a DOI, title, or full citation string.
   A `paper_id` from an earlier Pendar result also works and resolves exactly.
2. Leave `direction` as `both` unless the user asked only about influence
   (`forward`) or only about foundations (`backward`).
3. Keep `papers_per_direction` small — 10 is usually enough. The breakdowns
   already cover every citing paper.
4. Set `include_graph: true` **only** when the user wants a diagram of the
   neighbourhood. It is large and adds nothing the breakdowns do not cover.

If `found` is false, present `candidates` and ask which paper was meant. Do not
analyse a paper the user did not ask about.

## Reading the result

- `momentum.trend` is one of `accelerating`, `steady`, `declining`, `too_new`,
  `unreliable`, or `unknown`.
  - **`unreliable` means report no trend at all.** The paper's per-year history
    contradicts itself, which happens when duplicate records of one paper are
    merged or split. Say the citation record appears damaged and a trend cannot
    be read from it. Do not describe such a paper as declining.
  - `too_new` means too little history, not lack of impact.
  - Use `momentum.interpretation` as written. `recent_3y_citations` against
    `prior_3y_citations` is the evidence behind it.
- `breakdowns_exact` — when true, `field_mix`, `institution_mix`,
  `top_citing_venues`, `citing_work_types`, and `citing_funders` are counted over
  all `citing_papers_total` citing papers, not a sample. When false, they cover
  `citer_sample_size` papers; say so whenever you quote a percentage.
- `field_mix` — the research areas of the citing papers, from a standard
  discipline taxonomy. Name both `subfield` and `field`. Subfields far from the
  paper's own are where the work crossed over. Do not reduce this to a single
  "interdisciplinarity" figure; field boundaries are administrative.
- `field_drift` — each subfield's share of the citing papers in the last three
  complete years against the three before (`drift_window` names the years),
  with a `share_trend` of `gaining`, `steady`, `losing`, or `new`. This is where
  the paper is being picked up now, as opposed to where it was picked up first.
  All `steady` is a finding too: the audience has not shifted.
- `institution_mix` — the share of citing papers with at least one author at a
  university, company, hospital, government lab, and so on. Shares overlap,
  because one paper can have authors in several sectors. A company or hospital
  share is the signal that research moved into practice.
- `citing_work_types` — articles, preprints, reviews, and so on. A large preprint
  share means the uptake is recent and not yet peer reviewed.
- `citing_funders` — funders acknowledged by the citing papers. Only papers that
  report funding are counted, so this is a lower bound.
- `top_citing_authors` — authors of the citing papers, from the public
  publication record, ranked by how many citing papers they appear on
  (`paper_count`). `total_citations` covers only the most-cited sample; do not
  quote it as a citation total. Report these authors as part of the citing
  literature, not as endorsements of the paper.
- `cited_by` and `references` — the most-cited individual papers in each
  direction. Abstracts are omitted; call `verify_paper` on one if the user wants
  its full record.
- `notes` — caveats about coverage. Repeat the ones that bear on what you claim.

## Never

- Equate citation count with quality or correctness. It measures attention.
  Retracted papers accumulate citations too, so check `seed.is_retracted`.
- Report a decline when `trend` is `unreliable`.
- Say a paper is uncited when `notes` reports that no citing papers are indexed —
  a recent paper and an unindexed one look identical from here.
- Draw conclusions about any person or organisation beyond what the publication
  record shows.
- Compare two papers' raw citation totals across different years or fields
  without saying that both vary by publication age and field size.

## Drawing

Offer charts when the user would benefit from them: the citation momentum as a
line of citing papers per year, with the current year marked as partial, and the
field mix as bars. Never draw a trend through the partial year, and never draw a
value that is not in the response.

## Reporting

1. **The paper** — canonical title, authors, year, venue, total citations, and
   any retraction.
2. **Trajectory** — accelerating, steady, declining, or unreadable, with the
   three-year counts and peak year behind it.
3. **Where it spread** — the citing fields, calling out any far from the paper's
   own subfield, then where it is being picked up now (`field_drift`).
4. **Which sectors cite it** — the institution mix, work types, and funders.
5. **What to read next** — the most-cited citing papers, and the references if
   the user asked what it was built on.
6. **Coverage and caveats** — whether the breakdowns are exact, and any notes.

## Related workflows

- To size the whole field rather than one paper's reach, use
  `survey-a-research-field`.
- To see how a researcher's whole body of work connects to the fields around
  it, use `map-my-research`.
- To confirm a reference exists and is not retracted, use `verify-citations`.
