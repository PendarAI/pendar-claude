---
name: profile-a-researcher
description: Summarise a researcher's published work from the public publication record — what they publish on, how their topics have shifted, where they publish, their output and citations over time, and their frequent co-authors. Use when the user asks what a named researcher works on, how their research focus has changed, or wants an overview of someone's publications before reading or citing their work — a collaborator, a reviewer, or a prospective advisor. For the user's own work and where it stands among the themes around it, use map-my-research instead.
metadata:
  short-description: Summarise a researcher's published work
---

# Profile a researcher

Describe a researcher's published work using only the public publication record.
`profile_researcher` resolves a name, ORCID, or author id to one author record
and returns counts over everything that author has published.

This is about the work, not the person. Report what the publication record
shows; do not add biographical details, personal characteristics, or opinions
about the researcher.

Use it for **another** researcher: a possible collaborator, a reviewer, a
prospective advisor. When the user asks about their own work, where it stands,
or what they might be missing, use `map-my-research`, which places the work among
the themes around it.

## Quick start

1. Call `profile_researcher` with the name as the user gave it. Add
   `institution` when the user mentioned one — it is the most reliable way to
   pick the right person among namesakes. An ORCID resolves exactly.
2. If `found` is false and `candidates` is non-empty, list them with their
   affiliation, main topic, and number of works, and ask which one was meant.
   Never pick one silently.
3. If `found` is false and there are no candidates, say the researcher could not
   be found and quote `reason`.

## Reading the result

- `profile.topics` — what the researcher publishes on, with `recent_papers`,
  `prior_papers`, and a `trend` per topic over the windows named in
  `profile.segment_window`. Topics that are `accelerating` or `too_new` (present
  only in the recent window) show where their work is heading; that answers
  "has their focus shifted". `unknown` means too few papers in both windows to
  call a trend — often a topic they worked on earlier in their career.
- `profile.output_by_year` — papers and citations per year.
- `profile.venues` — where they publish most.
- `profile.affiliations` — institutions on their papers, with the years each
  appears. Affiliation data comes from paper metadata and can lag real moves.
- `profile.top_coauthors` — the authors they share the most papers with.
- `profile.h_index`, `profile.i10_index`, `profile.works_count`,
  `profile.cited_by_count` — standard counts. Author records are assembled
  automatically and can merge two people with the same name or split one person
  into several records; `notes` flags when that looks likely.

## Never

- Rank, rate, or judge a researcher, or compare people by h-index. The counts
  depend on field, career length, and record quality.
- Add contact details, personal information, or anything not in the result.
- Infer someone's opinions, plans, or current employment from their papers.
- Present a candidate as the requested person without the user confirming it.

## Reporting

1. **Who** — the resolved name, current affiliation as recorded, and ORCID if
   present.
2. **What they work on** — the main topics, and which are growing.
3. **Output** — papers and citations over time, and where they publish.
4. **Collaboration** — frequent co-authors.
5. **Caveats** — anything in `notes` about record quality.

## Related workflows

- To place the user's own work among the themes around it, use
  `map-my-research`.
- To map the field around this researcher's work, use `map-a-field`.
- To see the field around their work, use `survey-a-research-field`.
- To look at one of their papers in depth, use `trace-citation-impact`.
