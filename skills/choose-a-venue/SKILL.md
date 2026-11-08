---
name: choose-a-venue
description: Find the journals and venues that actually publish a topic — ranked by how much of the literature they carry, whether that share is growing, and facts about each venue such as open access, publication fees, and citation statistics. Use when the user asks where work on a topic gets published, which journals to consider for a paper, or which open-access venues cover a subject.
metadata:
  short-description: Find the venues that publish a topic
---

# Choose a venue

Show where the literature on a topic is published. `find_venues` counts every
paper matching the topic by venue, so the ranking reflects what each journal
actually publishes rather than its reputation.

This informs a choice; it does not make it. Report facts about each venue and
leave the judgement to the user.

## Quick start

1. Reduce the user's topic to a **short noun phrase of two to six words**, as for
   `survey-a-research-field`.
2. Call `find_venues` once. Add `year_from` if the user cares only about recent
   publishing.
3. Filter the returned venues by what the user asked for — open access, fee
   ceiling, excluding preprint servers — using the fields below.

## Reading the result

Each entry in `venues`:

- `papers` and `share_pct` — how much of the topic's literature appears there.
- `recent_papers`, `prior_papers`, `trend` — whether the venue is publishing more
  or less on the topic than three years earlier.
- `venue_type` — `journal`, `repository`, `conference`, and so on. Repositories
  and preprint servers are not peer-reviewed venues; say so if one ranks high.
- `is_oa`, `is_in_doaj`, `apc_usd` — whether the venue is fully open access,
  whether it is listed in the open-access journal directory, and its listed
  article processing charge in US dollars, when known.
- `publisher`, `issn` — identifying facts.
- `mean_citedness_2yr` and `h_index` — citation statistics for the venue as a
  whole. `mean_citedness_2yr` is the average citations per paper over two years.
  It is **not** the Journal Impact Factor; never call it that.

`notes` carries caveats about the corpus and the venue data; repeat the relevant
ones.

## Never

- Call any number here an impact factor, or rank venues by prestige.
- Call a venue predatory or low quality. If `is_in_doaj` is false for an
  open-access journal, report that fact and let the user weigh it.
- Predict acceptance chances or review times — the result says nothing about
  either.
- Add venues from memory that are not in the result.

## Reporting

1. The corpus size behind the ranking, and what it was matched on.
2. The top venues with papers, share, and trend.
3. The facts relevant to the user's constraints — open access, fees, peer review.
4. Caveats from `notes`.

## Related workflows

- To size the field itself, use `survey-a-research-field`.
- To see the neighbouring fields around the topic, use `map-a-field`.
- To check references before submitting, use `audit-a-bibliography`.
