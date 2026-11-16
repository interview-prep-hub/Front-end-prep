# 35. Git, CI, deployment, and observability

Previous: [34. Component design](34-component-and-design-system-interviews.md) | Next: [36. Machine coding](36-machine-coding-rounds.md)

## Git and review

Know working tree, index, commits, branches, remotes, merge, rebase, cherry-pick, revert, reset, conflict resolution, and bisect conceptually. Revert creates a new inverse commit; reset moves a reference and can affect local work depending on mode. A shared-branch history rewrite needs coordination.

In a practice Git repository, inspect status and create a feature branch before committing. This study folder does not need Git initialization just to use these notes.

```powershell
git status --short --branch
git switch -c feature/listing-filters
git diff
git diff --staged
```

Keep commits focused. Use an imperative short subject and a body explaining why. Run hooks and required checks. Do not commit directly to main, bypass hooks, or force-push without agreement. In review, explain user-visible behavior, relevant tradeoffs, validation, and material limitations.

## CI design

A typical pull-request pipeline installs from a lockfile, lints, type-checks, runs meaningful tests, builds, and performs targeted browser checks. Cache dependencies with keys that include the lockfile/runtime requirements; do not use stale cache artifacts as proof of a successful current build.

Use separate jobs only where parallel execution and clear dependencies help. A framework build may not run lint or all tests, so include those explicitly. Keep secrets unavailable to untrusted code paths. Use permissions and deployment environments appropriate to the workflow. The [GitHub Actions syntax reference](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax) documents triggers, jobs, permissions, and environments.

Do not memorize one YAML file as universal. Explain the stages and failure policy, then adapt commands to scripts that actually exist. The checks for a static Vite app and a full-stack server-rendered app overlap but are not identical.

## Deployment and rollback

Build reproducible artifacts, separate public build-time config from private runtime config, and know which changes need rebuild versus restart. Static assets can use content hashes and long cache lifetimes. HTML/runtime output must receive a policy that does not trap clients on broken/stale references.

Plan deep-link routing, base paths, API reverse proxy/CORS, cookies, headers, and health checks. Keep compatible older chunks during rollout where needed. Rolling back application code may not roll back a database or API contract safely, so maintain compatibility through staged changes.

Feature flags support controlled exposure and rollback, but stale flags become maintenance work. Decide who owns the flag, how it is measured, and when it will be removed. An unexposed feature still requires server authorization.

## Observe the experience

Capture useful errors with release identifiers, route/action context, and request correlation where available. Separate user-facing messages from diagnostic detail. Protect tokens and private form data from logs, analytics, screenshots, and session replay.

Track meaningful metrics: request failures, failed submissions, interaction latency, web vitals, abandoned workflows, job lag, and release regressions. A frontend error can originate in UI code, an API contract, the network, a stale asset, or an upstream service. Trace across layers rather than assigning blame from the status code alone.

Source maps can improve debugging; control publication/access according to the deployment model. Test what users actually receive, including CSP, CDN headers, redirects, and production error boundaries.

## Incident exercise and questions

A release causes some users to see a blank page on lazy navigation. Check release IDs, missing chunk requests, HTML/cache versions, base paths, and deployment asset retention. Mitigate with a verified rollout/rollback and capture a regression scenario. Do not immediately clear every cache without knowing which layer is wrong.

**Merge or rebase?** Explain history shape and team policy, avoiding unexpected shared-history rewrites. **Why did CI pass but production fail?** Compare env/runtime, secrets, real origins, stale assets, data shape, and missing production-flow checks. **What belongs in a PR?** Problem, resulting behavior, reviewable scope, and relevant validation.

Exit gate: explain a change from branch to monitored deployment, including how to diagnose and safely undo a bad release.
