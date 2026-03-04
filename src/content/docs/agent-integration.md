---
title: Agent Integration
weight: 5
---

PlyDB integrates with AI agents through two methods: **MCP** (Model Context
Protocol) and **CLI** (via Agent Skills). Both give your agent the ability to
discover schemas, run SQL queries, and analyze data autonomously.

## MCP vs CLI

|                     | MCP                                            | CLI (Agent Skill)                            |
| :------------------ | :--------------------------------------------- | :------------------------------------------- |
| **How it works**    | PlyDB runs as a persistent MCP server          | Agent invokes `plydb` CLI commands directly  |
| **Reconfiguration** | Requires server restart                        | Agent can edit config files between queries  |
| **Best for**        | Agents with MCP support, stable configurations | Dynamic workflows, evolving semantic context |
| **Limitations**     | Config changes need restart                    | Requires shell access                        |

{{< callout type="tip" >}} When first setting up or evolving your configuration,
we recommend pairing a CLI-capable agent with PlyDB. This lets the agent assist
with data source setup and capture semantics into context overlays. Once your
configuration stabilizes, transition to whichever method works best for your
preferred agent. {{< /callout >}}

## MCP

AI agents can connect to PlyDB via [MCP](https://modelcontextprotocol.io). PlyDB
exposes two MCP tools:

- **`query`** — Execute SQL queries against configured data sources.
- **`get_semantic_context`** — Retrieve structured schema and metadata so the
  agent understands your data.

### Claude Desktop

Open the Claude Desktop configuration file:

| OS      | Path                                                              |
| :------ | :---------------------------------------------------------------- |
| macOS   | `~/Library/Application Support/Claude/claude_desktop_config.json` |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json`                     |

You can also open it from Claude Desktop via **Settings > Developer > Edit
Config**.

Add PlyDB as an MCP server:

```json
{
  "mcpServers": {
    "plydb": {
      "command": "plydb",
      "args": ["mcp", "--config", "/absolute/path/to/config.json"]
    }
  }
}
```

{{< callout type="warning" >}} All paths in the MCP config and the PlyDB config
file must be **absolute**. Relative paths will not work because Claude Desktop
does not run from your project directory. {{< /callout >}}

Restart Claude Desktop, then confirm PlyDB is connected by clicking the **+**
icon in the message input area and checking under **Connectors**.

If PlyDB does not appear, check the MCP server logs:

| OS      | Log path                                     |
| :------ | :------------------------------------------- |
| macOS   | `~/Library/Logs/Claude/mcp-server-plydb.log` |
| Windows | `%APPDATA%\Claude\logs\mcp-server-plydb.log` |

### Other MCP clients

PlyDB works with any MCP-compatible client. Follow your agent's MCP
configuration instructions:

- [ChatGPT](https://platform.openai.com/docs/guides/developer-mode)
- [OpenCode](https://opencode.ai/docs/mcp-servers/)
- [Gemini](https://geminicli.com/docs/tools/mcp-server/)

### Adding semantic context overlays

You can enrich the semantic context returned by the MCP server using overlay
files:

```sh
plydb mcp \
  --config config.json \
  --semantic-context-overlay overlay.yaml
```

Or embed overlays directly in the
[config file](/docs/semantic-context/#overlays).

## CLI (Agent Skill) {#cli-agent-skill}

AI agents can also use PlyDB directly via the `plydb` CLI. To teach an agent how
to use PlyDB, install the PlyDB [Agent Skill](https://agentskills.io).

### Setup

1. [Install PlyDB](/docs/installation/).
2. Download the Agent Skill bundle (`plydb_skill.zip`) from the
   [Releases](https://github.com/kineticloom/plydb/releases) page.
3. Follow your agent's instructions for installing skills.

The skill gives your agent built-in knowledge of:

- The `plydb query` and `plydb semantic-context` CLI commands and their flags
- The PlyDB [config file schema](/docs/data-sources/)
- How to read and write
  [semantic context overlays](/docs/semantic-context/#overlays)

{{< callout type="info" >}} You do not need the skill installed to run `plydb`
CLI commands — any agent with shell access can invoke the binary. The skill
simply gives the agent guidance on how to configure and use PlyDB effectively.
{{< /callout >}}

### Supported agents

- [Claude Code](https://code.claude.com/docs/en/skills)
- [Claude](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview#claude-ai)
- [Codex](https://developers.openai.com/codex/skills)
- [Gemini CLI](https://geminicli.com/docs/cli/skills/)
- [OpenClaw](https://docs.openclaw.ai/tools/skills)
- [OpenCode](https://opencode.ai/docs/skills/)

### CLI advantages

The CLI approach enables workflows that aren't possible with a persistent MCP
server:

**Dynamic reconfiguration** — Your agent can edit the PlyDB config file between
queries, adding new data sources or adjusting settings without restarting
anything:

```sh
# Agent adds a new data source to config, then immediately queries it
plydb query \
  --config updated_config.json \
  "SELECT * FROM new_source.default.\"table\" LIMIT 5"
```

**Evolving semantic context** — After a data analysis session, ask your agent to
distill what it learned into a semantic context overlay file. Future sessions
start with that knowledge already in place:

```sh
plydb query \
  --config config.json \
  --semantic-context-overlay learnings.yaml \
  "SELECT * FROM sales.default.\"Q1\""
```

### Claude Code specifics

Claude Code can run any tool available on your system (with your permission), so
there are no sandboxing restrictions when using PlyDB via CLI. Both relative and
absolute paths work since Claude Code runs from your project directory.

### Claude Cowork specifics

Claude Cowork operates in an isolated VM sandbox with some limitations:

- The `plydb` binary must be in a directory you've granted Claude access to
  (e.g. your project workspace).
- Network access is restricted from within the sandbox, so PlyDB cannot connect
  to networked data sources (PostgreSQL, MySQL, S3, Google Sheets).

For local file sources (CSV, JSON, etc.), the CLI works fine. For networked data
sources, use MCP instead.

## Verifying the connection

Regardless of integration method, you can verify everything is working by asking
your agent:

> What data sources are available? List all tables and their columns.

The agent will call `get_semantic_context` (MCP) or `plydb semantic-context`
(CLI) to inspect the configured sources and describe the available data.

## Example prompts

Once connected, try these prompts to exercise the integration:

- **"How many customers are in each city?"** — Simple aggregation query.
- **"Which customer placed the most orders? Show their name, email, and total
  number of orders."** — Cross-table join with aggregation.
- **"Analyze the order data and give me insights about purchasing trends."** —
  Open-ended analysis where the agent runs multiple queries and synthesizes
  results.
- **"What is the total amount ordered for each product? Rank them from highest
  to lowest."** — Sorting and aggregation.
