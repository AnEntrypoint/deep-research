# deep-research

**Turn one research question into a cited, narrative report.**

An agent skill that splits a question into subtopics, runs parallel research subagents that save sourced notes, and has a report writer synthesise those notes into a single report.

```sh
npx skills add AnEntrypoint/deep-research
```

Manual (print-first): https://anentrypoint.github.io/deep-research/

## Contents

- [What it does](#what-it-does)
- [Install](#install)
- [Use](#use)
- [How a run works](#how-a-run-works)
- [Output](#output)
- [Requirements](#requirements)
- [Repository layout](#repository-layout)
- [Author and license](#author-and-license)

## What it does

For a question that needs several sources, the skill:

1. asks follow-up questions only when the answer would change the research,
2. chooses a title and creates its folders,
3. splits the question into mutually exclusive subtopics (1 to 6+ researchers),
4. runs one research subagent per subtopic, in parallel, each saving cited notes,
5. optionally runs one extra research round to close a critical gap,
6. has a report writer synthesise the notes into `reports/<title>.md`,
7. delivers the report.

The coordinator never researches or writes the report itself. Subagents do that work.

## Install

```sh
npx skills add AnEntrypoint/deep-research
```

The `skills` CLI reads `skills/deep-research/SKILL.md` from this repository. The file uses the agent skills format: `name` matches its folder, and `description` tells the agent when to load it.

## Use

Ask for research that needs several sources, a comparison, or a synthesis:

> Compare CRM platforms for small businesses across pricing, features, integrations and user reviews.

The skill loads when a request covers research across sources, comparing options, trends or history, markets, or literature, and needs a narrative report. A single factual question is handled with one researcher.

## How a run works

| Stage | What happens |
|-------|--------------|
| 1. Clarify | Follow-up questions, asked only when they change how research is conducted. |
| 2. Title and folders | A 3 to 6 word title; creates `research_notes/<title>/` and `reports/`. |
| 3. Decompose | Mutually exclusive subtopics. Default is 3 researchers when in doubt. |
| 4. Research | One foreground subagent per subtopic, all spawned in one turn. Each reads `references/researcher.md`. |
| 5. Extra round | Optional, once only, for a critical gap. |
| 6. Report | One report writer reads the notes and `references/report-writer.md`. |
| 7. Deliver | Sends the report to the user, copies it to an outputs folder, or leaves it in `reports/`. |

## Output

```
research_notes/<title>/<topic>.md   one file per subtopic, with cited findings
reports/<title>.md                  the final report
```

Titles are 3 to 6 words, sentence case, under 50 characters, with letters, numbers and single spaces only. Returning to an earlier topic adds a distinguishing phrase or a number, so an earlier report is never overwritten.

## Requirements

- An agent with a tool for spawning subagents (`Agent` or `Task`). Without one, the agent researches the question directly.
- Subagents with web access, so findings can carry source URLs.

## Repository layout

```
skills/deep-research/
├── SKILL.md                     coordinator instructions and frontmatter
└── references/
    ├── researcher.md            brief for each research subagent
    └── report-writer.md         brief for the report writer
docs/                            GitHub Pages manual, laid out for print
```

## Manual

`docs/index.html` is a single static page with no scripts. Open the published site, or open the file locally, then print it with the browser's print dialog, or save it as PDF. It is set for A4 with a page break before each section and external links printed with their URLs.

## Author and license

Author: [lanmower](https://github.com/lanmower).

No license is declared yet.
