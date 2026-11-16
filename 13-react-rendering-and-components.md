# 13. React rendering and components

Previous: [12. Vite and tooling](12-tooling-and-react-with-vite.md) | Next: [14. Hooks and effects](14-react-hooks-and-effects.md)

## A useful mental model

A component describes UI for particular props, state, and context. Rendering calls components to calculate a tree; committing applies changes to the host environment. A render is not automatically a DOM mutation. React can perform rendering work that it later abandons, so render must stay pure: no subscriptions, random IDs that affect initial output, DOM writes, or mutations of existing external objects.

Props are inputs. State is retained component memory. Event handlers observe values from the render that created them. Setting state queues work rather than changing that handler's current variables. The [React quick start](https://react.dev/learn) introduces component composition, events, lists, and shared state.

```tsx
import { useState } from 'react';

type Task = { id: string; title: string; done: boolean };
export function Tasks({ initialTasks }: { initialTasks: Task[] }) {
  const [tasks, setTasks] = useState(initialTasks);
  const remaining = tasks.filter(task => !task.done).length;

  function toggle(id: string) {
    setTasks(previous => previous.map(task =>
      task.id === id ? { ...task, done: !task.done } : task,
    ));
  }

  return <section aria-label="Resident tasks">
    <p>{remaining} remaining</p>
    <ul>{tasks.map(task => <li key={task.id}>
      <label>
        <input type="checkbox" checked={task.done}
          onChange={() => toggle(task.id)} />
        {task.title}
      </label>
    </li>)}</ul>
  </section>;
}
```

`initialTasks` initializes local state once; later prop changes do not automatically replace that state. Decide whether the parent owns the tasks, whether this is an editable draft, or whether changing a record should reset the component. Do not add a prop-to-state effect without deciding that ownership first.

## Identity, reconciliation, and keys

React uses element type, tree position, and keys to match retained component identity. Keys are unique among siblings and should be stable for the same logical item. They are not ordinary props passed through to a child. An array index can be acceptable for a truly fixed list, but editable/reordered lists can attach state to the wrong item when indexed.

Changing a key intentionally can reset a form for a different record. Generating a fresh random key on every render instead remounts children, losing state and focus. Defining a component function inside another component can also create unstable type identity.

State updates often batch. Three calls to `setCount(count + 1)` in one handler use the same captured count; functional updates compose based on pending previous state. Updaters and reducers must remain pure.

## Composition and rendering details

Use children and composition to keep reusable components focused. Lift shared state to the nearest appropriate owner. Avoid a universal component with many unrelated boolean props when distinct components or a discriminated API express the cases better.

Know controlled versus uncontrolled inputs, conditional rendering, fragments, JSX expression rules, and React's escaping of ordinary text. `0 && <Message />` renders zero; use a boolean condition when that is intended. Passing an event handler differs from calling it while rendering.

Context changes and local state changes can trigger rendering even when parent props look unchanged. Memoization is an optimization, not a correctness mechanism. Treat React's internal reconciliation details as deeper study after you can debug visible behavior.

## Practice and questions

Create an editable task list. Reorder it while a child input has focus, then compare stable IDs with index keys. Add two updates in one event and explain the result. Reset a task editor intentionally when the selected record changes.

**Does every parent render update every child DOM node?** No; separate render work, memoization, reconciliation, and commit. **Why avoid copying derived values into state?** It adds a second source of truth that must be synchronized. **Why immutable updates?** They preserve predictable snapshots and make change detection meaningful.

Exit gate: explain state snapshots and identity, fix a key bug, and implement a derived value without an effect.
