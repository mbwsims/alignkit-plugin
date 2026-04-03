# Convention Categories

Analyze each category below when discovering conventions. For each, look for consistent
patterns across sampled files. Skip categories that aren't relevant to the project's
stack.

## Import & Module Patterns

**What to look for:**
- Path alias usage (`@/`, `~/`, `#/`) vs relative paths (`../`)
- Import ordering (external → internal → types → styles)
- Named exports vs default exports
- Barrel files / index re-exports
- Dynamic imports vs static imports
- Module system (ESM vs CJS)

**How to detect:**
- Grep for `import .* from` patterns across source files
- Check for `export default` vs `export { ... }` or `export function/const`
- Look at tsconfig.json `paths` configuration
- Count relative imports with `../` vs alias imports

**Example discoveries:**
- "All 34 files use @/ alias — zero ../.. relative imports"
- "Named exports exclusively — 0 default exports outside Next.js pages"
- "Imports ordered: external packages, then @/ internal, then types"

## Naming Conventions

**What to look for:**
- File naming: kebab-case, camelCase, PascalCase, snake_case
- Component naming patterns
- Function naming (handle*, use*, get*, create*, etc.)
- Variable naming for specific concepts (isX, hasX for booleans)
- Directory naming conventions
- Test file naming (*.test.ts, *.spec.ts, __tests__/)

**How to detect:**
- List files and observe naming patterns
- Grep for function declarations and observe naming
- Check for consistent prefixes/suffixes

**Example discoveries:**
- "All React components use PascalCase files matching component name"
- "All boolean variables prefixed with is/has/should"
- "Test files co-located as *.test.ts next to source"

## Error Handling

**What to look for:**
- Try/catch patterns in handlers
- Error response shapes ({ error: string }, { message, code }, etc.)
- Custom error classes or error factories
- Error logging patterns
- Validation approaches (zod, joi, manual checks)
- Error boundary patterns (React)

**How to detect:**
- Grep for `catch` blocks and observe handling patterns
- Grep for `throw new` to find error creation patterns
- Check API routes for response format on errors
- Look for validation library usage in package.json

**Example discoveries:**
- "All 8 API routes return { error: string } with try/catch"
- "Zod schemas validate all API inputs — no manual validation"
- "Custom AppError class used consistently for domain errors"

## API & Route Patterns

**What to look for:**
- Route organization (file-based, directory-based)
- HTTP method handling patterns
- Request parsing approach
- Response format consistency
- Authentication/authorization patterns
- Middleware usage

**How to detect:**
- List all route files and observe structure
- Read 3-4 route handlers and compare patterns
- Check for shared middleware or wrapper functions
- Look for auth checks and where they appear

**Example discoveries:**
- "Every API route wraps handler in withAuth() middleware"
- "All POST/PUT routes validate body with zod schema as first step"
- "Responses always include { data: T } on success, { error: string } on failure"

## Data Access Patterns

**What to look for:**
- ORM usage (Prisma, Drizzle, TypeORM, Sequelize)
- Raw SQL presence or absence
- Repository pattern vs direct ORM calls
- Transaction patterns
- Query builder patterns
- Data access location (centralized in lib/db/ vs scattered)

**How to detect:**
- Check package.json for ORM dependencies
- Grep for raw SQL queries (queryRaw, executeRaw, pool.query, etc.)
- Grep for ORM calls and where they originate
- Look for data access layer directories

**Example discoveries:**
- "All DB access through Prisma — zero raw SQL anywhere in src/"
- "Data access centralized in src/lib/db/ — no ORM calls in route handlers"
- "Every mutation wrapped in prisma.$transaction"

## Testing Patterns

**What to look for:**
- Test framework and runner
- Test file location and naming
- Describe/it vs test() style
- Mock patterns
- Fixture patterns
- Coverage configuration
- Integration vs unit test separation

**How to detect:**
- Check package.json for test dependencies and scripts
- Find test files and read 2-3 to observe style
- Check for test configuration files
- Look for mock directories or setup files

**Example discoveries:**
- "Vitest with describe/it blocks — no standalone test() calls"
- "Tests in __tests__/ directories adjacent to source"
- "Database tests use a shared setupTestDB() helper — no raw connection setup"

## Type Patterns (TypeScript/Flow)

**What to look for:**
- Type file organization (*.types.ts, types/ directory, inline)
- Interface vs type alias preference
- Enum usage vs const objects vs union types
- Generic patterns
- Strict mode usage and any/unknown handling
- Type assertion patterns

**How to detect:**
- Check tsconfig.json for strictness settings
- Grep for `interface` vs `type` declarations
- Grep for `as any`, `@ts-ignore`, `@ts-expect-error`
- Look for type files and where types are defined

**Example discoveries:**
- "Interfaces for object shapes, type aliases for unions — consistent split"
- "Zero @ts-ignore or @ts-expect-error in codebase"
- "All shared types in src/types/ — no inline type exports from modules"

## State & Configuration Patterns

**What to look for:**
- Environment variable handling (.env, config files)
- Configuration loading patterns
- State management approach (React context, stores, etc.)
- Feature flags
- Constants organization

**How to detect:**
- Check for .env files and how they're loaded
- Grep for process.env or import.meta.env
- Look for config/ or constants/ directories
- Check for state management libraries

**Example discoveries:**
- "All env vars accessed through src/lib/env.ts — never raw process.env"
- "Constants centralized in src/constants/ — no magic strings in source"
- "React context for auth state, server components for everything else"

## Git & Workflow Patterns

**What to look for:**
- Commit message format
- Branch naming conventions
- PR template presence
- CI/CD configuration
- Pre-commit hooks (husky, lint-staged)
- Changelog maintenance

**How to detect:**
- Read recent git log for commit message patterns
- Check for .github/, .husky/, .commitlintrc
- Look for CI configuration files
- Check for PR templates

**Example discoveries:**
- "Commit messages follow Conventional Commits (feat:, fix:, chore:)"
- "All PRs require passing CI checks — GitHub Actions in .github/workflows/"
- "Husky pre-commit runs lint-staged on changed files"

## Architecture Boundaries

**What to look for:**
- Layer separation (UI → API → data)
- Import restrictions between directories
- Shared vs feature-specific code boundaries
- Monorepo package boundaries
- Dependency injection patterns

**How to detect:**
- Map which directories import from which
- Look for index.ts barrel exports that define public APIs
- Check for eslint import restrictions
- Observe which modules are imported most widely

**Example discoveries:**
- "Components never import from lib/db/ directly — always through API routes"
- "src/shared/ is imported by all features — features never import from each other"
- "Each package has index.ts that defines its public API — no deep imports"

## Analysis Tips

**Sampling strategy:**
- Read files from at least 3 different directories
- Include both "core" and "leaf" modules
- Include both recent and older files (check git dates)
- Prioritize files with the most imports (likely central/important)

**Threshold for reporting:**
- 90%+ consistency → Strong convention, high-confidence rule
- 70-89% consistency → Likely convention with exceptions, note the exceptions
- Below 70% → Not a convention, don't report it

**What NOT to report:**
- Framework-imposed patterns (Next.js routing structure is not a "convention")
- Obvious language features (using TypeScript interfaces in a TypeScript project)
- Single-instance patterns (one file does something unique — not a convention)
- Patterns already documented in CLAUDE.md or .claude/rules/
