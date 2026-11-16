# 11. TypeScript for front-end engineering

Previous: [10. Coding patterns](10-data-structures-and-javascript-coding.md) | Next: [12. Vite and tooling](12-tooling-and-react-with-vite.md)

## Compile-time model

TypeScript checks code before execution; types are generally erased. It does not validate a server response at runtime. Prefer `unknown` at an untrusted boundary, validate it, and then use the narrowed value. `any` allows unchecked operations and spreads missing guarantees through a program. A type assertion describes an assumption, not a check.

Know primitives, object types, interfaces, optional/readonly fields, unions, intersections, tuples, literal types, inference, nullability, and structural compatibility. Use strict checking. Interfaces support declaration merging; type aliases can express unions and other compositions. Choose by the shape and conventions, not a universal rule.

## Model valid states

```ts
type LoadState<T> =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success'; data: T }
  | { status: 'error'; message: string };

function describe<T>(state: LoadState<T>): string {
  switch (state.status) {
    case 'idle': return 'Ready';
    case 'loading': return 'Loading';
    case 'success': return 'Loaded';
    case 'error': return state.message;
    default: {
      const impossible: never = state;
      return impossible;
    }
  }
}
```

This prevents reading `data` before success and makes new cases visible to the compiler. Discriminant checks and other guards narrow types; see the [TypeScript narrowing handbook](https://www.typescriptlang.org/docs/handbook/2/narrowing.html).

For background refresh, expand the model deliberately instead of insisting that loading and available data can never coexist. A type should match the domain, not merely demonstrate a language feature.

## Generics and reusable constraints

```ts
function indexBy<T, K extends PropertyKey>(
  values: readonly T[],
  getKey: (value: T) => K,
): Map<K, T> {
  return new Map(values.map(value => [getKey(value), value]));
}
type House = { id: string; title: string; monthlyRent: number };
const labels = { id: 'ID', title: 'Title', monthlyRent: 'Monthly rent' }
  satisfies Record<keyof House, string>;
```

Generics preserve relationships between inputs and outputs. They are more useful than replacing every type with a broad union. Understand `keyof`, indexed access, `typeof` in type positions, and `T extends U ? X : Y` conceptually. Practice mapped types, `Pick`, `Omit`, `Partial`, `Required`, `Readonly`, `Record`, `ReturnType`, and `Awaited`. Do not build a type puzzle when a named domain type is clearer.

`as const` preserves narrow literal types and readonly aspects of an expression. `satisfies` checks compatibility while retaining useful inferred types. Neither freezes a runtime object. `readonly` is a compile-time constraint, not deep runtime immutability.

## React and external boundaries

Type component props, events, nullable refs, reducer actions, and custom-hook return values. Prefer a discriminated union when props have distinct valid modes. For a callback, express the event/value the caller actually needs. Native element props can be derived with React utility types rather than recreated manually.

Parse API JSON with a schema or explicit guards, then map transport fields into UI/domain types. Handle null versus missing fields, dates represented as strings, and IDs represented as strings versus numbers. Use `catch (error)` narrowing before reading an error message.

Avoid `!` assertions on refs or session data unless a real invariant proves presence. Enable additional checks such as unchecked-index access when appropriate for the project and understand the errors they reveal.

## Practice and questions

Model a payment as pending, confirmed, or failed; ensure confirmed has a receipt and failed has a reason. Type a generic table column definition keyed to its row shape. Validate an unknown housing response before rendering it.

**Does shared server/client typing secure an API?** No; runtime inputs and authorization still need checks. **Union or intersection?** A union accepts alternatives; an intersection combines requirements. **Why use never?** It represents an impossible value and supports exhaustiveness checks.

Exit gate: implement a discriminated union, a useful generic, and a runtime boundary without `any` or unproven assertions.
