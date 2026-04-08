# Markdown Guide

A complete reference for writing and formatting Markdown.

---

## Table of Contents

1. [What is Markdown?](#what-is-markdown)
2. [Headings](#headings)
3. [Text Formatting](#text-formatting)
4. [Paragraphs and Line Breaks](#paragraphs-and-line-breaks)
5. [Lists](#lists)
6. [Links](#links)
7. [Images](#images)
8. [Blockquotes](#blockquotes)
9. [Code](#code)
10. [Horizontal Rules](#horizontal-rules)
11. [Tables](#tables)
12. [Task Lists](#task-lists)
13. [Escaping Characters](#escaping-characters)
14. [HTML in Markdown](#html-in-markdown)

---

## What is Markdown?

Markdown is a lightweight markup language that lets you format plain text using simple symbols. It was created by John Gruber in 2004. You write plain text with special characters, and a Markdown renderer converts it to styled HTML.

**Why use Markdown?**

- Simple to learn and read even in raw form
- Widely supported (GitHub, Reddit, Notion, VS Code, and many more)
- Converts cleanly to HTML
- Portable — it's just a `.md` text file

---

## Headings

Use `#` symbols to create headings. One `#` is the largest, six `######` is the smallest.

```
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

**Renders as:**

# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6

> **Rule:** Always put a space between the `#` and your heading text.

---

## Text Formatting

### Bold

Wrap text in `**double asterisks**` or `__double underscores__`.

```
**This is bold**
__This is also bold__
```

**This is bold**

### Italic

Wrap text in `*single asterisks*` or `_single underscores_`.

```
*This is italic*
_This is also italic_
```

*This is italic*

### Bold and Italic

Combine them with `***triple asterisks***`.

```
***This is bold and italic***
```

***This is bold and italic***

### Strikethrough

Wrap text in `~~double tildes~~`.

```
~~This text is crossed out~~
```

~~This text is crossed out~~

### Inline Code

Wrap text in backticks `` ` ``.

```
Use the `print()` function to output text.
```

Use the `print()` function to output text.

---

## Paragraphs and Line Breaks

### Paragraphs

Separate paragraphs with a **blank line**.

```
This is the first paragraph.

This is the second paragraph.
```

### Line Breaks

To force a line break within a paragraph, end a line with **two or more spaces**, then press Enter.

```
Line one  
Line two (note: two spaces at end of line one)
```

Line one  
Line two

---

## Lists

### Unordered Lists

Use `-`, `*`, or `+` followed by a space.

```
- Item one
- Item two
- Item three
```

- Item one
- Item two
- Item three

### Ordered Lists

Use numbers followed by a period.

```
1. First item
2. Second item
3. Third item
```

1. First item
2. Second item
3. Third item

> The numbers don't have to be in order — Markdown will auto-number them. But using `1. 2. 3.` is the clearest practice.

### Nested Lists

Indent with 2 or 4 spaces (or a tab) to create sub-items.

```
- Fruits
  - Apple
  - Banana
    - Cavendish variety
  - Cherry
- Vegetables
  - Carrot
  - Broccoli
```

- Fruits
  - Apple
  - Banana
    - Cavendish variety
  - Cherry
- Vegetables
  - Carrot
  - Broccoli

---

## Links

### Inline Links

```
[Link text](https://www.example.com)
```

[Link text](https://www.example.com)

### Links with a Title (tooltip on hover)

```
[Link text](https://www.example.com "This is a tooltip")
```

### Reference-style Links

Useful for keeping text readable when you have many links.

```
This is a [reference link][ref1] and another [link][ref2].

[ref1]: https://www.example.com
[ref2]: https://www.another.com
```

### Bare URLs

Wrap a URL in angle brackets to display it as a clickable link.

```
<https://www.example.com>
```

<https://www.example.com>

---

## Images

Similar to links, but with a `!` at the start.

```
![Alt text](image.png)
![Alt text](image.png "Optional title")
```

The **alt text** appears when the image fails to load and is important for accessibility.

### Reference-style Images

```
![Alt text][img1]

[img1]: image.png "Optional title"
```

---

## Blockquotes

Use `>` at the start of a line.

```
> This is a blockquote.
```

> This is a blockquote.

### Multi-line Blockquotes

```
> Line one of the quote.
> Line two of the quote.
```

> Line one of the quote.
> Line two of the quote.

### Nested Blockquotes

```
> Outer quote.
>
> > Inner nested quote.
```

> Outer quote.
>
> > Inner nested quote.

### Blockquotes with Other Formatting

```
> **Bold inside a quote.**
> - A list inside a quote
> - Second item
```

> **Bold inside a quote.**
> - A list inside a quote
> - Second item

---

## Code

### Inline Code

Use single backticks for short code snippets within a sentence.

```
Run `npm install` to get started.
```

Run `npm install` to get started.

### Fenced Code Blocks

Use triple backticks (` ``` `) before and after the code block.

````
```
function greet(name) {
  return "Hello, " + name;
}
```
````

```
function greet(name) {
  return "Hello, " + name;
}
```

### Syntax Highlighting

Add the language name after the opening triple backticks.

````
```python
def greet(name):
    return f"Hello, {name}"
```
````

```python
def greet(name):
    return f"Hello, {name}"
```

````
```javascript
const greet = (name) => `Hello, ${name}`;
```
````

```javascript
const greet = (name) => `Hello, ${name}`;
```

Common language identifiers: `python`, `javascript`, `js`, `typescript`, `ts`, `html`, `css`, `bash`, `sh`, `sql`, `json`, `yaml`, `go`, `rust`, `java`, `c`, `cpp`

### Indented Code Blocks (legacy)

Indent 4 spaces or one tab to create a code block (no syntax highlighting).

```
    This is a code block
    using indentation.
```

---

## Horizontal Rules

Use three or more hyphens `---`, asterisks `***`, or underscores `___` on their own line.

```
---
```

---

```
***
```

***

> **Tip:** Leave a blank line before and after a horizontal rule to avoid conflicts with heading syntax.

---

## Tables

Use pipes `|` and hyphens `-` to create tables.

```
| Name    | Age | City       |
|---------|-----|------------|
| Alice   | 30  | New York   |
| Bob     | 25  | London     |
| Charlie | 35  | Tokyo      |
```

| Name    | Age | City       |
|---------|-----|------------|
| Alice   | 30  | New York   |
| Bob     | 25  | London     |
| Charlie | 35  | Tokyo      |

### Column Alignment

Use colons `:` in the separator row to control alignment.

```
| Left         | Center       | Right        |
|:-------------|:------------:|-------------:|
| aligned left | centered     | aligned right|
| text         | text         | text         |
```

| Left         | Center       | Right        |
|:-------------|:------------:|-------------:|
| aligned left | centered     | aligned right|
| text         | text         | text         |

> You don't need to perfectly align the pipes — Markdown will still parse it correctly.

---

## Task Lists

Use `- [ ]` for an unchecked box and `- [x]` for a checked box.

```
- [x] Write the introduction
- [x] Add examples
- [ ] Proofread
- [ ] Publish
```

- [x] Write the introduction
- [x] Add examples
- [ ] Proofread
- [ ] Publish

> Task lists are a GitHub Flavored Markdown (GFM) feature and may not render in all editors.

---

## Escaping Characters

To display a character that Markdown would normally interpret as formatting, put a backslash `\` before it.

Characters you may need to escape:

```
\  `  *  _  {}  []  ()  #  +  -  .  !  |
```

**Example:**

```
\*This will not be italic\*
```

\*This will not be italic\*

---

## HTML in Markdown

Most Markdown parsers allow you to write raw HTML directly.

```html
<strong>Bold using HTML</strong>

<details>
  <summary>Click to expand</summary>
  Hidden content here.
</details>
```

<strong>Bold using HTML</strong>

<details>
  <summary>Click to expand</summary>
  Hidden content here.
</details>

> Use HTML sparingly — it reduces readability of the raw source and may not be supported everywhere.

---

## Quick Reference Cheat Sheet

| Element         | Syntax                          |
|-----------------|---------------------------------|
| Heading 1       | `# H1`                          |
| Heading 2       | `## H2`                         |
| Bold            | `**bold**`                      |
| Italic          | `*italic*`                      |
| Bold + Italic   | `***bold italic***`             |
| Strikethrough   | `~~text~~`                      |
| Inline code     | `` `code` ``                    |
| Code block      | ` ```language `                 |
| Link            | `[text](url)`                   |
| Image           | `![alt](image.png)`             |
| Blockquote      | `> quote`                       |
| Unordered list  | `- item`                        |
| Ordered list    | `1. item`                       |
| Task list       | `- [ ] task`                    |
| Horizontal rule | `---`                           |
| Table           | `\| col \| col \|`              |
| Escape char     | `\*`                            |

---
