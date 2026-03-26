# AGENTS.md — Codex CLI Agent Definitions

This file follows the [Codex CLI AGENTS.md convention](https://github.com/openai/codex).

## memoriant-test-coverage-skill

**Purpose:** Audit Python projects for semantic test coverage gaps and generate pytest skeleton files to close them.

**Activation:** `codex run test-coverage-agent`

### Agent Behavior

The agent scans a Python project using AST inspection, maps which public functions already have tests, identifies every untested function, and writes ready-to-fill pytest skeleton files. It never overwrites existing tests, never modifies source code, and always emits a structured gap report alongside its generated stubs.

### Inputs

| Name | Type | Required | Default | Description |
|------|------|----------|---------|-------------|
| `project_path` | string | No | `.` | Root of the Python project to scan |
| `output_dir` | string | No | `tests/` | Directory for generated skeleton files |
| `include_private` | boolean | No | `false` | Include `_private` functions in analysis |
| `format` | string | No | `json` | Gap report format: `json` or `markdown` |

### Outputs

| Name | Description |
|------|-------------|
| `coverage_gaps.json` | Structured report of all untested functions |
| `tests/test_<module>.py` | Generated pytest skeleton files (one per module with gaps) |
| Console summary | Plain-text table of scanned/covered/gap counts |

### Agent File

See [`agents/test-coverage-agent.md`](agents/test-coverage-agent.md) for the full system prompt.

### Skill File

See [`skills/semantic-test-coverage/SKILL.md`](skills/semantic-test-coverage/SKILL.md) for the full methodology.

---

## Usage with Codex CLI

```bash
# Basic run against current directory
codex run test-coverage-agent

# Custom project path
codex run test-coverage-agent --var project_path=./my_project

# Include private functions and write markdown report
codex run test-coverage-agent --var include_private=true --var format=markdown
```

---

## Integration Notes

- Compatible with Claude Code, Codex CLI, and any agent runtime that supports Markdown-based agent definitions.
- No external API keys required for AST analysis. An LLM key is only needed if you use the agent interactively via Claude Code.
- Safe to run in CI pipelines — read-only analysis, writes only to `tests/` and the gap report.
