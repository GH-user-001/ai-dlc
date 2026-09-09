# Spec: claims status self-service
Status: draft
Related intent: claims status self-service

## Summary
This feature allows customers to view the current status of a claim, the next required action, and the expected completion date without contacting a claims handler. The goal is to reduce status-only call volume while keeping access within the existing authenticated portal experience.

## Problem
Customers phone the contact center to ask where their claim is.
Handlers spend roughly a third of call time on status-only queries.

## Goals
- Reduce inbound calls for basic claim-status questions
- Provide a self-serve status view in the portal
- Increase transparency for claim milestones and next steps
- Keep access aligned with existing authentication and data protections

## Non-goals
- Allow claim changes or adjudication decisions in the portal
- Introduce new identity or consent flows beyond current authentication
- Expose internal claims notes or sensitive operational data

## Users and personas
### Customer
A claimant or policyholder who wants to know current claim status and next steps.

### Claims handler
A team member who needs to spend less time answering repetitive status questions.

### Portal team
Owns the front-end experience, access controls, and UX consistency.

### Claims-core API team
Owns the underlying claim-status data source and service contracts.

## User stories
- As a customer, I want to see my claim status in the portal so I can understand where my claim is.
- As a customer, I want to see the next step and expected date so I know what to expect next.
- As a claims handler, I want status-only calls reduced so I can focus on complex cases.
- As a portal team member, I want the solution to follow existing authentication and privacy rules.

## Functional requirements
1. The portal must authenticate the user using the current supported portal authentication flow.
2. After successful authentication, a logged-in user must be able to view the status of an active claim associated with their account.
3. The portal must show, for each claim:
   - current status
   - next action required
   - expected completion or next milestone date
   - last update timestamp
4. When no claim is found or the user is not authorized, the portal must show a clear, non-sensitive message and avoid exposing internal claim identifiers.
5. If the upstream claims service is unavailable or returns an error, the portal must fail gracefully with a retry-friendly message and an appropriate error state.
6. The feature must support responsive access across desktop and mobile portal layouts.

## Non-functional requirements
- P95 page load for claim status must be under 3 seconds under normal traffic conditions.
- No new personally identifiable information may be added to the portal session or browser storage.
- The solution must use existing authentication and authorization only.
- The UI must be accessible to keyboard and screen-reader users.
- The claim-status view must have basic audit logging for access and failures without exposing full sensitive payloads.

## Design constraints
- Existing authentication only
- No new PII in portal session state
- Must align with current portal patterns and accessibility standards
- Must use existing claims-core APIs if available; no parallel data store is required

## Proposed user experience
The customer enters the portal and navigates to a claim summary section. A status card shows:
- Claim status
- Next step
- Expected date
- Last updated time

The design should avoid displaying raw internal comment threads, attorney notes, or operational details that are not intended for customer-facing use.

## API and integration considerations
### Consumer-facing portal behavior
The portal reads a claim-status summary from an authenticated API call and renders the status, next step, and expected date.

### Backend contract
The claims-core API should return a minimal claim-status payload with fields such as:
- claimId
- status
- nextStep
- expectedDate
- lastUpdatedAt

The API response must not include unsupported or sensitive data fields without explicit product and security approval.

## Acceptance criteria
- [ ] A logged-in customer can view claim status in the portal without calling the contact center.
- [ ] The portal shows status, next step, and expected date for a valid claim.
- [ ] The portal displays a clear message when a claim is missing or access is denied.
- [ ] No new PII is stored in the portal session or browser state.
- [ ] The feature uses the existing authentication model and does not require a new identity workflow.
- [ ] Error states are handled gracefully and do not expose unsupported claim information.

## Risks and open questions
- Do third-party loss adjusters need access to the same status view?
- Are there additional claim states that need customer-friendly wording?
- What is the expected SLA for the claims-core API when the backend is degraded?
- Should claim status be available only for open claims or also for closed claims with a final decision?

## Definition of done
This work is complete when the portal experience is available to authenticated customers, the status view meets the acceptance criteria, and the design, constraints, and integrations are understood by product, engineering, and security stakeholders.
