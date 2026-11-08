---
name: survey-a-research-field
description: Size and characterise a research area from exact publication counts — how many papers exist, whether output is growing or fading, which sub-areas are growing fastest, which disciplines, countries, and sectors produce it, who funds it, where it is published, and what to read first. Use when the user asks how large, how active, or how crowded a field is, wants an overview of a topic's literature, or is deciding whether to enter an area.
metadata:
  short-description: Measure a research field by exact publication counts
---

# Survey a research field

Describe a body of literature by counting it. `survey_literature` returns exact
counts over every paper matching the topic — not a sample — so the answer can
state how many papers exist, how that number moved year by year, and who
produced them.

The counts are the substance here. A list of relevant papers comes back too, but
it is the smaller half of the answer, and leading with it wastes what the tool is
for.

## Quick start

1. Reduce the user's question to a **short noun phrase of two to six words**.
   "How much work has been done on solid-state electrolytes for lithium metal
   batteries?" becomes `solid state electrolytes lithium metal`.
2. Call `survey_literature` once. Add `year_from` or `year_to` only if the user
   asked about a period.
3. Read `corpus.total_papers` together with `corpus.matched_on` before saying
   anything about size — the second changes what the first means.
4. Lead with size and trend, then composition, then the reading list.
5. Repeat every caveat in `notes` that bears on what you claim.

## Choosing the query

The search requires **every** word to appear, so each extra word shrinks the
corpus, and question words are stripped before searching.

- Drop question framing, verbs, and articles: keep the subject.
- Keep the phrase specific enough to mean one field and no more.
- If `total_papers` is 0 or in the low single digits, the terms were too narrow.
  Retry once with the two or three most central words, and tell the user you
  broadened it and to what.
- If the number looks implausibly large for the subject, the terms were too
  generic. Retry with the distinguishing term restored.

Never present a retry as a different question. State which phrase produced the
numbers you are reporting; `corpus.searched_for` shows what was actually
searched after stripping.

When the question is how big a field is, one phrasing gives one count, and a
field rarely has one name. Try two or three common phrasings and report the
range, with the phrase behind each number.

## Reading the result

- `corpus.total_papers` with `corpus.matched_on`:
  - `title_and_abstract` — a conservative count. Papers that mention the topic
    only in passing are excluded.
  - `full_text` — too few papers had the topic in title or abstract, so the
    corpus was widened to anything whose full text mentions it. Report the total
    as an **upper bound**, because passing mentions are now included.
- `corpus.trend` — `interpretation` is already written from the exact per-year
  counts; use it rather than recomputing a trend from `papers_by_year`. Treat
  `too_new` as insufficient history, not as a lack of growth. `unknown` means too
  few papers, or a year range too short, to call a direction; quote the
  interpretation rather than guessing one. The current year is partial, so never
  describe a field as declining on the strength of the latest bar alone.
- `corpus.topics` versus `corpus.disciplines` — topics are the **sub-areas the
  work divides into**; disciplines are the **fields whose researchers produce
  it**. Keep them apart. A topic list answers "what is this field about", and a
  discipline list answers "which disciplines work on it". Sub-area labels are
  tags assigned automatically, so read them as broad pointers, not exact fields.
- Segment trends — `topics`, `disciplines`, and `venues` segments carry
  `recent_papers` (last three complete years), `prior_papers` (the three before),
  and a `trend`. This is how to answer "which parts are growing fastest": a small
  segment that is `accelerating` can matter more than a large one that is
  `steady`. A segment without a trend had too little data in one window.
- `corpus.funders` — funders acknowledged by the papers. Only papers that report
  funding are counted, so treat shares as a lower bound and say so.
- `corpus.organization_types` — the share of papers with at least one author at
  a university, company, hospital, government body, and so on. Shares overlap.
- `corpus.top_authors` — authors appearing on the most papers in the corpus,
  from the public publication record. Report them as prolific contributors to
  the literature, not as a ranking of quality.
- `corpus.citation_profile` — how many papers in the corpus have never been
  cited. A high uncited share is normal for a young or fast-growing field.
- `corpus.countries` and `corpus.institutions` — shares overlap and total well
  above 100%, because a paper written across four institutions counts once in
  each. Say so if you quote them.
- `corpus.venues` — preprint servers and repositories rank here beside journals.
  A repository near the top is a finding: the field circulates before peer
  review.
- `corpus.work_types` — a large preprint share means a fast-moving field where
  the published record lags.
- `corpus.retracted_papers` — mention any retractions if the user intends to rely
  on the field's findings.
- `papers` — the most relevant papers, with abstract snippets. Summarise what
  they cover; do not present them as the field's most important work, since they
  are ranked by relevance to the query rather than by influence.

## Never

- Read percentages from a small corpus as proportions of a research area. Exact
  arithmetic over six papers still prints as "33%". When `notes` flags a small
  corpus, describe the individual papers instead.
- Compare two topics' totals without saying whether both were matched the same
  way. A `full_text` total against a `title_and_abstract` total is not a
  comparison.
- Add papers, counts, or trends from memory. Everything reported must come from
  the response.
- Call the tool repeatedly to page through a field. One survey covers the whole
  corpus; more calls only re-count it.

## Drawing

Offer a chart when the user would benefit from one: papers per year as a line,
with the current year marked as partial. Never draw a trend through the partial
year, and never draw a value that is not in the response.

## Reporting

1. **Size** — the total, what it was matched on, and the years covered.
2. **Trajectory** — the trend, with the recent versus prior three-year counts
   behind it and the peak year.
3. **Where the growth is** — the sub-areas and disciplines with the strongest
   segment trends.
4. **Composition** — the disciplines and countries producing the work, the
   sectors and funders behind it, where it is published, and how much is peer
   reviewed.
5. **What to read** — the returned papers, grouped by what they address.
6. **Caveats** — the corpus definition and any small-corpus, funding-coverage,
   or retraction note.

## Related workflows

- To see the themes a field is made of and which neighbouring fields are growing,
  use `map-a-field`.
- To see where the user's own work sits among those themes, use
  `map-my-research`.
- To measure one paper's influence rather than a field's size, use
  `trace-citation-impact`.
- To see which journals publish the topic, use `choose-a-venue`.
- To check a specific reference the user already has, use `verify-citations`.
