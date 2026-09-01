# PROMPTS.md: Living Prompt Pack

> Module 3 · Prompt Chaining. Re-architect the build with prompt chains; capture the reusable ones here.

## How to use this pack

_Each prompt is a reusable step. Chain them: the output of one becomes the input to the next._

## Prompt chain: Activation hardening chain

### Step 1: Organize the Team Onboarding flow 
```
Build the next phase of this app in a strict sequence:
1. Add a screen to allow the admin to choose between the virtual card and the physical card. This is step 01.
2. Only when the user has selected a virtual card, allow step 2 to invite new team members. 1 or more and provide an option to add the admin's team. 
3. As step 3 allow the admin user to configure the limit policy. 
4. Step 4 can be achieved after step 3 is completed. The simulation flow about a user accepting the card can be completed but the first swipe can only be completed after the limit policy is set.
```

### Step 2: Behavior, hard-code the states
```
Apply the following logic constraints to the Launch Path flow:
- Use skeleton screens for the launch path states (Program Activated, Card Type Chosen, Team Invited, Policy set, Card Program Monitoring).
- If no data is present, show the empty state: "No Team members added. Let's start moving!!".
- On fetch failure, trigger the error state: "An error has occurred. Please contact FlowCard Admin".

Maintain the same design language throughout and tether all behavior strictly to these rules.
```

### Step 3: Refine, one surgical polish
```
The dashboard needs a professional polish.
1. Start by giving more priority to the 3 KPIs (First swipe, Card issued, Invited).
2. Each step should have a different screen or modal: 
Step 01 (modal): Select Card Type.
Step 02 (modal): Invite members.
Step 03 (screen): Card Limits Policy
Step 04 (screen): Card Program monitoring (Invited, Card Issued, First Swipe). Each record should be clickable to view the team expenses. 

Don't change anything else in the project or touch the underlying logic.
```

## Reusable techniques learned

- Providing exact error messages for specific scenarios helped refining the flow.

## What broke (and the fix)

_Where a single mega-prompt failed and chaining fixed it._

Step 02 was activated only when Virtual Card was selected. Repromoted and it fix it. 
Prompting the change from a widget to a screen did not work. 
