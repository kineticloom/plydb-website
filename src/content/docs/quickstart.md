---
title: Quickstart
weight: 1
---

This tutorial walks you through setting up PlyDB with a CLI-capable AI agent
(such as [Claude Code](https://claude.ai/code)) — from installation to querying
data to recording semantic learnings for future sessions.

By the end, you will have:

1. Installed PlyDB
2. Installed the PlyDB Agent Skill
3. Created a data source config file (with your agent's help)
4. Queried your data through PlyDB
5. Written a semantic context overlay to capture what your agent learned

{{< callout type="info" >}} This guide targets agents with CLI / shell access
(Claude Code, Codex, OpenClaw, etc.) and uses the PlyDB CLI. If your agent only
supports MCP, see the [MCP setup instructions](/docs/agent-integration/#mcp)
instead. {{< /callout >}}

## 1. Install PlyDB

**macOS / Linux:**

```sh
curl -fsSL https://raw.githubusercontent.com/kineticloom/plydb/main/install.sh | sh
```

**Windows (PowerShell):**

```powershell
irm https://raw.githubusercontent.com/kineticloom/plydb/main/install.ps1 | iex
```

Verify the install:

```sh
plydb --version
```

See the [Installation](/docs/installation/) page for additional options.

## 2. Install the PlyDB Agent Skill

The Agent Skill teaches your agent how to use PlyDB — including config file
creation, CLI commands, and semantic context overlays — so it can assist you
through the rest of this tutorial.

1. Download the Agent Skill bundle (`plydb_skill.zip`) from the
   [Releases](https://github.com/kineticloom/plydb/releases) page.
2. Follow your agent's instructions for installing skills
   ([Claude Code](https://code.claude.com/docs/en/skills#where-skills-live),
   [Codex](https://developers.openai.com/codex/skills#where-to-save-skills),
   [OpenClaw](https://docs.openclaw.ai/tools/skills),
   [others](/docs/agent-integration/#supported-agents)).

Once installed, your agent will know how to configure and use PlyDB without you
having to explain the details.

## 3. Create sample data

PlyDB works with a [wide variety of data sources](/docs/supported-data-sources/)
— CSV, JSON, Parquet, Excel, SQLite, DuckDB, PostgreSQL, MySQL, S3, and Google
Sheets. If you already have a dataset you'd like to explore, feel free to use it
and skip ahead to [step 4](#4-create-a-config-file).

Otherwise, create a couple of CSV files to use for this tutorial. Save the
following as `customers.csv`:

```csv
id,name,email,city,signup_date
1,Alice Kim,alice@example.com,Seattle,2025-01-15
2,Bob Martinez,bob@example.com,Portland,2025-02-20
3,Carol Johnson,carol@example.com,Seattle,2025-03-08
4,Dave Okonkwo,dave@example.com,Denver,2025-04-12
5,Erin Novak,erin@example.com,Portland,2025-05-01
```

And this as `orders.csv`:

```csv
order_id,customer_id,product,amount,order_date,status
1001,1,Widget A,29.99,2025-06-01,1
1002,2,Widget B,49.99,2025-06-03,1
1003,1,Widget C,19.99,2025-06-05,1
1004,3,Widget A,29.99,2025-06-07,2
1005,4,Widget B,49.99,2025-06-10,1
1006,5,Widget A,29.99,2025-06-12,3
1007,2,Widget C,19.99,2025-06-15,1
1008,1,Widget B,49.99,2025-06-18,2
1009,3,Widget C,19.99,2025-06-20,1
1010,4,Widget A,29.99,2025-06-22,1
```

{{< callout type="tip" >}} The `status` column uses numeric codes: `1` =
completed, `2` = pending, `3` = cancelled. We'll use these later to demonstrate
how semantic context overlays can capture this kind of domain knowledge.
{{< /callout >}}

## 4. Create a config file

Now ask your agent to create a PlyDB config file for you. Copy and paste the
following prompt into your conversation:

> **Create a PlyDB config file called `plydb-config.json` in the current
> directory. Add `customers.csv` and `orders.csv` as data sources.**

Your agent will use its knowledge from the PlyDB skill to generate a config file
that looks something like this:

```json
{
  "databases": {
    "customers": {
      "metadata": {
        "name": "Customers",
        "description": "Customer records."
      },
      "type": "file",
      "path": "customers.csv",
      "header_row": true
    },
    "orders": {
      "metadata": {
        "name": "Orders",
        "description": "Order transactions."
      },
      "type": "file",
      "path": "orders.csv",
      "header_row": true
    }
  }
}
```

See [Configuring Data Sources](/docs/data-sources/) for the full reference.

## 5. Query your data

Ask your agent to explore the data using PlyDB. Try copy-pasting any of these
prompts:

> **What data sources are available? List all tables and their columns.**

> **How many customers are in each city?**

> **Which customer placed the most orders? Show their name, email, and total
> number of orders.**

> **What is the total amount ordered for each product? Rank them from highest to
> lowest.**

Behind the scenes, your agent will run `plydb query` and
`plydb semantic-context` commands against your config file and return the
results.

Now try a prompt that requires understanding domain context — this gives your
agent a chance to learn something that isn't in the raw schema:

> **What do the status codes in the orders table mean? Break down the order
> counts by status.**

Your agent will explore the data and likely infer (or ask you to confirm) that
`1` = completed, `2` = pending, and `3` = cancelled. This is exactly the kind of
learning we'll capture in the next step.

Try an open-ended analysis prompt to see your agent run multiple queries and
synthesize results:

> **Analyze the order data and give me insights about purchasing trends.**

You can also ask your agent to write up its work so you can audit the full
thought process:

> **Write a markdown document summarizing your analysis. Include the queries you
> ran, the raw results, your reasoning, and your conclusions.**

This gives you a reviewable artifact with the agent's queries, data, and logic
laid out step by step — useful for verifying results or sharing findings with
others.

## 6. Record learnings into a semantic overlay

During the conversation, your agent likely discovered things about your data
that aren't captured in the raw schema — like the meaning of the `status` codes
in `orders.csv`. These learnings can come from many sources: your conversation
history, the agent's own data exploration, or even your codebase where enum
values, validation rules, and business logic live.

Ask your agent to save those learnings and wire them into your config:

> **Write a PlyDB semantic context overlay file that records what you've learned
> about the data — including the meaning of the status codes in the orders
> table. Then update `plydb-config.json` to reference the overlay file.**

Your agent will create a YAML overlay file following the
[Open Semantic Interchange (OSI)](https://github.com/open-semantic-interchange/OSI)
specification. It might look something like this:

```yaml
version: "1.0"

semantic_model:
  - name: quickstart_data
    description: Quickstart tutorial — customers and orders

    datasets:
      - name: orders
        source: orders.default.orders
        primary_key: [order_id]
        description: Order transactions
        fields:
          - name: status
            expression:
              dialects:
                - dialect: ANSI_SQL
                  expression: status
            description: >
              Numeric order status code. 1 = completed, 2 = pending, 3 =
              cancelled.
          - name: customer_id
            expression:
              dialects:
                - dialect: ANSI_SQL
                  expression: customer_id
            description: References the customer who placed the order.

      - name: customers
        source: customers.default.customers
        primary_key: [id]
        description: Customer records

    relationships:
      - name: orders_to_customers
        from: orders
        to: customers
        from_columns: [customer_id]
        to_columns: [id]
```

Your agent will also update `plydb-config.json` to reference the overlay, so
future queries automatically include the semantic context:

```json
{
  "databases": {
    "customers": { ... },
    "orders": { ... }
  },
  "semantic_context": {
    "overlays": ["overlay.yaml"]
  }
}
```

With the overlay embedded in the config, every `plydb query` and
`plydb semantic-context` call picks it up automatically — no extra CLI flags
needed.

### Semantic context compounds over time

The overlay you just created is a starting point. Every future conversation is
an opportunity to refine and expand it — whether your agent discovers new
patterns in the data, learns business rules from your codebase, or picks up
domain context from something you mention in passing.

At the end of any analysis session, ask your agent to update the overlay with
what it learned. Over time, these incremental additions compound into rich
institutional knowledge that future sessions and agents inherit from day one.

See [Semantic Context](/docs/semantic-context/) for more on how overlays work
and how they're applied.

## Next steps

- **Add more data sources** — PlyDB supports
  [PostgreSQL, MySQL, SQLite, DuckDB, S3, Google Sheets, and more](/docs/data-sources/).
  Add them to your config file and query across sources with cross-source joins.
- **Grow your semantic context** — After each analysis session, ask your agent
  to update the overlay file with new learnings. Context compounds over time.
- **Try MCP** — Once your configuration stabilizes, you can switch to the
  [MCP integration](/docs/agent-integration/#mcp) for agents that support it.
- **Explore the docs** — See the full
  [Agent Integration](/docs/agent-integration/) guide and [FAQ](/docs/faq/) for
  more.
