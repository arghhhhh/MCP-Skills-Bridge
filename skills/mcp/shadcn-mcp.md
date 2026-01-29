# shadcn MCP Skill

Use this skill when the user wants to work with shadcn/ui components, search registries, view component details, or add components to their project.

## When to Use
- User asks about shadcn components
- User wants to add UI components to their project
- User needs component examples or demos
- User wants to search for components in registries

## Commands

### Get Project Registries
```bash
npx mcporter call shadcn.get_project_registries
```

### List Items in Registries
```bash
npx mcporter call shadcn.list_items_in_registries registries:'["@shadcn"]' limit:10
```

### Search for Components
```bash
npx mcporter call shadcn.search_items_in_registries registries:'["@shadcn"]' query:"button"
```

### View Component Details
```bash
npx mcporter call shadcn.view_items_in_registries items:'["@shadcn/button", "@shadcn/card"]'
```

### Get Usage Examples
```bash
npx mcporter call shadcn.get_item_examples_from_registries registries:'["@shadcn"]' query:"button-demo"
```

### Get Add Command
```bash
npx mcporter call shadcn.get_add_command_for_items items:'["@shadcn/button", "@shadcn/card"]'
```

### Get Audit Checklist
```bash
npx mcporter call shadcn.get_audit_checklist
```

## Common Workflows

### Adding a Component
1. Search for the component: `search_items_in_registries`
2. View details: `view_items_in_registries`
3. Get examples: `get_item_examples_from_registries`
4. Get add command: `get_add_command_for_items`
5. Run the add command in terminal
6. Verify with audit checklist: `get_audit_checklist`

### Exploring Available Components
1. Get registries: `get_project_registries`
2. List all items: `list_items_in_registries`
3. Search for specific needs: `search_items_in_registries`

## Tips
- Array parameters must be JSON formatted: `registries:'["@shadcn"]'`
- Common example patterns: `{item-name}-demo`, `{item-name} example`
- Use `get_audit_checklist` after adding new components to verify setup
