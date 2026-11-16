# 07. Functions, objects, modules, and memory

Previous: [06. JavaScript foundations](06-javascript-foundations.md) | Next: [08. Async and event loop](08-javascript-async-and-event-loop.md)

## Closures and lexical scope

A function keeps access to its lexical environment. The captured binding can change over time; a closure does not necessarily freeze a copy of a value. Each factory call can create an independent environment. See [MDN on closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures) for the underlying language model.

```js
function createCounter(start = 0) {
  let value = start;
  return {
    increment() { value += 1; return value; },
    read() { return value; },
  };
}
const a = createCounter();
const b = createCounter(10);
a.increment();
console.log(a.read(), b.read()); // 1, 10
```

Understand why `var` in a loop can make callbacks share one binding, while `let` loop iterations can have distinct bindings. In React, each render creates a new environment, so a callback from an older render may read that render's values. This explains stale closures more precisely than blaming asynchrony alone.

## Invocation and `this`

Regular-function `this` depends on invocation. A method call supplies the receiver; a detached function loses that receiver. In strict mode, an ordinary standalone call receives undefined as `this`. Arrow functions capture lexical `this` and cannot be used as constructors.

```js
const resident = {
  name: 'Amina',
  greet() { return this.name; },
};
const greet = resident.greet.bind(resident);
console.log(greet()); // Amina
```

`call` invokes with explicit `this` and positional arguments; `apply` accepts an array-like argument list; `bind` returns a function with a bound receiver and optionally initial arguments. Do not replace every method with an arrow without understanding how instances and prototypes will work.

## Objects and prototypes

Property lookup follows the prototype chain when an own property is absent. Classes provide syntax over prototype-based behavior, with additional language rules. `Object.hasOwn` checks an own property rather than inherited membership. Know `in`, own keys, enumerability at a practical level, and how getters differ from stored values.

Use a Map for keyed collections when arbitrary key types or collection operations fit. Use a Set for uniqueness. A plain object works for many fixed records, but be deliberate about arbitrary user-provided keys. WeakMap and WeakSet hold eligible objects weakly, which helps associate metadata without forcing their lifetime; they are not enumerable caches.

`structuredClone` handles many structured values, including cycles, but does not clone functions or preserve every custom class behavior. JSON round-tripping loses or rejects various values. A shallow update of a known state shape is often clearer than indiscriminate deep cloning.

## Modules, iterators, and cleanup

ES modules expose explicit imports/exports with live bindings and their own scope. Module evaluation is cached within a module graph, so module-level mutable data can outlive a component and, on a server, may be shared across requests. Circular dependencies can produce initialization surprises. Dynamic `import()` returns a promise and can support code splitting.

An iterable supplies an iterator, which yields values through `next()`. Generators offer a convenient way to produce sequences. Learn enough to explain `for...of`, spread over iterables, and why a plain object is not automatically iterable.

Garbage collection is based on reachability, not whether you personally consider a variable finished. Leaks commonly arise from retained listeners, timers, subscriptions, detached DOM references, and unbounded caches. A closure is useful, not inherently a leak; retaining it too long can retain its environment.

## Practice and questions

Implement an event emitter with `subscribe`, `emit`, and unsubscribe. Define what happens when a listener unsubscribes during an emission. Take a snapshot of listeners if that matches your contract. Demonstrate cleanup after ten mount/unmount cycles.

**Why does a detached method fail?** Invocation changed its receiver. **Why is deep cloning not always a fix?** It may be expensive, alter semantics, and hide poor state ownership. **Can a module singleton leak private SSR data?** Yes, if request-specific data is stored at module scope.

Exit gate: explain a stale closure, an invocation bug, and one memory-retention bug using a small reproduction for each.
