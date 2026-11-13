# Study note: recursion

This note covers the ideas and tools you need for this question. It doesn't solve it for you.

## What a recursive function is

A recursive function solves a problem by calling itself on a **smaller** version of the same problem. Every recursive function has two parts:

- **Base case:** a version of the problem small enough to answer directly, without another call. Without one, the function never stops.
- **Recursive case:** break the problem into smaller pieces, call the function on each piece, and **combine** the answers.

Example: adding up an array.

```
function sumArray(arr) {
  if (arr.length === 0) {
    return 0;
  }
  return arr[0] + sumArray(arr.slice(1));
}
```

- Base case: an empty array adds up to `0`.
- Recursive case: the first number, plus the sum of the rest.

## Tracing it

`sumArray([4, 1, 2])` makes these calls. Each one waits for the call below it, then finishes.

| Call | Returns |
|---|---|
| `sumArray([4, 1, 2])` | `4 + 3` = `7` |
| `sumArray([1, 2])` | `1 + 2` = `3` |
| `sumArray([2])` | `2 + 0` = `2` |
| `sumArray([])` | `0` (base case) |

When you're stuck, trace a small example by hand like this. Write down each call and what it returns.

## More than one recursive call

Some problems branch: one call leads to **several** smaller calls, one for each branch. Draw them as a tree, with each call's children below it. Each call combines what its children return, for example by adding them, counting them, or picking the largest or smallest.

Ask yourself two questions:

1. What should a call return when it has **no** children? That's the base case.
2. Given the answers from all the children, how do I build **this** call's answer?

## Going round in circles

If A leads to B, B leads to C, and C leads back to A, then following the links never ends. A recursive function that follows them will keep calling itself until JavaScript stops it with a `RangeError` ("too much recursion" or "Maximum call stack size exceeded").

To spot a cycle, keep track of where you are on the current path. If you arrive somewhere that's already on the path, you've gone round a circle.

## Tools in JavaScript

**Objects as dictionaries**

| Code | Result |
|---|---|
| `const g = { a: ["b", "c"], b: [] };` | |
| `g["a"]` | `["b", "c"]` |
| `g["b"].length` | `0` |
| `g["z"]` | `undefined` (no such key) |
| `"z" in g` | `false` |

**Sets**

| Code | What it does |
|---|---|
| `const s = new Set();` | an empty set |
| `s.add("a")` | adds `"a"` (adding it twice keeps one copy) |
| `s.has("a")` | `true` if `"a"` is in the set |
| `s.delete("a")` | removes `"a"` |
| `s.size` | how many items |

**Largest value**

| Code | Result |
|---|---|
| `Math.max(3, 7)` | `7` |
| `Math.max(-Infinity, 2)` | `2` |
| `Math.max()` | `-Infinity` |
| `-Infinity + 1` | `-Infinity` |

`-Infinity` is smaller than every number, so `Math.max` never picks it while any real number is there.

**Default parameters**

```
function greet(name, greeting = "Hello") {
  return greeting + ", " + name;
}
greet("Sam");          // "Hello, Sam"
greet("Sam", "Hi");    // "Hi, Sam"
```

A parameter with `= value` gets that value when the caller leaves it out. If the default is `new Set()`, a fresh empty set is made each time the caller leaves it out.
