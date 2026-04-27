# Website Forms, Analytics, And Delivery Conventions

## Forms

Forms on the marketing website should be simple, trustworthy, and conversion-minded.

Conventions:

- keep form state local to the smallest sensible client component
- validate obvious issues before submission
- show clear loading, success, and failure feedback
- post to a stable backend or API route boundary rather than scattering third-party calls directly in components
- keep personally identifiable information handling minimal and intentional

If a form flow becomes substantial, treat it like a feature with dedicated component boundaries rather than an inline page widget.

## API Boundaries

The website should remain a thin marketing surface.

Conventions:

- use internal API routes or shared backend endpoints as the integration boundary
- keep external service credentials off the client
- isolate request shaping and response handling in one place per form or integration
- fail gracefully when analytics or lead-capture services are unavailable

## Analytics

Analytics should be centralized and explicit.

Conventions:

- initialize PostHog or other analytics in a dedicated provider boundary
- keep environment-variable wiring explicit and documented
- track meaningful user actions, not every possible click
- name events consistently so marketing and product analysis stay usable over time
- avoid sprinkling raw analytics initialization logic across route files

## Performance

Marketing sites need speed as part of the product impression.

Conventions:

- prefer server rendering where interactivity is not required
- keep client bundles small by limiting `use client` scope
- optimize images and media intentionally
- avoid unnecessary dependencies in the route layer when `core` already has the primitive needed

## Deployment Workflow

Deployment must respect the private `core` submodule setup.

Conventions:

- keep `prepare-deploy` and `vercel-build` scripts accurate and documented
- treat `GITHUB_ACCESS_TOKEN` as a required deployment dependency where submodule fetch is needed
- document all required public environment variables for analytics and related integrations
- do not hide critical deploy setup in undocumented CI-only steps

## Review Expectations

Website work should not merge if it introduces:

- duplicated shared UI that belongs in `core`
- hardcoded colors or typography outside the token system without a clear reason
- unscoped client components where server rendering would work
- analytics calls scattered through unrelated components
- conversion-critical mobile layout regressions