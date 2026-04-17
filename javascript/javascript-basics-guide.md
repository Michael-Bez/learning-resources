# JavaScript Basics Guide

A practical introduction to JavaScript programming.

---

## Table of Contents

1. [Running JavaScript](#running-javascript)
2. [Variables](#variables)
3. [Data Types](#data-types)
4. [Strings](#strings)
5. [Numbers](#numbers)
6. [Arrays](#arrays)
7. [Objects](#objects)
8. [Operators](#operators)
9. [Conditionals](#conditionals)
10. [Loops](#loops)
11. [Functions](#functions)
12. [Scope and Closures](#scope-and-closures)
13. [Array Methods](#array-methods)

---

## Running JavaScript

**In the browser:** Open DevTools with `F12` or `Cmd+Option+I` → Console tab.

**In Node.js (terminal):**
```bash
node script.js         # Run a file
node                   # Open an interactive REPL (Ctrl+D to quit)
```

---

## Variables

Use `const` by default. Use `let` when a variable needs to be reassigned. Avoid `var`.

```javascript
const name = "Alice";        // Cannot be reassigned
let count = 0;               // Can be reassigned
count = 1;

const PI = 3.14;

// var is function-scoped and hoisted — avoid it in modern JS
var oldStyle = "avoid this";
```

---

## Data Types

JavaScript has seven primitive types plus objects.

```javascript
// Primitives
"hello"           // string
42                // number
3.14              // number (no separate int/float)
true              // boolean
false             // boolean
null              // intentional absence of value
undefined         // declared but not yet assigned
Symbol("id")      // unique identifier (advanced use)

// Object types
{}                // object
[]                // array (a type of object)
function() {}     // function (a type of object)

// Check the type
typeof "hello"    // "string"
typeof 42         // "number"
typeof true       // "boolean"
typeof null       // "object"  ← known JavaScript quirk
typeof undefined  // "undefined"
typeof []         // "object"
Array.isArray([]) // true
```

---

## Strings

```javascript
const s = "Hello, World";

// Template literals (backticks) — preferred for interpolation
const name = "Alice";
`Hello, ${name}!`        // "Hello, Alice!"
`2 + 2 = ${2 + 2}`       // "2 + 2 = 4"

// Multi-line string
const text = `Line one
Line two`;

// Common methods
s.length                 // 12
s.toUpperCase()          // "HELLO, WORLD"
s.toLowerCase()          // "hello, world"
s.trim()                 // Remove whitespace from both ends
s.includes("World")      // true
s.startsWith("Hello")    // true
s.endsWith("World")      // true
s.indexOf("o")           // 4  (-1 if not found)
s.slice(0, 5)            // "Hello"
s.slice(-5)              // "World"
s.replace("World", "JS") // "Hello, JS"
s.replaceAll("l", "L")   // "HeLLo, WorLd"
s.split(", ")            // ["Hello", "World"]
"  hi  ".trim()          // "hi"
"ha".repeat(3)           // "hahaha"
String(42)               // "42"
Number("42")             // 42
```

---

## Numbers

```javascript
42 + 3      // 45
10 - 3      // 7
4 * 5       // 20
10 / 3      // 3.3333...
10 % 3      // 1  (remainder)
2 ** 10     // 1024  (power)

Math.round(3.6)    // 4
Math.floor(3.9)    // 3
Math.ceil(3.1)     // 4
Math.abs(-5)       // 5
Math.max(1, 5, 3)  // 5
Math.min(1, 5, 3)  // 1
Math.sqrt(16)      // 4
Math.random()      // Random float between 0 and 1

// Integer between 1 and 10
Math.floor(Math.random() * 10) + 1

// Parsing
parseInt("42px")   // 42
parseFloat("3.14") // 3.14
Number("42")       // 42
Number("hello")    // NaN

// Special values
Infinity           // Positive infinity
-Infinity          // Negative infinity
NaN                // Not a Number
isNaN("hello")     // true
isFinite(Infinity) // false
```

---

## Arrays

```javascript
const fruits = ["apple", "banana", "cherry"];

// Access
fruits[0]              // "apple"
fruits[fruits.length - 1]  // "cherry"

// Add / Remove
fruits.push("mango")            // Add to end → returns new length
fruits.pop()                    // Remove from end → returns removed item
fruits.unshift("kiwi")          // Add to start
fruits.shift()                  // Remove from start

fruits.splice(1, 1)             // Remove 1 item at index 1
fruits.splice(1, 0, "lime")     // Insert "lime" at index 1

// Find
fruits.indexOf("banana")        // 1  (-1 if not found)
fruits.includes("banana")       // true
fruits.find(f => f.length > 5)  // "banana"  (first match)
fruits.findIndex(f => f.length > 5) // 1

// Sorting
fruits.sort()                   // Alphabetical sort (mutates array)
[3, 1, 2].sort((a, b) => a - b) // Numeric sort: [1, 2, 3]

// Other
fruits.reverse()                // Reverse in place
[1, [2, [3]]].flat()            // [1, 2, [3]]
[1, [2, [3]]].flat(Infinity)    // [1, 2, 3]
fruits.join(" - ")              // "apple - banana - cherry"
fruits.slice(0, 2)              // ["apple", "banana"]  (non-mutating)
fruits.length                   // 3
Array.from("hello")             // ["h", "e", "l", "l", "o"]
Array.from({length: 3}, (_, i) => i)  // [0, 1, 2]
```

---

## Objects

```javascript
const user = {
  name: "Alice",
  age: 30,
  greet() {                     // Shorthand method
    return `Hi, I'm ${this.name}`;
  }
};

// Access
user.name                       // "Alice"
user["name"]                    // "Alice"
user.greet()                    // "Hi, I'm Alice"

// Add / Update / Delete
user.email = "alice@example.com";
user.age = 31;
delete user.age;

// Check if key exists
"name" in user                  // true
user.hasOwnProperty("name")     // true

// Get keys, values, entries
Object.keys(user)               // ["name", "email"]
Object.values(user)             // ["Alice", "alice@example.com"]
Object.entries(user)            // [["name", "Alice"], ...]

// Merge objects
const merged = { ...defaults, ...overrides };
const copy = Object.assign({}, user);

// Loop
for (const [key, value] of Object.entries(user)) {
  console.log(key, value);
}
```

---

## Operators

```javascript
// Comparison
5 === 5         // true  — strict equality (checks type AND value)
5 == "5"        // true  — loose equality (coerces types) — avoid
5 !== 6         // true
5 > 3           // true

// Logical
true && false   // false  (AND)
true || false   // true   (OR)
!true           // false  (NOT)

// Nullish coalescing — use right side only if left is null/undefined
const val = user.name ?? "Anonymous";

// Optional chaining — safely access nested properties
user?.address?.city   // undefined instead of TypeError

// Logical assignment
a ||= "default"       // a = a || "default"
a &&= transform(a)    // a = a && transform(a)
a ??= "fallback"      // a = a ?? "fallback"

// Ternary
const label = age >= 18 ? "Adult" : "Minor";
```

---

## Conditionals

```javascript
if (score >= 90) {
  console.log("A");
} else if (score >= 70) {
  console.log("B");
} else {
  console.log("C");
}

// Switch
switch (day) {
  case "Saturday":
  case "Sunday":
    console.log("Weekend");
    break;
  default:
    console.log("Weekday");
}
```

---

## Loops

```javascript
// For loop
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// For...of — iterate over values (arrays, strings, Maps, Sets)
for (const fruit of fruits) {
  console.log(fruit);
}

// For...in — iterate over object keys
for (const key in user) {
  console.log(key, user[key]);
}

// While
let i = 0;
while (i < 5) {
  console.log(i++);
}

// Break and continue
for (const n of numbers) {
  if (n === 0) continue;   // Skip
  if (n < 0) break;        // Stop
}
```

---

## Functions

```javascript
// Function declaration (hoisted — can be called before it's defined)
function add(a, b) {
  return a + b;
}

// Function expression
const add = function(a, b) {
  return a + b;
};

// Arrow function (concise; does not have its own `this`)
const add = (a, b) => a + b;             // Implicit return
const square = x => x * x;              // Single parameter, no parens needed
const greet = () => "Hello!";           // No parameters

// Multi-line arrow function needs explicit return
const process = (x) => {
  const doubled = x * 2;
  return doubled + 1;
};

// Default parameters
function greet(name = "Guest") {
  return `Hello, ${name}!`;
}

// Rest parameters
function sum(...numbers) {
  return numbers.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3, 4);   // 10
```

---

## Scope and Closures

```javascript
// Block scope: const and let are scoped to the nearest {}
{
  const x = 10;
  let y = 20;
}
// x and y are not accessible here

// Closure: a function that "remembers" its outer scope
function makeCounter() {
  let count = 0;
  return function() {
    count++;
    return count;
  };
}

const counter = makeCounter();
counter();   // 1
counter();   // 2
counter();   // 3
```

---

## Array Methods

These methods accept a callback function and are the backbone of modern JS:

```javascript
const numbers = [1, 2, 3, 4, 5];

// map — transform each element, return a new array
numbers.map(n => n * 2);               // [2, 4, 6, 8, 10]

// filter — keep elements that pass a test
numbers.filter(n => n % 2 === 0);      // [2, 4]

// reduce — accumulate a single value
numbers.reduce((sum, n) => sum + n, 0); // 15

// forEach — run a function for each element (no return value)
numbers.forEach(n => console.log(n));

// find — first element that passes a test
numbers.find(n => n > 3);             // 4

// some — true if at least one passes
numbers.some(n => n > 4);             // true

// every — true if all pass
numbers.every(n => n > 0);            // true

// Chaining
const result = [1, 2, 3, 4, 5, 6]
  .filter(n => n % 2 === 0)            // [2, 4, 6]
  .map(n => n * 10);                   // [20, 40, 60]
```

---
