# Airport Passenger Flow & Security Checkpoint Automation

This repository contains a conceptual and technical design exercise focused on **airport passenger processing**, with special emphasis on **security checkpoint access automation and intelligent queue assignment**.

## � System Overview

This repository contains the technical design and requirements specification for the **Airport Security Checkpoint Automation System**.

The solution encompasses the end-to-end passenger journey analysis and the detailed software architecture for the automated e-gate workstations. It addresses the functional requirements for passenger processing as well as non-functional requirements regarding system resilience, hardware integration, and data privacy.

---

## 🧭 How to Navigate This Repository

1.  Start with **`airport_flow.md`** to understand the high-level passenger context.
2.  Review **`checkpoint.md`** for detailed functional and non-functional requirements.
3.  Check the **Swagger UI** to see the technical contract.
4.  Examine the **UML Diagram** (`uml.png`) to visualize the decision logic and fallback paths.

---

## 📁 Repository Contents

### 1. Passenger Journey – Process Design

📄 **[`airport_flow.md`](./airport_flow.md)**

Describes the **end-to-end passenger journey** from entering the departure terminal to boarding the aircraft.

- **Scope**: Terminal Entry → Boarding.
- **Focus**: Inputs, outputs, participants, and system integrations at each step.

### 2. Security Access API – Development Ticket

**[`checkpoint.md`](./checkpoint.md)**

Defines the development task for the **Backend API** that powers the security checkpoint.

- **Focus**: REST API implementation (`/scan`, `/decision`).
- **Scope**: Backend logic, Pre-Production testing and Integration.
- **Exclusions**: Hardware drivers and Physical UI (handled in separate tickets).

### 3. Backend API Specification

📄 **OpenAPI Specification (Swagger UI)**
🔗 **[View rendered API documentation](https://editor.swagger.io/?url=https://raw.githubusercontent.com/juliohurtado/assaia/refs/heads/main/openapi.yaml)**

OpenAPI 3.0 specification for the **Airport Security Access & Queue Assignment API**.

- **Key Capabilities**: Includes sessions, scan validation, decision logic, gate control, and a `/heartbeat` endpoint for hardware health monitoring.

### 4. UML Diagram – Activity Flow

![Security Checkpoint e-Gate UML Diagram](./uml.png)

Illustrates the end-to-end flow with a focus on **Request-Response cycles** and **Resilience**:

- Visualizes the extensive **Offline Fallback** path.
- Clarifies synchronous context fetching (Flight Status, Queue Load) vs. asynchronous events.
- Handles scan errors and access denial flows.

---

## 🏗️ Architectural Design Decisions

The system architecture incorporates the following design patterns to meet operational requirements:

1.  **Offline Operations Strategy**:
    To maintain throughput during network outages, the workstation performs local cryptographic validation of IATA BCBP signatures. This allows for basic access control and static queue assignment (e.g., Round Robin) when the central decision engine is unreachable.

2.  **Data Privacy Compliance**:
    Logs and operational events implement automatic masking of Personally Identifiable Information (PII) to comply with data protection regulations. Operational metrics are decoupled from passenger identity.

3.  **Hardware Abstraction Layer (HAL)**:
    A dedicated abstraction layer isolates business logic from specific hardware drivers, allowing for the integration of different gate and scanner vendors without modifying the core application code.

4.  **System Observability**:
    The API includes a `Heartbeat` mechanism to report the real-time status of the workstation and its connected peripherals (scanner, printer, gate mechanisms) to the central monitoring system.

---

## 📌 Notes

This repository is a **design and analysis exercise**. All processes, rules, and integrations are based on common airport operations and best-practice engineering assumptions.
