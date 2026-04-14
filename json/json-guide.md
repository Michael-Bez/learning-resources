# JSON Guide

A complete reference for reading and writing JSON.

---

## Table of Contents

1. [What is JSON?](#what-is-json)
2. [Data Types](#data-types)
3. [Objects](#objects)
4. [Arrays](#arrays)
5. [Nesting](#nesting)
6. [Syntax Rules](#syntax-rules)
7. [JSON in JavaScript](#json-in-javascript)
8. [JSON in Python](#json-in-python)
9. [Common Patterns](#common-patterns)
10. [Quick Reference Cheat Sheet](#quick-reference-cheat-sheet)

---

## What is JSON?

JSON (JavaScript Object Notation) is a lightweight, text-based format for storing and exchanging data. Despite its name, it is language-independent and supported by virtually every programming language.

**JSON is used for:**
- API requests and responses
- Configuration files (e.g. `package.json`, `tsconfig.json`)
- Storing and transporting structured data

**JSON is not:**
- A programming language
- A database (though it can be stored in one)
- The same as a JavaScript object (though the syntax is very similar)

---

## Data Types

JSON supports six value types:

| Type | Example |
|------|---------|
| String | `"Hello, world"` |
| Number | `42`, `3.14`, `-7`, `1.5e10` |
| Boolean | `true`, `false` |
| Null | `null` |
| Object | `{ "key": "value" }` |
| Array | `[1, 2, 3]` |

### String

Must use **double quotes** — single quotes are not valid JSON.

```json
"Hello, world"
"Line one\nLine two"
"Tab\there"
"Escaped quote: \""
"Unicode: \u0041"
```

### Number

No distinction between integers and floats. No surrounding quotes.

```json
42
-7
3.14
1.5e10
```

### Boolean

Lowercase only — `True` and `False` are not valid.

```json
true
false
```

### Null

Represents the intentional absence of a value.

```json
null
```

---

## Objects

An object is an **unordered** collection of key-value pairs, wrapped in `{}`.

```json
{
  "name": "Alice",
  "age": 30,
  "active": true,
  "score": 9.5,
  "nickname": null
}
```

- Keys must be **strings in double quotes**
- Keys and values are separated by `:`
- Pairs are separated by `,`
- No trailing comma after the last pair

### Accessing Values

In JavaScript: `obj.name` or `obj["name"]`  
In Python: `obj["name"]`

---

## Arrays

An array is an **ordered** list of values, wrapped in `[]`.

```json
["apple", "banana", "cherry"]
```

Arrays can hold any mix of types:

```json
[1, "two", true, null, { "x": 3 }, [4, 5]]
```

### Array of Objects (very common)

```json
[
  { "id": 1, "name": "Alice" },
  { "id": 2, "name": "Bob" },
  { "id": 3, "name": "Charlie" }
]
```

---

## Nesting

Objects and arrays can be nested to any depth.

```json
{
  "user": {
    "id": 101,
    "name": "Alice",
    "address": {
      "city": "London",
      "postcode": "EC1A 1BB"
    },
    "tags": ["admin", "editor"],
    "preferences": {
      "theme": "dark",
      "notifications": true
    }
  }
}
```

**Accessing nested values:**

- JavaScript: `data.user.address.city`
- Python: `data["user"]["address"]["city"]`

---

## Syntax Rules

These are the most common mistakes when writing JSON:

| Rule | Correct | Incorrect |
|------|---------|-----------|
| Strings use double quotes | `"name": "Alice"` | `'name': 'Alice'` |
| No trailing commas | `"a": 1, "b": 2` | `"a": 1, "b": 2,` |
| No comments | *(not supported)* | `// a comment` |
| Booleans are lowercase | `true`, `false` | `True`, `False` |
| Null is lowercase | `null` | `Null`, `NULL` |
| Keys must be strings | `"key": "value"` | `key: "value"` |
| No functions or undefined | *(not supported)* | `undefined`, `function(){}` |

> **Tip:** Use a JSON validator (e.g. jsonlint.com or your editor's built-in linting) to find syntax errors quickly.

---

## JSON in JavaScript

### Parse JSON String → Object

```javascript
const jsonString = '{"name": "Alice", "age": 30}';
const obj = JSON.parse(jsonString);

console.log(obj.name);  // "Alice"
console.log(obj.age);   // 30
```

### Serialize Object → JSON String

```javascript
const user = { name: "Alice", age: 30, active: true };
const jsonString = JSON.stringify(user);

console.log(jsonString);
// '{"name":"Alice","age":30,"active":true}'
```

### Pretty-Print (indented output)

```javascript
console.log(JSON.stringify(user, null, 2));
// {
//   "name": "Alice",
//   "age": 30,
//   "active": true
// }
```

### Fetch JSON from an API

```javascript
const response = await fetch("https://api.example.com/users");
const data = await response.json();
console.log(data);
```

### Things JavaScript Silently Drops

`JSON.stringify` ignores: `undefined`, functions, and `Symbol` values.

```javascript
JSON.stringify({ a: 1, b: undefined, c: () => {} });
// '{"a":1}'
```

---

## JSON in Python

### Parse JSON String → Dict

```python
import json

json_string = '{"name": "Alice", "age": 30}'
obj = json.loads(json_string)

print(obj["name"])  # Alice
print(obj["age"])   # 30
```

### Serialize Dict → JSON String

```python
user = {"name": "Alice", "age": 30, "active": True}
json_string = json.dumps(user)
print(json_string)
# '{"name": "Alice", "age": 30, "active": true}'
```

### Pretty-Print

```python
print(json.dumps(user, indent=2))
# {
#   "name": "Alice",
#   "age": 30,
#   "active": true
# }
```

### Read JSON from a File

```python
with open("data.json", "r") as f:
    data = json.load(f)
```

### Write JSON to a File

```python
with open("data.json", "w") as f:
    json.dump(data, f, indent=2)
```

### Python ↔ JSON Type Mapping

| Python | JSON |
|--------|------|
| `dict` | object `{}` |
| `list`, `tuple` | array `[]` |
| `str` | string |
| `int`, `float` | number |
| `True` / `False` | `true` / `false` |
| `None` | `null` |

---

## Common Patterns

### Config File

```json
{
  "app": {
    "name": "My App",
    "version": "1.0.0",
    "debug": false
  },
  "database": {
    "host": "localhost",
    "port": 5432,
    "name": "mydb"
  },
  "allowedOrigins": ["https://example.com", "https://app.example.com"]
}
```

### API Response (single resource)

```json
{
  "status": "success",
  "data": {
    "id": 42,
    "name": "Alice",
    "email": "alice@example.com",
    "createdAt": "2024-01-15T09:30:00Z"
  }
}
```

### API Response (list with pagination)

```json
{
  "status": "success",
  "data": [
    { "id": 1, "name": "Alice" },
    { "id": 2, "name": "Bob" }
  ],
  "pagination": {
    "page": 1,
    "perPage": 20,
    "total": 150
  }
}
```

### Error Response

```json
{
  "status": "error",
  "code": 404,
  "message": "User not found"
}
```

---

## Quick Reference Cheat Sheet

| Concept | Syntax |
|---------|--------|
| String | `"text"` |
| Number | `42`, `3.14` |
| Boolean | `true`, `false` |
| Null | `null` |
| Object | `{ "key": "value" }` |
| Array | `[1, 2, 3]` |
| Nested object | `{ "a": { "b": 1 } }` |
| Array of objects | `[{ "id": 1 }, { "id": 2 }]` |
| JS parse | `JSON.parse(str)` |
| JS stringify | `JSON.stringify(obj)` |
| JS pretty-print | `JSON.stringify(obj, null, 2)` |
| Python parse | `json.loads(str)` |
| Python stringify | `json.dumps(obj)` |
| Python read file | `json.load(file)` |
| Python write file | `json.dump(obj, file, indent=2)` |

---

*Part of the learning-resources collection.*
