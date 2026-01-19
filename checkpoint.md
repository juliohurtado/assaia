# Jira Ticket: APT-SEC-201

## Develop Security Checkpoint e-Gate with Intelligent Queue Assignment

**Epic:** Airport Passenger Flow Optimization
**Type:** Story
**Priority:** High
**Reporter:** Systems Analyst (Airport IT Department)
**Assignee:** Security Systems Development Team
**Sprint:** Q1 2026

---

## Description

Design and implement software for an automated **Security Checkpoint e-Gate workstation** that controls passenger access to the security screening area and dynamically assigns passengers to the most appropriate security lane (queue) based on real-time operational context and passenger characteristics.

The workstation is a PC-based system connected to the airport network and installed at the entrance of the security screening area. It validates passenger eligibility to access the airside area and optimizes queue distribution to improve throughput and reduce the risk of passengers missing their flights.

This system operates under airport responsibility and does not perform airline check-in or boarding authorization.

---

## Objectives

- Control and automate passenger access to the security screening area.
- Optimize distribution of passengers across available security lanes.
- Prioritize passengers with imminent boarding times or special needs.
- Reduce congestion and imbalance between queues.
- Provide clear guidance to passengers and actionable information to operations staff.

---

## Scope

### In Scope

- Boarding pass validation at security access point.
- Real-time assessment of passenger urgency and needs.
- Automatic assignment of a security lane (queue).
- Integration with security access hardware (gate/turnstile).
- Passenger guidance via on-screen instructions and signage.
- Event logging and operational metrics.

### Out of Scope

- Physical security screening (X-ray, body scanners).
- Airline check-in, seat assignment, or boarding authorization.
- Manual security procedures performed by officers.
- Biometric identity verification beyond boarding pass validation.

---

## Functional Requirements

### 1. Passenger Access Validation

- The system shall scan and validate the passenger boarding pass (2D barcode / QR / Aztec).
- Validation rules include:
  - Flight date and airport match.
  - Access time window (e.g. not earlier than X hours before departure).
  - Flight status (not cancelled).
- If validation fails, the passenger shall be redirected to security staff.

---

### 2. Passenger Context Evaluation

Upon successful validation, the system shall determine passenger context using available data:

- Time remaining until boarding or departure.
- Passenger category (e.g. standard, PRM, senior, minor), if available.
- Eligibility for priority or assisted lanes.
- Group or family indicators (if provided).

---

### 3. Queue (Lane) State Assessment

The system shall retrieve real-time information about available security lanes, including:

- Lane type (standard, fast-track, PRM/assisted, family).
- Current queue length or estimated waiting time.
- Lane operational status (open / closed).
- Capacity constraints or incidents.

---

### 4. Intelligent Queue Assignment

The system shall automatically assign the passenger to the most appropriate lane based on:

- Passenger urgency (e.g. proximity to boarding time).
- Mandatory rules (e.g. PRM passengers to assisted lanes).
- Operational balancing (avoid overloading a single lane).

The passenger shall not manually choose the lane in blocking scenarios.

---

### 5. Passenger Guidance

- The system shall display clear instructions indicating the assigned lane.
- Instructions shall include visual cues (lane number, color, icon) and text.
- Guidance shall be shown in the passenger’s preferred UI language when available.

---

### 6. Access Control

- Upon successful assignment, the system shall open the security e-gate or turnstile.
- If no suitable lane is available, the passenger shall be redirected to security staff.

---

### 7. Monitoring and Logging

- The system shall log events including:
  - Boarding pass validation result.
  - Assigned lane.
  - Timestamp and urgency classification.
- Events shall be forwarded to central airport monitoring systems.

---

### 8. Offline & Resilience

- **Offline Validation Mode:** In case of backend connectivity loss, the system shall validate the 2D Barcode (BCBP) signature locally using cached keys.
- **Fail-safe Operation:** If intelligent assignment is offline, the system shall fallback to a local Round-Robin or static lane assignment algorithm.

## Inputs

- Boarding pass data (barcode content).
- Current time and flight schedule.
- Real-time lane status and queue metrics.
- Passenger category indicators (if available).

