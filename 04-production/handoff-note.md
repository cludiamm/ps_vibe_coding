# Engineering Handoff Note

> Module 4 · Production Specs. Open the black box, make the build legible to an engineer.

## What this is

_One paragraph an engineer can read in 60 seconds._

FlowCard Admin Prototype is a React + Vite, TypeScript frontend prototype for testing whether a finance admin will keep moving after card-program setup when the console presents a clear activation path. The app starts on an empty Overview, requires a Physical or Virtual card selection, then guides the admin through People & Cards, Spend Controls, and Monitor Card Program. All behavior is simulated in browser memory: invites add local teammates, policy selection unlocks card-activation simulations, first swipes unlock expense details, and the progress card tracks movement toward three of five teammates making a first purchase. There is no production backend, authentication, persistence, real email delivery, card issuing, payment processing, or multi-user synchronization.

## Architecture (plain language)

- **Frontend:** React + Vite with TypeScript. Tailwind CSS and project-specific FlowCard utility classes provide the visual system. src/App.tsx owns the Wouter route boundary. The dashboard is currently served at /. src/pages/dashboard.tsx is the state coordinator. It owns local state, derived progress, guards, and callbacks, then passes data and actions into named screen components. src/components/layout/FlowCardShell.tsx contains shared navigation, header, account context, protected-funds panel, reset control, and notices. Feature folders keep screen code grouped by product surface: src/features/overview/ src/features/people-and-cards/ src/features/spend-controls/ src/features/shared/
- **Backend / data:** The API server currently exposes only a health check and FlowCard does not call it for this experience. A Supabase migration scaffold now exists at supabase/migrations/20260901000000_flowcard_core.sql, but it has not been applied because the Supabase connector was not authorized.  Demo data is exported from feature-local data.ts files. Shared domain types live in src/features/shared/types.ts. React useState holds the current card selection, teammates, policy, modal state, reminders, active screen, and simulated loading state. useMemo derives the first-swipe count. Refreshing the page or using Reset returns the experience to its initial state. The migration models organizations, card programs, cardholders, spend policies, activation events, expenses, and reminders with organization-scoped row-level security.
- **Key flows:** Overview → card selection

The admin opens the card modal and chooses Physical or Virtual.
Either selection unlocks People & Cards.
Physical selection intentionally clears the current simulated team and policy so it behaves like restarting that setup choice.
People & Cards → invitations

The admin can invite one email, submit comma/newline-separated bulk emails, or load the five-person demo team.
Values are trimmed, filtered to email-like strings, and deduplicated case-insensitively against existing teammates and duplicates in the same submission.
A teammate progresses from invited to issued to swiped.
People & Cards → Spend Controls

Spend Controls stays locked until at least one teammate exists.
When a teammate exists without a policy, People & Cards displays a direct reminder banner linking to Spend Controls.
Spend Controls → monitoring

The admin chooses $50, $100, $250, or $500 per purchase.
Saving the policy redirects to People & Cards and unlocks the first-swipe simulation.
Expense details are only available for teammates with swiped status.
Stalled teammate and progress

Jordan Bell is the intentionally stalled demo teammate.
Once monitoring is unlocked, the admin can send a reminder or simulate Jordan’s first swipe.
The summary and boss-ready card update as first swipes are completed.

## What's solid vs. what's duct tape

| Area | State | Notes |
|---|---|---|
| The main product sequence is explicit and enforced with UI guards. | solid | _____ |
| Screen names, shared types, static data, and layout chrome have clear ownership. | rough | _____ |

## Risks & assumptions for the team

The interaction design is directionally testable, but the product hypothesis is not empirically validated: there are no real users, analytics events, transactions, or experiment results.
A browser refresh loses all invites, policies, reminders, and simulated progress. Do not treat the current state as business data.
The client is trusted to enforce the sequence. Any production implementation must repeat authorization, ownership, validation, and state-transition rules on the server.
Email validation is intentionally lightweight (includes("@")) and is not suitable for production invitation delivery.
The five-person demo team is a scenario fixture, not an organization membership source.
The current UI copy says “real time,” but the current implementation only updates the current browser session synchronously.
Introducing persistence or a backend will require deciding the canonical cardholder, policy, invitation, transaction, and event models before wiring API calls.
If the onboarding experiment proceeds, add analytics around card selection, invite completion, policy save, reminder send, card acceptance, first swipe, and drop-off between steps.

## How to run it

```
From the repository root:

pnpm --filter @workspace/flowcard-prototype run dev

Useful checks:

pnpm --filter @workspace/flowcard-prototype run typecheck
pnpm --filter @workspace/flowcard-prototype run build

In Replit, use the configured workflow:

artifacts/flowcard-prototype: web

The app is registered as the flowcard-prototype web artifact and is served from the artifact’s preview path. The current app route is /. To test the main scenario manually:

Select a card type.
Open People & Cards and add one person or use Add my team.
Open Spend Controls, select a limit, and save it.
Return to People & Cards.
Advance teammates through card acceptance and first swipe.
Open expense details only after a first swipe.
Use Reset to return to the empty starting state.
```
