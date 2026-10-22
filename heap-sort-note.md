# Heap Sort — study note

## 1. What a max-heap is

A **max-heap** is a binary tree where **every parent is greater than or equal to its children**. So the largest value is always at the root.

We don't build tree nodes. We store the tree in the array itself, level by level:

```
index:   0    1    2    3    4    5
value:  13   11   12    5    6    7

              13            ← index 0
            /    \
          11      12        ← indices 1, 2
         /  \    /
        5    6  7           ← indices 3, 4, 5
```

For the element at index `i`:

- left child is at `2i + 1`
- right child is at `2i + 2`
- a child only exists if its index is **less than `n`**, the size of the heap

## 2. bubbleDown — fixing one node

`bubbleDown(arr, n, i)` handles a node at index `i` that may be smaller than its children, assuming everything below it is already a heap.

1. Look at the node and its children that exist (index < `n`).
2. Find the largest of those (up to three) values.
3. If the node itself is the largest, stop — it's in the right place.
4. Otherwise, swap it with its larger child, move to that child's index, and repeat from step 1.

The value keeps sinking until it is at least as large as both its children, or it reaches the bottom.

## 3. buildMaxHeap — turning any array into a heap

Call `bubbleDown` on **every node that has a child**, starting from the last one and moving backwards to index 0.

- The last node with a child is at index `n/2 − 1`. Everything after it is a leaf.
- A leaf on its own is already a heap, so there's nothing to fix there.
- Going backwards matters: when you fix a node, the subtrees below it have already been fixed — which is exactly what `bubbleDown` assumes.

**Example** — the array from the starter code, `[12, 11, 13, 5, 6, 7]`, with `n = 6`:

| Step | bubbleDown from | What happens | Array |
|---|---|---|---|
| start | | | `[12, 11, 13, 5, 6, 7]` |
| 1 | index 2 (13) | 13 > its child 7 — no swap | `[12, 11, 13, 5, 6, 7]` |
| 2 | index 1 (11) | 11 > its children 5, 6 — no swap | `[12, 11, 13, 5, 6, 7]` |
| 3 | index 0 (12) | larger child is 13 — swap, then 12 > 7, stop | `[13, 11, 12, 5, 6, 7]` |

Now it's a max-heap: the largest value, 13, is at index 0.

## 4. sort — using the heap to sort

After `buildMaxHeap`, the largest value is at index 0. Then repeat:

1. **Swap** index 0 with the **last element of the heap**. The largest value is now in its final sorted place at the end.
2. **Shrink the heap by one**, so that value is never touched again.
3. **bubbleDown from index 0** with the new, smaller size, to restore the heap.

Stop when the heap has one element left.

| Heap part (before `\|`) | Sorted part | What just happened |
|---|---|---|
| `[13, 11, 12, 5, 6, 7]` | | max-heap built |
| `[12, 11, 7, 5, 6]` | `[13]` | 13 moved to the end; 7 sank below 12 |
| `[11, 6, 7, 5]` | `[12, 13]` | 12 moved to the end; 6 sank below 11 |
| `[7, 6, 5]` | `[11, 12, 13]` | 11 moved to the end; 5 sank below 7 |
| `[6, 5]` | `[7, 11, 12, 13]` | 7 moved to the end; 5 sank below 6 |
| `[5]` | `[6, 7, 11, 12, 13]` | 6 moved to the end — done |

Result: `[5, 6, 7, 11, 12, 13]`.

Everything happens by swapping inside the one array — no second array — so heap sort is **in-place**. It runs in **O(n log n)** time: each `bubbleDown` moves down at most the height of the tree, which is about log n.

## 5. Common mistakes

- **Forgetting to shrink the heap** in `sort`. If `bubbleDown` still uses the full array length, it pulls already-sorted values back into the heap.
- **Checking a child without checking it exists** — always test `index < n` before reading `arr[index]`.
- **Swapping with the wrong child** — swap with the *larger* child, or the heap property breaks.
- **Running buildMaxHeap forwards** (from 0 up). It has to go backwards from `n/2 − 1`.
