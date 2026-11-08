# Pendar

Go beyond finding papers. Pendar shows how research fields are structured, how ideas
connect, and where science is heading. Explore your own research and the areas you
have not yet explored, map any field and its growing neighbours, measure a field's
size and direction, trace a paper's influence, find journals and researchers, and
verify references. Every count comes from the indexed scholarly record, not from an
estimate or from Claude's memory.

## What the plugin contains

- **The Pendar connector**, declared in `.mcp.json`, pointing at
  `https://pendar.ai/mcp`. This is the same server as the Pendar connector in the
  directory, so having both gives you one set of tools.
- **Eight skills** that tell Claude when to use each tool, how to read the result,
  and how to draw the maps:

| Skill | Use it to | Tool it calls |
| --- | --- | --- |
| `map-my-research` | See where your research stands: your home themes, who builds on your work, and close, growing themes you have not published in | `my_position` |
| `map-a-field` | See the themes a field is made of, which neighbouring fields are growing, and which the field is moving toward | `field_map` |
| `survey-a-research-field` | See how large and how active a field is, what is growing, who funds it, and what to read first | `survey_literature` |
| `trace-citation-impact` | See who cites a paper, which fields picked it up, and whether its citations are rising | `graph_hopper` |
| `profile-a-researcher` | Summarise what another researcher publishes on and how their focus has shifted | `profile_researcher` |
| `choose-a-venue` | Find the journals that publish a topic, which are growing, and which are open access | `find_venues` |
| `verify-citations` | Check that one or two cited papers exist, match the citation, and are not retracted | `verify_paper` |
| `audit-a-bibliography` | Check a whole reference list (up to 25 entries) in one pass | `audit_bibliography` |

The skills carry rules the tools cannot enforce on their own: a map describes
where work stands and is never advice on what to work on, a citation that cannot
be confirmed is reported as not found and never replaced with a similar-looking
paper, and the tools' caveats stay in the answer.

## Getting started

1. Install the plugin, then connect Pendar from the plugin's Connectors tab.
2. Sign in to your Pendar account, or create one at https://pendar.ai. New
   accounts start with free credits.
3. On the Pendar approval page, review the tools and their prices and approve.
4. Ask a research question, for example "Where does my research stand? I publish
   as Jane Smith at the University of Edinburgh", "Since COVID, where is
   wastewater-based epidemiology heading, and which nearby fields are growing
   fastest?", or "Check these references for my manuscript" followed by the list.

Full instructions, including Claude Code and how to disconnect:
https://pendar.ai/support/connectors/

## Data and privacy

- The plugin sends data to one place only: `https://pendar.ai/mcp`, Pendar's own
  server, and only when Claude calls one of the eight tools.
- Each call sends the tool's arguments and nothing else: a researcher's name,
  institution, ORCID or Pendar author id; a topic or a theme id from an earlier
  map; a paper's DOI, arXiv or PubMed ID, title or citation; a reference list; or
  a journal subject. Pendar does not receive your conversation, your files, or
  data from your other connectors.
- Pendar looks the request up in a public scholarly bibliographic index from its
  own server and returns bibliographic records and counts: titles, authors,
  years, venues, DOIs, citation counts, research themes, and retraction status.
- Every tool only reads. None of them create, change, or delete anything, in
  Pendar or anywhere else.
- Sign-in uses OAuth on pendar.ai. The plugin contains no API keys, passwords,
  or other credentials, and reads none from your computer.
- Each successful call uses credits from your Pendar balance, at the prices
  shown on the approval page; a failed call is not charged. Pendar records each
  call (tool, time, outcome, credits) for billing.
- The plugin itself stores nothing and contains no scripts, hooks, or programs.
  It is Markdown and JSON only.

Privacy policy: https://pendar.ai/privacy/
Terms of service: https://pendar.ai/terms/

## Support

Use the form at https://pendar.ai/support/ or email admin@pendar.ai.