---

## Outputs

- Gate open / deny signal.
- Assigned security lane identifier.
- Passenger-facing instructions.
- Operational and monitoring events.

---

## Integrations

- Security access control hardware (gates/turnstiles).
- Flight and airport operational data systems.
- Queue monitoring systems (sensors, counters, or camera-based estimations).
- Central logging and monitoring platforms.

---

## Error and Exception Handling

- Invalid or unreadable boarding pass → redirect to security staff.
- Flight not eligible for access → deny gate opening.
- No suitable lane available → redirect to security staff.
- System or network failure → fail-safe mode with manual processing.

---

## Detailed Acceptance Scenarios

### Scenario 1: Standard Access (Happy Path)

- **Given** a passenger with a valid boarding pass for a flight departing in 2 hours
- **And** all security lanes are operating at normal capacity
- **When** the passenger scans the boarding pass
- **Then** the gate opens within 1 second [Ref: AC6]
- **And** the passenger is assigned to a "Standard" lane
- **And** the instructions are shown in the language inferred from the flight destination (or default).

### Scenario 2: Priority Access via Urgency

- **Given** a passenger with a flight departing in < 45 minutes (Imminent)
- **When** the passenger scans the boarding pass
- **Then** the gate opens
- **And** the system assigns the "Fast Track" lane (regardless of ticket class)
- **And** a "Prioritized" event is logged.

### Scenario 3: PRM / Assisted Access

- **Given** a passenger identified as Person with Reduced Mobility (PRM) (via BCBP code or backend context)
- **When** the passenger scans the boarding pass
- **Then** the system assigns a "PRM/Assisted" lane (wider gate)
- **And** the UI displays the universal accessibility symbol.

### Scenario 4: Access Denied / Invalid Pass

- **Given** a passenger with an invalid, expired, or wrong-airport boarding pass
- **When** the passenger scans the document
- **Then** the gate REMAINS CLOSED
- **And** the UI displays "Access Denied - Please contact staff" in Red
- **And** a "Security Alert" event is sent to the monitoring system.

### Scenario 5: Offline Resilience (Connectivity Loss)

- **Given** the backend API is unreachable (Heartbeat timeout)
- **When** a passenger scans a valid IATA BCBP
- **Then** the system validates the digital signature locally
- **And** the gate opens (Fail-Open behavior for valid signatures)
- **And** the passenger is assigned a lane via "Round Robin" logic
- **And** an "Offline Validation" event is queued for later sync.

### Scenario 6: Privacy & Logging Compliance

- **Given** a successful or failed passenger scan
- **When** the operational logs are inspected
- **Then** the entry MUST contain the Session ID, Lane Decision, and Timestamp
- **But** MUST NOT contain the Passenger Name or raw Barcode string (PII)
- **And** the PII fields must be replaced with `[MASKED]` or a non-reversible hash.

### Scenario 7: Hardware Fault Handling

- **Given** the physical turnstile is jammed or unresponsive
- **When** the system attempts to open the gate
- **Then** a "Hardware Fault" error is raised via HAL
- **And** the UI shows "Please seek assistance"
- **And** a partial "Health Check" event is sent to the backend.

---

## Non-Functional Requirements

- **User Interface:** High-contrast, clear instructions suitable for all lighting conditions.
- High availability during airport operating hours.
- Secure communication with backend systems.
- Configurable business rules (e.g. time thresholds, lane priorities).
- Deployable on standard airport workstation hardware (Windows/Linux).
- **Data Privacy:** Logs and events MUST NOT contain plain-text PII (Personally Identifiable Information). Passenger names must be masked or hashed.
- **Hardware Abstraction:** The system shall implement a Hardware Abstraction Layer (HAL) to support different gate/scanner vendors without core code changes.
- **Configuration:** Critical parameters (lane status, priority rules) must be hot-reloadable without software restarts.

---

## Definition of Done

- Functional requirements implemented and tested.
- Integration with gate hardware validated.
- Operational metrics visible in monitoring dashboards.
- Documentation available for operations and support teams.
- Demo completed with airport security stakeholders.
