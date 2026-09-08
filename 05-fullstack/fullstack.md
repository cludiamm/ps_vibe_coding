# Full-Stack: Data, Access Rules, Edge Cases, Deploy

> Module 5 · Full-Stack. Add data schemas, access rules, and edge cases; stress-test and deploy.

## Deployed link

_The working, shareable link that survives real users._

_____

## Data schema

| Entity | Key fields | Notes |
|---|---|---|
| organizations | clerk_organization_id text unique   slug text unique | _____ |
| organization_members | id uuid primary key | _____ |

## Access rules

_Who can see / do what? Where are the auth boundaries?_

For every endpoint and RPC, test:

Signed out
Active admin
Active member
Active viewer
Suspended member
Revoked member
User from another organization
Valid user with a forged organization ID
Valid user with a forged resource ID
The expected result must be explicit for every combination.

## Edge cases hardened

| Case | Before | After |
|---|---|---|
| Empty / first-run state | Expired or revoked Clerk session. | _____ |
| Bad / malicious input | Clerk user exists but has no active membership. | _____ |
| Failure / offline | User has memberships in multiple organizations and no organization is selected. | _____ |

## Stress test results

_What you threw at it, and what held / broke._

_____
