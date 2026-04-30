# README — Car Insurance System: Activity Diagram

---

## Overview

This document describes the UML Activity Diagram for the **Car Insurance System**. The diagram models the complete business process of applying for, purchasing, and managing a car insurance policy — from initial application submission through risk assessment, payment processing, parallel policy generation, and optional cancellation.

---

## System Description

The Car Insurance System handles the end-to-end workflow when a customer applies for a car insurance policy. It includes automated document verification, risk assessment, flexible payment options, parallel back-office actions, and the ability to cancel an active policy at any time before the contract expires.

---

## Diagram Components

### Activity Frame

The entire diagram is enclosed within a large rounded rectangle labelled **"Car Insurance"** — this is the activity partition boundary, representing the scope of the process being modelled.

---

### Nodes

| Node Type | Symbol | Instances in this Diagram |
|-----------|--------|--------------------------|
| Initial node | Solid black circle | 1 — process start |
| Action node | Rounded rectangle | Submit Application, Verify Documents, Assess Risk & Quote, Process Card Payment, Setup Direct Debit, Generate Policy Doc, Notify Customer, Policy Active, Cancel Policy, Reject Application |
| Decision node | Diamond | Documents OK?, Payment type? |
| Merge node | Diamond | After payment paths rejoin |
| Fork bar | Thick horizontal bar | Before parallel actions begin |
| Join bar | Thick horizontal bar | After parallel actions complete |
| Flow Final node | Circle with X | End of rejected/cancelled path |
| Activity Final node | Bullseye | End of the main successful flow |

---

### Actions (Nodes) in Order

| Step | Action | Description |
|------|--------|-------------|
| 1 | **Submit application** | Customer submits personal details, vehicle info, and driving history. |
| 2 | **Verify documents** | System or agent checks that all required documents are valid and complete. |
| 3 | **Assess risk & quote** | System calculates risk score and generates a premium quote for the customer. |
| 4a | **Process card payment** | Customer pays via credit or debit card (one-time or recurring). |
| 4b | **Setup direct debit** | Customer authorises a monthly direct debit mandate. |
| 5 | **Generate policy doc** | System creates the official policy document (PDF, policy number, terms). |
| 6 | **Notify customer** | System sends confirmation email/SMS with policy details. |
| 7 | **Policy active** | The policy is live and the customer is covered. |
| 8 | **Cancel policy** | Customer requests cancellation; system processes the termination. |
| — | **Reject application** | Documents were invalid; application is refused. |

---

### Control Flow (Edges) & Guards

| From | To | Guard / Label |
|------|----|---------------|
| Submit application | Verify documents | *(automatic)* |
| Verify documents | Decision: Docs OK? | *(automatic)* |
| Decision: Docs OK? | Assess risk & quote | `[yes]` |
| Decision: Docs OK? | Reject application | `[no]` |
| Assess risk & quote | Decision: Payment type? | *(automatic)* |
| Decision: Payment type? | Process card payment | `[Credit card]` |
| Decision: Payment type? | Setup direct debit | `[Direct debit]` |
| Process card payment | Merge node | *(automatic)* |
| Setup direct debit | Merge node | *(automatic)* |
| Merge node | Fork bar | `Payment confirmed` |

---

### Combined Fragments & Advanced Features

#### 1. Decision Node — Document Verification

A **diamond decision node** after "Verify documents" splits the flow:

- `[yes]` → proceeds to risk assessment
- `[no]` → routes to "Reject application" and terminates that path with a **Flow Final node (X)**

This prevents invalid applications from progressing further and wastes no system resources.

#### 2. Decision Node — Payment Type

A second **diamond decision node** after risk assessment splits the flow by payment method:

- `[Credit card]` → "Process card payment"
- `[Direct debit]` → "Setup direct debit"

Both paths converge at a **Merge node** before continuing.

#### 3. Fork / Join — Parallel Processing

After payment is confirmed, a **Fork bar** splits execution into two concurrent (parallel) actions:

- **Generate policy doc** — creates the official insurance document
- **Notify customer** — sends a confirmation message to the customer

Both actions are independent and happen simultaneously. A **Join bar** synchronises the two threads — the process only continues once **both** actions have completed.

This models the real-world scenario where document generation and customer notification happen in parallel in the back office.

#### 4. Interruptible Activity Region — Cancel Policy

A dashed rectangle marks an **Interruptible Region** around the "Policy active" state. At any point while the policy is active, the customer can trigger a `[request]` event that routes to the "Cancel policy" action, bypassing the normal end of the activity.

This is the UML mechanism for modelling interruptions that can happen at any time during a region of activity, rather than only at a specific decision point.

---

## Key UML Notation Used

| Element | Notation |
|---------|----------|
| Initial node | Solid filled circle |
| Action node | Rounded rectangle (pill shape) |
| Decision node | Diamond with labelled outgoing arrows (guards) |
| Merge node | Diamond with multiple incoming, one outgoing arrow |
| Fork | Thick bar — one incoming, multiple outgoing flows |
| Join | Thick bar — multiple incoming, one outgoing flow |
| Control flow | Solid arrow between nodes |
| Guard | Text in square brackets `[condition]` on an edge |
| Interruptible region | Dashed rounded rectangle |
| Flow Final | Circle with X — terminates one path |
| Activity Final | Bullseye — terminates the entire activity |

---

## Design Decisions

1. **Two payment paths** are provided (credit card and direct debit) to reflect common real-world insurance payment options. Both merge before proceeding to policy generation, keeping downstream logic unified.
2. **Parallel fork/join** is used for document generation and customer notification because these actions do not depend on each other — modelling them in parallel is more accurate and efficient than sequential ordering.
3. **Rejection** uses a **Flow Final node (X)** rather than the Activity Final node, because the rejection only terminates the rejected customer's path — it does not terminate the entire insurance system process.
4. The **interruptible region** is placed around "Policy active" rather than earlier states, because cancellation is only meaningful once the policy has been issued and is running.
5. Document verification is placed **before** risk assessment to avoid performing expensive computation on incomplete or fraudulent submissions.

---

## How to Read the Diagram

1. Start at the filled black circle at the top.
2. Follow the arrows downward through each action node.
3. At each diamond (decision node), check the guard label on each outgoing arrow to understand which path is taken.
4. When you reach a **thick bar (fork)**, the process splits — both branches below it execute simultaneously.
5. The next **thick bar (join)** means: wait here until all parallel branches above it finish.
6. The dashed box marks an **interruptible region** — a cancellation event can break out of it at any time.
7. The process ends at the bullseye (Activity Final node) after successful policy completion.

---
