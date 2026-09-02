# Living PRD

> Module 4 · Production Specs. Refactor for readability; extract a living PRD that stays true as the build evolves.

## Problem

_What user problem does this solve? Tie to the validated hypothesis._

After a card program is set up, finance admins still need to drive the team through several activation steps: choose a card format, invite teammates, define spending controls, confirm card acceptance, and prompt first purchases. Without a visible sequence, admins may stall because the next action, dependency, or program status is unclear.

The working hypothesis validated by this prototype is:

If admins receive a clear, visible path immediately after customer onboarding, they will be less likely to stall before their team makes its first purchases.

The prototype provides directional validation of this hypothesis at the interaction-design level: it makes the next step explicit, gates dependent actions, surfaces progress, redirects admins between screens, and provides a reminder for stalled activity. It does not provide empirical validation yet. There are no real users, production telemetry, email sends, card transactions, or measured completion-rate comparisons.

The desired outcome is visible progress toward:

3 of 5 teammates made their first purchase.

## Users & jobs

- **Primary user:** Primary user: Finance administrator or card-program owner responsible for launching and monitoring a company debit-card program.
- **Job to be done:** When my card program is ready but my team has not started spending, I want a clear, ordered set of actions to invite teammates, establish spending controls, monitor activation, and nudge stalled cardholders, so I can move the program toward its first meaningful purchases with confidence.

## Scope

- **In:** Post-setup admin dashboard with:
Overview screen
People & Cards screen
Spend Controls screen
Four-step activation path:
Select Card Type
People & Cards
Spend Controls
Monitor Card Program
Physical and Virtual card options with:
Benefit description
Best-fit use case
Availability timing
Invite flow supporting:
Single email invites
Bulk comma- or newline-separated invites
Loading a five-person demo team
Duplicate-email prevention, case-insensitive
Gating rules:
Card type must be selected before inviting
At least one teammate must exist before configuring Spend Controls
Spend Controls must be saved before simulating card acceptance or first swipe
Spend Controls:
Selectable per-purchase limits of $50, $100, $250, or $500
Save confirmation
Redirect to Step 04 / Card Monitoring after saving
People & Cards monitoring:
Teammate activation status
Invited → Card Issued → First Swipe progression
First-swipe progress counters
Stalled teammate reminder
Expense-detail gating:
Expense details available only after a teammate completes a first swipe
Invited and card-issued teammates show the prerequisite instead
Progress and feedback:
KPI cards for invited, issued, and first-swipe counts
Launch progress bar
Completion count
Toast-style notices
Loading and error states for the launch path
Reset behavior for repeatable usability testing.
- **Out (explicitly):** Real user authentication, organizations, roles, or permissions.
Persistent user, teammate, policy, or invite data.
Real invitation email delivery.
Real card ordering, issuing, activation, or acceptance.
Real payment authorization, settlement, or transaction data.
Real expense records or merchant enrichment.
Backend APIs, database synchronization, or cross-session state.
Real-time multi-user updates.
Production notification delivery or reminder scheduling.
Production audit logs or compliance controls.
Per-person policy customization beyond the demonstrated default limit.
Measurement infrastructure needed to prove reduced stall rate or increased activation rate.
The current “View card order” action beyond a simulated confirmation notice.

## Requirements

| # | Requirement | Priority | Acceptance criteria |
|---|---|---|---|
| 1 | Show a post-setup Overview with a clear activation path | Must | Given the admin opens the console, when the page loads, then Overview shows the program-ready state, the three activation KPIs, and the ordered four-step path. |
| 2 | Gate the activation sequence in order | Must | Given no card type is selected, when the admin attempts to invite teammates, then the invite action is blocked. Given no teammate exists, when the admin opens Spend Controls, then policy configuration is blocked. Given no saved policy exists, when the admin attempts to monitor first swipes, then monitoring actions remain locked. |

## Data & events

_What gets stored, what gets tracked._

All current data is mocked or stored in local React state. Nothing persists across refreshes, users, or sessions.

Teammate

id
name
email
initials
status: invited, issued, or swiped
invitedAt
Card selection

cardType: physical or virtual
Static benefit, availability, and best-fit copy
Spend Controls

policy: one of $50, $100, $250, or $500
Stored only in local component state
Demo expenses

Static merchant, category, amount, and date rows
Shown only for teammates with swiped status
Not connected to card transactions
UI state

Active navigation screen
Invite modal state
Invite mode and form values
Reminder-sent state
Launch-path loading/error state
Expense-detail modal state

These events are not currently emitted by the prototype; they are recommended for validating the hypothesis:

activation_overview_viewed
card_type_selected
people_and_cards_opened
invite_flow_opened
invite_submitted
duplicate_invite_skipped
teammate_invited
spend_controls_opened
spend_policy_saved
redirected_to_card_monitoring
card_acceptance_recorded
first_swipe_recorded
expense_details_opened
expense_details_blocked
stalled_teammate_reminder_sent
activation_step_completed
activation_flow_reset
activation_milestone_reached
Production events should include, at minimum:

organization_id
actor_user_id
teammate_id where applicable
card_program_id
event timestamp
source screen
relevant step number
result or failure reason

## Open questions

What production behavior defines “card issued,” “activated,” and “first swipe”? Are these separate provider events or internal states?
Should duplicate-email prevention apply only to currently active teammates, or also to previously invited, revoked, or archived users?
Should a duplicate invite submission fail entirely, or should valid unique addresses still be sent while duplicates are skipped?
Is the 3-of-5 milestone always fixed at five teammates, or should the denominator adapt to the organization’s invited team size?
Should the initial card-type choice create a real card order, or only configure a preference before ordering?
Are Physical and Virtual card availability rules organization-specific, region-specific, or account-specific?
Is Spend Controls a default policy for the whole program, or should admins configure policies per teammate, role, merchant category, or card?
Should saving a policy always redirect to Card Monitoring, or only when at least one teammate is ready for activation?
What notification channel should reminders use: email, in-app notification, Slack, or another channel?
What reminder frequency and suppression rules prevent admins from repeatedly nudging the same teammate?
What expense data may an admin see, and are there privacy, approval, or role-based access requirements?
Should expense details be available after authorization, settlement, or only after a completed purchase?
Which backend or card-issuing provider supplies teammate status and transaction events?
What is the target baseline stall rate, and what measurable improvement would confirm the hypothesis?
Which usability-test cohort and sample size are needed before treating the hypothesis as empirically validated?
Should the launch path survive refreshes and support multiple admins working in the same organization?
What audit, compliance, and authorization requirements apply before real card and expense data are exposed?
