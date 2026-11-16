# 06. JavaScript foundations

Previous: [05. Responsive styling](05-responsive-css-and-ui-styling.md) | Next: [07. Functions, objects, memory](07-javascript-functions-objects-and-memory.md)

## Values, variables, and scope

JavaScript has primitive values, including string, number, bigint, boolean, undefined, symbol, and null, plus objects. Arrays and functions are objects. `typeof null` is a historical exception that returns `'object'`; `Array.isArray` identifies arrays more precisely.

`let` and `const` are block scoped. Their bindings exist before their declarations are evaluated but cannot be accessed during the temporal dead zone. `var` is function scoped and initialized to undefined before the declaration runs. Function declarations are generally callable before their declaration is reached in their scope. Prefer an accurate explanation of declaration behavior over saying every declaration is simply moved to the top.

`const` protects a binding from reassignment; it does not freeze an object. Object values are accessed through references. A function receives a value; for an object, that value is a reference, so mutation is visible through other references, but reassigning the parameter does not reassign the caller's variable.

```js
const original = { title: 'House', tags: ['quiet'] };
const copy = { ...original };
copy.title = 'Apartment';
copy.tags.push('central');
console.log(original.title); // House
console.log(original.tags.length); // 2: the nested array is shared
```

## Coercion and equality

Use strict equality for ordinary comparisons, but understand the other operations. `Object.is(NaN, NaN)` is true while `NaN === NaN` is false. `Object.is(0, -0)` is false while strict equality treats them as equal. Objects compare by identity, not by equal-looking contents.

Falsy values include false, zero, negative zero, empty string, null, undefined, NaN, and `0n`. An empty array or object is truthy. `||` replaces any falsy value; `??` replaces only null and undefined. Optional chaining stops on nullish values, not every falsy value.

```js
const quantity = 0;
console.log(quantity || 10); // 10
console.log(quantity ?? 10); // 0
console.log([] === []); // false
console.log('5' + 1); // '51'
console.log('5' - 1); // 4
```

Explicitly parse external values. `Number('')` yields zero; that may be wrong for an empty form field. Financial calculations require an agreed representation and rounding policy. Binary floating point cannot represent every decimal fraction exactly; use integer minor units or an appropriate decimal approach for money.

## Everyday language skills

Know destructuring defaults, rest/spread, template strings, optional chaining, nullish coalescing, default parameters, and short-circuit evaluation. Defaults apply to undefined, not null. `map` transforms, `filter` selects, `find` returns one match, `some`/`every` answer predicates, and `reduce` accumulates. Choose clarity over forcing every operation into `reduce`.

`sort` mutates and needs a numeric comparator for numeric ordering. `toSorted` provides a copying alternative where supported. Spread and `slice` are shallow. `for...of` iterates values of an iterable; `for...in` enumerates enumerable string keys, including inherited ones, so it is not the usual array iteration tool.

Understand `try`/`catch`/`finally`, throwing Error objects, and preserving meaningful error context. A `return` in `finally` can override a return or thrown error, so avoid it in ordinary control flow.

## Practice

Given inventory entries, return a new list containing available items sorted by price. Do not mutate input. Define behavior for missing prices and duplicate IDs. Explain time complexity and which nested objects remain shared.

## Questions and answer cues

**Is JavaScript pass-by-reference?** It passes values; objects are represented by reference values. Demonstrate mutation versus parameter reassignment.

**What is hoisting?** Explain binding creation, initialization, and the temporal dead zone separately.

**Why can a React update fail after mutation?** Mutation preserves identity and violates state assumptions; explain the framework-specific comparison in chapter 13.

Exit gate: predict the examples, implement immutable list operations, and explain coercion without relying on memorized trivia alone.
