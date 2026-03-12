# MCP Skills Bridge with MCPorter

A skills-based approach for using [MCPorter](https://github.com/steipete/mcporter) with Claude Code. This system saves context tokens by loading MCP tool documentation only when needed, through Claude's skills routing system.

## The Problem

MCP servers expose many tools, but loading all their schemas and documentation into every Claude conversation wastes context. You end up with thousands of tokens of tool definitions Claude may never use.

## The Solution

This project uses a two-tier system:

1. **CLAUDE.md** - A lightweight router (~50 lines) that matches trigger phrases to skill files
2. **Skill files** - Detailed documentation for each MCP, loaded only when relevant

When you say "add a button component", Claude reads CLAUDE.md, sees it matches shadcn triggers, then loads the full shadcn skill file. Tools are invoked via MCPorter CLI rather than native MCP connections.

## Project Structure

```
~/.claude/
├── CLAUDE.md              # Router with trigger phrases
└── skills/
    ├── mcp-setup.md       # Meta-skill: how to add new MCPs
    └── mcp/               # MCP-specific skill files
        ├── context7-docs.md
        ├── blender-mcp.md
        └── shadcn-mcp.md
```

> **Note:** Skill files can be in subdirectories. Just update the paths in CLAUDE.md accordingly (e.g., `~/.claude/skills/mcp/blender-mcp.md`).

## Installation

### 1. Install MCPorter

```bash
npm install -g mcporter
```

### 2. Copy Skills to Your Claude Config

Copy the `CLAUDE.md` and `skills/` folder to your Claude configuration directory:

**macOS / Linux / Git Bash:**
```bash
# Create the skills directory
mkdir -p ~/.claude/skills

# Copy files
cp CLAUDE.md ~/.claude/
cp -r skills/* ~/.claude/skills/
```

**Windows (cmd.exe):**
```cmd
mkdir %USERPROFILE%\.claude\skills\mcp
copy CLAUDE.md %USERPROFILE%\.claude\
xcopy skills %USERPROFILE%\.claude\skills\ /E /I
```

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "$HOME\.claude\skills\mcp"
Copy-Item CLAUDE.md "$HOME\.claude\"
Copy-Item -Recurse skills\* "$HOME\.claude\skills\"
```

### 3. Configure Your MCP Servers

Add MCP servers to your MCP config file (e.g., `~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@context7/mcp"]
    },
    "shadcn": {
      "command": "npx",
      "args": ["-y", "@anthropic/shadcn-mcp"]
    }
  }
}
```

### 4. Verify Setup

```bash
# List available servers
npx mcporter list

# List tools for a server
npx mcporter list context7 --all-parameters
```

## Usage

Just talk to Claude naturally. The router handles the rest:

| You say... | Claude loads... |
|------------|-----------------|
| "How do I use hooks in React?" | `context7-docs.md` |
| "Add a card component" | `shadcn-mcp.md` |
| "Create a 3D cube in Blender" | `blender-mcp.md` |
| "Set up the GitHub MCP" | `mcp-setup.md` |

## How It Works

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────┐
│ User prompt │ -> │  CLAUDE.md  │ -> │ Skill file  │ -> │ MCPorter│
│             │    │  (router)   │    │ (full docs) │    │  CLI    │
└─────────────┘    └─────────────┘    └─────────────┘    └─────────┘
                         │                   │
                   ~50 lines            Only loaded
                   always loaded        when needed
```

### MCPorter Commands

```bash
# List all configured servers
npx mcporter list

# List tools with parameters
npx mcporter list <server> --all-parameters

# Call a tool
npx mcporter call <server>.<tool> param:"value"

# Examples
npx mcporter call context7.resolve-library-id libraryName:"react"
npx mcporter call shadcn.search_items_in_registries query:"button"
```

## Adding New MCPs

Use the included `mcp-setup.md` skill. Tell Claude:

> "Add the GitHub MCP server"

Or follow the manual process:

1. Add server to your MCP config
2. Discover tools: `npx mcporter list <server> --all-parameters`
3. **Test tools** - MCPs often have undocumented requirements
4. Create skill file at `~/.claude/skills/mcp/<server>.md`
5. Add router entry to CLAUDE.md

See [skills/mcp-setup.md](skills/mcp-setup.md) for the complete workflow.

## Skill File Template

```markdown
# [Server Name] Skill

Use this skill when [describe trigger conditions].

## When to Use
- [Condition 1]
- [Condition 2]

## Commands

### [Tool Name]
\`\`\`bash
npx mcporter call <server>.<tool> param:"value"
\`\`\`

## Common Workflows

### [Workflow Name]
1. Step 1
2. Step 2

## Quirks & Gotchas
- [Document hidden requirements discovered during testing]
```

## Included Example Skills

| Skill | Description | Trigger Phrases |
|-------|-------------|-----------------|
| `context7-docs.md` | Library documentation lookup | "how do I", "docs for", "latest API" |
| `shadcn-mcp.md` | UI component library | "shadcn", "add button", "UI component" |
| `blender-mcp.md` | 3D scene control | "in Blender", "3D model", "render" |
| `mcp-setup.md` | Add new MCP servers | "add MCP", "new MCP server" |

## Why MCPorter?

[MCPorter](https://github.com/steipete/mcporter) bridges the gap between MCP servers and CLI-based workflows:

- **No native connection required** - Works with any Claude interface
- **Testable** - Debug MCP calls directly in terminal
- **Composable** - Chain with other CLI tools
- **Transparent** - See exactly what's being sent/received

## Tips

- Keep CLAUDE.md under ~50 lines - it's loaded on every conversation
- Put all detailed documentation in skill files
- Test MCP tools before writing skill files - they often have undocumented requirements
- Use the `Quirks & Gotchas` section to document workarounds
- Organize skills into subdirectories if you have many

## License

MIT
