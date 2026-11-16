# 04. CSS foundations and layout

Previous: [03. HTML and accessibility](03-html-semantics-accessibility-and-seo.md) | Next: [05. Responsive styling](05-responsive-css-and-ui-styling.md)

## Cascade and sizing

When a rule loses, inspect the cascade before increasing specificity. Origin and importance, cascade layers, specificity, scoping proximity where relevant, and source order all matter. An ID selector is not an unconditional winner over every declaration. Inheritance applies to selected properties; it does not copy an entire parent's style. Use [MDN's cascade explanation](https://developer.mozilla.org/en-US/docs/Web/CSS/Guides/Cascade/Introduction) when working through competing declarations.

The content box, padding, border, and margin determine sizing. `box-sizing: border-box` includes padding and border in declared width/height. Margins remain outside. Vertical margins in normal block layout can collapse; flex/grid gaps do not behave the same way.

```css
*, *::before, *::after { box-sizing: border-box; }
body { margin: 0; font-family: system-ui, sans-serif; }
img { display: block; max-inline-size: 100%; }
.page { inline-size: min(100% - 2rem, 72rem); margin-inline: auto; }
```

Understand `display: block`, inline, inline-block, flex, grid, and none. Normal flow is the starting point; absolute positioning should solve an actual positioning problem rather than replace layout.

## Flexbox versus Grid

Flexbox distributes items along a main axis and an optional wrapping cross axis. Use it for a toolbar, a row of controls, or alignment within a card. Grid defines tracks across two dimensions and suits page sections or aligned card collections. Both can solve overlapping classes of problems; explain the constraints that made you choose one.

```css
.toolbar { display: flex; align-items: center; gap: .75rem; flex-wrap: wrap; }
.toolbar__search { flex: 1 1 16rem; min-inline-size: 0; }
.results { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 1rem; }
.card__title { overflow-wrap: anywhere; }
```

`min-width: auto` on flex children can prevent shrinking; `min-width: 0` often fixes overflowing content. A grid track defined as `1fr` can still be constrained by minimum content size; `minmax(0, 1fr)` is useful when shrinkage is intended. Avoid hiding overflow until you understand what is overflowing.

Know `flex-basis`, grow, shrink, alignment, auto margins, grid placement, implicit tracks, and `gap`. Explain why visual reordering can disagree with DOM/keyboard order.

## Positioning and stacking

Relative positioning retains normal-flow space. Absolute positioning leaves normal flow and uses a containing block, often the nearest positioned ancestor. Fixed positioning usually follows the viewport, but transforms and other properties can change containing-block behavior. Sticky positioning depends on a scroll container and a non-auto inset.

A large `z-index` cannot escape an ancestor stacking context. Transforms, opacity, positioning, and isolation can create contexts. Portals may help place overlays outside clipping/stacking ancestors, but still need focus and event design.

`visibility: hidden` retains layout space; `display: none` removes the element from layout. Opacity alone does not remove an element from hit testing or focus behavior. Choose based on interaction as well as visuals.

## Practice and debugging

Create a page with a header, sticky filters, a flexible search input, and three cards. Seed three bugs: an unbroken long title, a modal below a header, and a sticky filter that does not stick. Fix each using computed styles and layout inspection.

## Questions and answer cues

**Why does `height: 100%` not fill the viewport?** Percentage heights need an appropriate containing-block height; inspect the ancestor sizing chain. Use a viewport-based minimum height when that matches the requirement.

**Why does `z-index: 999999` fail?** Inspect stacking contexts and clipping rather than increasing the number.

**How do you center a child?** Explain whether you mean inline text, an item on one axis, both axes, or an overlay. Choose layout based on that requirement.

Exit gate: implement a layout from a screenshot and explain every overflow/positioning fix without using arbitrary offsets.
