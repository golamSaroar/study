# Study note: strings and sets

This note covers the tools you need for this question. It doesn't solve it for you.

## Splitting a string

`split(sep)` cuts a string wherever `sep` appears and gives back a **list** of the pieces. The separator itself is removed.

| Expression | Result |
|---|---|
| `"red-green-blue".split("-")` | `['red', 'green', 'blue']` |
| `"-green-blue".split("-")` | `['', 'green', 'blue']` |
| `"red".split("-")` | `['red']` |

- If the string starts or ends with the separator, you get an **empty string** `''` in the list.
- The number of pieces is always one more than the number of separators. `len(...)` tells you how many pieces there are.

## Changing and checking strings

| Expression | Result | What it does |
|---|---|---|
| `"HeLLo".lower()` | `'hello'` | every letter in lowercase |
| `"  hi ".strip()` | `'hi'` | spaces removed from both ends |
| `"abc".isalpha()` | `True` | `True` only if every character is a letter |
| `"ab3".isalpha()` | `False` | there's a digit |
| `"".isalpha()` | `False` | an empty string has no letters |
| `"_" in "a_b"` | `True` | is this character in the string? |
| `"a_b_c".count("_")` | `2` | how many times it appears |

## Sets

A **set** holds each value at most once and has no order.

| Expression | Result |
|---|---|
| `set("banana")` | `{'b', 'a', 'n'}` (in some order) |
| `set("")` | `set()` (an empty set) |
| `{1, 2, 3} & {2, 3, 4}` | `{2, 3}`: values in **both** sets |
| `{1, 2, 3} \| {3, 4}` | `{1, 2, 3, 4}`: values in **either** set |
| `{1, 2, 3} - {2}` | `{1, 3}`: in the first but not the second |
| `len({'a', 'b'})` | `2` |

- `set(some_string)` gives you the set of characters in that string.
- `&` can be chained: `a & b & c` keeps only what is in all three.
- `set()` is the empty set. `{}` is an empty **dictionary**, not a set.

## Sorting

`sorted(x)` takes a list, a set or a string and returns a **new list** in ascending order.

| Expression | Result |
|---|---|
| `sorted({'c', 'a', 'b'})` | `['a', 'b', 'c']` |
| `sorted("cab")` | `['a', 'b', 'c']` |
| `sorted(set())` | `[]` |

## Checking the input with `assert`

`assert condition, "message"` stops the function with an `AssertionError` when the condition is `False`. Put your checks at the top of the function, before you use the input, the same way the lab's other tasks do.

## A way to plan any function like this

1. Write down what a valid input looks like, and what makes an input invalid.
2. Break the input into the pieces you need.
3. Decide which tool answers the question (a loop, a set operation, ...).
4. Make sure the output has the exact form asked for (type, order, upper or lower case).
5. Try your function on each example by hand, including the edge cases.
