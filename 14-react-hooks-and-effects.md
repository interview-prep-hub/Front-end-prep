# 14. React hooks and effects

Previous: [13. React rendering](13-react-rendering-and-components.md) | Next: [15. Advanced React](15-advanced-react-and-modern-apis.md)

## Choose a hook for its job

| Hook | Purpose | Common misuse |
| --- | --- | --- |
| useState | Local state | Duplicating derivable values |
| useReducer | Explicit state transitions | Mutating existing state or hiding side effects in a reducer |
| useRef | Persistent mutable value / host reference | Expecting a ref write to trigger rendering |
| useEffect | Synchronize with an external system | Running calculations or event-specific actions unnecessarily |
| useLayoutEffect | Layout-dependent work before repaint | Blocking paint for ordinary synchronization |
| useMemo | Cache an expensive calculation where helpful | Depending on it for correctness |
| useCallback | Cache a function identity where helpful | Wrapping every function without a consumer that benefits |
| useId | Accessibility-related stable IDs | Using it as a list key derived from data |

Call ordinary hooks at the top level of React components or custom hooks. Hooks cannot be conditional merely because a branch happens to remain stable today. The `use` API has specific exceptions discussed in 15.

## Effect lifecycle

An effect has setup and, when needed, cleanup. On changed dependencies, React cleans up the previous synchronization before setting up the next. Unmount also cleans up. Dependencies describe reactive values the effect uses and are compared with `Object.is`. They are not a user-chosen schedule that justifies omitting values. Effects run on the client, not during server rendering. See the [useEffect reference](https://react.dev/reference/react/useEffect).

```tsx
import { useEffect, useState } from 'react';

export function ConnectionStatus({ roomId }: { roomId: string }) {
  const [connected, setConnected] = useState(false);
  useEffect(() => {
    setConnected(false);
    const socket = new WebSocket(`/rooms/${encodeURIComponent(roomId)}`,
      'resident-events');
    const onOpen = () => setConnected(true);
    const onClose = () => setConnected(false);
    socket.addEventListener('open', onOpen);
    socket.addEventListener('close', onClose);
    return () => {
      socket.removeEventListener('open', onOpen);
      socket.removeEventListener('close', onClose);
      socket.close();
    };
  }, [roomId]);
  return <p>{connected ? 'Connected' : 'Disconnected'}</p>;
}
```

The example illustrates ownership and teardown. A production socket needs a valid deployment URL, supported protocol, authentication, error handling, reconnect policy, and protection from late events after switching rooms. Do not copy it as a complete socket client.

Development Strict Mode deliberately stresses purity and cleanup with extra checks, including an extra effect setup/cleanup cycle when configured at the root. It is not evidence that production will duplicate every effect. A ref flag that suppresses setup can conceal broken cleanup rather than fix it.

## Removing unnecessary effects

Calculate filtered lists during render. Perform a submit action in its event or form action. Initialize predictable state with a lazy initializer where useful. Reset a component intentionally through identity when appropriate. If you need to notify an external service, define ownership and idempotency rather than assuming an empty dependency list means exactly once forever.

A timer that reads old state can often use a functional update. A ref can hold a latest value for an appropriate integration, but must not conceal a reactive dependency. Current React also provides Effect Events for nonreactive reads inside effect logic; use them only within their documented calling boundaries, not as a general dependency escape hatch. See [useEffectEvent](https://react.dev/reference/react/useEffectEvent).

Custom hooks share behavior and conventions, not one implicit shared state instance. Two calls to `useCounter` ordinarily create two independent counters. Share actual state through a common owner or store if needed.

## Practice and questions

Fix these bugs: interval stuck on 1; duplicate subscription after remount; effect looping because it depends on a fresh object; filtered list synchronized through an effect; search response after selection changed. Explain the appropriate fix for each before writing code.

**useRef or useState?** Does changing the value need a render? **Why not an async effect callback?** The callback must provide cleanup or nothing, not a promise; start async work inside it and return synchronous cleanup. **Why did cleanup run without unmount?** Dependencies may have changed, or development checks may be exercising cleanup.

Exit gate: implement a subscription and a cancellable async operation with cleanup, then remove two unnecessary effects.
