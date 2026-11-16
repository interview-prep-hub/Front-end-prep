# 32. React Native and Expo

Previous: [31. Rails and migration](31-react-on-rails-hotwire-and-migration.md) | Next: [33. System design](33-front-end-system-design.md)

## Transfer the React model, then learn the host

Props, state, hooks, composition, and query identity transfer from React web. The host uses native components such as View, Text, Pressable, TextInput, FlatList, and platform modules rather than DOM elements. Web CSS and browser APIs are not universally available. Styling has platform-specific behavior, and native accessibility needs appropriate labels, roles, state, and real-device testing.

Discuss the architecture used by your project's actual React Native/Expo versions. Fabric, TurboModules, and JSI are useful conceptual terms; do not describe every current app as using only the older serialized bridge model. See the [React Native architecture guide](https://reactnative.dev/architecture/landing-page).

## Lists, navigation, and lifecycle

Use a virtualized list for many rows rather than mounting every item in a ScrollView. Stable keys, immutable data, sensible item rendering, image sizing, and measured windowing decisions matter. Fixed-layout optimizations are only correct if item geometry really is known. See [FlatList](https://reactnative.dev/docs/flatlist) and the [performance overview](https://reactnative.dev/docs/performance).

Know screen focus versus component mount, app foreground/background transitions, deep-link handling, safe areas, keyboard avoidance, permissions, and platform differences. A screen can remain mounted while unfocused. Decide when to refresh data, stop subscriptions, or preserve drafts.

Deep links and notification taps are external inputs. Parse destinations, restore navigation safely, and recheck session/tenant permissions before showing a resource. Handle cold start and an already-running app. A push payload should prompt retrieval of current data, not become unquestioned authoritative task/payment state.

## Session and offline behavior

Use an appropriate secure credential mechanism. Expo SecureStore provides platform-backed protected storage with platform-specific persistence/backup/biometric behavior; verify those constraints in the [SecureStore docs](https://docs.expo.dev/versions/latest/sdk/securestore/). Ordinary async key/value storage is useful for preferences but is not a universal secret store.

Mobile networks disconnect, requests finish after backgrounding, and users can act on old data. An offline task queue needs stable operation IDs, pending indicators, retry/idempotency, ordering, and conflict handling. Do not queue or confirm sensitive financial operations without an explicit product/server design.

Cache partitioning and logout cleanup still matter. A resident switching houses or organizations should not see old private drafts or subscriptions simply because the component stayed mounted.

## Expo delivery

Distinguish development builds, production binaries, store submission, and compatible over-the-air updates. EAS Build produces binaries; submission is a separate stage. JavaScript/assets can be updated only within the native runtime compatibility and service/store rules in use. A new native capability commonly needs a new compatible binary; an update cannot simply add missing native code. See [building for stores](https://docs.expo.dev/deploy/build-project/) and [runtime versions](https://docs.expo.dev/eas-update/runtime-versions/).

Know build profiles, environment configuration, signing at a conceptual level, staged release, crash monitoring, and rollback/update compatibility. Keep credentials out of public bundles. Test both iOS and Android relevant flows, including denied permission, background/resume, poor network, and expired sessions.

## Resume practice and questions

Choose one real feature: events, tasks, split bills, rent payments, or notifications. Explain navigation, API contract, state ownership, permission boundary, error/offline behavior, testing, and release path. State whether you owned the build/release, assisted it, or only used the pipeline.

**Why is a list janky?** Measure JS work, row rendering, image cost, and platform behavior in a release build. **Does sharing TypeScript mean sharing all UI?** No; contracts and business rules can share while host interactions differ. **What happens when tapping a notification after logout?** Restore/redirect through auth and revalidate access, rather than opening private cached data.

Exit gate: give a concrete feature walk-through and explain native runtime compatibility during an update.
