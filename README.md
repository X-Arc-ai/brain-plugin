<p align="center">
  <img src="https://raw.githubusercontent.com/X-Arc-ai/brain-cli/main/assets/hero.svg" alt="brain" width="700">
</p>

<p align="center">
  <strong>Give your AI coding agent a knowledge graph that compounds.</strong><br>
  Entities, relationships, signals. All local. Zero config.
</p>

---

## Quick Start

```bash
pip install xarc-brain
claude plugin marketplace add X-Arc-ai/brain-plugin
claude plugin install brain@x-arc
```

Then in any project:

```bash
cd your-project
brain init --yes
```

That's it. Your agent maintains the graph automatically.

---

## How It Works

Every conversation follows a cognitive loop:

1. **Scan** -- query the graph for context before responding
2. **Respond** -- with full awareness of entities, relationships, and history
3. **Write** -- capture new information back to the graph

This loop is enforced by hooks that fire automatically. You don't need to
remember to use the brain. It's architecturally guaranteed.

<p align="center">
  <img src="https://raw.githubusercontent.com/X-Arc-ai/brain-cli/main/assets/demo-scan.svg" alt="brain scan" width="640">
</p>

---

## What the Plugin Provides

### Skills (slash commands)

| Skill | Purpose |
|---|---|
| `/brain-init` | Initialize brain for the current project |
| `/brain-dream` | Run maintenance: hygiene checks, signals, conversation replay |

### Hooks (automatic enforcement)

| Hook | When | What |
|---|---|---|
| **Brain reminder** | Before each prompt | Reminds Claude to scan the graph for context |
| **Write validation** | Before `brain write` | Validates required edges (goals need people, blockers need targets) |
| **Cognitive loop check** | On session end | Blocks if brain was never queried or dual-write was missed |
| **Auto-dream** | On session end | Triggers `brain dream` if >24h since last run |

### CLAUDE.md instructions

The plugin includes instructions that teach Claude the cognitive loop:
1. **Scan** before responding (topology map)
2. **Assess** which nodes are relevant
3. **Dive** into key nodes (deep context)
4. **Dual-write** after responding (file + graph)

---

## What It Tracks

Brain stores **entities** (projects, people, goals, tasks, blockers, decisions, events),
**relationships** between them (who owns what, what blocks what, what depends on what),
and **temporal signals** (what's stale, what's stuck, what just shipped).

<p align="center">
  <img src="https://raw.githubusercontent.com/X-Arc-ai/brain-cli/main/assets/demo-signals.svg" alt="brain signals" width="640">
</p>

---

## What It Looks Like in Production

This is the actual brain of CCL, the AI agent that built this tool.
320 nodes, 975 edges, 6 months of compounding memory across multiple
companies, projects, and people.

<p align="center">
  <img src="https://raw.githubusercontent.com/X-Arc-ai/brain-cli/main/assets/brain-production.png" alt="CCL's production brain -- 320 nodes, 975 edges" width="800">
</p>

<p align="center">
  <em>Your agent builds this over time. Every conversation adds to the graph.</em>
</p>

---

## Optional Features

### Semantic Search

```bash
pip install 'xarc-brain[embeddings]'
# Set OPENAI_API_KEY in your environment
brain embed backfill
brain search-semantic "authentication flow"
```

### Conversation History Replay

```bash
pip install xarc-memory
# brain init and brain dream will index past Claude Code conversations
```

---

## Architecture

```
your-project/
  .brain/              Brain data (add to .gitignore)
    db/                Kuzu embedded graph database
    exports/           Visualization data
    viz/               Cytoscape.js graph visualization
    config.json        Type tiers, custom settings
  CLAUDE.md            Brain instructions (auto-installed)
```

---

## How It's Built

- [Kuzu](https://kuzudb.com/) -- embedded graph database, no server
- [Rich](https://github.com/Textualize/rich) -- terminal formatting
- [Click](https://click.palletsprojects.com/) -- CLI framework
- [Cytoscape.js](https://js.cytoscape.org/) -- graph visualization (bundled offline)

~3,500 lines of Python. Fully auditable. No magic.

**Nothing leaves your machine.** No cloud services. No telemetry. The only
optional external call is OpenAI for semantic search embeddings, and that's
opt-in via `pip install 'xarc-brain[embeddings]'`.

---

## Links

- [brain-cli on PyPI](https://pypi.org/project/xarc-brain/)
- [brain-cli on GitHub](https://github.com/X-Arc-ai/brain-cli)
- [X-Arc AI](https://x-arc.ai)

---

## How This Was Built

This project was built by CCL, an AI agent deployed on [X-Arc](https://x-arc.ai)'s
CCX platform. CCL manages operations, builds tools, and ships code across
multiple projects. Brain started as CCL's internal memory system. After 6 months
of daily use (320 nodes, 975 edges, 5 signal types, 7 hygiene checks running
nightly), CCL packaged and open-sourced it.

X-Arc deploys AI agents that ship real work. Manage it like a hire. It works like ten.

[x-arc.ai](https://x-arc.ai) | [GitHub](https://github.com/x-arc-ai)
