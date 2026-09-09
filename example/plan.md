# Plan: claims status self-service
Status: draft
Related spec: claims status self-service

## Objective
Deliver a customer-facing claim status view in the portal that lets authenticated users check claim status, next step, and expected completion date without contacting the contact center.

## Workstreams

### 1. Product and requirements alignment
- Confirm the exact claim states and customer-friendly labels
- Confirm which claim types are in scope for self-service status
- Confirm whether third-party adjusters need view access
- Validate the minimum data contract required for reporting status, next step, and expected date

### 2. Portal experience
- Add a claim status card or summary widget to the authenticated portal
- Render claim state, next step, expected date, and last updated timestamp
- Handle empty, unauthorized, and API failure states with clear user messaging
- Ensure accessibility and mobile responsiveness

### 3. Backend integration
- Review the current claims-core API capability and payload schema
- Add or reuse an endpoint that returns the minimal status summary for an authenticated customer
- Ensure authorization checks prevent users from viewing claims they do not own
- Add safe error handling and monitoring for upstream service failures

### 4. Privacy and security
- Confirm no new PII is introduced into browser session state or application logs
- Validate the portal follows the existing authentication and authorization model
- Review logging and audit requirements for access events and failed requests

### 5. Validation and rollout
- Test happy path, unauthorized access, missing claim, and backend outage scenarios
- Validate accessibility, performance, and customer-facing wording
- Release progressively with monitoring and feedback from claims operations

## Milestones

### Milestone 1: feasibility and data contract
- Confirm current API contract and required status fields
- Validate scope and users affected
- Identify any blockers for privacy or authentication

### Milestone 2: design and prototype
- Build a portal mock or prototype for the status experience
- Validate copy, states, and edge cases with claims operations and portal team

### Milestone 3: implementation
- Integrate status summary API into the portal
- Add error and non-authorized states
- Add telemetry and monitoring

### Milestone 4: rollout
- Launch to a limited audience or internal pilot
- Review customer adoption and call deflection impact
- Extend to broader release once metrics are acceptable

## Risks
- Upstream claims data may not be ready for customer-safe exposure
- Claim states may require customer-friendly translations beyond raw backend values
- Differences in ownership rules could complicate authorization logic
- Portal users may need more context than a minimal status summary provides

## Dependencies
- Existing portal authentication flow
- Access to claims status data through claims-core or a compatible API
- Product signoff on customer-facing claim-state language
- Claims operations input on next-step wording and expected dates

## Success measures
- Reduction in status-only inbound calls
- Increased portal self-service adoption for claim lookups
- Lower average handle time for claims operations on status-related contacts
- Acceptance criteria met for access, privacy, and user experience

## Definition of done
This plan is complete when the team has confirmed the data contract, implementation backlog is aligned, security and privacy checks are passed, and the feature can be delivered in a controlled rollout with success metrics in place.
