# 01. Resume and interview scope

Previous: [00. Roadmap](00-study-roadmap.md) | Next: [02. Browser and HTTP](02-web-browser-and-http.md)

## What your resume invites interviewers to ask

Your resume describes a full-stack engineer with 1+ year of experience and a broad stack. Prepare strong early-career to mid-level fundamentals, then use project depth for stretch questions. Do not claim senior ownership simply because a product has many users.

| Resume area | Likely front-end follow-up | Study |
| --- | --- | --- |
| CrystalCommerce and PriceCharting | Inventory freshness, job progress, stale prices, external API failures, large tables | 18, 20, 22, 33, 38 |
| Colochousing web | React inside Rails, page lifecycle, forms, API/session boundaries | 13-23, 31 |
| Marketplace migration | Route parity, SEO, rendering, incremental migration, rollback | 24-30, 31, 35 |
| Multi-tenant TypeScript platform | Tenant-scoped data, query keys, membership checks, server boundaries | 11, 18, 23, 28-30 |
| Resident mobile app | Lists, navigation, deep links, notifications, offline state, EAS releases | 16, 20, 32 |
| Next.js internship | Routing, server/client responsibilities, API contracts, production behavior | 24-27 |
| LMS with subscriptions and quizzes | Role-based UI, entitlement freshness, autosave, form state, payment status | 19-23, 33, 37 |
| Teaching assistant work | Clear explanation, debugging someone else's code, feedback | 06-11, 38 |

The final-year project can support a general problem-solving story, but this curriculum keeps its focus on front-end engineering. Backend technologies matter here when they affect API contracts, authentication, latency, rendering, or migration.

## Evidence inventory

For each project, write: what users needed, what you personally changed, which files/layers you owned, what failed, how you verified the result, and what you would change now. Keep employer/client source private. Use a simplified practice implementation when explaining a pattern publicly.

The resume's store counts, customer counts, resident counts, cities, and application counts describe product scale. They are not automatically traffic handled by your feature, measured performance improvements, or individual achievements. Attribute scale to the platform and give your specific contribution separately.

If two internships overlap in time, explain the arrangement factually if asked. Do not invent an allocation of hours. If a project was team work, say what another engineer owned and where you collaborated.

## Baseline diagnostic, 90 minutes

1. In 10 minutes, explain what happens between entering a URL and interacting with a page.
2. In 15 minutes, create a semantic form with a label, inline error, and keyboard-friendly submit behavior.
3. In 15 minutes, implement deduplication by ID without mutating the input. State how duplicates are handled.
4. In 10 minutes, predict an event-loop example and explain a closure.
5. In 20 minutes, build a React filterable list without copying filtered data into state.
6. In 10 minutes, describe why a late search response can overwrite newer results.
7. In 10 minutes, explain SSR versus Server Components and why tenant access needs server enforcement.

Score each 0-3: 0 cannot start; 1 needs prompts; 2 works but misses an edge case; 3 works and explains boundaries. A score below 2 is a priority review, even if the topic looks familiar.

## Expected depth

For every listed technology, prepare a 30-second definition, a two-minute real example, one bug you can explain, and one tradeoff. For your strongest two projects, prepare a five-minute architecture walk-through. For technologies used briefly, state the limited scope honestly.

Use this answer structure: requirement, chosen approach, alternative, failure mode, verification. Example practice answer: a listing filter belongs in the URL when users need refresh and shareable links; draft text may remain local; requests must be canceled or stale results ignored. Tie it to actual experience only when true.

## Interview questions and answer cues

**Why move toward a front-end role from full-stack work?** Connect actual enjoyment and contributions to UI correctness, interaction, accessibility, or performance. Explain how backend knowledge helps you negotiate contracts and debug a complete request.

**Which technology are you strongest in?** Choose based on work you can demonstrate. A shorter credible answer is better than claiming identical depth across the entire skills list.

**What did you do on a large platform?** Separate platform context, team responsibility, your change, and measured evidence. Unknown numbers should remain unknown.

Exit gate: complete the diagnostic and prepare a factual 60-second introduction plus an evidence outline for two projects.
