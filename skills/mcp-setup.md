# MCP Setup Skill

Use this skill when the user wants to add a new MCP server and create a skill file for it.

## Workflow

### Step 1: Add MCP Server to Config

Add the server to `~/.cursor/mcp.json` (or the user's preferred MCP config location):

```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@package/mcp-server"]
    }
  }
}
```

For servers requiring API keys:
```json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@package/mcp-server", "--api-key", "KEY_HERE"]
    }
  }
}
```

### Step 2: Discover Available Tools

List all tools and their parameters:
```bash
npx mcporter list <server-name> --all-parameters
```

### Step 3: Test Tools & Discover Hidden Requirements

**IMPORTANT**: Always test a few tools before writing the skill file. MCPs often have undocumented requirements.

```bash
# Try a simple read-only tool first
npx mcporter call <server>.<simple_tool>
```

**Common issues to watch for:**
- Missing required parameters (like blender's `user_prompt`)
- Parameter name differences from docs
- Authentication errors
- API version mismatches

**If a call fails**, read the error message carefully:
```
Error: 1 validation error for get_scene_info
user_prompt
  Field required [type=missing...]
```
This reveals hidden required params not shown in `--all-parameters`.

**Test pattern:**
1. Try the most basic tool with no params
2. Add params one by one based on errors
3. Document the ACTUAL working syntax in the skill file

### Step 4: Create Skill File

Create `~/.claude/skills/mcp/<server-name>.md` with this structure:

```markdown
# [Server Name] Skill

Use this skill when [describe trigger conditions].

## When to Use
- [Condition 1]
- [Condition 2]
- [Condition 3]

## Commands

### [Tool 1 Name]
```bash
npx mcporter call <server>.<tool> param1:"value" param2:"value"
```

### [Tool 2 Name]
```bash
npx mcporter call <server>.<tool> param1:"value"
```

## Common Workflows

### [Workflow 1]
1. Step 1
2. Step 2

## Quirks & Gotchas
- [Document any hidden requirements discovered during testing]
- [Parameter naming issues]
- [Workarounds for known bugs]

## Tips
- [Tip 1]
- [Tip 2]
```

### Step 4: Update CLAUDE.md Router

Add a brief entry to `~/.claude/CLAUDE.md`:

```markdown
## [Server Name] - [Brief Description]

When [trigger condition], read `~/.claude/skills/mcp/<server-name>.md` and follow its instructions.
```

Keep CLAUDE.md entries to 2-3 lines max - all details go in the skill file.

## Example: Adding a New MCP

User: "Add the GitHub MCP server"

1. **Config** - Add to `~/.cursor/mcp.json`
2. **Discover** - `npx mcporter list github --all-parameters`
3. **Create** - Write `~/.claude/skills/mcp/github.md` with tool docs
4. **Route** - Add to CLAUDE.md:
   ```
   ## GitHub - Repository and PR Management
   When user needs GitHub operations (repos, PRs, issues), read `~/.claude/skills/mcp/github.md`.
   ```

## Maintenance Tips

- **CLAUDE.md**: Max ~50 lines, just triggers/pointers
- **Skill files**: Can be detailed, only loaded when needed
- **Periodic cleanup**: Remove unused MCP entries
- **Consolidate**: If multiple MCPs serve similar purposes, combine skill files
