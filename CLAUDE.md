# Global Rules

## How to Use MCPorter Skills

When a task matches one of the triggers below, read the corresponding skill file and follow its instructions. Use `npx mcporter call <server>.<tool>` to execute commands.

---

## Adding New MCPs

When user wants to add a new MCP server or create a skill file, read `~/.claude/skills/mcp-setup.md`.

Trigger phrases: "add MCP", "new MCP server", "create skill for", "set up [tool] MCP"

---

## Context7 - Library Documentation

When user needs documentation, code examples, or API references for any programming library/framework, read `~/.claude/skills/mcp/context7-docs.md`.

Trigger phrases: "how do I", "docs for", "latest API", "code examples", library/framework questions

---

## Blender - 3D Scene Control

When user wants to control Blender, create 3D scenes, or manipulate objects, read `~/.claude/skills/mcp/blender-mcp.md`.

Trigger phrases: "in Blender", "3D model", "shader", "texture", "render"

---

## shadcn/ui - Component Library

When user wants to add UI components, search shadcn registries, or view component examples, read `~/.claude/skills/mcp/shadcn-mcp.md`.

Trigger phrases: "shadcn", "add button", "add card", "UI component", "component example"

---

## General MCPorter Usage

- List servers: `npx mcporter list`
- List tools: `npx mcporter list <server> --all-parameters`
- Call tool: `npx mcporter call <server>.<tool> param:"value"`
- All skill files are in `~/.claude/skills/`
