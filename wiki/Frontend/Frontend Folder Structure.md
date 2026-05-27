---
type: legacy-note
tags: [frontend, architecture, react, nextjs]
sources: []
updated: 2026-05-27
note: Pre-schema hand-written page. Not yet integrated with raw/ sources or [[index]]. Re-ingest as a wiki page (with proper citations + wikilinks) when relevant raw sources are added.
---

# Large-Scale React/Next.js Folder Structure

Feature-first + shared layer. Scale to 100+ devs. App Router assumed.

## Top Level

```
.
├── app/                  # Next.js App Router (routes only, thin)
├── src/
│   ├── features/         # Domain modules (vertical slices)
│   ├── shared/           # Cross-feature reusables
│   ├── entities/         # Domain models + types
│   ├── widgets/          # Composite UI blocks (cross-feature)
│   ├── lib/              # Framework-agnostic helpers
│   ├── config/           # Env, constants, feature flags
│   ├── styles/           # Globals, tokens, themes
│   └── types/            # Global TS types/ambient
├── public/               # Static assets
├── tests/                # E2E (Playwright/Cypress)
├── scripts/              # Codegen, migration, tooling
├── .storybook/
├── next.config.ts
├── tsconfig.json
└── package.json
```

## `app/` — Routes Only

Pages thin. Delegate to features. Co-locate route concerns.

```
app/
├── (marketing)/          # Route groups
│   └── page.tsx
├── (dashboard)/
│   ├── layout.tsx
│   ├── orders/
│   │   ├── page.tsx              # Server Component, calls feature
│   │   ├── loading.tsx
│   │   ├── error.tsx
│   │   └── [id]/
│   │       └── page.tsx
│   └── settings/
├── api/                  # Route handlers
│   └── webhooks/
│       └── stripe/route.ts
├── layout.tsx
├── not-found.tsx
└── global-error.tsx
```

Rule: no business logic in `app/`. Import from `features/` or `widgets/`.

## `src/features/` — Vertical Slices

One folder = one domain capability. Own UI, hooks, actions, schema.

```
features/
└── orders/
    ├── components/           # Feature-scoped React components
    │   ├── OrderTable.tsx
    │   ├── OrderForm.tsx
    │   └── OrderRow.tsx
    ├── hooks/                # Feature hooks
    │   ├── useOrders.ts
    │   └── useOrderMutation.ts
    ├── actions/              # Server Actions ('use server')
    │   ├── create-order.ts
    │   └── update-order.ts
    ├── api/                  # Client fetchers / RPC calls
    │   └── orders.client.ts
    ├── server/               # Server-only data access
    │   ├── queries.ts
    │   └── mutations.ts
    ├── schemas/              # Zod schemas
    │   └── order.schema.ts
    ├── lib/                  # Feature utils
    │   └── format-order.ts
    ├── store/                # Zustand/Jotai slice (if needed)
    │   └── order.store.ts
    ├── types.ts
    ├── constants.ts
    └── index.ts              # Public API (barrel — explicit exports)
```

Rule: features never import siblings directly. Cross-feature talk via `entities/` or `widgets/`.

## `src/shared/` — Reusable Primitives

UI kit, generic hooks, design system. No domain logic.

```
shared/
├── ui/                   # Design system primitives
│   ├── Button/
│   │   ├── Button.tsx
│   │   ├── Button.stories.tsx
│   │   ├── Button.test.tsx
│   │   └── index.ts
│   ├── Input/
│   ├── Modal/
│   ├── Toast/
│   └── index.ts
├── hooks/                # Generic hooks
│   ├── useDebounce.ts
│   ├── useMediaQuery.ts
│   ├── useLocalStorage.ts
│   └── useIntersectionObserver.ts
├── utils/                # Pure functions
│   ├── date.ts
│   ├── string.ts
│   ├── currency.ts
│   └── cn.ts             # className merge (clsx+tailwind-merge)
├── api/                  # HTTP client, interceptors
│   ├── client.ts         # axios/fetch wrapper
│   ├── errors.ts
│   └── types.ts
├── providers/            # Global React providers
│   ├── QueryProvider.tsx
│   ├── ThemeProvider.tsx
│   └── AuthProvider.tsx
└── icons/                # SVG icons
```

## `src/entities/` — Domain Models

Shared types, schemas, base components for a domain entity.

```
entities/
├── user/
│   ├── model/            # Types, schemas
│   │   └── user.schema.ts
│   ├── ui/               # UserAvatar, UserBadge (read-only repr)
│   └── index.ts
└── product/
```

## `src/widgets/` — Composite Blocks

Cross-feature UI compositions. Header, Sidebar, Footer.

```
widgets/
├── Header/
├── Sidebar/
├── CommandPalette/
└── NotificationCenter/
```

## `src/lib/` — Framework-Agnostic

Pure libs. No React. Adapters to external systems.

```
lib/
├── db/                   # Prisma client, query builders
│   └── prisma.ts
├── auth/                 # NextAuth/Clerk config
├── analytics/            # PostHog, Segment wrappers
├── logger.ts
├── cache.ts              # Redis client
└── email/                # Resend/SES wrapper
```

## `src/config/`

```
config/
├── env.ts                # T3 env, Zod-validated
├── site.ts               # Site metadata
├── nav.ts                # Nav config
└── flags.ts              # Feature flags
```

## Naming

- Components: `PascalCase.tsx`
- Hooks: `useThing.ts`
- Utils: `kebab-case.ts`
- Server Actions: `verb-noun.ts` (`create-order.ts`)
- Tests: co-located `*.test.ts(x)`
- Stories: co-located `*.stories.tsx`

## Import Boundaries

Enforce via `eslint-plugin-boundaries` or `dependency-cruiser`:

```
app        → features, widgets, shared, entities
widgets    → features, entities, shared
features   → entities, shared, lib
entities   → shared
shared     → (nothing internal)
lib        → (nothing internal)
```

No upward imports. No sibling feature imports.

## Path Aliases (`tsconfig.json`)

```json
{
  "compilerOptions": {
    "paths": {
      "@/app/*": ["./app/*"],
      "@/features/*": ["./src/features/*"],
      "@/shared/*": ["./src/shared/*"],
      "@/entities/*": ["./src/entities/*"],
      "@/widgets/*": ["./src/widgets/*"],
      "@/lib/*": ["./src/lib/*"],
      "@/config/*": ["./src/config/*"]
    }
  }
}
```

## Server vs Client Split

- Server Components default. Mark `'use client'` only at leaves.
- `features/*/server/` = server-only (`import 'server-only'`).
- `features/*/api/` = client fetchers.
- Server Actions in `features/*/actions/`, mark `'use server'` top of file.

## Testing Layout

```
features/orders/
├── components/OrderTable.tsx
├── components/OrderTable.test.tsx        # Unit (Vitest + RTL)
└── actions/create-order.test.ts          # Integration

tests/
├── e2e/
│   └── orders.spec.ts                    # Playwright
└── fixtures/
```

## Decision Rules

| Question | Answer |
|----------|--------|
| Used in 1 route? | `features/<x>/` |
| Used across features? | `widgets/` or `shared/` |
| Has domain meaning, no logic? | `entities/` |
| Pure function, no React? | `shared/utils/` or `lib/` |
| Talks to DB/external? | `lib/` or `features/*/server/` |
| Global config/env? | `config/` |

## References

- Feature-Sliced Design: feature-sliced.design
- Bulletproof React: github.com/alan2207/bulletproof-react
- Next.js docs: nextjs.org/docs/app/building-your-application/routing/colocation
