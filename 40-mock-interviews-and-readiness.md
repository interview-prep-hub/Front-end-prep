# 40. Mock interviews and readiness

Previous: [39. Question bank](39-interview-question-bank.md) | Next: [41. Final revision](41-final-revision-cheatsheet.md)

## Run three different rounds

Run them on separate sessions first, then as one interview loop with short breaks. A friend can interview you, or you can record a timed self-review. Do not read the answer cues while speaking.

### Round A: fundamentals and debugging, 45 minutes

Use 10 minutes for browser/HTML/CSS, 10 for JavaScript/TypeScript, 15 for React state/effects, and 10 for one debugging scenario. Draw or write code where an explanation is ambiguous.

Suggested prompts: explain a browser request; fix a flex overflow; predict promise/timer order; model a discriminated union; explain index-key bugs; fix an interval stale closure; diagnose a late search response. Follow up with a changed requirement after the first answer.

### Round B: machine coding, 60 minutes

Choose a prompt from 36 without looking at your previous solution. Clarify the acceptance criteria, build a working core, cover an unhappy path, and test keyboard behavior. Use a fixture/API simulator with controllable delays and failure.

At minute 35, introduce one change: the user switches organizations, requests finish out of order, an item disappears, or a mutation fails. Assess whether your state model can absorb the change without many unrelated effects.

### Round C: framework, design, and resume, 60 minutes

Spend 15 minutes comparing Vite, Next.js, and Start for a concrete product, 25 on one system-design prompt, and 20 on your actual experience. Trace a single request with auth, tenant scope, cache, and failure behavior. Separate work you did from a proposed architecture.

Suggested follow-ups: which code runs in the browser; what gets cached and for whom; what happens after mutation; how to deny a direct different-tenant call; how to roll back a route migration; what evidence supports your project result.

## Score each dimension from 0 to 4

| Dimension | What a strong answer demonstrates |
| --- | --- |
| Requirements | Clarifies input, user behavior, and constraints |
| Fundamentals | Correct JavaScript/browser/React mental models |
| Implementation | Working core with clear data and component boundaries |
| State and async | Ownership, identity, cancellation, and consistency |
| Accessibility | Native semantics, keyboard/focus, useful error/progress states |
| Security | Trusted server validation, auth, tenant/resource permissions |
| Verification | Appropriate cases, evidence, and actual failure reproduction |
| Communication | Clear reasoning, scope honesty, and tradeoff explanation |

0 means unable to proceed; 1 needs substantial prompts; 2 completes a basic answer but misses important boundaries; 3 works independently with relevant edge cases; 4 handles follow-ups clearly and justifies alternatives.

A useful self-assessment gate is at least 24/32 twice on different prompts, with every dimension at least 2. This is a study heuristic, not a prediction of hiring outcomes. Correct any critical misconception even if the overall score passes. Treat client-only authorization, exposed secrets, unhandled tenant leakage, or unsupported claims of personal ownership as issues to fix before the next mock.

## Readiness evidence

- Implement a filterable React feature without a copied tutorial.
- Diagnose a stale closure, unstable key, async race, and direct-link hosting failure.
- Explain TypeScript's runtime limit and validate an external value.
- Demonstrate meaningful tests and a measured performance investigation.
- Distinguish SSR/RSC/hydration and framework cache models accurately.
- Enforce a resource/tenant boundary in a server implementation or clearly explain a tested design.
- Finish one capstone flow and explain a rejected alternative.
- Deliver factual resume stories with your own contribution and verified result.

## Review and repeat

After each mock, write the three highest-impact corrections, not a long unprioritized list. Assign each to a chapter and an exercise. Repeat weak behavior within two days, then use a new prompt a week later. Repeating the identical memorized solution does not prove transfer.

The day before an interview, use 41, review your project evidence, check the interview environment, and stop adding major new libraries. If the role is mobile-heavy or framework-specific, redistribute the final revision to match the actual job description.

Exit gate: complete the three rounds, record evidence and scores, and show improvement on a changed follow-up rather than the same memorized question.
