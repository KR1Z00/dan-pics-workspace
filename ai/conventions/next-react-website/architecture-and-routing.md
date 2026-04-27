# Website Architecture And Routing Conventions

## App Router Structure

The `app/` directory should remain the route layer, not the component library.

Conventions:

- route files should compose shared sections and page templates rather than define large bespoke component trees inline
- route-level `page.tsx` files should stay readable at a glance
- `layout.tsx` should own app-wide wrappers, providers, fonts, metadata defaults, and persistent shells
- route groups or nested layouts should be introduced when they simplify structure, not preemptively

## The Core Package

`core/` should remain the shared website UI package.

It should contain:

- reusable sections for landing and marketing pages
- generic layout primitives
- shared hooks and utilities
- design tokens and shared styles
- headless or semi-headless UI primitives used by multiple surfaces

It should not contain:

- page-specific glue that only one route uses once
- route metadata or page-level SEO decisions
- app-specific provider wiring

## Import Boundaries

Preferred import behavior:

- app routes import stable public exports from `core/index` where practical
- direct deep imports into core are acceptable only when the export surface is intentionally narrow or still evolving
- if a deep import becomes common, promote it to the public export surface deliberately

Avoid inconsistent import styles for the same component family.

## Page Composition

Pages should be assembled from clearly named sections.

Preferred pattern:

1. route file selects the page composition
2. sections come from `core`
3. route-local styling is only used when the page truly needs route-specific treatment

If multiple routes share a pattern, extract a core component instead of copying JSX.

## Server And Client Boundaries

Next.js App Router makes client boundaries explicit, and the code should stay disciplined.

Conventions:

- default to server components when interactivity is not needed
- add `use client` only where state, effects, browser APIs, animation runtime hooks, or analytics require it
- keep client islands focused and small where possible
- do not turn whole routes client-side just to support one interactive form or animated section

## Metadata And SEO

Marketing routes should treat metadata as part of the feature.

Conventions:

- keep route metadata explicit and reviewable
- use stable page titles and descriptions per route
- centralize site-wide defaults in the root layout or shared helpers
- avoid burying critical SEO behavior inside deeply nested components