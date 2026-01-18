# 1. Process Design: Passenger Journey Through Main Airport Formalities

Based on common passenger experiences at medium and large European airports, this section describes the main airport formalities from the passenger’s point of view. The process is based on observation and reasonable assumptions, given the absence of detailed operational inputs, and is formalized into a structured, step-by-step flow.

The scope covers the journey **from entering the departure terminal to boarding the aircraft**, focusing on standard passenger flows.

---

## Overall Process Overview

### Scope
From arrival at the departure terminal to aircraft boarding.

### Main Participants
- **Passenger** (primary actor)
- **Airline staff** (check-in and boarding agents)
- **Airport security personnel**
- **Border control / immigration officers** (for international flights)
- **Airport operations staff**
- **Automated systems** (kiosks, scanners, access gates, information displays)

### Key Assumptions
- The passenger holds a valid ticket and travel document.
- No major operational disruptions (e.g. strikes or airport closures).
- Standard economy passenger flow.
- International flight used as baseline; domestic flights skip immigration.

### Possible System Integrations
- Airline reservation and departure control systems.
- Airport operational systems (flight status, gate assignment, announcements).
- Security and access control systems.
- Border control and passport verification systems.
- Mobile applications for notifications and digital boarding passes.
- Biometric systems for automated document or identity verification.

---

## Step-by-Step Process Flow

### 1. Entry into Departure Terminal

**Description**  
The passenger arrives at the airport via ground transportation and enters the departure terminal.

**Participants**
- Passenger  
- Airport operations or security staff (optional)

**Inputs**
- Flight ticket (digital or printed)  
- Travel document (ID or passport)  
- Luggage  

**Outputs**
- Access to the check-in area

**Alternative Scenarios**
- Late arrival: passenger may seek express routes or assistance.
- Passenger with reduced mobility: assistance requested at terminal entry.

**Integrations**
- Digital signage connected to flight information systems.
- Mobile notifications providing terminal or gate guidance.

---

### 2. Check-In and Baggage Drop

**Description**  
The passenger proceeds to airline check-in counters or self-service kiosks to confirm the booking, obtain a boarding pass, and drop checked baggage if applicable.

From the passenger’s perspective, check-in is perceived as part of the airport departure process, even though operational responsibility lies with the airline.

**Participants**
- Passenger  
- Airline check-in agents or self-service kiosks  
- Baggage handling staff  

**Inputs**
- Booking reference or e-ticket  
- Passport or ID  
- Checked luggage  

**Outputs**
- Boarding pass (physical or digital)  
- Baggage tag and receipt  
- Seat assignment  

**Alternative Scenarios**
- Online check-in completed: passenger proceeds directly to bag drop or security.
- No checked baggage: skip baggage drop.
- Overweight luggage: additional fees required.
- Invalid booking: rebooking or redirection to airline staff.

**Integrations**
- Airline reservation and seat management systems.
- Baggage tracking and handling systems.
- Payment systems for excess baggage fees.

---

### 3. Security Screening

**Description**  
The passenger queues at the security checkpoint, presents the boarding pass, and passes through screening equipment.

This step represents an airport-controlled process that regulates access to the sterile airside area.

**Participants**
- Passenger  
- Airport security personnel  
- Screening equipment operators  

**Inputs**
- Boarding pass  
- Personal belongings and cabin luggage  

**Outputs**
- Cleared passenger and belongings  
- Access to the airside area  

**Alternative Scenarios**
- Fast-track or priority screening lanes.
- Secondary or random screening.
- Prohibited items detected and confiscated.
- Extended delays increasing the risk of missed flights.

**Integrations**
- Access control and boarding pass validation systems.
- Security screening equipment.
- Queue monitoring and throughput measurement systems.

---

### 4. Immigration / Passport Control (International Flights Only)

**Description**  
For international departures, the passenger passes through border control via manual counters or automated e-gates.

This step is operated by government border authorities rather than the airport or airline.

**Participants**
- Passenger  
- Immigration officers or automated e-gates  

**Inputs**
- Passport  
- Visa (if required)  
- Boarding pass  

**Outputs**
- Exit clearance (stamp or digital approval)  
- Access to post-immigration airside area  

**Alternative Scenarios**
- Domestic flight: this step is skipped.
- Visa or document issues: additional questioning or denial of exit.
- Automated gate failure: fallback to manual processing.

**Integrations**
- National border control and immigration databases.
- Biometric verification systems linked to e-passports.
- Airline passenger manifest validation systems.

---

### 5. Waiting Area, Duty-Free, and Gate Boarding

**Description**  
The passenger waits in the departure lounge, optionally uses retail or dining services, and proceeds to the assigned gate for boarding.

**Participants**
- Passenger  
- Gate agents  
- Retail and airport service staff  

**Inputs**
- Boarding pass  

**Outputs**
- Successful boarding of the aircraft  

**Alternative Scenarios**
- Flight delays or gate changes.
- Priority boarding for specific passenger categories.
- No-shows leading to seat reassignment.
- Medical or operational incidents at the gate.

**Integrations**
- Flight Information Display Systems (FIDS).
- Public address and notification systems.
- Boarding gate scanners and boarding management systems.

---

## Observed Pain Points and Improvement Opportunities (Hypotheses)

- **Bottlenecks** at security screening and border control due to uneven passenger distribution.
- **Limited real-time awareness** of queue length and passenger urgency.
- **Missed flights** caused by long security or immigration queues.
- **Special passenger flows** (PRM, families, tight connections) requiring manual intervention.

Potential improvements could include predictive queue monitoring, dynamic lane assignment, and real-time operational decision support to optimize passenger throughput and reduce delays.

---

### Summary Flow (High-Level)

Start → Terminal Entry → Check-In (Decision: online / baggage) → Security Screening → Immigration (if applicable) → Waiting Area → Boarding → End
