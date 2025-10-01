# CSS

## Selectors (selectors specificity calculation)

CSS specificity determines which rule wins when multiple rules target the same element.

- How specificity is calculated (weights):
    - Inline styles: 1000 (e.g., style="...")
    - ID selectors: 100 (e.g., #hero)
    - Class, attribute, and pseudo-class selectors: 10 (e.g., .btn, [disabled], :hover, :nth-child)
    - Type selectors and pseudo-elements: 1 (e.g., div, h1, ::before)
    - Universal selector (*) and combinators (>, +, ~, space) add 0

- Special cases:
    - :not(X) takes the specificity of X; :not itself adds 0.
    - :is(...) takes the specificity of its most specific argument.
    - :where(...) always has 0 specificity regardless of its arguments (great for defaults).
    - !important overrides normal specificity. If multiple have !important, the usual specificity rules (and source order) apply among them.
    - Source order tiebreaker: when specificity is equal, the rule that appears later in the CSS wins.

- Examples:
    - div p → 1 (div) + 1 (p) = 2
    - .card .title → 10 + 10 = 20
    - #app .title → 100 + 10 = 110
    - a:hover → 1 + 10 = 11
    - :where(.btn .primary) → 0 (regardless of inside selectors)
    - :is(h1, .title) → max(1, 10) = 10

Tips:

- Prefer low-specificity, composable selectors (classes) to keep CSS maintainable.
- Avoid over-qualifying (e.g., div.card) unless necessary; it increases specificity with little benefit.
- Reserve IDs and !important for edge cases.

What are selectors?

- Selectors are patterns used to target elements in the DOM to apply styles. A rule is `selector { property: value; }`.
- Browsers match selectors from right to left (key selector first), then walk up the tree to verify the rest; write selectors that are clear
  and not overly long.

How to use selectors effectively:

- Prefer class-based styling for reuse and low specificity: `.btn`, `.card--primary`.
- Scope styles by chaining classes instead of IDs or long descendants: `.card .title` rather than `#app main section .card .title`.
- Use state classes instead of toggling inline styles: `.is-open`, `.is-active`.
- BEM naming helps avoid collisions: `.block`, `.block__element`, `.block--modifier`.

Variations and types (with examples):

- Universal: `*` (matches any element) → specificity 0. Use sparingly.
- Type (tag): `div`, `h1`, `p` → specificity 1.
- Class: `.btn`, `.error` → specificity 10.
- ID: `#header` → specificity 100 (avoid in component CSS when possible).
- Attribute: `[disabled]`, `[type="email"]`, `[lang|=en]`, `[class^="icon-"]`, `[data-role~="primary"]`, `[href$=".pdf"]` → specificity 10.
- Pseudo-classes: `:hover`, `:focus`, `:active`, `:visited`, `:disabled`, `:checked`, `:empty`, `:required`, `:nth-child(2n+1)`,
  `:nth-of-type(3)`, `:first-child`, `:last-child`, `:not(.hidden)`, `:is(article, .post)`, `:where(nav a)` (0 specificity), `:has(img)` (
  parent/relational) → specificity 10 (except :where which is 0; :is takes the most specific argument).
- Pseudo-elements: `::before`, `::after`, `::placeholder`, `::marker`, `::selection` → specificity 1.
- Combinators:
    - Descendant: `.card .title` (any depth)
    - Child: `.list > li` (direct children)
    - Adjacent sibling: `h2 + p` (next sibling)
    - General sibling: `h2 ~ p` (subsequent siblings)
    - Column: `th || td` (table column axis; limited support)

Examples for sibling combinators:

```css
/* Adjacent sibling: only the first <p> immediately following each <h2> */
h2 + p {
  color: crimson;
}

/* General sibling: all subsequent <p> siblings after each <h2> */
h2 ~ p {
  font-weight: bold;
}
```

```html

<article>
  <h2>Section</h2>
  <p>A. Immediately after h2 (matches both h2 + p and h2 ~ p)</p>
  <div>Not a paragraph</div>
  <p>B. Later sibling (matches only h2 ~ p)</p>
  <h3>Subheading</h3>
  <p>C. After h3 (still sibling of the h2; matches only h2 ~ p)</p>
  <div>
    <p>D. Nested inside a div (NOT a sibling of the h2; matches neither)</p>
  </div>
</article>
```

Notes:

- `h2 + p` selects only the first `<p>` immediately following each `<h2>`.
- `h2 ~ p` selects all `<p>` elements that are later siblings of the `<h2>` within the same parent (not descendants of other elements).

- Grouping (selector list): `h1, h2, h3 { ... }` applies the same rules to multiple selectors; specificity is evaluated per list item.

Practical patterns:

- Use `:where()` to provide default, easily-overridden styles: `:where(.btn){ font: inherit; }`.
- Use `:is()` to simplify long lists: `.nav :is(a, button){ ... }`.
- Use `:not()` to exclude: `.menu-item:not(.active){ opacity:.6; }`.
- Use `:has()` (where supported) for parent-state styling without extra classes: `.card:has(img.cover){ padding:0; }`.
- Avoid chaining too many classes/tags purely for specificity; prefer architectural conventions.

Examples for practical patterns:

```css
/* 1) :where() — low-specificity defaults that are easy to override */
:where(.btn) {
  font: inherit;
  padding: .5rem 1rem;
  border: 1px solid #ccc;
  background: #fff;
}

.btn.primary { /* overrides :where(.btn) without fighting specificity */
  background: #0d6efd;
  color: #fff;
}
```

```html

<button class="btn">Default</button>
<button class="btn primary">Primary</button>
```

```css
/* 2) :is() — simplify long selector lists */
/* Before (repetitive): */
.nav a, .nav button, .nav [role="link"] {
  text-decoration: none;
}

/* After (equivalent): */
.nav :is(a, button, [role="link"]) {
  text-decoration: none;
}
```

```html

<nav class="nav">
  <a href="#">Home</a>
  <button>Menu</button>
  <span role="link" tabindex="0">Help</span>
</nav>
```

```css
/* 3) :not() — exclude specific cases */
.menu-item:not(.active) {
  opacity: .6;
}

.menu-item.active {
  opacity: 1;
}
```

```html

<li class="menu-item">Item</li>
<li class="menu-item active">Active Item</li>
```

```css
/* 4) :has() — parent-state styling (where supported) */
.form-group:has(input:invalid) {
  outline: 2px solid #dc3545;
}
```

```html
<label class="form-group">
  Email
  <input type="email" required value="not-an-email"/>
</label>
```

```css
/* 5) Avoid over-chaining for specificity — prefer simple, reusable classes */
/* Over-chained: high specificity and brittle */
.main .content .card .card__title {
  font-weight: 600;
}

/* Better: attach a class and style it directly */
.title {
  font-weight: 600;
}
```

```html
<h3 class="title">Product</h3>
```

Debugging specificity conflicts:

- In DevTools, inspect the element and check the “Styles” pane; compare selector lists and their specificities and source order.
- Reduce specificity when possible instead of escalating with more IDs or !important.

## Box Model (box-sizing)

Every element is a rectangular box consisting of:

- content → padding → border → margin (outer spacing)

Width/height calculations depend on box-sizing:

- content-box (default):
    - set width/height applies to content only.
    - total rendered size = content + padding + border (+ margin outside).
- border-box:
    - set width/height includes content + padding + border.
    - easier to reason about fixed-size components.

Example:

- With width: 200px; padding: 20px; border: 2px solid; margin: 10px
    - content-box total width = 200 + 2*20 + 2*2 = 244px (excluding margins)
    - border-box content width shrinks so total remains 200px (excluding margins)

Common best practice to standardize:

- Apply once at the root and inherit:

```css
html {
  box-sizing: border-box;
}

*, *::before, *::after {
  box-sizing: inherit;
}
```

Notes:

- Padding contributes to background painting area; margins are always transparent.
- Overflow and scrollbars interact with the box; overflow: auto/hidden can affect layout.

## Layout and position

The layout defines how boxes are placed. Key concepts:

- Normal flow & display:
    - display: block → starts on new line, expands to fill inline axis of container.
    - display: inline → flows with text; width/height don’t apply; padding/border affect line box.
    - display: inline-block → flows inline but accepts width/height.
    - Modern layout modes: flex and grid (see level2 for advanced usage):
        - Flexbox (1D: lay out items in a row or a column) — best for aligning and distributing space along a single axis; great for
          components, navbars, and centering.
            - Container: `display: flex | inline-flex`; `flex-direction: row | row-reverse | column | column-reverse`;
              `flex-wrap: nowrap | wrap | wrap-reverse`; `gap`; `justify-content`; `align-items`; `align-content` (multi-line wrap only).
            - Items: `flex: <grow> <shrink> <basis>` (e.g., `flex: 1 1 200px`); `order`; `align-self`.
            - Axes: main axis follows `flex-direction`; cross axis is perpendicular. `justify-*` aligns along main axis; `align-*` along
              cross.
            - Common patterns: center content (`display:flex; justify-content:center; align-items:center;`), equal-height columns,
              responsive nav bars, reordering (visual only; DOM order unchanged).
        - Grid (2D: rows and columns) — best for defining two-dimensional layouts with explicit rows and columns and precise placement of
          items across both axes.
            - Container: `display: grid | inline-grid`; `grid-template-columns/rows`; `gap`; `grid-auto-flow`; `justify-items`/
              `align-items`; `justify-content`/`align-content`.
            - Tracks & sizing: `repeat()`, `minmax()`, `auto-fit`/`auto-fill`, `fr` unit for proportional space; explicit vs auto tracks.
            - Items: `grid-column`/`grid-row` (e.g., `grid-column: 1 / -1`), `grid-area`; `justify-self`/`align-self`.
            - Common patterns: full-page layouts with header/sidebar/main/footer, card galleries, dense auto-placement, masonry-like effects
              via auto rows.
            - Tips: Use Grid for two-dimensional placement; combine with Flexbox inside grid cells for 1D alignment.
    - Margin collapsing occurs between vertical margins of block boxes in normal flow.

- Positioning:
    - position: static (default): element stays in normal flow; top/right/bottom/left ignored.
    - position: relative: stays in flow but can be visually offset; establishes a containing block for absolutely positioned descendants.
    - position: absolute: removed from normal flow; positioned relative to the nearest ancestor that is positioned (non-static), else the
      initial containing block.
    - position: fixed: anchored to the viewport; unaffected by page scroll (except within transformed/scrolling containers in some
      contexts).
    - position: sticky: toggles between relative and fixed based on scroll, within its scroll container and thresholds (
      top/right/bottom/left).

- Stacking and z-index:
    - z-index works on positioned elements (and flex/grid children in some cases).
    - New stacking contexts are created by properties like position with z-index, opacity < 1, transform, filter, etc.
    - Higher stacking contexts sit above lower ones; z-index only compares within the same stacking context.

# HTML

## Document structure

A minimal, well-formed HTML document:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1"/>
  <title>Page title</title>
  <link rel="stylesheet" href="styles.css"/>
  <script defer src="app.js"></script>
</head>
<body>
<header>...</header>
<nav>...</nav>
<main>
  <section>...</section>
  <article>...</article>
  <aside>...</aside>
</main>
<footer>...</footer>
</body>
</html>
```

Key points:

- <!doctype html> enables standards mode.
- <html lang="..."> sets document language (important for accessibility and SEO).
- <head> contains metadata (title, meta, links, preloads, scripts not meant to render content).
- <body> contains the visible content.
- Use semantic elements (header, nav, main, section, article, aside, footer) for structure and accessibility.
- Keep a single <h1> per page sectioning root (often the page’s main heading), and nest headings in order.

## DOM

The Document Object Model (DOM) is the in-memory, tree-like representation of the HTML document that scripts can read and modify.
The HTML DOM is a standard object model and programming interface for HTML. It defines:

- The HTML elements as objects
- The properties of all HTML elements
- The methods to access all HTML elements
- The events for all HTML elements
- In other words: The HTML DOM is a standard for how to get, change, add, or delete HTML elements.

- Node types:
    - document, element, text, comment, documentFragment.
- Selecting:
    - document.getElementById('id') (single, fast)
    - document.querySelector('.class [attr]') (first match, CSS selectors)
    - document.querySelectorAll('selector') (static NodeList)
    - getElementsByClassName/TagName (live HTMLCollection)
- Reading/updating:
    - el.textContent, el.innerHTML (be careful of XSS when injecting HTML), el.innerText (layout-aware)
    - el.getAttribute/setAttribute, el.dataset
    - el.classList.add/remove/toggle/contains
    - el.style.property or better: toggle classes
- Creating/inserting/removing:
    - document.createElement('div'), node.append/appendChild, prepend, before, after, replaceWith, remove
    - Use DocumentFragment for batching DOM updates
- Events:
    - el.addEventListener('click', handler)
    - Event bubbling/capturing; use event delegation for lists