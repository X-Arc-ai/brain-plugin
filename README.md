# Brain Plugin for Claude Code

Give your Claude Code agent a persistent knowledge graph. Entities, relationships, signals. All local.

## Install

### 1. Install the CLI

```bash
pip install xarc-brain
```

### 2. Add the marketplace and install the plugin

```bash
claude plugin marketplace add X-Arc-ai/brain-plugin
claude plugin install brain@x-arc
```

### 3. Initialize in your project

```bash
cd your-project
brain init --yes
```

Or use the `/brain-init` skill from within Claude Code.

## What the plugin provides

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

## Optional extras

```bash
# Semantic search (requires OpenAI API key)
pip install 'xarc-brain[embeddings]'

# Conversation history replay
pip install xarc-memory
```

## Links

- [brain-cli on PyPI](https://pypi.org/project/xarc-brain/)
- [brain-cli on GitHub](https://github.com/X-Arc-ai/brain-cli)
- [X-Arc AI](https://x-arc.ai)
