---
name: map-a-field
description: Map the themes a research field is made of and the neighbouring fields around it — which neighbours are growing, which the field is moving toward, and which are fading. Use when the user asks which fields are adjacent to a topic, where a field is heading, which nearby areas are growing, or wants to explore around a theme from an earlier map.
metadata:
  short-description: Map a field's themes and its growing neighbours
---

# Map a field

Show what a research field is made of and what lies around it. `field_map` finds
the core themes of a topic, then the neighbouring themes that share papers with
them, and says for each whether it is growing, fading, or one the field is moving
toward. Every theme comes with a position to plot it at.

After the first step the field is its core themes, not the phrase the user typed,
so the map holds up even when the wording is loose.

## Quick start

1. Give exactly one of:
   - `topic` — a **short noun phrase of two to six words**, as for a survey:
     "Which fields are next to solid-state battery electrolytes?" becomes
     `solid state battery electrolytes`;
   - `theme_id` — a theme from an earlier `my_position` or `field_map` result, to
     explore around it.
2. Call `field_map` once.
3. Read `field.core_coverage_pct` before describing the field (see below).
4. Draw the map when you can (see Drawing), then report.

## Reading the result

- `field` — what was searched (`searched_for`, `matched_on`), how many papers
  matched (`corpus_papers`), and `core_coverage_pct`: the share of those papers
  the core themes cover. Low coverage means the phrase spans several fields; say
  so, and offer to map a narrower phrase or one of the core themes.
- `themes[].role` — `core` (the field itself) or `neighbour`.
- `themes[].status`:
  - `core` — a core theme. A `slowing` tag means it is declining.
  - `converging` — a close neighbour that is growing and taking a larger share of
    the field's papers: the field is moving toward it.
  - `growing_neighbour` — a neighbour with `accelerating` momentum.
  - `fading_neighbour` — a neighbour with `declining` momentum.
  - `neighbour` — everything else.
- `share_of_field_pct` — for a core theme, its share of the field's papers; for a
  neighbour, the share of core papers also tagged with it.
  `prior_share_of_field_pct` is the same for the earlier window.
- `drift` — `gaining`, `losing`, `steady` or `new`: whether the field's papers are
  moving toward a neighbour, between the two windows of `segment_window`.
- `similar`, `momentum`, `momentum_vs_area` and `why` read as in
  `map-my-research`: `similar` means "tagged on the same papers", `unknown`
  momentum means too few papers to say, and `why` is the rule and its numbers in
  words.
- `areas` — the surrounding areas, how many themes each holds, and how many of
  those are growing.
- `links` — from each neighbour's closest core theme; `strength` repeats
  `similar`.
- `notes` — caveats; repeat the ones that bear on what you say.

## Drawing

Draw the map by default when you can: an artifact or code in Claude, the Python
tool in ChatGPT. When you cannot draw, give a table instead.

- **Position:** the given `x` and `y`. Never recompute or move a theme.
- **Bubble size:** `papers_recent`, on a log scale.
- **Fill:** `momentum` in three steps (accelerating, steady, declining); grey for
  `unknown` and `too_new`.
- **Outline:** thick for `core`, dashed for `converging`.
- **Sticks:** `links`, thicker for a higher `strength`.
- **Labels:** core and converging themes only; list the rest in a legend or table.

Never draw a value that is not in the response.

## Reporting

1. **What the field is made of** — the core themes, their shares and momentum,
   and the coverage.
2. **Where it is heading** — the `converging` neighbours, each with its `why`.
3. **Other growing neighbours.**
4. **Fading neighbours.**
5. **The areas around it** — from `areas`.
6. **Caveats** — from `notes`.

## Never

- Present a growing or converging neighbour as advice on what to work on.
- Read `drift` as cause and effect. It says where the papers moved, not why.
- Compare two fields' maps without saying how well each phrase was covered.
- Add themes, counts, or papers from memory.

## Related workflows

- To see where the user's own work sits on this map, offer `map-my-research`.
- To size one of the themes, use `survey-a-research-field`.
- To find journals for the field, use `choose-a-venue`.
