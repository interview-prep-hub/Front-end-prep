# 10. Data structures and JavaScript coding

Previous: [09. Browser APIs](09-dom-and-browser-apis.md) | Next: [11. TypeScript](11-typescript-for-front-end.md)

## What a coding answer should include

Clarify input shape, duplicates, ordering, invalid input, mutation, and output requirements. Walk through an example, implement the simplest suitable approach, test edge cases, and state time/space complexity. Do not optimize against an unstated constraint.

Know arrays, objects, Map, Set, stack, queue, linked lists conceptually, trees, graphs, heaps conceptually, and tries as a stretch. Practice hash lookup, two pointers, sliding window, sorting, binary search, recursion/iteration, BFS, and DFS. Front-end rounds often use these inside nested menus, filters, grouping, autocomplete, and pagination.

## Worked example: merge inventory records

Contract: preserve first-seen ID order but retain the last record for each ID. Records have valid string IDs. Return a new array; do not mutate records. Map lookup is typically expected constant time in interview analysis, with implementation-dependent details.

```js
function dedupeById(records) {
  const byId = new Map();
  for (const record of records) byId.set(record.id, record);
  return [...byId.values()];
}
console.log(dedupeById([
  { id: 'a', price: 1 }, { id: 'b', price: 2 }, { id: 'a', price: 3 },
]));
// [{ id: 'a', price: 3 }, { id: 'b', price: 2 }]
```

Expected O(n) time and O(k) extra space for k distinct IDs under usual Map assumptions. Map replacement does not move a key to the end. If the requirement says last-occurrence ordering, this implementation needs a different policy.

## Worked example: breadth-first menu search

```js
function findNode(roots, targetId) {
  const queue = [...roots];
  const seen = new Set();
  for (let head = 0; head < queue.length; head += 1) {
    const node = queue[head];
    if (seen.has(node)) continue;
    seen.add(node);
    if (node.id === targetId) return node;
    for (const child of node.children ?? []) queue.push(child);
  }
  return undefined;
}
```

Using a head index avoids repeatedly removing the first array element. The set protects against cycles by object identity. Under a graph model, traversal is O(V + E); queue and visited storage can be O(V). On a tree, discuss maximum breadth for the queue and the extra visited set. Do not claim recursive DFS uses constant space.

## Practice progression and expected approaches

| Problem | Approach | Cases to demonstrate |
| --- | --- | --- |
| Frequency count / first unique item | Map counts, then ordered scan | Empty input, no unique value |
| Two-sum | Map previously seen complements | Duplicate values and distinct indices |
| Balanced brackets | Stack | Misordered closing bracket, empty string |
| Longest substring without repeats | Sliding window and last-seen positions | Repeated input, Unicode interpretation |
| Merge sorted pages | Two pointers | Equal keys, empty page, stable order |
| Find first eligible price | Binary search on a monotonic predicate | None/all match, boundary indices |
| Flatten nested categories | DFS or explicit stack | Very deep input, cycles if allowed |
| Group listings by city | Map or deliberate record shape | Missing city and output ordering |
| Memoize a pure computation | Explicit cache key policy | Objects, invalidation, unbounded growth |
| Limit concurrent requests | Worker pool / active count | Rejection, empty queue, cancellation |

Practice debounce/throttle and autocomplete in 36. Promise combinators, `bind`, and array-method reimplementations are useful interview drills, but state when your educational version omits full language-spec behavior.

## Complexity and correctness traps

Sorting generally dominates a linear scan with O(n log n) comparisons in standard interview analysis, but language/library algorithm details matter. Nested loops are not automatically O(n squared) if each pointer only advances n times. Repeated array spreading inside an accumulator can introduce avoidable copying. Recursion can overflow on deeply nested user input.

String indexing measures UTF-16 code units. `for...of` iterates code points, not complete user-perceived grapheme clusters. Clarify what character means before implementing a Unicode-sensitive requirement.

Exit gate: solve six different patterns, narrate their invariants, and produce tests for empty, normal, duplicate, and boundary inputs.
