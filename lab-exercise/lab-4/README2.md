# README — Medical Clinic System: State Diagram


## Overview

This document describes the UML State Diagram for the **Medical Clinic System**. The diagram models the lifecycle of a patient record/appointment object, capturing every state it can be in — from initial registration through to final discharge — and all the transitions triggered by events in the clinic workflow.

---

## System Description

The Medical Clinic System manages patient appointments and their progression through the clinic. A patient object is created when a person registers and is destroyed (reaches a final state) upon discharge. Along the way it passes through several distinct states, some of which contain substates (composite states).

---

## Diagram Components

### Initial & Final Pseudo-States

| Symbol | Meaning |
|--------|---------|
| Solid black circle | Initial pseudo-state — the starting point when a patient registers |
| Bullseye (circle within circle) | Final pseudo-state — the end of the patient lifecycle (discharged) |

---

### States

| State | Description |
|-------|-------------|
| **Registered** | The patient has created an account in the system. No appointment exists yet. |
| **Appointment Scheduled** | The patient has booked a specific date and time with a doctor. |
| **Waiting Room** | The patient has physically checked in and is waiting to be called. |
| **In Consultation** | The patient is actively being seen by the doctor. |
| **Diagnostics** *(Composite)* | The patient is undergoing tests. Contains two substates: **Lab Tests** and **Imaging / X-Ray**. |
| **Under Treatment** | The doctor has diagnosed the issue and is administering or prescribing treatment. |
| **Prescription Issued** | A prescription has been written and given to the patient for outpatient care. |
| **Admitted (Inpatient)** | The patient requires hospitalisation and has been admitted as an inpatient. |
| **Cancelled** | The appointment was cancelled before the patient checked in. |
| **Discharged** | The patient has completed treatment, settled payment, and left the clinic. |

---

### Transitions & Triggers

| From State | To State | Trigger / Guard |
|------------|----------|-----------------|
| Initial | Registered | `register` |
| Registered | Appointment Scheduled | `book appointment` |
| Appointment Scheduled | Waiting Room | `check in` |
| Appointment Scheduled | Cancelled | `cancel` |
| Waiting Room | In Consultation | `called by doctor` |
| In Consultation | Diagnostics | `[tests needed]` |
| In Consultation | Under Treatment | `[no tests required]` |
| Diagnostics | Under Treatment | `results ready` |
| Under Treatment | Prescription Issued | `prescribe medication` |
| Under Treatment | Admitted (Inpatient) | `[admission required]` |
| Prescription Issued | Discharged | `payment processed` |
| Admitted (Inpatient) | Discharged | `recovery complete` |
| Discharged | Final | *(automatic)* |

---

### Composite State — Diagnostics

The **Diagnostics** state is a composite state containing two parallel or sequential substates:

- **Lab Tests** — blood work, urine analysis, etc.
- **Imaging / X-Ray** — radiological investigation

Both substates can occur independently depending on the doctor's orders. The composite state exits when all ordered results are ready.

---

## Key UML Notation Used

| Element | Notation |
|---------|----------|
| State | Rounded rectangle |
| Composite state | Rounded rectangle with inner substates and a label in the top-left corner |
| Transition | Solid arrow between states |
| Trigger | Label on the transition arrow |
| Guard condition | Label in square brackets `[condition]` on a transition |
| Initial pseudo-state | Filled black circle |
| Final pseudo-state | Bullseye symbol |

---

## Design Decisions

1. **Cancellation** is modelled as a transition from `Appointment Scheduled` only — once the patient has checked in, cancellation is no longer available in the standard flow.
2. **Diagnostics** is modelled as a composite state rather than a simple state to reflect that multiple test types can occur within it.
3. **Two discharge paths** exist — via `Prescription Issued` (outpatient) and via `Admitted (Inpatient)` (inpatient) — reflecting real clinic workflows.
4. The `Cancelled` state is a terminal state (no outgoing transitions) to keep the model clean; re-booking would create a new patient appointment object.

---

## How to Read the Diagram

1. Start at the filled black circle (top of diagram).
2. Follow solid arrows downward — each arrow is labelled with the event or guard that causes the transition.
3. When a diamond or branching arrow is shown, a guard condition `[...]` decides which path is taken.
4. The composite `Diagnostics` box shows that the system may perform multiple substates internally before exiting.
5. The diagram ends at the bullseye symbol after `Discharged`.

---
