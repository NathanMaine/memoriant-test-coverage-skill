# memoriant-test-coverage-skill

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Claude%20Code%20%7C%20Codex%20CLI%20%7C%20Gemini%20CLI-lightgrey)
![Python](https://img.shields.io/badge/python-3.8%2B-yellow)

A Claude Code plugin that finds every untested function in a Python project using AST inspection and generates ready-to-fill pytest skeleton files for each gap.

Built on the methodology of [`NathanMaine/semantic-test-coverage-agent`](https://github.com/NathanMaine/semantic-test-coverage-agent) and packaged as a portable Claude Code skill + agent.

---

## What It Does

1. Scans `src/` recursively using Python's `ast` module — no mutation testing, no heuristic scoring.
2. Maps every public function and method to existing `test_*` functions in `tests/` using naming-convention matching.
3. Produces a structured JSON gap report listing every uncovered function.
4. Generates a `tests/test_<module>.py` skeleton file for each source module with gaps — complete with imports, one `class Test<Name>` per function, and a `_happy_path` / `_edge_case` stub pair.
5. Never overwrites existing tests. Never touches source code.

---

## Installation

### Claude Code

```bash
claude mcp add memoriant-test-coverage-skill
```

Or clone and reference locally:

```bash
git clone https://github.com/NathanMaine/memoriant-test-coverage-skill ~/.claude/plugins/memoriant-test-coverage-skill
```

### Codex CLI

```bash
codex install NathanMaine/memoriant-test-coverage-skill
```

### Gemini CLI

```bash
gemini extension install NathanMaine/memoriant-test-coverage-skill
```

---

## Usage

### In Claude Code (natural language)

```
Find what I'm missing in my test suite
```

```
Generate pytest stubs for all untested functions in src/
```

```
Run semantic test coverage on ./backend and write skeletons to ./backend/tests
```

### Via skill invocation

```
/semantic-test-coverage --project-path ./my_project --output-dir ./my_project/tests
```

### Via Codex CLI

```bash
codex run test-coverage-agent --var project_path=./my_project
```

---

## Plugin Structure

```
memoriant-test-coverage-skill/
├── .claude-plugin/
│   └── plugin.json              # Plugin manifest
├── skills/
│   └── semantic-test-coverage/
│       └── SKILL.md             # Full methodology for Claude Code
├── agents/
│   └── test-coverage-agent.md  # Autonomous agent definition
├── AGENTS.md                    # Codex CLI agent definitions
├── gemini-extension.json        # Gemini CLI extension manifest
├── SECURITY.md                  # Security policy
├── README.md                    # This file
└── LICENSE                      # MIT
```

---

## Configuration

| Parameter | Default | Description |
|-----------|---------|-------------|
| `project_path` | `.` | Root of the Python project to scan |
| `output_dir` | `tests/` | Where to write generated skeleton files |
| `include_private` | `false` | Also generate stubs for `_private` functions |
| `format` | `json` | Gap report format: `json` or `markdown` |

---

## Output

### Gap Report (`coverage_gaps.json`)

```json
{
  "scanned_at": "2026-03-25T10:00:00Z",
  "project_path": "/home/dev/my_project",
  "total_source_functions": 42,
  "total_covered": 31,
  "gap_count": 11,
  "gaps": {
    "src/generator.py": [
      {"type": "function", "name": "render_skeleton"},
      {"type": "function", "name": "write_output_file"}
    ]
  }
}
```

### Console Summary

```
Semantic Test Coverage Report
─────────────────────────────
Source functions found : 42
Already covered        : 31
Gaps identified        : 11
Skeleton files written : 3

New stubs written to:
  tests/test_generator.py      (2 functions)
  tests/test_analyzer.py       (7 functions)
  tests/test_main.py           (2 functions)
```

---

## Source Reference

This plugin is derived from [`NathanMaine/semantic-test-coverage-agent`](https://github.com/NathanMaine/semantic-test-coverage-agent), a CLI tool that scans Python projects via AST inspection and produces coverage gap reports and test skeletons.

---

## License

MIT — see [LICENSE](LICENSE).
