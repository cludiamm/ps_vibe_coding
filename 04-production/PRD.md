# Living PRD

> Module 4 · Production Specs. Refactor for readability; extract a living PRD that stays true as the build evolves.

## Problem

_What user problem does this solve? Tie to the validated hypothesis._

FlowCard sells B2B debit card issuing and spend management to finance/ops leads who don't want to build card infrastructure. 30% of new company accounts churn within 90 days of go-live.

Three drivers were on the table (per lab-1-run-sheet-scenario-01.md): day-1 activation drop-off (5–7 day physical fulfilment), invisible value (no spend visibility for 2–3 weeks), and second-user stall (admin activates, team never does). The build bets on the gap that contains all three: the account completes setup and then nothing happens. Setup success is not activation, and the product currently gives the admin no path, no proof, and no first transaction between "company verified" and "someone spent money."

## Users & jobs

- **Primary user:** Primary user: the FlowCard company admin — a finance or ops lead at an SMB/fintech, day 0–14 post-signup, who has already verified the company and is now responsible for getting cards into other people's hands. Single-player on screen; multi-player in outcome.
- **Job to be done:** Job to be done: When I've finished setting FlowCard up, I want to get spend authority into my team's hands and see money actually move — so I can prove to my boss this was worth buying, this week and not next quarter.

## Scope

- **In:** Post-setup activation dashboard: one page, three explicit steps (Invite your team → Set a spend policy → See your first swipe), with step 3 locked until a teammate exists.
Card-type choice at issue time: Virtual (Issued instantly) vs Physical (Arrives in 5-7 days), each with a detail modal and an explicit Choose Virtual / Choose Physical action.
Bulk invite by pasted emails (commas or newlines), with a live parse count on the button (Send invites (2)), plus a demo shortcut (Invite my 5-person team).
Per-teammate status funnel: Invited → Card issued → First swipe, card-type chip (PENDING / VIRTUAL), dormancy signal (card unused for 3 days), and first-transaction detail ($18.40 at Figma).
One-decision spend policy: three presets — $500 Ad-hoc / $2,500 Most common / $10,000 Vendor budget — applied to every card issued.
Progress framing that flips on the first swipe: You're 0 of 3 steps from a spending team → 🎉 1 of 2 teammates made their first purchase.
- **Out (explicitly):** Signup, KYB, company verification, funding the account — the story starts after setup succeeds.
Real card issuance: no PAN/expiry/CVV, no wallet provisioning, no card controls (freeze/reissue), no card detail view.
Real invites: no email is sent, no teammate acceptance flow, no second-user experience at all.
Per-person or per-category limits, custom amounts, approval workflows, receipts, reconciliation, exports.
Any measurement: no analytics, no time-to-first-swipe metric, no before/after comparison on screen.
Multi-account, roles/permissions, authentication (Welcome back, Claudia is a static string).

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | R1 — Post-setup activation home [P] The admin lands on an explicit 3-step path to a spending team, with progress state persisted per company. | Must | Given setup is complete and no teammate exists, the dashboard shows 0 of 3 with step 1 open, step 3 locked with the reason shown (Unlocks once you've invited a teammate). Step count increments only on a completed step and survives reload. |
| 2 | R2 — Bulk invite with parse feedback [P] Admin pastes emails separated by commas or newlines and sends invites in one action. | Must | Button label reflects the parsed count live (Send invites (2)) and is disabled when the field is empty; invalid entries are rejected with a per-entry error (not observed in the prototype — I did not see any validation, and it was not tested with malformed input); each valid address becomes one teammate row within 1s. |

## Data & events

_What gets stored, what gets tracked._

What exists today (real, but local):
{ "team": [ { "id": 1787707741271, "name": "Rosa", "email": "rosa@acme.co",
              "status": "invited | issued | swiped", "stalled": true } ],
  "limit": 2500, "cardType": "virtual | physical | null", "reminded": false }

Entities the real feature needs: Company, Admin, Teammate (invite state), Card (type, status, issued_at, first_swipe_at), SpendPolicy (limit, scope), Transaction (amount, merchant, occurred_at), Nudge (sent_at, channel).

activation_dashboard_viewed, card_type_selected, invites_sent, spend_policy_set, card_issued, first_swipe, card_dormant_flagged, nudge_sent / nudge_converted, activation_completed

## Open questions

What did the stranger actually say?
Which driver is this build actually betting on?
Should the fork be real before the next Swap?
Nudge: ship it or cut the copy?
Is the invite gate defensible?
What is the real dormancy threshold?
Does a spend policy of one flat limit hold?
