# Regex Guide

A short practical guide to regular expressions.

---

## Table of Contents

1. [What is Regex?](#what-is-regex)
2. [Literal Characters](#literal-characters)
3. [Character Classes](#character-classes)
4. [Anchors](#anchors)
5. [Quantifiers](#quantifiers)
6. [Groups and Alternation](#groups-and-alternation)
7. [Lookahead and Lookbehind](#lookahead-and-lookbehind)
8. [Flags](#flags)
9. [Using Regex in JavaScript](#using-regex-in-javascript)
10. [Using Regex in Python](#using-regex-in-python)
11. [Common Patterns](#common-patterns)
12. [Quick Reference Cheat Sheet](#quick-reference-cheat-sheet)

---

## What is Regex?

A **regular expression** (regex) is a pattern used to search, match, and manipulate text. Instead of looking for a fixed string like `"hello"`, you write a pattern that describes a range of possible strings.

```
Pattern:  \d{3}-\d{4}
Matches:  555-1234, 999-0000, 123-4567
```

Regex is supported in almost every programming language and in tools like `grep`, VS Code find, and most text editors.

---

## Literal Characters

Most characters match themselves exactly:

```
Pattern:  cat
Matches:  "The cat sat"  →  cat
```

**Special characters** have reserved meaning and must be escaped with `\` to be used literally:

```
. * + ? ^ $ { } [ ] | ( ) \
```

```
Pattern:  3\.14
Matches:  "3.14"  (not "3x14")
```

---

## Character Classes

Match **one character** from a defined set.

```
[aeiou]       One lowercase vowel
[a-z]         One lowercase letter
[A-Z]         One uppercase letter
[0-9]         One digit
[a-zA-Z]      One letter (upper or lower)
[a-zA-Z0-9]   One letter or digit
[^aeiou]      One character that is NOT a vowel  (^ inside [] = negation)
```

### Shorthand Classes

| Shorthand | Equivalent | Meaning |
|-----------|------------|---------|
| `\d` | `[0-9]` | Any digit |
| `\D` | `[^0-9]` | Any non-digit |
| `\w` | `[a-zA-Z0-9_]` | Any word character |
| `\W` | `[^a-zA-Z0-9_]` | Any non-word character |
| `\s` | `[ \t\n\r\f]` | Any whitespace |
| `\S` | `[^ \t\n\r\f]` | Any non-whitespace |
| `.` | *(any)* | Any character except newline |

---

## Anchors

Anchors match a **position** in the string, not a character.

| Anchor | Matches |
|--------|---------|
| `^` | Start of the string (or line in multiline mode) |
| `$` | End of the string (or line in multiline mode) |
| `\b` | Word boundary (between `\w` and `\W`) |
| `\B` | Not a word boundary |

```
Pattern:  ^hello
Matches:  "hello world"  but NOT  "say hello"

Pattern:  world$
Matches:  "hello world"  but NOT  "worldwide"

Pattern:  \bcat\b
Matches:  "the cat sat"  but NOT  "catch" or "concatenate"
```

---

## Quantifiers

Quantifiers control **how many times** the preceding element must match.

| Quantifier | Meaning |
|------------|---------|
| `*` | 0 or more |
| `+` | 1 or more |
| `?` | 0 or 1 (optional) |
| `{n}` | Exactly n times |
| `{n,}` | n or more times |
| `{n,m}` | Between n and m times (inclusive) |

```
Pattern:  colou?r
Matches:  "color" and "colour"  (u is optional)

Pattern:  \d+
Matches:  "1", "42", "1000"  (one or more digits)

Pattern:  \d{3}-\d{4}
Matches:  "555-1234"

Pattern:  [a-z]{2,5}
Matches:  "hi", "hello", "hey"  but NOT "a" or "toolong"
```

### Greedy vs Lazy

By default quantifiers are **greedy** — they match as much as possible.

Add `?` after a quantifier to make it **lazy** — match as little as possible.

```
Input:    <b>bold</b> and <i>italic</i>
Greedy:   <.+>    matches  <b>bold</b> and <i>italic</i>  (too much)
Lazy:     <.+?>   matches  <b>  then  </b>  then  <i>  then  </i>
```

---

## Groups and Alternation

### Capturing Group `( )`

Groups part of a pattern and **captures** it for later use:

```
Pattern:  (\d{4})-(\d{2})-(\d{2})
Input:    "2024-01-15"
Group 1:  "2024"
Group 2:  "01"
Group 3:  "15"
```

### Non-Capturing Group `(?: )`

Groups without capturing — useful for applying quantifiers to a sub-expression:

```
Pattern:  (?:ha)+
Matches:  "ha", "haha", "hahaha"
```

### Named Group `(?<name> )`

```
Pattern:  (?<year>\d{4})-(?<month>\d{2})
```

Access as `match.groups.year` (JavaScript) or `match.group("year")` (Python).

### Alternation `|`

Matches either the left or right side:

```
Pattern:  cat|dog
Matches:  "I have a cat" and "I have a dog"

Pattern:  gr(a|e)y
Matches:  "gray" and "grey"
```

### Backreferences

Refer back to a previously captured group using `\1`, `\2`, etc.:

```
Pattern:  (\w+) \1
Matches:  "hello hello", "the the"  (repeated word)
```

---

## Lookahead and Lookbehind

These match a position based on what's **around** it, without including those surrounding characters in the match.

| Syntax | Name | Meaning |
|--------|------|---------|
| `(?=...)` | Positive lookahead | Followed by ... |
| `(?!...)` | Negative lookahead | Not followed by ... |
| `(?<=...)` | Positive lookbehind | Preceded by ... |
| `(?<!...)` | Negative lookbehind | Not preceded by ... |

```
Pattern:  \d+(?= dollars)
Input:    "100 dollars and 50 euros"
Matches:  "100"  (not "50", which is followed by "euros")

Pattern:  (?<=\$)\d+
Input:    "Price: $42"
Matches:  "42"  (preceded by $, but $ not included)
```

---

## Flags

Flags modify how the pattern is applied. They come after the closing `/` in JavaScript or as parameters in Python.

| Flag | JS | Python | Effect |
|------|-----|--------|--------|
| Case-insensitive | `i` | `re.IGNORECASE` | `A` matches `a` |
| Multiline | `m` | `re.MULTILINE` | `^` and `$` match start/end of each line |
| Dotall | `s` | `re.DOTALL` | `.` also matches newline |
| Global | `g` | *(use `findall`)* | Find all matches, not just the first |

```javascript
// JavaScript
/hello/i.test("Hello World")  // true
```

```python
# Python
re.search(r"hello", "Hello World", re.IGNORECASE)
```

---

## Using Regex in JavaScript

```javascript
const pattern = /\d{3}-\d{4}/;

// Test if a pattern matches
pattern.test("555-1234");             // true

// Find first match
"Call 555-1234 or 555-5678".match(/\d{3}-\d{4}/);
// ["555-1234"]

// Find all matches (global flag)
"Call 555-1234 or 555-5678".match(/\d{3}-\d{4}/g);
// ["555-1234", "555-5678"]

// Extract capture groups
const m = "2024-01-15".match(/(\d{4})-(\d{2})-(\d{2})/);
// m[0] = "2024-01-15", m[1] = "2024", m[2] = "01", m[3] = "15"

// Replace
"Hello World".replace(/world/i, "Regex");
// "Hello Regex"

// Replace with captured groups
"2024-01-15".replace(/(\d{4})-(\d{2})-(\d{2})/, "$3/$2/$1");
// "15/01/2024"

// Split
"one,two,,three".split(/,+/);
// ["one", "two", "three"]
```

---

## Using Regex in Python

```python
import re

pattern = r"\d{3}-\d{4}"   # Use raw strings (r"...") to avoid escaping backslashes

# Test if pattern matches anywhere
re.search(pattern, "Call 555-1234")       # Match object or None

# Test if pattern matches from the start
re.match(r"\d+", "123 abc")               # Match object

# Find all matches
re.findall(r"\d{3}-\d{4}", "555-1234 or 555-5678")
# ["555-1234", "555-5678"]

# Find all with groups
re.findall(r"(\d{4})-(\d{2})", "2024-01 and 2023-12")
# [("2024", "01"), ("2023", "12")]

# Replace
re.sub(r"\s+", "-", "hello world foo")   # "hello-world-foo"

# Replace with backreferences
re.sub(r"(\d{4})-(\d{2})-(\d{2})", r"\3/\2/\1", "2024-01-15")
# "15/01/2024"

# Compile for reuse
phone = re.compile(r"\d{3}-\d{4}")
phone.findall("555-1234 or 555-5678")

# Split
re.split(r",+", "one,two,,three")         # ["one", "two", "three"]
```

---

## Common Patterns

| Purpose | Pattern |
|---------|---------|
| Integer | `^-?\d+$` |
| Decimal number | `^-?\d+(\.\d+)?$` |
| Email (basic) | `^[\w.+-]+@[\w-]+\.[a-zA-Z]{2,}$` |
| URL (basic) | `https?://[\w./-]+` |
| IPv4 address | `\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b` |
| Date (YYYY-MM-DD) | `\d{4}-(?:0[1-9]\|1[0-2])-(?:0[1-9]\|[12]\d\|3[01])` |
| Time (HH:MM) | `^([01]\d\|2[0-3]):[0-5]\d$` |
| Hex colour | `^#([0-9a-fA-F]{3}\|[0-9a-fA-F]{6})$` |
| Slug (URL-safe) | `^[a-z0-9]+(?:-[a-z0-9]+)*$` |
| Postcode (UK) | `^[A-Z]{1,2}\d[A-Z\d]? ?\d[A-Z]{2}$` |
| Zip code (US) | `^\d{5}(-\d{4})?$` |
| Username | `^[a-zA-Z0-9_]{3,20}$` |
| Strong password | `^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*\W).{8,}$` |
| HTML tag | `<([a-zA-Z][a-zA-Z0-9]*)\b[^>]*>` |
| Blank line | `^\s*$` |
| Repeated word | `\b(\w+)\s+\1\b` |
| Trailing whitespace | `\s+$` |

---

## Quick Reference Cheat Sheet

| Pattern | Matches |
|---------|---------|
| `.` | Any character except newline |
| `\d` | Digit `[0-9]` |
| `\w` | Word character `[a-zA-Z0-9_]` |
| `\s` | Whitespace |
| `\D`, `\W`, `\S` | Negations of above |
| `[abc]` | a, b, or c |
| `[^abc]` | Not a, b, or c |
| `[a-z]` | Lowercase letter |
| `^` | Start of string |
| `$` | End of string |
| `\b` | Word boundary |
| `*` | 0 or more |
| `+` | 1 or more |
| `?` | 0 or 1 |
| `{n}` | Exactly n |
| `{n,m}` | Between n and m |
| `*?` `+?` | Lazy (non-greedy) |
| `(abc)` | Capture group |
| `(?:abc)` | Non-capturing group |
| `a\|b` | a or b |
| `(?=...)` | Lookahead |
| `(?<=...)` | Lookbehind |
| `\1` | Backreference to group 1 |

---

*Part of the learning-resources collection.*
