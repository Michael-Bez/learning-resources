# HTML Guide

A complete reference for writing and structuring HTML.

---

## Table of Contents

1. [What is HTML?](#what-is-html)
2. [Document Structure](#document-structure)
3. [Elements and Tags](#elements-and-tags)
4. [Headings](#headings)
5. [Paragraphs and Text](#paragraphs-and-text)
6. [Links](#links)
7. [Images](#images)
8. [Lists](#lists)
9. [Tables](#tables)
10. [Forms](#forms)
11. [Semantic Elements](#semantic-elements)
12. [Div and Span](#div-and-span)
13. [Attributes](#attributes)
14. [Comments](#comments)
15. [Quick Reference Cheat Sheet](#quick-reference-cheat-sheet)

---

## What is HTML?

HTML (HyperText Markup Language) is the standard language for creating web pages. It describes the **structure** and **content** of a page using elements represented by tags.

**HTML is not:**
- A programming language (it has no logic or variables)
- Responsible for styling (that's CSS)
- Responsible for behaviour (that's JavaScript)

**HTML is:**
- The skeleton of every web page
- A hierarchy of nested elements
- Read top-to-bottom by browsers

---

## Document Structure

Every HTML file follows this base structure:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page Title</title>
  </head>
  <body>
    <!-- Page content goes here -->
  </body>
</html>
```

| Part | Purpose |
|------|---------|
| `<!DOCTYPE html>` | Tells the browser this is HTML5 |
| `<html lang="en">` | Root element; `lang` helps screen readers |
| `<head>` | Metadata — not visible on the page |
| `<meta charset="UTF-8">` | Character encoding (supports all symbols) |
| `<meta name="viewport">` | Makes the page responsive on mobile |
| `<title>` | Text shown in the browser tab |
| `<body>` | Everything visible on the page |

---

## Elements and Tags

An **element** is an opening tag, its content, and a closing tag:

```html
<tagname>Content goes here</tagname>
```

Some elements are **self-closing** (void elements) — they have no content and no closing tag:

```html
<br>
<hr>
<img src="photo.jpg" alt="A photo">
<input type="text">
<meta charset="UTF-8">
```

### Nesting

Elements can be nested inside one another. Indentation makes nesting readable:

```html
<div>
  <p>This paragraph is <strong>inside</strong> a div.</p>
</div>
```

> **Rule:** Always close tags in the reverse order you opened them. Never overlap elements.

---

## Headings

There are six heading levels, `<h1>` through `<h6>`.

```html
<h1>Heading 1</h1>
<h2>Heading 2</h2>
<h3>Heading 3</h3>
<h4>Heading 4</h4>
<h5>Heading 5</h5>
<h6>Heading 6</h6>
```

> **Use `<h1>` only once per page** — it signals the main topic to search engines. Use `<h2>`–`<h6>` for subsections in logical order without skipping levels.

---

## Paragraphs and Text

### Paragraph

```html
<p>This is a paragraph. It wraps text into a block with spacing above and below.</p>
```

### Line Break

Forces a new line within a block of text:

```html
<p>Line one.<br>Line two.</p>
```

### Horizontal Rule

Draws a horizontal divider line:

```html
<hr>
```

### Text Formatting

```html
<strong>Bold (important)</strong>
<em>Italic (emphasis)</em>
<u>Underline</u>
<s>Strikethrough</s>
<mark>Highlighted text</mark>
<small>Smaller text</small>
<sup>Superscript</sup> and <sub>Subscript</sub>
<code>Inline code</code>
<pre>Preserved whitespace and line breaks</pre>
<blockquote>A long quotation from another source.</blockquote>
<abbr title="HyperText Markup Language">HTML</abbr>
```

> Prefer `<strong>` over `<b>` and `<em>` over `<i>` — the semantic versions carry meaning for screen readers.

---

## Links

The anchor tag `<a>` creates a hyperlink.

### External Link

```html
<a href="https://www.example.com">Visit Example</a>
```

### Open in New Tab

```html
<a href="https://www.example.com" target="_blank" rel="noopener noreferrer">Open in new tab</a>
```

> Always add `rel="noopener noreferrer"` with `target="_blank"` for security.

### Link to Another Page (relative)

```html
<a href="about.html">About</a>
```

### Link to a Section on the Same Page

```html
<!-- The link -->
<a href="#contact">Jump to Contact</a>

<!-- The target section -->
<h2 id="contact">Contact</h2>
```

### Email Link

```html
<a href="mailto:hello@example.com">Send Email</a>
```

---

## Images

```html
<img src="photo.jpg" alt="A description of the photo">
```

| Attribute | Purpose |
|-----------|---------|
| `src` | Path or URL to the image file |
| `alt` | Text description (shown if image fails to load; required for accessibility) |
| `width` | Width in pixels |
| `height` | Height in pixels |

```html
<img src="logo.png" alt="Company logo" width="200" height="100">
```

> Always include `alt`. Leave it empty (`alt=""`) only for purely decorative images.

### Figure with Caption

```html
<figure>
  <img src="chart.png" alt="Sales chart for Q1">
  <figcaption>Figure 1: Q1 Sales Data</figcaption>
</figure>
```

---

## Lists

### Unordered List

```html
<ul>
  <li>Apples</li>
  <li>Bananas</li>
  <li>Cherries</li>
</ul>
```

### Ordered List

```html
<ol>
  <li>Preheat the oven</li>
  <li>Mix the ingredients</li>
  <li>Bake for 30 minutes</li>
</ol>
```

### Nested List

```html
<ul>
  <li>Fruits
    <ul>
      <li>Apple</li>
      <li>Banana</li>
    </ul>
  </li>
  <li>Vegetables</li>
</ul>
```

### Description List

Used for glossaries or key-value pairs:

```html
<dl>
  <dt>HTML</dt>
  <dd>The standard markup language for web pages.</dd>
  <dt>CSS</dt>
  <dd>A language for styling HTML elements.</dd>
</dl>
```

---

## Tables

```html
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
      <th>City</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Alice</td>
      <td>30</td>
      <td>New York</td>
    </tr>
    <tr>
      <td>Bob</td>
      <td>25</td>
      <td>London</td>
    </tr>
  </tbody>
</table>
```

| Tag | Purpose |
|-----|---------|
| `<table>` | The table container |
| `<thead>` | Group of header rows |
| `<tbody>` | Group of body rows |
| `<tr>` | A table row |
| `<th>` | A header cell (bold and centered by default) |
| `<td>` | A data cell |

### Spanning Columns and Rows

```html
<td colspan="2">Spans 2 columns</td>
<td rowspan="3">Spans 3 rows</td>
```

---

## Forms

Forms collect user input and can send it to a server.

```html
<form action="/submit" method="post">

  <label for="username">Username:</label>
  <input type="text" id="username" name="username" placeholder="Enter username">

  <label for="password">Password:</label>
  <input type="password" id="password" name="password">

  <label for="email">Email:</label>
  <input type="email" id="email" name="email">

  <label for="message">Message:</label>
  <textarea id="message" name="message" rows="4"></textarea>

  <label for="role">Role:</label>
  <select id="role" name="role">
    <option value="dev">Developer</option>
    <option value="design">Designer</option>
  </select>

  <label>
    <input type="checkbox" name="agree"> I agree to the terms
  </label>

  <button type="submit">Submit</button>

</form>
```

### Common Input Types

| Type | Usage |
|------|-------|
| `text` | Single-line text |
| `password` | Masked text |
| `email` | Email address (with validation) |
| `number` | Numeric input |
| `checkbox` | On/off toggle |
| `radio` | One choice from a group |
| `file` | File upload |
| `date` | Date picker |
| `submit` | Submit button |
| `hidden` | Hidden data to be sent with the form |

> Always pair `<label>` with an input using matching `for` and `id` values. This improves usability and accessibility.

---

## Semantic Elements

Semantic elements describe the **meaning** of their content, not just how it looks. Prefer them over generic `<div>` elements.

```html
<header>
  <!-- Site header, logo, top navigation -->
</header>

<nav>
  <!-- Navigation links -->
</nav>

<main>
  <!-- The main content of the page (use only once) -->

  <article>
    <!-- A self-contained piece of content (e.g., a blog post) -->
  </article>

  <section>
    <!-- A thematic group of content with a heading -->
  </section>

  <aside>
    <!-- Sidebar content, tangentially related to main content -->
  </aside>

</main>

<footer>
  <!-- Footer with copyright, links, contact info -->
</footer>
```

**Why use semantic elements?**
- Screen readers use them to navigate the page
- Search engines use them to understand content hierarchy
- Makes code easier to read and maintain

---

## Div and Span

When no semantic element fits, use `<div>` and `<span>` as generic containers.

| Element | Type | Use when |
|---------|------|----------|
| `<div>` | Block | Grouping block-level elements for layout or styling |
| `<span>` | Inline | Targeting a piece of text inline for styling |

```html
<div class="card">
  <p>This card contains <span class="highlight">highlighted</span> text.</p>
</div>
```

---

## Attributes

Attributes provide extra information about an element. They go in the opening tag as `name="value"` pairs.

### Common Global Attributes

| Attribute | Purpose |
|-----------|---------|
| `id` | Unique identifier for one element on the page |
| `class` | One or more CSS class names (space-separated) |
| `style` | Inline CSS styles |
| `title` | Tooltip text shown on hover |
| `hidden` | Hides the element |
| `data-*` | Custom data attributes (e.g., `data-user-id="42"`) |

```html
<p id="intro" class="lead text-dark" title="Introduction paragraph">
  Welcome to the guide.
</p>
```

---

## Comments

Comments are not displayed in the browser. Use them to annotate your code.

```html
<!-- This is a comment -->

<!--
  Multi-line comment.
  Useful for explaining sections.
-->
```

---

## Quick Reference Cheat Sheet

| Element | Tag |
|---------|-----|
| Document root | `<html>` |
| Page metadata | `<head>` |
| Visible content | `<body>` |
| Heading (largest) | `<h1>` |
| Heading (smallest) | `<h6>` |
| Paragraph | `<p>` |
| Line break | `<br>` |
| Horizontal rule | `<hr>` |
| Bold | `<strong>` |
| Italic | `<em>` |
| Inline code | `<code>` |
| Link | `<a href="url">text</a>` |
| Image | `<img src="file" alt="desc">` |
| Unordered list | `<ul>` + `<li>` |
| Ordered list | `<ol>` + `<li>` |
| Table | `<table>` + `<tr>` + `<td>` |
| Form | `<form>` + `<input>` + `<button>` |
| Generic block | `<div>` |
| Generic inline | `<span>` |
| Page header | `<header>` |
| Page footer | `<footer>` |
| Navigation | `<nav>` |
| Main content | `<main>` |
| Article | `<article>` |
| Section | `<section>` |
| Comment | `<!-- text -->` |

---

*Part of the learning-resources collection.*
