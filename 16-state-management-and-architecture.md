# 16. State management and application architecture

Previous: [15. Advanced React](15-advanced-react-and-modern-apis.md) | Next: [17. Routing and URL state](17-client-routing-and-url-state.md)

## Classify state before picking a library

| State | Example | Usual owner |
| --- | --- | --- |
| Local interaction | Is a dialog open? | Nearest component |
| Shared client workflow | Unsaved multi-step application | Common owner, reducer, or client store |
| URL state | City, sorting, page, selected detail route | Router/search parameters |
| Server state | Listings, bills, session-derived data | Loader/query cache with server authority |
| Form draft | Edited but unsaved address | Form owner/library |
| Derived state | Remaining task count | Calculate from canonical inputs |

A value can move categories when requirements change. For example, a filter becomes URL state when users need shareable results. Avoid copying server data into a global store just because many components read it.

## Reducers, context, and stores

A reducer makes events and transitions explicit. It is useful when several fields change together or when the transition rules deserve tests. Keep effects outside the reducer.

```ts
type EditorState = { title: string; status: 'editing' | 'saving'; error: string | null };
type EditorAction =
  | { type: 'titleChanged'; title: string }
  | { type: 'saveStarted' }
  | { type: 'saveFailed'; message: string };

function editorReducer(state: EditorState, action: EditorAction): EditorState {
  switch (action.type) {
    case 'titleChanged': return { ...state, title: action.title, error: null };
    case 'saveStarted': return { ...state, status: 'saving', error: null };
    case 'saveFailed': return { ...state, status: 'editing', error: action.message };
  }
}
```

This is a partial editor model; a real save-success event must decide whether to close, reset, or retain the canonical saved record. A discriminated state machine can enforce stricter transitions when the workflow needs them.

Context distributes a value through the tree. It is not automatically a finely subscribed state store. Provider value identity and update frequency affect consumers. Split unrelated contexts when appropriate, and avoid a root context containing every frequently changing field.

Redux Toolkit provides structured actions/reducers, middleware, tooling, and conventions; see [Redux Essentials](https://redux.js.org/tutorials/essentials/part-1-overview-concepts). Zustand is another store option with a smaller API surface and selector-based consumption. Choose according to team conventions, debugging needs, update patterns, SSR, and complexity. Query libraries handle a different primary concern: remote data lifetime and synchronization. RTK Query is a server-state option in the Redux ecosystem.

External store integrations should provide consistent snapshots and correct subscriptions. React's [useSyncExternalStore](https://react.dev/reference/react/useSyncExternalStore) exists for that boundary; prefer an established integration over a naive subscription effect when appropriate.

## Application boundaries

Group by stable feature responsibility: listings, applications, resident tasks, billing. Separate transport adapters, domain transformations, UI components, and route composition where those distinctions reduce coupling. Avoid a giant generic `utils` folder containing unrelated business rules.

Normalize entities if repeated nested copies create consistency problems, but do not normalize every tiny response by habit. Use selectors for meaningful projections. Persist only deliberate state with schema versioning and user/tenant scope. A client store on the server should be created with the correct request lifetime; a singleton with private state can leak across requests.

## Practice and questions

Draw ownership for a marketplace with shareable filters, saved favorites, an application draft, and a confirmation dialog. Introduce two bugs: URL and local filter disagree; switching organizations shows the previous organization's draft. Fix ownership and scope before adding synchronization effects.

**When does prop drilling justify context?** Consider depth, component API clarity, composition, and whether many layers are merely forwarding unrelated data. **Why not global state everywhere?** It expands coupling/lifetime and makes ownership harder. **Context or Redux?** Compare distribution with a structured store; the choice depends on requirements, not application size alone.

Exit gate: justify where each value lives, how it resets, and how it behaves on refresh, navigation, logout, and tenant switch.
