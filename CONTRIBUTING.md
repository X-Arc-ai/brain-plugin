# Contributing to Brain Plugin

Thanks for your interest in improving Brain for Claude Code.

## Plugin Structure

```
.claude-plugin/
  plugin.json          # Plugin manifest
  marketplace.json     # Marketplace listing
plugins/brain/
  .claude-plugin/
    plugin.json        # Plugin manifest (for marketplace discovery)
  CLAUDE.md            # Instructions injected into Claude's context
  hooks/
    hooks.json         # Hook registration (uses ${CLAUDE_PLUGIN_ROOT})
    brain-reminder.sh  # UserPromptSubmit: cognitive loop reminder
    validate-brain-write.sh  # PreToolUse: edge validation before writes
    verify-cognitive-loop.sh # Stop: enforce scan + dual-write
    dream-hook.sh      # Stop: auto-dream if >24h since last run
    should-dream.sh    # Helper: check if dream is due
  skills/
    brain-init/SKILL.md   # /brain-init slash command
    brain-dream/SKILL.md  # /brain-dream slash command
```

## What to Contribute

### New Skills

Add a new skill by creating `plugins/brain/skills/<skill-name>/SKILL.md`. Skills are markdown files with YAML frontmatter:

```yaml
---
name: my-skill
description: What it does
context: fork
---

# My Skill

Instructions for Claude to execute when the user runs /my-skill.
```

### Hook Improvements

Hooks live in `plugins/brain/hooks/`. Each hook is a shell script that receives JSON on stdin and exits with:
- `0` -- allow
- `2` -- block (with a message on stderr)

Register new hooks in `hooks/hooks.json` using `${CLAUDE_PLUGIN_ROOT}` for paths.

### CLAUDE.md Refinements

The `plugins/brain/CLAUDE.md` file is injected into Claude's system context when the plugin is active. Keep it concise -- every token counts.

## Development

### Prerequisites

```bash
pip install xarc-brain     # The CLI that powers everything
```

### Testing Locally

```bash
# Install plugin from local path
claude plugin marketplace add /path/to/brain-plugin
claude plugin install brain@x-arc

# Or use --plugin-dir for a single session
claude --plugin-dir /path/to/brain-plugin/plugins/brain
```

### Testing Hooks

```bash
# Test a hook directly
echo '{"tool_input": {"command": "ls"}}' | bash plugins/brain/hooks/validate-brain-write.sh
echo $?  # Should be 0 (pass -- not a brain write)

# Test with a brain write that should be blocked
echo '{"tool_input": {"command": "brain write batch --json-data '\''[{\"op\":\"create_node\",\"id\":\"g1\",\"type\":\"goal\",\"title\":\"X\"}]'\'' "}}' | bash plugins/brain/hooks/validate-brain-write.sh
echo $?  # Should be 2 (blocked -- goal without person edge)
```

### Validating the Plugin

```bash
claude plugin validate plugins/brain
```

## Submitting Changes

1. Fork the repo
2. Create a feature branch
3. Run `claude plugin validate plugins/brain` to verify the manifest
4. Test your hooks manually (see above)
5. Submit a PR with a clear description

## The CLI

The plugin is a thin integration layer. The real logic lives in [brain-cli](https://github.com/X-Arc-ai/brain-cli). If your change involves CLI behavior, graph operations, or hygiene rules, contribute there instead.

## License

Apache 2.0. By contributing, you agree your contributions will be licensed under the same terms.
