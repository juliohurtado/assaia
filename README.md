# Airport Passenger Flow & Security Checkpoint Automation

This repository contains a conceptual and technical design exercise focused on **airport passenger processing**, with special emphasis on **security checkpoint access automation and intelligent queue assignment**.

The goal is to demonstrate **process design**, **system analysis**, and **requirement definition** skills in an airport IT context, based on observed passenger journeys and reasonable assumptions in the absence of complete input data.

---

## 📁 Repository Contents

### 1. Passenger Journey – Process Design
📄 **[`airport_flow.md`](./airport_flow.md)**

Describes the **end-to-end passenger journey** from entering the departure terminal to boarding the aircraft, from the passenger’s point of view.

This document covers:
- Main airport formalities
- Participants involved at each step
- Inputs and outputs
- Alternative scenarios and edge cases
- Possible system integrations

This section corresponds to **Exercise 1** of the assignment.

---

### 2. Security Checkpoint e-Gate – Automated Workstation Design
📄 **[`checkpoint.md`](./checkpoint.md)**

Defines an **automated security checkpoint e-gate workstation** operated by the airport, including:
- Boarding pass validation at security access
- Passenger context evaluation (urgency, special needs)
- Intelligent assignment to security lanes (queues)
- Passenger guidance and access control
- Error handling and redirection to staff

This section corresponds to **Exercise 2** of the assignment and focuses on an airport-owned process rather than airline check-in.

---

### 3. Backend API Specification
📄 **OpenAPI Specification (Swagger UI)**  
🔗 **[View rendered API documentation](https://editor.swagger.io/?url=https://raw.githubusercontent.com/juliohurtado/assaia/refs/heads/main/openapi.yaml)**

OpenAPI 3.0 specification for the **Airport Security Access & Queue Assignment API** used by the security checkpoint workstation.

The API:
- Manages security access sessions
- Validates boarding passes
- Computes lane assignment decisions
- Controls gate/turnstile access
- Emits operational and audit events

External integrations (airline systems, queue monitoring, airport operations data) are abstracted behind this API.

---

### 4. UML Diagram – Security Checkpoint e-Gate Flow

The following diagram illustrates the **end-to-end activity flow** for the automated security checkpoint e-gate, including passenger interaction, workstation behavior, backend API calls, decision logic, and gate control.

![Security Checkpoint e-Gate UML Diagram](./uml.png)

---

## 🧭 How to Navigate This Repository

1. Start with **`airport_flow.md`** to understand the overall passenger journey.
2. Continue with **`checkpoint.md`** to see how one specific airport process is automated.
3. Open the **Swagger UI link** to explore the backend API contract interactively.
4. Review the **embedded UML diagram** to visualize the automated flow.

---

## 🧩 Design Principles Highlighted

- Clear separation between **airport-controlled** and **airline-controlled** processes.
- Explicit handling of **blocking vs non-blocking scenarios**.
- State-driven and auditable system design.
- Support for real-time operational decision-making.
- Consideration of accessibility and passenger experience.

---

## 📌 Notes

This repository is a **design and analysis exercise**, not an implementation.  
All processes, rules, and integrations are based on common airport operations and informed assumptions.

---
