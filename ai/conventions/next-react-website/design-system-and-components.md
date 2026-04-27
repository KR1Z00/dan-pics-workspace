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

```css
/* core/src/styles/globals.css — single source of truth for tokens */
:root {
  --primary:           #ABEE1E; /* lime green — CTAs, actions */
  --on-primary:        #0C1002;
  --secondary:         #0B81F0; /* electric blue */
  --surface:           #0D0F13;
  --surface-container: #25272B;
  --on-surface:        #F8FAFE;
  --outline:           #868D9B;
  --error:             #FF5555;
}
```

```tsx
// ✅ use tokens through Tailwind config or CSS vars — not hardcoded hex
<button className="bg-primary text-on-primary">Get started</button>

// ❌ wrong — one-off hardcoded color that bypasses the design system
<button style={{ backgroundColor: '#ABEE1E' }}>Get started</button>
```

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

```tsx
// core/src/components/generic/layout/generic-content-section.tsx
import { cn } from 'core/src/lib/utils';
import { BackgroundStyle } from 'core/src/model/background-style';

interface GenericContentSectionProps {
  children: React.ReactNode;
  background?: BackgroundStyle;
  className?: string;
}

export function GenericContentSection({
  children,
  background = BackgroundStyle.Surface,
  className,
}: GenericContentSectionProps) {
  return (
    <section
      className={cn(
        'margin-container py-16',
        background === BackgroundStyle.SurfaceContainer && 'bg-surface-container',
        className,
      )}
    >
      {children}
    </section>
  );
}
```

```tsx
// cn() helper usage for conditional classes
import { cn } from 'core/src/lib/utils';

// merges Tailwind classes safely, last one wins on conflict
const classes = cn(
  'px-4 py-2 rounded',
  isPrimary && 'bg-primary text-on-primary',
  isDisabled && 'opacity-50 cursor-not-allowed',
);
```

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