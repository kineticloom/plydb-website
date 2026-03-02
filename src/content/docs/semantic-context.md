---
title: Semantic Context
weight: 3
---

Raw schemas tell an agent _what_ columns exist. Semantic context tells it what
they _mean_. PlyDB bridges that gap automatically - scanning your data sources
to generate structured context, and providing an overlay system that lets agents
deepen their understanding over time.

## Automatic discovery

PlyDB scans your data sources and generates structured semantic context -
schemas, tables, columns, types, and database `COMMENT` metadata - all without
any manual setup.

```sh
plydb semantic-context --config config.json
```

This outputs an
[Open Semantic Interchange (OSI)](https://github.com/open-semantic-interchange/OSI)
YAML document describing your data model, giving agents a machine-readable map
of your data.

## Knowledge beyond the schema

Schemas describe structure, but agents can learn meaning from richer sources.
**Your codebase** reveals enum values, validation rules, and business logic.
**Your conversations** during analysis sessions teach domain context that no
schema can express.

For example, an agent that encounters `status = 3` can check your code to learn
it means "churned" - or learn it from your conversation history.

PlyDB provides a semantic context overlay system to persist these learnings so
they compound over time.

## Overlays

{{< callout type="tip" >}} If you have the
[PlyDB Agent Skill](/docs/agent-integration/#cli-agent-skill) installed, your
agent will know how to write an overlay file. At the end of a session, ask you
agent to record it's learnings into an overlay file for future sessions to
use.{{< /callout >}}

Overlays are YAML documents following the
[Open Semantic Interchange (OSI)](https://github.com/open-semantic-interchange/OSI)
specification.

Pass overlays via the CLI:

```sh
plydb semantic-context \
  --config config.json \
  --semantic-context-overlay business_glossary.yaml \
  --semantic-context-overlay team_annotations.yaml
```

Or embed them in the config file:

```json
{
  "semantic_context": {
    "overlays": [
      "/path/to/business_glossary.yaml",
      "/path/to/column_descriptions.yaml"
    ]
  }
}
```

Config-file overlays are applied first, then CLI flag overlays, in order.

## How it compounds

Every insight - whether discovered from schemas, learned from source code, or
explained by a human - can be recorded into semantic overlay files that persist
across sessions and agents. Overlays follow the open
[OSI standard](https://github.com/open-semantic-interchange/OSI) and compound
over time. Future agents inherit institutional knowledge from day one.

The result: agents that ask better questions, write more accurate queries, and
deliver answers you can trust.
