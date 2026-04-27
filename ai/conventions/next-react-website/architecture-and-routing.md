# Website Architecture And Routing Conventions

## App Router Structure

The `app/` directory should remain the route layer, not the component library.

Conventions:

- route files should compose shared sections and page templates rather than define large bespoke component trees inline
- route-level `page.tsx` files should stay readable at a glance
- `layout.tsx` should own app-wide wrappers, providers, fonts, metadata defaults, and persistent shells
- route groups or nested layouts should be introduced when they simplify structure, not preemptively

```tsx
// app/page.tsx — thin route file, no layout logic
import { LandingHero } from 'core/src/components/landing/hero/landing-hero';
import { LandingFeatures } from 'core/src/components/landing/features/landing-features';
import { LandingSolutions } from 'core/src/components/landing/solutions/landing-solutions';
import { LandingCta } from 'core/src/components/landing/cta/landing-cta';

export default function HomePage() {
  return (
    <>
      <LandingHero />
      <LandingFeatures />
      <LandingSolutions />
      <LandingCta />
    </>
  );
}
```

```tsx
// app/layout.tsx — providers, fonts, metadata
import { Montserrat, Source_Sans_3 } from 'next/font/google';
import Providers from './providers';
import { LayoutDefault } from 'core/src/layouts/layout-default';

export const metadata = {
  title: 'PeakReps — Build Your Coaching App',
  description: 'The platform for professional fitness coaches.',
};

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body className={`${montserrat.variable} ${sourceSans.variable}`}>
        <Providers>
          <LayoutDefault>{children}</LayoutDefault>
        </Providers>
      </body>
    </html>
  );
}
```

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

```tsx
// ✅ server component — no "use client" needed
// app/pricing/page.tsx
import { PricingSection } from 'core/src/components/landing/pricing/pricing-section';

export default function PricingPage() {
  return <PricingSection />;
}
```

```tsx
// ✅ client island — only the form needs interactivity
// core/src/components/sign-up/sign-up-form.tsx
'use client';

import { useState, type FormEvent } from 'react';

export function SignUpForm() {
  const [email, setEmail] = useState('');
  const [status, setStatus] = useState<'idle' | 'loading' | 'success' | 'error'>('idle');

  async function handleSubmit(e: FormEvent) {
    e.preventDefault();
    setStatus('loading');
    try {
      await fetch('/api/sign-up', { method: 'POST', body: JSON.stringify({ email }) });
      setStatus('success');
    } catch {
      setStatus('error');
    }
  }

  return <form onSubmit={handleSubmit}>{/* fields */}</form>;
}
```

```tsx
// ❌ wrong — entire route becomes client-side for one interactive element
'use client'; // at the top of app/page.tsx
```

## Metadata And SEO

Marketing routes should treat metadata as part of the feature.

Conventions:

- keep route metadata explicit and reviewable
- use stable page titles and descriptions per route
- centralize site-wide defaults in the root layout or shared helpers
- avoid burying critical SEO behavior inside deeply nested components

```tsx
// app/about/page.tsx
import type { Metadata } from 'next';

export const metadata: Metadata = {
  title: 'About PeakReps',
  description: 'Learn how PeakReps helps fitness coaches build scalable businesses.',
  openGraph: {
    title: 'About PeakReps',
    description: 'Learn how PeakReps helps fitness coaches build scalable businesses.',
  },
};

export default function AboutPage() {
  return <AboutSection />;
}
```