# 15. Advanced React and modern APIs

Previous: [14. Hooks](14-react-hooks-and-effects.md) | Next: [16. State architecture](16-state-management-and-architecture.md)

## Concurrency and perceived responsiveness

Concurrent rendering lets React schedule and interrupt render work. It is not parallel execution of your component code on multiple CPU cores. Keep render pure because work can restart or be abandoned.

`useTransition` marks suitable updates as nonurgent and exposes pending state. `startTransition` marks updates without returning that hook state. Keep controlled text-input updates urgent. `useDeferredValue` lets a value used by expensive UI lag behind urgent input; it is not a fixed-delay debounce and does not itself reduce network requests. Check the [transition reference](https://react.dev/reference/react/startTransition) for async-boundary limitations in your installed version.

An expensive uninterrupted JavaScript function still blocks while it runs. Simplify it, split work appropriately, virtualize output, or use a worker if necessary. A transition cannot rescue every CPU bottleneck.

## Suspense and boundaries

```tsx
import { lazy, Suspense } from 'react';
const Reports = lazy(() => import('./Reports'));

export function ReportsPage() {
  return <Suspense fallback={<p role="status">Loading reports...</p>}>
    <Reports />
  </Suspense>;
}
```

The imported module needs the expected component export. Suspense handles supported suspending resources, such as lazy code and framework-integrated data. An arbitrary fetch started in an effect does not automatically activate the nearest Suspense fallback. Place boundaries around meaningful UI units so a slow panel does not unnecessarily replace the entire page.

Error boundaries handle errors from rendering descendants and related React work. They do not generally catch ordinary event-handler exceptions or arbitrary timer/promise errors. Use local request/action error handling as well. Understand how the framework layers route errors and not-found states over React boundaries.

Portals change host DOM placement while preserving the React tree relationship. They help overlays escape a container, but do not automatically implement keyboard focus, dismissal, or accessible naming.

## Modern API recognition

| API / feature | What to explain | Boundary |
| --- | --- | --- |
| useActionState | Form/action result state and pending state | An action is not automatically authorized server code |
| useFormStatus | Status of a containing form | Must be read from the appropriate descendant |
| useOptimistic | Temporary UI while an action is pending | Reconcile success/failure and overlapping actions |
| use | Read a supported resource/context during render | Suspense/error integration and resource lifetime matter |
| Ref as a prop | Modern function-component ref patterns | Older libraries may still use forwardRef |
| Activity | Preserve hidden UI state while managing activity | Hidden effects and cleanup have specific semantics |
| React Compiler | Build-time memoization optimization | Verify configuration, compatibility, and profiling |

`use` may be called in conditions/loops under its documented rules, unlike ordinary hooks. Do not generalize that exception. Use a stable supported promise/resource rather than creating a fresh uncached promise every render. See [use](https://react.dev/reference/react/use).

[useActionState](https://react.dev/reference/react/useActionState) and [useOptimistic](https://react.dev/reference/react/useOptimistic) help manage pending actions and temporary UI; neither replaces runtime validation or a canonical server response. A payment should never become confirmed solely because the client optimistically changed a label.

[Activity](https://react.dev/reference/react/Activity) can preserve UI/state while hidden and clean up effects during inactivity. [React Compiler](https://react.dev/learn/react-compiler) can reduce manual memoization when enabled. Learn these after ordinary rendering and effects; do not assume every workplace project has adopted them.

## Practice and questions

Lazy-load a reports panel and add an independent error boundary. Compare a deferred expensive list with a debounced API request. Explain why they solve different problems. Design optimistic task completion with a visible pending marker and failure recovery.

**Does memo prevent every render?** No, local state/context and comparison details still matter. **Does Suspense fetch data?** No, it coordinates supported pending work. **What changes with the compiler?** Some memoization can be automated; purity, state ownership, and measurement still matter.

Exit gate: explain one scheduling example, place useful boundaries, and identify which modern APIs are supported by the version you are using.
