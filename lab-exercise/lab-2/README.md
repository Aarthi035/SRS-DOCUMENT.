# ATM System — Use Case Diagram

## Overview

This document describes the UML Use Case Diagram for an **Automated Teller Machine (ATM) System**. The diagram models all interactions between the system's actors (Customer and Bank) and the functional use cases the system supports, including authentication, cash withdrawal, balance inquiry, fund deposit, PIN management, and card registration.

Use case diagrams are a core part of UML (Unified Modeling Language) and are used in the early stages of software design to capture **what the system does** from the perspective of its users, without specifying how it does it.

---

## System Boundary

The system is enclosed within a boundary labeled **ATM**. Everything inside this boundary represents functionality provided by the ATM system. Actors (Customer and Bank) exist outside this boundary and interact with the system through use cases.

---

## Actors

| Actor | Type | Description |
|---|---|---|
| **Customer** | Primary Actor | The end user who interacts directly with the ATM to perform transactions. Initiates most use cases. |
| **Bank** | Secondary Actor | The banking institution that processes and validates transactions on the backend. Receives requests from the ATM system. |

### Actor Roles

- **Customer** is a **primary actor** — they initiate interactions with the system. They are connected to all core use cases within the ATM boundary.
- **Bank** is a **secondary actor** — it does not initiate interactions but participates in certain use cases (balance checking and fund deposit) to validate and process data.

---

## Use Cases

### Core Use Cases (Inside System Boundary)

#### 1. Authenticate
- **Actor(s):** Customer
- **Description:** The customer inserts their card and enters their PIN. The system validates the credentials against the bank's records. This is typically the entry point for all other transactions.
- **Relationships:**
  - Has an `«exclude»` relationship with **Invalid card or PIN** (if authentication fails, the error case is triggered).

#### 2. Withdraw Cash
- **Actor(s):** Customer
- **Description:** The customer requests a specific amount of cash. The system checks available balance, dispenses the cash, and updates the account record.
- **Relationships:**
  - Has an `«extend»` relationship with **Print mini statement** (the customer may optionally request a mini statement after withdrawing cash).

#### 3. Check Balance
- **Actor(s):** Customer, Bank
- **Description:** The customer requests to view their current account balance. The ATM queries the bank system and displays the balance on screen.
- **Relationships:**
  - Connected to both Customer and Bank actors.

#### 4. Deposit Funds
- **Actor(s):** Customer, Bank
- **Description:** The customer deposits cash or cheques into their account. The ATM records the deposit and notifies the bank system to update the account balance.
- **Relationships:**
  - Connected to both Customer and Bank actors.

#### 5. Change PIN
- **Actor(s):** Customer
- **Description:** The customer changes their existing PIN to a new one. The system validates the old PIN, accepts the new PIN (with confirmation), and updates the record.
- **Relationships:**
  - Connected to the Customer actor only.

#### 6. Register New Card
- **Actor(s):** Customer
- **Description:** A new customer or existing customer registers a new card with the ATM/bank system. This use case includes sub-processes for filling in registration details and retrieving a card ID.
- **Relationships:**
  - Has `«include»` with **Fill form** — filling in a registration form is a mandatory part of this use case.
  - Has `«include»` with **Get card ID** — retrieving the card ID is a mandatory part of this use case.

---

### Extended / Included Use Cases (Outside or Sub-Use Cases)

#### 7. Invalid Card or PIN
- **Type:** Exception / Error case
- **Description:** Triggered when the customer provides invalid card credentials or an incorrect PIN during authentication. The system displays an error message and may lock the card after repeated failures.
- **Relationship:** `«exclude»` from **Authenticate** — this use case is invoked when authentication fails.

#### 8. Print Mini Statement
- **Type:** Optional extension
- **Description:** An optional feature where the customer receives a printed summary of recent transactions. This can be triggered after certain transactions such as cash withdrawal.
- **Relationship:** `«extend»` into **Withdraw Cash** — this use case extends the withdrawal flow optionally.

#### 9. Fill Form
- **Type:** Included sub-use case
- **Description:** As part of registering a new card, the customer must fill in a registration form with personal details.
- **Relationship:** `«include»` from **Register New Card** — always executed as part of card registration.

#### 10. Get Card ID
- **Type:** Included sub-use case
- **Description:** During card registration, the system retrieves or assigns a unique card ID to the newly registered card.
- **Relationship:** `«include»` from **Register New Card** — always executed as part of card registration.

