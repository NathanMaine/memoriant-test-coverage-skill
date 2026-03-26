# Security Policy

## Supported Versions

| Version | Supported |
|---------|-----------|
| 1.0.x   | Yes       |

## Scope

`memoriant-test-coverage-skill` is a Claude Code plugin that performs read-only static analysis of Python source files and writes pytest skeleton files. It contains no executable server code, no network calls, no authentication logic, and no credential handling.

## What This Plugin Does

- Reads `.py` files in a project directory using Python AST parsing.
- Writes skeleton test files to a `tests/` directory.
- Writes a JSON gap report.

It does **not**:
- Execute arbitrary code from the scanned project.
- Make network requests.
- Store or transmit any code or data outside the local filesystem.
- Require API keys or credentials of any kind.

## Reporting a Vulnerability

If you discover a security concern in this plugin — for example, a path traversal issue in file scanning, or a prompt injection risk in the skill instruction files — please report it responsibly.

**Contact:** Open a [GitHub Security Advisory](https://github.com/NathanMaine/memoriant-test-coverage-skill/security/advisories/new) on this repository.

Please include:
- A clear description of the issue.
- Steps to reproduce.
- The potential impact.

Do not open a public issue for security vulnerabilities.

## Response Timeline

- Acknowledgement within 5 business days.
- Assessment and patch (if warranted) within 30 days.

## Audit Notes

All skill and agent definitions in this plugin are plain Markdown files. They contain no executable code, no shell scripts, no binary blobs, and no network-facing logic. They are safe to audit as plain text.
