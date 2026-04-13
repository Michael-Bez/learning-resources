# CSS Guide

A complete reference for styling web pages with CSS.

---

## Table of Contents

1. [What is CSS?](#what-is-css)
2. [Adding CSS to HTML](#adding-css-to-html)
3. [Selectors](#selectors)
4. [The Box Model](#the-box-model)
5. [Colors](#colors)
6. [Typography](#typography)
7. [Display and Visibility](#display-and-visibility)
8. [Positioning](#positioning)
9. [Flexbox](#flexbox)
10. [Grid](#grid)
11. [Backgrounds](#backgrounds)
12. [Borders](#borders)
13. [Transitions and Animations](#transitions-and-animations)
14. [Pseudo-classes and Pseudo-elements](#pseudo-classes-and-pseudo-elements)
15. [Variables (Custom Properties)](#variables-custom-properties)
16. [Responsive Design and Media Queries](#responsive-design-and-media-queries)
17. [Quick Reference Cheat Sheet](#quick-reference-cheat-sheet)

---

## What is CSS?

CSS (Cascading Style Sheets) controls the **visual presentation** of HTML. It describes how elements should look — size, colour, spacing, layout, and more.

**The "cascading" part** means that when multiple rules apply to the same element, they are resolved by:
1. **Specificity** — more specific rules win
2. **Order** — later rules override earlier ones
3. **Importance** — `!important` overrides everything (use sparingly)

---

## Adding CSS to HTML

### External Stylesheet (recommended)

```html
<head>
  <link rel="stylesheet" href="styles.css">
</head>
```

Store your CSS in a separate `.css` file and link it in `<head>`. This keeps styles reusable across multiple pages.

### Internal Style Block

```html
<head>
  <style>
    p {
      color: navy;
    }
  </style>
</head>
```

### Inline Style

```html
<p style="color: navy; font-size: 18px;">Text</p>
```

> Inline styles have the highest specificity and are hard to maintain. Use them only for one-off overrides or dynamic values set by JavaScript.

---

## Selectors

Selectors target which HTML elements a rule applies to.

### Basic Selectors

```css
/* All elements of a type */
p { color: black; }

/* By class (reusable, multiple per element) */
.highlight { background: yellow; }

/* By id (unique, one per page) */
#header { font-size: 2rem; }

/* All elements */
* { box-sizing: border-box; }
```

### Combinators

```css
/* Descendant: any <p> inside a <div> */
div p { margin: 0; }

/* Child: direct <li> children of <ul> only */
ul > li { list-style: none; }

/* Adjacent sibling: <p> immediately after <h2> */
h2 + p { font-weight: bold; }

/* General sibling: all <p> after <h2> within the same parent */
h2 ~ p { color: gray; }
```

### Attribute Selectors

```css
/* Has the attribute */
input[required] { border-color: red; }

/* Exact value */
input[type="email"] { border: 1px solid blue; }

/* Starts with */
a[href^="https"] { color: green; }

/* Ends with */
a[href$=".pdf"] { color: orange; }

/* Contains */
a[href*="example"] { text-decoration: underline; }
```

### Grouping

Apply the same rules to multiple selectors by separating them with commas:

```css
h1, h2, h3 {
  font-family: Georgia, serif;
}
```

### Specificity

When multiple rules target the same element, the more specific rule wins.

| Selector type | Specificity weight |
|--------------|-------------------|
| Inline style | Highest |
| `#id` | High |
| `.class`, `[attr]`, `:pseudo-class` | Medium |
| `element` (e.g. `p`, `div`) | Low |
| `*` (universal) | None |

---

## The Box Model

Every HTML element is a rectangular box. The box model describes the layers around its content:

```
┌─────────────────────────────┐
│           margin            │
│  ┌───────────────────────┐  │
│  │        border         │  │
│  │  ┌─────────────────┐  │  │
│  │  │     padding     │  │  │
│  │  │  ┌───────────┐  │  │  │
│  │  │  │  content  │  │  │  │
│  │  │  └───────────┘  │  │  │
│  │  └─────────────────┘  │  │
│  └───────────────────────┘  │
└─────────────────────────────┘
```

```css
div {
  width: 300px;
  height: 200px;
  padding: 20px;        /* space inside the border */
  border: 2px solid black;
  margin: 10px;         /* space outside the border */
}
```

### Box-Sizing

By default, `width` applies to the content only. Use `border-box` so `width` includes padding and border:

```css
* {
  box-sizing: border-box;
}
```

> This is almost always what you want. Add it to every project.

### Shorthand for Margin and Padding

```css
/* All four sides */
margin: 10px;

/* Top/bottom, Left/right */
margin: 10px 20px;

/* Top, Left/right, Bottom */
margin: 10px 20px 15px;

/* Top, Right, Bottom, Left (clockwise) */
margin: 10px 20px 15px 5px;
```

---

## Colors

```css
/* Named color */
color: red;

/* Hex */
color: #ff0000;
color: #f00;        /* shorthand for #ff0000 */

/* RGB */
color: rgb(255, 0, 0);

/* RGBA (with transparency: 0 = invisible, 1 = opaque) */
color: rgba(255, 0, 0, 0.5);

/* HSL (hue, saturation, lightness) */
color: hsl(0, 100%, 50%);

/* HSLA */
color: hsla(0, 100%, 50%, 0.5);
```

The `color` property sets text colour. Use `background-color` for the element's background.

```css
p {
  color: #333333;
  background-color: #f5f5f5;
}
```

---

## Typography

```css
p {
  font-family: 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  font-weight: bold;       /* normal, bold, or 100–900 */
  font-style: italic;      /* normal, italic, oblique */
  line-height: 1.6;        /* unitless multiplier is recommended */
  letter-spacing: 0.05em;
  text-align: left;        /* left, center, right, justify */
  text-decoration: underline; /* none, underline, line-through */
  text-transform: uppercase;  /* uppercase, lowercase, capitalize */
  white-space: nowrap;        /* prevents text wrapping */
}
```

### Font Units

| Unit | Description |
|------|-------------|
| `px` | Pixels — fixed size |
| `em` | Relative to the element's own font size |
| `rem` | Relative to the root (`<html>`) font size |
| `%` | Percentage of the parent's font size |
| `vw` / `vh` | Percentage of the viewport width / height |

> Prefer `rem` for font sizes and `em` for spacing. This makes scaling easier.

### Google Fonts

```html
<!-- In <head> -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;700&display=swap" rel="stylesheet">
```

```css
body {
  font-family: 'Inter', sans-serif;
}
```

---

## Display and Visibility

```css
/* Block: starts on new line, full width */
display: block;

/* Inline: sits within text flow, no width/height */
display: inline;

/* Inline-block: inline flow, but supports width/height */
display: inline-block;

/* Flexbox container */
display: flex;

/* Grid container */
display: grid;

/* Remove from layout entirely */
display: none;

/* Hide but keep space */
visibility: hidden;

/* Make transparent but keep space */
opacity: 0;
```

---

## Positioning

```css
/* Default — positioned in normal document flow */
position: static;

/* Offset relative to its normal position */
position: relative;
top: 10px;
left: 20px;

/* Positioned relative to its nearest positioned ancestor */
position: absolute;
top: 0;
right: 0;

/* Stays fixed in the viewport when scrolling */
position: fixed;
bottom: 20px;
right: 20px;

/* Like relative, but sticks at a threshold when scrolling */
position: sticky;
top: 0;
```

### Z-Index

Controls stacking order of positioned elements. Higher values appear on top:

```css
.modal {
  position: fixed;
  z-index: 1000;
}
```

> `z-index` only works on elements with a `position` other than `static`.

---

## Flexbox

Flexbox is a one-dimensional layout system (row or column).

### Container Properties

```css
.container {
  display: flex;
  flex-direction: row;          /* row | row-reverse | column | column-reverse */
  justify-content: center;      /* aligns items along the main axis */
  align-items: center;          /* aligns items along the cross axis */
  flex-wrap: wrap;              /* allows items to wrap to a new line */
  gap: 16px;                    /* space between items */
}
```

### `justify-content` Values

| Value | Effect |
|-------|--------|
| `flex-start` | Pack to the start |
| `flex-end` | Pack to the end |
| `center` | Center |
| `space-between` | Equal space between items |
| `space-around` | Equal space around items |
| `space-evenly` | Perfectly equal gaps |

### `align-items` Values

| Value | Effect |
|-------|--------|
| `flex-start` | Align to the start of the cross axis |
| `flex-end` | Align to the end |
| `center` | Center |
| `stretch` | Stretch to fill (default) |
| `baseline` | Align to text baseline |

### Item Properties

```css
.item {
  flex: 1;           /* shorthand for flex-grow: 1; flex-shrink: 1; flex-basis: 0 */
  flex-grow: 1;      /* how much the item grows to fill space */
  flex-shrink: 0;    /* prevent shrinking */
  flex-basis: 200px; /* starting size before growing/shrinking */
  align-self: flex-end; /* override align-items for this item */
  order: 2;          /* change visual order without changing HTML */
}
```

---

## Grid

CSS Grid is a two-dimensional layout system (rows AND columns).

### Container Properties

```css
.grid {
  display: grid;

  /* Define columns */
  grid-template-columns: 200px 1fr 1fr;

  /* Define rows */
  grid-template-rows: auto 1fr auto;

  /* Shorthand: fr = fractional unit (fills remaining space) */
  grid-template-columns: repeat(3, 1fr);

  gap: 20px;            /* space between all rows and columns */
  column-gap: 20px;     /* space between columns only */
  row-gap: 10px;        /* space between rows only */
}
```

### Item Placement

```css
.item {
  /* Span from column line 1 to line 3 */
  grid-column: 1 / 3;

  /* Span 2 rows */
  grid-row: span 2;
}
```

### Named Areas

```css
.grid {
  display: grid;
  grid-template-areas:
    "header header"
    "sidebar main"
    "footer footer";
  grid-template-columns: 250px 1fr;
}

header { grid-area: header; }
aside  { grid-area: sidebar; }
main   { grid-area: main; }
footer { grid-area: footer; }
```

---

## Backgrounds

```css
div {
  background-color: #f0f0f0;
  background-image: url('image.jpg');
  background-size: cover;     /* cover | contain | 100px | 50% */
  background-position: center;
  background-repeat: no-repeat;
  background-attachment: fixed; /* scrolls with page or stays fixed */

  /* Shorthand */
  background: #f0f0f0 url('image.jpg') no-repeat center / cover;
}
```

### Gradients

```css
/* Linear gradient */
background: linear-gradient(to right, #ff0000, #0000ff);
background: linear-gradient(135deg, #f06, #4a90e2);

/* Radial gradient */
background: radial-gradient(circle, #ff0000, #0000ff);
```

---

## Borders

```css
div {
  border: 2px solid #333;
  border-width: 2px;
  border-style: solid;    /* solid | dashed | dotted | double | none */
  border-color: #333;

  /* Individual sides */
  border-top: 3px dashed red;
  border-bottom: none;

  /* Rounded corners */
  border-radius: 8px;
  border-radius: 50%;     /* makes a circle if width = height */
}
```

---

## Transitions and Animations

### Transitions

Smoothly animate a property change triggered by a state change (e.g. `:hover`):

```css
button {
  background-color: blue;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: darkblue;
}
```

```css
/* Transition multiple properties */
transition: background-color 0.3s ease, transform 0.2s ease-out;

/* Transition all properties */
transition: all 0.3s ease;
```

### Animations

Define keyframes and apply them for looping or complex animations:

```css
@keyframes slideIn {
  from {
    transform: translateX(-100%);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

.box {
  animation: slideIn 0.5s ease-out forwards;
}
```

| Property | Description |
|----------|-------------|
| `animation-name` | Name of the `@keyframes` rule |
| `animation-duration` | How long one cycle takes |
| `animation-timing-function` | `ease`, `linear`, `ease-in`, `ease-out` |
| `animation-delay` | Wait before starting |
| `animation-iteration-count` | Number of times to repeat (`infinite`) |
| `animation-direction` | `normal`, `reverse`, `alternate` |
| `animation-fill-mode` | `forwards` keeps the final state after finishing |

---

## Pseudo-classes and Pseudo-elements

### Pseudo-classes

Target elements in a specific **state**:

```css
a:hover   { color: red; }         /* mouse is over the element */
a:visited { color: purple; }      /* link has been clicked */
a:active  { color: orange; }      /* element is being clicked */
input:focus { outline: 2px solid blue; } /* element has keyboard focus */
input:disabled { opacity: 0.5; }

li:first-child  { font-weight: bold; }
li:last-child   { border-bottom: none; }
li:nth-child(2) { color: gray; }
li:nth-child(odd) { background: #f9f9f9; }
p:not(.intro)   { color: #555; }
```

### Pseudo-elements

Target a **part** of an element:

```css
p::first-line   { font-weight: bold; }
p::first-letter { font-size: 2em; }

/* Insert content before or after an element */
.required::after {
  content: " *";
  color: red;
}

/* Style selected text */
::selection {
  background: yellow;
  color: black;
}
```

---

## Variables (Custom Properties)

Define reusable values and reference them throughout your stylesheet:

```css
:root {
  --primary-color: #3498db;
  --font-size-base: 16px;
  --spacing-unit: 8px;
}

button {
  background-color: var(--primary-color);
  font-size: var(--font-size-base);
  padding: calc(var(--spacing-unit) * 2);
}

/* Fallback value if variable is undefined */
color: var(--accent-color, black);
```

> Define variables on `:root` to make them globally available. They are also inheritable and can be overridden for specific components.

---

## Responsive Design and Media Queries

Media queries apply styles based on the browser or device conditions.

```css
/* Applies only when screen is 768px or narrower */
@media (max-width: 768px) {
  .sidebar {
    display: none;
  }
}

/* Applies only when screen is 1024px or wider */
@media (min-width: 1024px) {
  .container {
    max-width: 1200px;
  }
}

/* Range */
@media (min-width: 600px) and (max-width: 900px) {
  body {
    font-size: 14px;
  }
}

/* Dark mode preference */
@media (prefers-color-scheme: dark) {
  body {
    background: #111;
    color: #eee;
  }
}
```

### Common Breakpoints

| Breakpoint | Width |
|-----------|-------|
| Mobile | `< 600px` |
| Tablet | `600px – 1024px` |
| Desktop | `> 1024px` |

> Design **mobile-first**: write base styles for mobile, then use `min-width` media queries to add styles for larger screens.

---

## Quick Reference Cheat Sheet

| Property | Common Values |
|----------|--------------|
| `color` | `#hex`, `rgb()`, named |
| `background-color` | `#hex`, `rgb()`, `transparent` |
| `font-size` | `16px`, `1rem`, `1.2em` |
| `font-weight` | `normal`, `bold`, `400`–`900` |
| `font-family` | `'Arial', sans-serif` |
| `text-align` | `left`, `center`, `right` |
| `margin` | `auto`, `10px`, `10px 20px` |
| `padding` | `10px`, `10px 20px` |
| `border` | `1px solid #333` |
| `border-radius` | `8px`, `50%` |
| `width` / `height` | `px`, `%`, `vw`, `auto` |
| `display` | `block`, `inline`, `flex`, `grid`, `none` |
| `position` | `static`, `relative`, `absolute`, `fixed`, `sticky` |
| `top` / `right` / `bottom` / `left` | `px`, `%` |
| `z-index` | Any integer |
| `flex-direction` | `row`, `column` |
| `justify-content` | `center`, `space-between`, `flex-start` |
| `align-items` | `center`, `stretch`, `flex-start` |
| `grid-template-columns` | `1fr 1fr`, `repeat(3, 1fr)` |
| `gap` | `16px` |
| `opacity` | `0` to `1` |
| `transition` | `property duration timing` |
| `cursor` | `pointer`, `default`, `not-allowed` |
| `overflow` | `visible`, `hidden`, `scroll`, `auto` |

---

*Part of the learning-resources collection.*
