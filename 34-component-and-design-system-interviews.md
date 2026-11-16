# 34. Component and design-system interviews

Previous: [33. System design](33-front-end-system-design.md) | Next: [35. Delivery](35-git-ci-deployment-and-observability.md)

## Start with the interaction contract

A component interview asks how a reusable unit behaves and composes. Clarify controlled/uncontrolled behavior, data shape, keyboard rules, loading/error/empty states, theming, localization, and expected extension points. Name invariants before choosing hooks.

For an autocomplete, distinguish typed query, highlighted option, selected value, open state, and request state. For a modal, define focus, dismissal, background behavior, and restoration. For a table, define row identity, sorting, selection across pages, pagination ownership, and whether editable cells exist.

## Design a focused API

```ts
type Option = { id: string; label: string; disabled?: boolean };
type SingleSelectProps = {
  options: readonly Option[];
  value: string | null;
  onChange: (id: string | null) => void;
  label: string;
  disabled?: boolean;
  error?: string;
};
```

This API represents one controlled single-select contract. A multi-select mode deserves distinct value/callback types, not a boolean that leaves every prop ambiguous. Decide whether custom option rendering is necessary and how accessible names remain correct when it is used.

Use composition and slots for stable extension needs. Keep visual primitives separate from domain-specific authorization or API calls when they are truly reusable. A generic button should not secretly depend on the current organization; a domain action can compose the button with that logic.

## Accessibility and state machines

Write transitions for open/close, pointer/keyboard selection, disabled states, async arrival, invalidated selection, and errors. A native select may satisfy the need better than a custom combobox. Custom widgets require their complete interaction pattern, not just ARIA attributes that look plausible.

For a dialog, use the [W3C modal pattern](https://www.w3.org/WAI/ARIA/apg/patterns/dialog-modal/) as an interaction reference. For other widgets, consult the corresponding official APG pattern before implementing them. Verify keyboard and assistive-technology behavior rather than treating a role as the implementation.

Separate focus from selection. When a list changes, ensure the highlighted ID remains valid, and decide what to do if the selected item disappears. Preserve stable identity so an update does not move focus to a different logical item.

## Design-system concerns

Use semantic tokens for color, type, spacing, and motion. Expose documented variants rather than unlimited styling switches. Test default, hover, focus-visible, active, selected, disabled, loading, and invalid states in relevant themes and directions.

Documentation should include real usage, keyboard behavior, do/don't examples, prop contracts, and migration notes for breaking changes. Component previews and visual regression help review appearances; interaction tests verify behavior. Versioning a shared component can affect many consumers, so treat API stability and deprecation deliberately.

For a large table, compare native table semantics with an interactive grid. A grid adds keyboard/interaction responsibilities. For virtualization, explain accessibility and offscreen-row concerns. For async selects, define request cancellation, query identity, caching, and loading announcements.

## Practice prompts and acceptance criteria

- Modal: open from two triggers, close with Escape, restore the correct trigger, retain accessible title, and prevent background interaction.
- Tabs: valid selected panel, arrow-key/focus policy matching the chosen pattern, dynamic tab removal, and stable IDs.
- Data table: sorting, row selection, empty/error states, long text, and selection policy across pages.
- Toast system: bounded queue, deduplication, meaningful durations, appropriate announcements, and a persistent alternative for important failures.
- Multi-step application: valid transitions, retained drafts, error recovery, and an explicit reset policy.

## Questions and answer cues

**When should a component be generic?** When multiple real use cases share a stable interaction/API contract. **How do you avoid prop explosion?** Use explicit modes, composition, and domain-specific wrappers. **How do you test a reusable component?** Test the public interaction contract, critical variants, and integration examples, not private hook layout.

Exit gate: design one component API, list its state transitions, implement the core behavior, and show keyboard plus error-state acceptance tests.
