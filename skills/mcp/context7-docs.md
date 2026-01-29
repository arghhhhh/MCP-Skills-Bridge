# Context7 Documentation Lookup Skill

Use this skill when the user needs up-to-date documentation, code examples, or API references for any programming library, framework, or package.

## When to Use
- User asks "how do I do X in [library]?"
- User needs current documentation (not training data)
- User wants code examples for a specific library
- User is debugging library-specific issues
- User mentions needing "latest docs" or "current API"

## Prerequisites
- Context7 MCP server must be configured

## Workflow

**Always follow this 2-step process:**

### Step 1: Resolve the Library ID
First, find the Context7-compatible library ID:
```bash
npx mcporter call context7.resolve-library-id query:"what the user wants to do" libraryName:"library name"
```

Example:
```bash
npx mcporter call context7.resolve-library-id query:"how to set up authentication" libraryName:"nextjs"
```

### Step 2: Query the Documentation
Use the library ID from step 1 to query docs:
```bash
npx mcporter call context7.query-docs libraryId:"/vercel/next.js" query:"how to set up authentication with middleware"
```

## Examples

### React Hooks
```bash
npx mcporter call context7.resolve-library-id query:"useEffect cleanup" libraryName:"react"
npx mcporter call context7.query-docs libraryId:"/facebook/react" query:"useEffect cleanup function examples"
```

### Next.js App Router
```bash
npx mcporter call context7.resolve-library-id query:"app router layouts" libraryName:"nextjs"
npx mcporter call context7.query-docs libraryId:"/vercel/next.js" query:"how to create nested layouts in app router"
```

### Python FastAPI
```bash
npx mcporter call context7.resolve-library-id query:"dependency injection" libraryName:"fastapi"
npx mcporter call context7.query-docs libraryId:"/tiangolo/fastapi" query:"dependency injection examples"
```

### Prisma ORM
```bash
npx mcporter call context7.resolve-library-id query:"relations" libraryName:"prisma"
npx mcporter call context7.query-docs libraryId:"/prisma/prisma" query:"how to define one-to-many relations"
```

## Tips
- Be specific in queries: "JWT authentication setup" > "auth"
- Include context: "React useEffect cleanup" > "hooks"
- Limit to 3 calls per question - use best result if not found
- Library IDs follow format: `/org/project` or `/org/project/version`

## Common Library IDs
- React: `/facebook/react`
- Next.js: `/vercel/next.js`
- Vue: `/vuejs/vue`
- Express: `/expressjs/express`
- FastAPI: `/tiangolo/fastapi`
- Prisma: `/prisma/prisma`
- Tailwind: `/tailwindlabs/tailwindcss`
