# Maharashtra Welfare Portal

MahaSetu is a public service guide that helps Maharashtra residents understand welfare schemes and reach official application portals.

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

- `artifacts/maha-welfare-portal/src/App.tsx` — public catalog, scheme details, Clerk routes, resident portal, and user activity view
- `artifacts/api-server/src/routes/schemes.ts` — welfare scheme data sourced from the provided PDF
- `artifacts/api-server/src/routes/portal.ts` — portal stats and authenticated user activity endpoints
- `lib/api-spec/openapi.yaml` — source of truth for API contracts
- `lib/db/src/schema/portal-users.ts` — database schema for registration and login activity
- `artifacts/maha-welfare-portal/src/index.css` — portal visual theme

## Architecture decisions

- Clerk handles authentication; the app does not store passwords or implement local auth.
- Portal user records are keyed by Clerk user ID and updated when the signed-in user opens the portal profile.
- Scheme content is kept in the API server so public pages use the same source as authenticated views.

## Product

- Search and filter 10 Maharashtra welfare schemes from the provided guide.
- Read purpose, eligibility, benefit, required documents, and official application links.
- Create an account or sign in with Clerk.
- View the signed-in resident portal and a protected registrations/login activity view at `/admin/users`.

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- Scheme requirements and benefit amounts may change; the UI reminds residents to verify details on the official portal.
- The registrations view requires a signed-in Clerk session.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
