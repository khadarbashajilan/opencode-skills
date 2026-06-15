# OpenCode Skills Collection

This repository contains reusable agent skills for [**OpenCode**](https://opencode.ai) — an open-source AI coding assistant. Each skill is defined by a `SKILL.md` file that teaches the agent how to perform a specific task.

These skills were originally authored for Claude (Anthropic's `SKILL.md` format) and are fully compatible with OpenCode, which adopts the same format.

---

## Prerequisites

You need **OpenCode** installed. If you don't have it yet:

### Linux / macOS (terminal)

```bash
curl -fsSL https://opencode.ai/install.sh | bash
```

Or via npm:

```bash
npm install -g @opencode/opencode
```

### Windows (PowerShell)

```powershell
powershell -c "irm https://opencode.ai/install.ps1 | iex"
```

Or via npm:

```powershell
npm install -g @opencode/opencode
```

> **Verify installation:** `opencode --version`

---

## Quick Start

### Linux / macOS

```bash
# Copy skills to your global opencode config
cp -r skills ~/.config/opencode/skills

# Run opencode — skills auto-discover
opencode
```

### Windows (PowerShell)

```powershell
# Copy skills to your global opencode config
Copy-Item -Path "skills\*" -Destination "$env:USERPROFILE\.config\opencode\skills\" -Recurse

# Run opencode — skills auto-discover
opencode
```

Skills are auto-discovered from these locations:

| Location | Scope |
|---|---|
| `~/.config/opencode/skills/<name>/SKILL.md` | Global (all projects) |
| `.opencode/skills/<name>/SKILL.md` | Per-project |
| `.claude/skills/<name>/SKILL.md` | Claude-compatible (also read by OpenCode) |

---

## How Skills Work in OpenCode

OpenCode reads the `name` and `description` from each `SKILL.md` frontmatter and lists them in the built-in `skill` tool. When the agent decides a skill matches your request, it loads the full `SKILL.md` content and follows its instructions.

**Example `SKILL.md` frontmatter:**

```yaml
---
name: pdf
description: Read, create, edit, and manipulate PDF files. Use this skill whenever the user mentions a .pdf file or asks to produce one.
---
```

The agent sees something like:

```
<available_skills>
  <skill>
    <name>pdf</name>
    <description>Read, create, edit, and manipulate PDF files...</description>
  </skill>
  ...
</available_skills>
```

You don't need to manually invoke skills — the agent loads them automatically when relevant.

---

## Available Skills

| Skill | Description |
|---|---|
| **canvas-design** | Create beautiful visual art in .png and .pdf using design philosophy |
| **doc-coauthoring** | Structured workflow for co-authoring documentation, proposals, and specs |
| **event-planning** | Plan events from birthday dinners to weddings — venues, vendors, timelines, budgets |
| **frontend-design** | Create distinctive, production-grade frontend interfaces |
| **gif-creator** | Create animated GIFs optimized for Slack (emoji & message formats) |
| **learn** | Interactive teaching mode — helps you understand concepts rather than just answering |
| **mcp-builder** | Build MCP (Model Context Protocol) servers in Python or TypeScript |
| **pdf** | Full PDF toolkit — create, merge, split, watermark, encrypt, OCR, extract text/tables |
| **pdf-reading** | Read and inspect PDFs — text extraction, page rasterization, table/form extraction |
| **skill-creator** | Create, edit, optimize, and benchmark OpenCode/Claude skills |
| **web-artifacts-builder** | Build complex single-file HTML artifacts with React, Tailwind, shadcn/ui |
| **word-document** | Create, read, edit, and manipulate .docx Word documents |

---

## Controlling Skill Access

You can restrict which skills agents can load using permissions in `opencode.json`:

```json
{
  "permission": {
    "skill": {
      "*": "allow",
      "internal-*": "deny",
      "experimental-*": "ask"
    }
  }
}
```

| Permission | Behavior |
|---|---|
| `allow` | Loads immediately |
| `deny` | Hidden from agent |
| `ask` | Prompts you for approval |

You can also override per-agent:

```json
{
  "agent": {
    "plan": {
      "permission": {
        "skill": {
          "documents-*": "allow"
        }
      }
    }
  }
}
```

---

## Creating Your Own Skills

Each skill is just a folder with a `SKILL.md` file:

```
~/.config/opencode/skills/my-skill/
  └── SKILL.md
```

**Required frontmatter:**

```yaml
---
name: my-skill
description: What this skill does, when to trigger it, and what output to expect
---
```

Name must be lowercase alphanumeric with hyphens (e.g., `my-skill`, `git-release`). Description must be 1–1024 characters. The rest of the file is plain markdown with instructions for the agent.

---

## Resources

- [OpenCode Docs — Agent Skills](https://opencode.ai/docs/skills/)
- [OpenCode GitHub](https://github.com/anomalyco/opencode)
- [OpenCode Discord](https://opencode.ai/discord)
