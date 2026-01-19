# Jira Ticket: APT-SEC-201

## Develop Security Access & Queue Assignment API (Backend)

- **Epic:** Airport Passenger Flow Optimization
- **Type:** Story
- **Priority:** High
- **Reporter:** Systems Analyst (Airport IT Department)
- **Assignee:** Security Systems Development Team
- **Sprint:** Q1 2026

---

## Description

Develop the **Security Access & Queue Assignment API** (Backend) defined in the OpenAPI specification. This API will be consumed by the physical e-Gate workstations (Acting as clients).

The development generally covers the creation of endpoints, validation logic, and integration with airport data sources. Testing will be conducted in a **Pre-Production Environment** using automated API tests (Postman/Curl/Integration Tests). Physical hardware testing is out of scope for this ticket.

---

## Objectives

- Implement the REST API endpoints defined in `openapi.yaml`.
- Enforce business logic for access control and intelligent queue assignment.
- Ensure high availability and sub-second response times.
- Secure endpoints using API Key authentication.
- Implement comprehensive logging for operational monitoring.

---

## Scope

### In Scope

- **API Implementation**: `/security-access/sessions`, `/security-access/sessions/{sessionId}/scan`, `/security-access/sessions/{sessionId}/decision`, `/security-access/sessions/{sessionId}/gate/open`, `/security-access/events`, `/security-access/heartbeat`.
- **Logic**: Boarding pass validation (format & business rules), Queue assignment algorithm.
- **Data**: Mock integration with AODB (Flight data) and Queue Monitors for Pre-prod.
- **Security**: API Key validation and PII masking in logs.
- **Pre-Prod Testing**: Automated functional and load testing.

### Out of Scope

- **Client Software**: The Kiosk UI and Hardware Drivers (HAL).
- **Physical Testing**: Actual gate movement or scanner hardware tests.
- **Network Infrastructure**: Firewall or Load Balancer setup (assumed ready).

---

## Functional Requirements (Backend)

### 1. Session & Scan Management

- **POST /security-access/sessions**: Create a unique session for a passenger transaction.
- **POST /security-access/sessions/{sessionId}/scan**: Submit BCBP payload to the active session.
- **Validation Rules**:
  - Flight must be today.
  - Airport code must match.
  - Access window (e.g. -4h to flight departure).

### 2. Decision Engine

- **POST /security-access/sessions/{sessionId}/decision**:
  - Retrieve flight status (Mock/Adapter).
  - Retrieve queue load (Mock/Adapter).
  - Apply assignment rules (Priority, PRM, Load Balancing).
  - Return `ALLOW`/`DENY` and `laneID`.

### 3. Gate Control & Eventing

- **POST /security-access/sessions/{sessionId}/gate/open**: Signal acceptance and trigger audit log.
- **POST /security-access/events**: Store operational events for dashboarding.

---

## Acceptance Criteria (Pre-Production Testing)

### AC1: Standard Access Flow (Happy Path)

### AC1: Standard Access Flow (Happy Path)

```gherkin
Given a valid API Key
And a POST /scan request with a valid BCBP payload for a future flight
When POST /decision is called
Then return status 200 OK
And response body contains result: ALLOW and laneAssignment: STANDARD
And response time is < 200ms
```

### AC2: Priority Logic Verification

```gherkin
Given a flight departing in < 45 mins (Mocked AODB response)
When POST /decision is requested
Then return laneAssignment: FAST_TRACK
And reasonCode: URGENCY_PRIORITY
```

### AC3: Access Denied (Validation Rules)

```gherkin
Given a BCBP payload for a flight yesterday
When /scan is submitted
Then return 200 OK (Scan accepted)
But subsequent POST /decision returns result: DENY
And reasonCode: FLIGHT_EXPIRED
```

### AC4: Security & Authentication

```gherkin
Given a request WITHOUT X-EGate-API-Key header
When any endpoint is called
Then return 401 Unauthorized
```

### AC5: API Idempotency & Stability

```gherkin
Given a POST /gate/open request is sent twice with the same gateCommandId
Then the second request returns 204 No Content (Success)
And ONLY ONE audit event is created in the database
```

### AC6: Privacy Compliance (Logs)

```gherkin
Given a passenger "John Doe" has processed successfully
When searching the Application Logs
Then the string "John Doe" MUST NOT be found
And the log entry must contain a masked identifier (e.g. J*** D**)
```

### AC7: Load Capability

```gherkin
Given a load of 50 requests/second
When running the decision engine
Then P95 Latency remains under 500ms
And Error rate is < 0.1%
```

---

## Definition of Done

- Functional requirements implemented and tested.
- Integration with gate hardware validated.
- Operational metrics visible in monitoring dashboards.
- Documentation available for operations and support teams.
- Demo completed with airport security stakeholders.
