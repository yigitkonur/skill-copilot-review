# skill-copilot-review

> **other skills by [@yigitkonur](https://github.com/yigitkonur):**
> [testing mcp servers](https://github.com/yigitkonur/skill-mcp-server-tester) · [extracting design dna from dashboards](https://github.com/yigitkonur/skill-design-soul-saas) · [converting saved webpages to next.js](https://github.com/yigitkonur/skill-snapshot-to-nextjs) · [generating greptile review config](https://github.com/yigitkonur/skill-greptile-init) · [generating devin review config](https://github.com/yigitkonur/skill-devin-review-init) · [reviewing mcp-use python apps](https://github.com/yigitkonur/skill-mcp-use) · [tauri observability & mcp bridge](https://github.com/yigitkonur/skill-tauri-mcp) · [mcp server for searching skills](https://github.com/yigitkonur/mcp-skills-as-context)

a claude code skill that generates `.github/copilot-instructions.md` and multiple `.github/instructions/*.instructions.md` micro-files for [github copilot](https://github.com/features/copilot) code review. analyzes your codebase and writes focused, path-specific review rules scoped via `applyTo` frontmatter — each file tuned to your actual stack, patterns, and architecture.

## what it does

github copilot code review reads instruction files from your repo to guide its automated pr analysis. this skill reads your actual code and generates a complete set of micro-files:

- **`copilot-instructions.md`** — repository-wide review standards applied to every file (security, quality, cross-cutting concerns)
- **multiple `*.instructions.md`** — focused micro-files in `.github/instructions/`, each scoped to specific file types via `applyTo` frontmatter

### why micro-files

copilot only reads the first **4,000 characters** of any instruction file — the rest is silently ignored. instead of one monolithic file that overflows, this skill generates 5-12 small, focused files:

- each stays within the 4,000-character processing window
- language-specific rules never bleed into unrelated files (typescript rules don't apply to python)
- teams can own and iterate on their relevant files independently
- adding a new concern means adding a new file, not editing a monolith

### what goes in each micro-file

every `*.instructions.md` gets:

- **`applyTo` frontmatter** — a glob pattern scoping the file to specific paths (e.g., `**/*.{ts,tsx}`, `**/api/**/*.ts`, `**/src-tauri/**/*.rs`)
- **prioritized rules** — security and critical issues near the top, conventions and testing lower
- **code examples** — before/after patterns from your actual codebase, not generic samples
- **semantic rules only** — things linters can't catch. if eslint or prettier already enforce it, the rule is skipped

### micro-file examples

a typical typescript + next.js dashboard produces:

```
.github/
├── copilot-instructions.md
└── instructions/
    ├── typescript.instructions.md        → applyTo: "**/*.{ts,tsx}"
    ├── nextjs-app.instructions.md        → applyTo: "**/app/**/*.{ts,tsx}"
    ├── server-actions.instructions.md    → applyTo: "**/actions/**/*.ts"
    ├── api-routes.instructions.md        → applyTo: "**/api/**/*.ts"
    ├── security.instructions.md          → applyTo: "**/{auth,admin}/**/*.ts"
    └── testing.instructions.md           → applyTo: "**/*.{test,spec}.{ts,tsx}"
```

## scenario coverage

the skill includes complete reference instruction sets for:

- typescript backend api (express/fastify + prisma)
- next.js admin dashboard (nextauth rbac)
- python django rest api (drf + celery)
- go microservice (chi + sqlx + grpc)
- tauri v2 desktop app (rust + react)
- mcp server (typescript, stdio transport)
- typescript monorepo (api + web + shared)
- next.js marketing website (app router + sanity cms)

plus a library of 20+ individual micro-file templates covering: typescript, python, rust, go, c#, react, next.js, django, express, tauri, angular, api routes, database/orm, security, testing, accessibility, mcp, ci/cd, docker, graphql, and state management.

these are references the agent uses to understand patterns, not templates that get copy-pasted. everything adapts to what it actually finds in your code.

## how copilot uses it

when reviewing a pr, copilot loads:

1. `copilot-instructions.md` — applied to every changed file
2. all `*.instructions.md` files whose `applyTo` pattern matches the changed file

a `.tsx` component file might receive rules from `typescript.instructions.md`, `react.instructions.md`, and `a11y.instructions.md` simultaneously. rules are additive across matching files.

## usage

```
set up copilot code review for this repo
```

```
generate copilot instruction files for this project
```

```
create .github/instructions for copilot review
```

```
our copilot reviews keep missing security issues, fix the instructions
```

### file overview

| file | lines | what it covers |
|------|-------|---------------|
| `SKILL.md` | 219 | main skill definition — 5-phase workflow, decision matrices, validation checklist |
| `references/instruction-spec.md` | 326 | full format spec — frontmatter syntax, glob patterns, character limits, supported features |
| `references/anti-patterns.md` | 276 | common mistakes and how to avoid them, debugging sequence, verification protocol |
| `references/scenarios.md` | 1,428 | 8 complete instruction sets with all micro-files (ts api, next.js, django, go, tauri, mcp, monorepo, marketing) |
| `references/micro-library.md` | 941 | 20+ individual `*.instructions.md` templates by language, framework, and domain |

### scope

**built for:** any git repository that uses [github copilot code review](https://docs.github.com/en/copilot/using-github-copilot/code-review/using-copilot-code-review). generates `copilot-instructions.md` and micro `*.instructions.md` files tailored to your actual codebase patterns.

**not for:** repos not using copilot for code review. for greptile, use [skill-greptile-init](https://github.com/yigitkonur/skill-greptile-init). for devin, use [skill-devin-review-init](https://github.com/yigitkonur/skill-devin-review-init).

## install

```bash
npx skills add yigitkonur/skill-copilot-review
```

> works with claude code, cursor, codex, copilot, windsurf, and [30+ other agents](https://skills.sh).

## license

mit
