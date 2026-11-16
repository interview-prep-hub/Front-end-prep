# 38. Resume deep dives and behavioral preparation

Previous: [37. Capstones](37-capstone-projects.md) | Next: [39. Question bank](39-interview-question-bank.md)

## Prepare truthful stories at three lengths

Write a 30-second project summary, a two-minute contribution story, and a five-minute technical walk-through for your strongest projects. Use situation, responsibility, action, result, and reflection. A result can be a verified feature or reduced failure mode without an invented percentage.

A factual introduction can begin: "I am a full-stack software engineer with experience in Rails, React, TypeScript, and React Native. My work includes API integrations, resident-app features, and contributions to a multi-tenant platform migration. I am strengthening my front-end depth in browser behavior, React architecture, and production delivery." Adapt this to your actual target role and current experience; do not present preparation topics as work already performed.

## CrystalCommerce / PriceCharting prompts

Explain the user need, external API contract, data mapping, background sync flow, and the exact part you implemented. Distinguish importing market prices from synchronizing a sales channel. Be ready for pagination, rate limits, retries, duplicate jobs, stale stock, partial failures, and user-visible sync state.

Front-end follow-up: how would you show last successful sync, a running job, per-item failures, and a retry action? If you did not build that screen, say so and present it as a design proposal. Platform scale gives context; explain your contribution separately from total stores/customers.

## Colochousing web and migration prompts

Draw Rails/ERB, React roots, API services, session/CSRF boundary, background jobs, and the new platform where relevant. Explain a marketplace page's data flow and how route/SEO behavior was preserved. For multi-tenancy, walk through a trusted session, organization membership, record scope, and response/cache handling.

Identify which migration decisions you made versus implemented from a team design. Prepare one schema/contract mismatch or access-control edge case you personally encountered. If no measured performance data exists, discuss correctness and verification rather than claiming speed gains.

## Resident app prompts

Choose one shipped area such as tasks, events, bills, rent, or notifications. Trace navigation, API request, UI state, success/error behavior, and release verification. For notifications, distinguish permission, token registration, delivery, tap handling, and authorized data retrieval.

Explain whether you ran EAS builds, configured profiles, tested releases, or coordinated store submission. Do not collapse every part of a release into "I deployed the app" if your role covered only one stage. Prepare a poor-network/background-resume scenario.

## LMS and internship prompts

Describe dashboard roles, quiz state, entitlement checks, subscription-status updates, and certificate access. For Stripe-related work, distinguish a UI redirect from server-confirmed payment/subscription state. Discuss backend/API work in the context of how the front-end consumed and recovered from it.

For Next.js/MongoDB work, explain the actual router/data pattern you used at the time and how current App Router patterns differ if relevant. For teaching-assistant work, prepare a story about explaining a concept or helping someone debug without taking over their work.

## Behavioral story inventory

| Prompt | Evidence to prepare |
| --- | --- |
| A difficult bug | Reproduction, hypothesis, diagnostic evidence, fix, regression check |
| A disagreement | Shared goal, alternatives, evidence, decision, respectful follow-through |
| An unfamiliar technology | Small learning slice, official references, feedback, delivered result |
| A mistake | Ownership, impact, correction, preventive change |
| A tight deadline | Explicit scope, communication, risk-based prioritization, outcome |
| Collaboration | Your role, handoffs, contract decisions, review feedback |
| A weak area | Specific gap and evidence of improvement, without a disguised strength |
| An ambiguous requirement | Questions asked, assumption recorded, prototype/feedback, revision |

Use real examples. If an interviewer asks about a situation you have not experienced, acknowledge that and explain how you would approach it. Never invent an outage, team-lead role, or metric to fill a story slot.

## Questions for the interviewer

Ask about the first feature you would own, code-review/testing expectations, frontend/backend contract ownership, supported devices, accessibility, performance measurement, release processes, and mentorship. Choose questions that help you evaluate the work rather than reciting a long prepared list.

Exit gate: deliver a 60-second introduction, three two-minute stories, and one five-minute architecture explanation with clear ownership and evidence.
