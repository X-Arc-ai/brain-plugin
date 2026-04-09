---
name: brain-init
description: Initialize brain knowledge graph for the current project
context: fork
---

# Brain Init

Initialize the brain knowledge graph for this project.

## Prerequisites

Brain CLI must be installed:
```bash
pip install xarc-brain
```

## Steps

### Step 1: Check installation
```bash
brain --version
```

If not installed, tell the user to run `pip install xarc-brain` first.

### Step 2: Initialize
```bash
brain init --yes --skip-viz
```

This will:
- Create `.brain/` directory with Kuzu database
- Analyze the project (pyproject.toml, package.json, git authors)
- Propose and create an initial graph
- Install CLAUDE.md instructions

### Step 3: Report
Show the user:
```bash
brain stats
brain scan <project-node-id>
```

Explain the cognitive loop:
- **Before responding**: `brain scan` for context, `brain context` for depth
- **After responding**: `brain write` to capture new information
- **Maintenance**: `/brain-dream` for hygiene and signals
