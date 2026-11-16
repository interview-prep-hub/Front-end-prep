# 21. Front-end testing

Previous: [20. APIs](20-api-contracts-and-real-time-ui.md) | Next: [22. Performance](22-performance-and-debugging.md)

## Test behavior at the cheapest meaningful layer

Use unit tests for pure transformations and state-transition rules. Use component/integration tests for user interaction, forms, and async states. Use browser end-to-end tests for important routes, actual focus/navigation behavior, authentication integration, and critical workflows. Contract tests catch API drift; visual regression catches appearance changes. Each layer has blind spots.

Prefer a few tests for meaningful risk over many tests that repeat implementation. Coverage percentage is a signal about executed code, not proof of correctness. A snapshot of a huge tree often says little about why the feature matters.

## Interaction test example

Assumptions: Vitest, a DOM test environment, jest-dom matchers registered in setup, Testing Library, user-event, and an application `ApplicationForm` component. The component takes an async `onSubmit` prop and displays a success message after it resolves.

```tsx
import { expect, it, vi } from 'vitest';
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { ApplicationForm } from './ApplicationForm';

it('submits the entered email and confirms success', async () => {
  const user = userEvent.setup();
  const submit = vi.fn().mockResolvedValue({ ok: true });
  render(<ApplicationForm onSubmit={submit} />);

  await user.type(screen.getByRole('textbox', { name: /email/i }),
    'resident@example.test');
  await user.click(screen.getByRole('button', { name: /apply/i }));

  expect(await screen.findByText(/application received/i)).toBeVisible();
  expect(submit).toHaveBeenCalledWith({ email: 'resident@example.test' });
});
```

Select by role and accessible name when possible; use labels for form controls. `getBy` fails when a required element is absent, `queryBy` supports absence checks, and `findBy` waits for an async appearance. See [Testing Library queries](https://testing-library.com/docs/queries/about/). Avoid testing internal state setters instead of the user's observable result.

## Async and network testing

Mock at a boundary that preserves the behavior under test. A network-level mock can exercise request construction, parsing, loading, and error UI together. Test empty success, error response, delayed response, cancellation/races, and a failed mutation. Do not mock the query hook itself when the question is whether the feature handles its query lifecycle correctly.

Use fake timers for actual timer behavior, restoring them afterward. Understand microtasks and user-event timer integration so a fake-clock test does not hang. Avoid arbitrary sleeps. In a DOM emulator, layout, browser navigation, and many platform APIs are incomplete; use a real browser when those behaviors matter.

For TanStack Query tests, create an isolated client per test and configure retries deliberately. Shared caches create order-dependent tests. For server authorization, test direct requests as well as hidden buttons.

## Browser tests

Use stable locators and web-first assertions. Isolate user data, avoid dependence on external third-party availability, and retain traces/screenshots on failure. [Playwright best practices](https://playwright.dev/docs/best-practices) explains these patterns. Verify a deep-link reload, keyboard form flow, login/logout, and cross-tenant denial in a controlled environment.

Component test setup and runner behavior vary; [Vitest's guide](https://vitest.dev/guide/) is the setup reference. Do not assume an example config from a different major is correct.

## Practice and questions

Write a reducer test for an invalid transition, an interaction test for a field error followed by correction, and a browser test for a direct detail URL. Introduce a race and show that the relevant test fails before fixing it.

**What should be mocked?** Uncontrolled external dependencies, while preserving the behavior being assessed. **Why is a test flaky?** Investigate shared state, uncontrolled time/network, nondeterministic data, and missing awaited behavior. **Why test keyboard behavior?** It is part of the feature's interaction contract, not a cosmetic extra.

Exit gate: explain why each test belongs at its layer and demonstrate that a real failure is caught.
