# Timothy Backend — NestJS API

You are an expert in TypeScript, NestJS, and scalable backend API development. You write functional, maintainable, performant, and secure code following NestJS and TypeScript best practices.

## Project Context

- **Project**: `timothy-back` — REST API backend for the Timothy ecosystem
- **Framework**: NestJS v12 with Express adapter
- **Module System**: ESM (ES Modules) — all files use `import`/`export`, `"type": "module"` in package.json
- **Companion Frontend**: `timothy-front` (Angular 21+ SPA) in sibling directory
- **Language**: TypeScript 6.0+ targeting ES2023
- **Testing**: Vitest 4.x (NOT Jest)
- **Linter**: oxlint (NOT ESLint)

## TypeScript Best Practices

- Use strict type checking (full `"strict": true` enabled)
- Prefer type inference when the type is obvious
- Avoid the `any` type; use `unknown` when type is uncertain
- Use `readonly` for properties that should not be mutated
- Prefer interfaces over types for object shapes

## ESM Module System

- All imports MUST use ESM syntax (`import`/`export`)
- Use `.js` extension in relative import paths (TypeScript compiles `.ts` → `.js`)
- Do NOT use `require()` or `module.exports`
- Use `import type` for type-only imports

## NestJS Architecture

- Follow the modular architecture pattern: one module per domain feature
- Each module should have its own directory under `src/` containing: module, controller, service, DTOs, entities
- Use barrel exports (`index.ts`) for cleaner imports
- Keep controllers thin — delegate business logic to services
- Use dependency injection via constructor parameters

### Module Structure Convention

```
src/
├── <feature>/
│   ├── <feature>.module.ts
│   ├── <feature>.controller.ts
│   ├── <feature>.service.ts
│   ├── dto/
│   │   ├── create-<feature>.dto.ts
│   │   └── update-<feature>.dto.ts
│   ├── entities/
│   │   └── <feature>.entity.ts
│   └── interfaces/
│       └── <feature>.interface.ts
├── common/
│   ├── decorators/
│   ├── filters/
│   ├── guards/
│   ├── interceptors/
│   └── pipes/
└── config/
    └── configuration.ts
```

### Controllers

- Use descriptive route paths following REST conventions
- Apply appropriate HTTP method decorators (`@Get`, `@Post`, `@Put`, `@Patch`, `@Delete`)
- Use DTOs for input validation — NestJS 12 supports Standard Schema (Zod, Valibot, ArkType) natively in `@Body()`, `@Query()`, `@Param()` as alternatives to `class-validator`
- Use `@HttpCode()` when the default status code isn't appropriate
- Always apply `@ApiTags()` and `@ApiOperation()` for Swagger documentation

### Services

- Design services around a single responsibility
- Use `@Injectable()` decorator
- Handle errors using NestJS built-in exceptions (`NotFoundException`, `BadRequestException`, etc.)
- Return typed responses, never raw database entities to controllers without transformation

### DTOs & Validation

- Prefer Standard Schema validation (Zod, Valibot) over class-validator for new code
- Separate Create and Update DTOs
- Group related validations logically

### Error Handling

- Use NestJS exception filters for consistent error responses
- Never expose internal errors or stack traces in production responses
- Use custom exception filters for domain-specific errors

## Database & ORM

- When configuring database, use `@nestjs/config` with validation schemas
- Define entities with clear column types and constraints
- Use migrations for schema changes, never synchronize in production
- Repository pattern for data access

## Security

- Use Guards for authentication and authorization
- Validate and sanitize all inputs
- Use environment variables for secrets (never hardcode)
- Apply rate limiting on public endpoints
- Enable CORS with explicit allowed origins

## Testing (Vitest)

- Write unit tests using Vitest (`describe`, `it`, `expect`)
- Use `vi.fn()` and `vi.spyOn()` for mocks (NOT `jest.fn()`)
- Test files must be co-located with source files (`*.spec.ts`)
- E2E tests go in `test/` directory using `vitest.config.e2e.ts`
- Use `supertest` for HTTP endpoint testing
- Vitest globals are configured — no need to import `describe`, `it`, `expect`

## Code Style

- Use Prettier with project config (singleQuote, trailingComma: all)
- Use oxlint for linting (`oxlint src/ test/`)
- Prefer `async/await` over raw Promises
- Use meaningful variable and function names in English
