# Next React Website Conventions

These conventions define how the marketing website should be built in `peakreps-website` using Next.js App Router, React, and the shared `core` package.

They describe the preferred way to structure pages, shared components, styling, analytics, and deployment work while staying aligned with the current website codebase.

## Scope

This stack covers:

- `app/` routes and layouts
- the `core/` shared website package
- styling through Tailwind, CSS Modules, and global CSS tokens
- forms, analytics, copy-heavy landing pages, and Vercel deployment concerns

## Golden Rules

1. Keep route files thin and compose from `core` whenever possible.
2. Put reusable UI in `core`, not duplicated across pages.
3. Preserve a coherent design system through shared tokens and wrappers.
4. Treat performance, accessibility, and responsive behavior as baseline requirements.
5. Keep marketing copy editable without entangling it with rendering complexity.
6. Centralize analytics and external integration wiring.
7. Prefer server-safe defaults and explicit client boundaries.
8. Protect the deploy path, especially the private core submodule workflow.

## Document Map

- `architecture-and-routing.md` covers route structure, layouts, the shared core package, and composition boundaries.
- `design-system-and-components.md` covers styling, typography, components, responsiveness, and accessibility.
- `forms-analytics-and-delivery.md` covers forms, analytics, API integration, and deployment workflow.