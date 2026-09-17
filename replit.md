# Student Management System

A focused student information system for reviewing, searching, and maintaining student records.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `lib/api-spec/openapi.yaml` — source of truth for the student CRUD and dashboard API contract
- `lib/db/src/schema/students.ts` — student table and insert schema
- `artifacts/api-server/src/routes/students.ts` — validated student and dashboard endpoints
- `artifacts/student-management/src/App.tsx` — dashboard, directory, detail view, and CRUD interactions
- `artifacts/student-management/src/index.css` — shared visual theme and responsive styles

## Architecture decisions

- API contracts are generated from OpenAPI so the backend and React client share request and response types.
- Calendar enrollment dates use PostgreSQL `date` values to avoid timezone shifts; timestamps remain timezone-aware instants.
- The dashboard summary is a read-only aggregate endpoint so counts and program mix stay consistent with the directory.

## Product

- Dashboard with live enrollment totals, program mix, recent additions, and service health.
- Searchable and filterable student directory.
- Create, view, edit, and delete student records with validation and cache refreshes.
- Responsive record detail view for contact and enrollment information.

## Gotchas

- After changing `lib/api-spec/openapi.yaml`, run `pnpm --filter @workspace/api-spec run codegen` before touching generated API hooks or server schemas.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