---

## Relationships Summary

| Relationship | From | To | Type | Meaning |
|---|---|---|---|---|
| Association | Customer | Authenticate | Association | Customer initiates authentication |
| Association | Customer | Withdraw Cash | Association | Customer initiates withdrawal |
| Association | Customer | Check Balance | Association | Customer checks balance |
| Association | Customer | Deposit Funds | Association | Customer deposits funds |
| Association | Customer | Change PIN | Association | Customer changes PIN |
| Association | Customer | Register New Card | Association | Customer registers a new card |
| Association | Bank | Check Balance | Association | Bank processes balance query |
| Association | Bank | Deposit Funds | Association | Bank processes deposit |
| «exclude» | Authenticate | Invalid Card or PIN | Exclude | If authentication fails, error case is triggered |
| «extend» | Print Mini Statement | Withdraw Cash | Extend | Mini statement is optionally printed after withdrawal |
| «include» | Register New Card | Fill Form | Include | Filling form is mandatory for registration |
| «include» | Register New Card | Get Card ID | Include | Getting card ID is mandatory for registration |

---

## UML Relationship Types Used

### `«include»`
An `«include»` relationship means the **base use case always invokes the included use case** as part of its execution. It is mandatory — the included use case cannot be skipped.

In this diagram:
- Register New Card always includes Fill Form.
- Register New Card always includes Get Card ID.

### `«extend»`
An `«extend»` relationship means the **extending use case adds optional or conditional behavior** to the base use case. The base use case can function without it.

In this diagram:
- Print Mini Statement extends Withdraw Cash — the customer may or may not choose to print a statement.

### `«exclude»`
(Sometimes written as `«excludes»` in informal UML) This relationship indicates that **a use case is specifically excluded or branched off** under a failure or alternate condition. It is not standard UML but is commonly used in academic and industry contexts to represent error or exception paths.

In this diagram:
- Invalid Card or PIN is triggered when Authenticate fails.

### Association
A plain line between an actor and a use case indicates the actor **participates in** that use case. No arrowhead means the interaction is bidirectional.

---

## Use Case Flow Descriptions

### Primary Flow: Customer Withdraws Cash

1. Customer inserts card → **Authenticate** is triggered.
2. Customer enters PIN → system validates credentials.
   - If invalid: **Invalid Card or PIN** use case is triggered → error displayed.
3. Customer selects "Withdraw Cash" → **Withdraw Cash** use case begins.
4. Customer enters amount → system checks balance and dispenses cash.
5. Optionally: **Print Mini Statement** extends this flow if the customer requests it.

### Primary Flow: Customer Registers a New Card

1. Customer selects "Register New Card" → use case begins.
2. System automatically includes **Fill Form** → customer fills in personal details.
3. System automatically includes **Get Card ID** → unique card ID is assigned/retrieved.
4. Registration is complete.

### Primary Flow: Check Balance

1. Customer authenticates (see above).
2. Customer selects "Check Balance".
3. ATM queries the **Bank** system.
4. Balance is displayed on screen.

---

## Design Notes

- The diagram follows **UML 2.x** notation conventions.
- The **system boundary** (rectangle) clearly separates internal ATM functionality from external actors.
- **Customer** is placed on the left (primary actor, initiates flows), and **Bank** is placed on the right (secondary actor, supports flows).
- Dashed arrows with stereotype labels (`«include»`, `«extend»`, `«exclude»`) distinguish relationship types from plain associations.
- Use cases are represented as **ovals/ellipses** as per UML standards.
- Actors are represented as **stick figures** with labels.

---

## Possible Extensions to This Diagram

The following use cases could be added in a more complete version of the ATM system:

- **Transfer Funds** — transfer between own accounts or to another account.
- **Request Cheque Book** — request a physical cheque book via ATM.
- **Pay Bills** — pay utility or other bills directly from the ATM.
- **Unlock Card** — unlock a card that was blocked due to too many failed PIN attempts.
- **Language Selection** — choose the display language before performing transactions.

---

## Tools & Standards

| Item | Detail |
|---|---|
| Diagram Type | UML Use Case Diagram |
| UML Version | UML 2.x |
| Actors | Customer (primary), Bank (secondary) |
| Total Use Cases | 10 |
| Relationship Types | Association, «include», «extend», «exclude» |
