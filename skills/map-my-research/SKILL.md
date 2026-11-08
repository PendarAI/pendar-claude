---
name: map-my-research
description: Show where a researcher's published work sits among the themes around it — their home themes and whether those are growing, which fields build on their work, close and growing themes they have not published in (blind spots), and nearby areas they could contribute to. Use when the user asks where their research stands, what they might be missing, which growing areas are close to their work, or how their work connects to other fields.
metadata:
  short-description: Map where your research stands and its blind spots
---

# Map my research

Place a researcher's published work on a map of the themes around it.
`my_position` returns every theme near their work with a status (home, blind
spot, audience, and so on), the counts behind that status, whether the theme is
growing, and a position to plot it at. The answer is a picture of where the work
stands, drawn from the published record.

The blind spots are usually what the user came for: close, growing themes where
they have not published. Present them as a description of the landscape, never
as advice on what to work on next.

## Quick start

1. Ask for the name they publish under **and** their institution, or an ORCID.
   A name alone often matches several people.
2. Call `my_position` once.
3. If `found` is false and `candidates` came back, list them — name,
   affiliation, main topic, number of works — and ask which one is right. Never
   pick one silently. Then call again with the chosen `author_id`.
4. If the user says their work is split across records (another spelling, a
   former name), pass the other records' ids from `candidates` as
   `extra_author_ids`.
5. Draw the map when you can (see Drawing), then report.

## Reading the result

- `researcher` — the record that was mapped: name, current affiliations, how many
  papers were placed (`placed_papers`, since `placed_since`), and how many records
  were merged. Say which record you mapped.
- `themes[].status`:
  - `home` — where they publish most. A `slowing` tag means the theme itself is
    declining, not their work.
  - `secondary` — a smaller theme they publish in regularly.
  - `blind_spot` — close to their home themes, growing, and they have at most one
    paper there and few citation links either way.
  - `under_connected` — very close but not growing, with no papers there.
  - `audience` — they do not publish there, but papers there cite their work.
  - `draws_on` — their papers cite work there, but they do not publish there.
  - `other` — everything else on the map.
- `why` — the rule and its numbers in words. Quote it rather than restating the
  numbers yourself.
- `similar` (`low` / `medium` / `high`) — how often the theme is tagged on the same
  papers as their home themes. It means "tagged together", not "the same subject".
- `your_link` — a personal measure: how much citing runs between their papers and
  the theme, for its size. It is about this researcher, not about how the two
  fields relate.
- `momentum` — `accelerating`, `steady` or `declining` over `segment_window`;
  `too_new` means not enough history and `unknown` means too few papers to say.
  `momentum_vs_area` says whether the theme grows `faster` or `slower` than its
  own area.
- `yours`, `yours_recent`, `cites_you`, `you_cite` — their papers in the theme,
  citing papers from it, and papers in it they cite.
- `reach` — how many papers cite their work (`citing_papers`, since `since`), the
  share from outside their home areas (`outside_home_areas_pct`), and the areas
  citing them most (`top_citing_areas`).
- `notes` — caveats. Repeat the ones that bear on what you say: themes are broad,
  automatically assigned tags; blind spots are not advice; an author record can
  merge two people or split one.

## Drawing

Draw the map by default when you can: an artifact or code in Claude, the Python
tool in ChatGPT. When you cannot draw, give a table instead.

- **Position:** the given `x` and `y`. Never recompute or move a theme.
- **Bubble size:** `papers_recent`, on a log scale.
- **Fill:** `momentum` in three steps (accelerating, steady, declining); grey for
  `unknown` and `too_new`.
- **Outline:** thick for `home`, dashed for `blind_spot`, dotted for `audience`
  and `draws_on`.
- **Sticks:** `links`, thicker for a higher `strength`.
- **Labels:** home themes and blind spots only; list the rest in a legend or
  table.

Never draw a value that is not in the response.

## Reporting

1. **Where you stand** — the home themes, their momentum, and any that are slowing.
2. **Who builds on your work** — `reach`, and the `audience` themes.
3. **Blind spots** — each with its `why`.
4. **Growing areas nearby** — other close themes with `accelerating` momentum.
5. **What you build on but do not publish in** — the `draws_on` themes.
6. **Caveats** — from `notes`.

## Never

- Rank, score, or judge the researcher. The map describes where the work sits.
- Present a blind spot as something they must do, or as a sure bet.
- Add themes, counts, or papers from memory.
- Call it "your" work unless the user said it is theirs. For someone else, use
  their name.
- Read a theme label as an exact field. Labels are broad and assigned
  automatically, and one can name a neighbouring field.

## Related workflows

- To explore around a blind spot or a home theme, use `map-a-field` with that
  theme's `theme_id`.
- To size one of the themes, use `survey-a-research-field`.
- To find journals for a theme, use `choose-a-venue`.
- To describe another researcher's work rather than place your own, use
  `profile-a-researcher`.
