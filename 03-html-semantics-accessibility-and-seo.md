# 03. HTML semantics, accessibility, and SEO

Previous: [02. Browser and HTTP](02-web-browser-and-http.md) | Next: [04. CSS foundations](04-css-foundations-and-layout.md)

## Semantic structure

Use landmarks such as `header`, `nav`, `main`, and `footer`; use headings to describe the content hierarchy. Choose an anchor for navigation and a button for an action. Use lists for collections and tables for genuinely tabular relationships. Browser-native elements provide behavior that a clickable `div` does not.

A button inside a form submits by default. Set `type='button'` for other actions. A disabled control cannot normally receive focus or participate in form submission; `aria-disabled` communicates a state but does not stop activation by itself.

```html
<main id="main">
  <h1>Find a house</h1>
  <form action="/houses" method="get">
    <label for="city">City</label>
    <input id="city" name="city" autocomplete="address-level2"
           aria-describedby="city-help">
    <p id="city-help">Enter the city you want to live in.</p>
    <button type="submit">Search houses</button>
  </form>
</main>
```

## Accessibility as interaction correctness

Prepare these concepts: accessible name, role, state, focus order, keyboard operation, screen-reader announcements, visible focus, contrast, zoom, and reduced motion. A name may come from visible text, a label, or ARIA. Avoid replacing a useful visible label with a contradictory `aria-label`.

Prefer native semantics. ARIA changes accessibility information, not keyboard behavior. For example, `role='button'` does not implement Enter/Space activation. Do not hide a focused control with `aria-hidden`.

For a field error, associate the error text using `aria-describedby`, set `aria-invalid` when invalid, keep entered values, and move focus thoughtfully after a failed submission. Use a polite status region for nonurgent async results, without announcing every keystroke. Toasts should not be the only way to discover a form failure.

For modal dialogs, plan an accessible title, initial focus, keyboard containment, Escape, inert background interaction, and focus restoration to a sensible trigger. The [W3C modal dialog pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/) describes these behaviors. Native `dialog.showModal()` supplies useful platform behavior; verify labeling, dismissal, and focus in the browsers you support.

Images need an `alt` appropriate to their purpose. Decorative images use empty alt text. Do not repeat a nearby linked title in an unnecessary second accessible name. Reserve image dimensions to avoid page jumps.

## SEO fundamentals

Use meaningful titles, descriptions, canonical URLs, crawlable links, appropriate indexing directives, a sitemap when useful, and structured data only when it matches visible content. A robots restriction is not access control. A private dashboard still needs authorization even if marked `noindex`.

Server-rendered content can improve initial availability to crawlers and link previews, but SSR alone does not guarantee ranking. Duplicate filter URLs, empty content, missing titles, poor performance, and incorrect status codes remain problems. Decide which marketplace filters should be indexable instead of allowing unbounded combinations.

Know HTML forms, constraint validation, input types, `name`, `fieldset`/`legend`, and why placeholder text is not a label. Know `picture`, `srcset`, and `sizes` for responsive images. Know that a heading styled as large text still needs the appropriate semantic level.

## Practice

Build a housing search page with a skip link, labeled filters, results heading, responsive image, empty state, and error summary. Complete the entire flow with a keyboard. Test at 200% zoom and a narrow viewport. Inspect the accessibility tree and run an automated check, then manually test the behavior it cannot prove.

## Questions and answer cues

**Can an automated accessibility score prove accessibility?** No. It catches a subset of issues; keyboard flows, announcements, reading order, and meaningful labels need human assessment.

**Should a menu be built for every navigation list?** No. Ordinary site navigation can be links in a list. Application menu semantics bring extra keyboard expectations.

**What is progressive enhancement?** Establish useful baseline content and native behavior, then add richer interaction without making unnecessary capabilities prerequisites.

Exit gate: demonstrate search and validation without a mouse, and explain why each interactive element has its chosen HTML semantics.
