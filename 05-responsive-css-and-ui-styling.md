# 05. Responsive CSS and UI styling

Previous: [04. CSS foundations](04-css-foundations-and-layout.md) | Next: [06. JavaScript foundations](06-javascript-foundations.md)

## Design for content and constraints

Start with a usable narrow layout, then add columns when the content has room. Choose breakpoints when the design breaks rather than naming a device. Test long translated strings, large text, touch input, landscape, and zoom. A desktop screenshot is one observation, not the complete specification.

```css
:root {
  --space-2: .5rem;
  --space-4: 1rem;
  --surface: #fff;
  --text: #182230;
}
.listing-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 16rem), 1fr));
  gap: var(--space-4);
}
.listing-title { font-size: clamp(1.1rem, 1rem + .5vw, 1.5rem); }
.listing-card { container-type: inline-size; }
@container (min-width: 28rem) {
  .listing-card__body { display: flex; gap: var(--space-4); }
}
@media (prefers-reduced-motion: reduce) {
  .listing-card { transition: none; }
}
```

Media queries respond to environment conditions such as viewport width or user preferences. Container queries let a component respond to its available container space. Logical properties such as `margin-inline` and `padding-block` make direction-aware layouts easier. Know when `rem`, `em`, percentages, `fr`, `ch`, and viewport units express the intended constraint.

## Styling choices

| Approach | Useful property | Tradeoff to discuss |
| --- | --- | --- |
| Plain CSS with conventions | Minimal tooling, native cascade | Naming and ownership need discipline |
| CSS Modules | Locally scoped class names | Shared tokens and global styles still need design |
| Utility CSS such as Tailwind | Fast composition of constrained values | Repeated patterns may need components; markup grows |
| Runtime CSS-in-JS | Dynamic composition in some systems | Evaluate runtime, SSR, streaming, and library compatibility |
| Build-time styling | Generated static styles | Tooling constraints and dynamic cases need planning |

Tailwind does not replace knowledge of Flexbox, Grid, accessibility, or the cascade. For unfamiliar projects, inspect the installed major version and configuration before copying setup instructions. Do not dynamically construct arbitrary utility names unless the build pipeline can discover them; map variants to complete class strings or use appropriate CSS variables.

Define tokens for spacing, colors, type, radius, and elevation. A token should communicate purpose, such as text-muted or surface-error, rather than lock every component to a raw color. Themes should retain contrast in every state, including disabled, hover, focus, and selected states.

## Animation, assets, and internationalization

Prefer animating transform or opacity where appropriate, but profile actual behavior rather than assuming every animation is cheap. Avoid layout shifts from unloaded images and fonts. Do not lazy-load a critical above-the-fold image just because lazy loading sounds faster.

Use `Intl.NumberFormat` for currency display and explicit locale/time-zone choices for dates. Store canonical values separately from presentation. A localized currency string is unsuitable for arithmetic. Test right-to-left layouts, long names, plural forms, and a date near a time-zone boundary.

For dark mode, test user preference and explicit override behavior, including initial rendering. A theme chosen only after mount can flash or mismatch hydration. Keep the server and client theme strategy aligned when SSR is involved.

## Practice

Implement a listing card in narrow and wide containers, a responsive results page, and a light/dark theme. Include loading, empty, error, selected, and disabled states. Test 320px width, 200% zoom, long content, and reduced motion. Do not fix long content by silently discarding meaningful information.

## Questions and answer cues

**When would you extract a repeated utility combination?** When repetition reflects a stable semantic component or repeated design rule, not simply because two elements share a few classes.

**Why is a media query insufficient for a reusable card?** The same viewport can contain differently sized sidebars, grids, and dialogs; a container query can track the actual space.

**How do you prevent a mobile layout from overflowing?** Inspect intrinsic sizing, fixed widths, long text, media, and flex/grid minimums before adding `overflow-x: hidden`.

Exit gate: adapt one component to three containers without duplicating its markup and explain your breakpoint, typography, and asset decisions.
