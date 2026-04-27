# Website Design System And Component Conventions

## Styling Model

The current hybrid of Tailwind, CSS Modules, and global CSS tokens is a good default.

Use each tool intentionally:

- Tailwind for layout, spacing, responsive utilities, and common utility composition
- CSS Modules for component-scoped styling that would become noisy or repetitive in utilities
- global CSS for theme tokens, shared utility classes, typography primitives, and cross-site conventions

Do not force every component into one styling mechanism.

## Tokens And Theme

Design tokens should remain centralized through CSS variables.

Conventions:

- define palette, surfaces, text colors, outlines, and brand accents in one shared global location
- add new tokens before introducing one-off hardcoded colors in components
- keep token names semantic enough that a palette refresh does not require renaming every usage

Typography should stay intentional:

- headings use the chosen brand heading font
- body and supporting content use the body font consistently
- type scale should be reused through shared classes or component styles, not rebuilt ad hoc on each page

## Shared Components

Components in `core` should be stable building blocks.

Conventions:

- organize reusable components by concern, such as generic, landing, sign-up, and UI primitives
- prefer prop-driven configuration over copy-pasted variants
- keep component APIs small and semantic
- use shared wrappers such as content sections or layout shells to enforce spacing and width consistency

If a component only works in one place because it depends on route-specific context, it probably belongs in `app/` until it proves reusable.

## Copy-Heavy Sections

Marketing sections often carry more copy than logic.

Conventions:

- keep content structure easy to edit
- avoid deeply nested markup that makes copy changes risky
- prefer descriptive prop names over generic `data` blobs when the shape is stable
- extract repeated section patterns into configurable shared components

## Responsiveness

Every section should be designed for narrow, medium, and wide layouts.

Conventions:

- test hero, pricing, comparison, and CTA sections at mobile widths first
- avoid hardcoded widths that fight the existing margin system
- keep content width and section padding controlled by shared wrappers where possible
- do not rely on desktop-only compositions for critical conversion paths

## Motion And Media

Animation should support communication, not distract from it.

Conventions:

- keep Framer Motion usage focused on meaningful reveal, emphasis, or state transitions
- centralize repeated animation timing patterns in shared helpers when the same motion language repeats
- avoid animation that blocks content comprehension or hurts scroll performance

## Accessibility

Accessibility should be standard review criteria.

Conventions:

- use semantic headings and landmark structure
- ensure color contrast remains acceptable across brand colors and dark surfaces
- make nav, forms, drawers, and dialogs keyboard-usable
- provide visible focus states
- use alt text intentionally for meaningful imagery