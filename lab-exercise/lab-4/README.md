# ATM Transaction Sequence Diagram — README

## Overview

This document describes the UML sequence diagram for a standard ATM (Automated Teller Machine) cash withdrawal transaction. The diagram models the interactions between four key participants across three conditional (alt) blocks that handle both success and failure paths.

---

## Participants / Actors

| Participant   | Role |
|---------------|------|
| **Customer**  | The end user physically interacting with the ATM machine. |
| **ATM**       | The hardware/software terminal that mediates all user interactions. |
| **Bank Server** | The backend authentication and transaction processing system. |
| **Bank Account** | The data store representing the customer's actual account and funds. |

---

## Message Flow

### Step 1 — Card Insertion

| # | From     | To          | Message       | Type    |
|---|----------|-------------|---------------|---------|
| 1 | Customer | ATM         | Insert card   | Sync    |
| 2 | ATM      | Bank Server | Verify card   | Sync    |

The customer initiates the session by inserting their card. The ATM immediately forwards a card verification request to the Bank Server.

---

### Step 2 — Card Validation (ALT Block 1)

**Condition: `[card valid]`**

| # | From        | To       | Message      | Type    |
|---|-------------|----------|--------------|---------|
| 3 | Bank Server | ATM      | Card OK      | Return  |
| 4 | ATM         | Customer | Request PIN  | Sync    |

**Condition: `[else]`**

| # | From | To       | Message     | Type   |
|---|------|----------|-------------|--------|
| 3b | ATM | Customer | Eject card  | Return |

If the card is recognised by the Bank Server, the ATM prompts the customer for their PIN. Otherwise, the card is ejected immediately.

---

### Step 3 — PIN Entry & Verification

| # | From     | To          | Message    | Type |
|---|----------|-------------|------------|------|
| 5 | Customer | ATM         | PIN entered | Sync |
| 6 | ATM      | Bank Server | Verify PIN  | Sync |

The customer enters their PIN which is transmitted securely to the Bank Server for verification.

---

### Step 4 — PIN Validation (ALT Block 2)

**Condition: `[PIN valid]`**

| # | From        | To       | Message        | Type   |
|---|-------------|----------|----------------|--------|
| 7 | Bank Server | ATM      | PIN OK         | Return |
| 8 | ATM         | Customer | Request amount | Sync   |

**Condition: `[else]`**

| # | From | To       | Message           | Type   |
|---|------|----------|-------------------|--------|
| 7b | ATM | Customer | PIN invalid, eject | Return |

If the PIN is correct, the ATM prompts the customer to enter the desired withdrawal amount. Otherwise, authentication fails and the card is ejected.

---

### Step 5 — Amount Entry & Transaction Start

| # | From     | To          | Message           | Type |
|---|----------|-------------|-------------------|------|
| 9  | Customer | ATM         | Amount entered    | Sync |
| 10 | ATM      | Bank Server | Start transaction | Sync |
| 11 | Bank Server | Bank Account | Sufficient funds? | Sync |

The customer specifies the withdrawal amount. The ATM triggers a transaction on the Bank Server, which then queries the Bank Account to check if sufficient funds are available.

---

### Step 6 — Funds Check & Dispensing (ALT Block 3)

**Condition: `[funds ok]`**

| # | From         | To          | Message           | Type   |
|---|--------------|-------------|-------------------|--------|
| 12 | Bank Account | Bank Server | Funds available   | Return |
| 13 | Bank Account | Bank Server | Withdraw amount   | Sync   |
| 14 | Bank Server  | ATM         | Transaction OK    | Return |
| 15 | ATM          | Customer    | Dispense cash     | Sync   |

**Condition: `[else]`**

| # | From        | To       | Message             | Type   |
|---|-------------|----------|---------------------|--------|
| 12b | Bank Server | ATM   | Insufficient funds  | Return |
| 13b | ATM         | Customer | Transaction failed | Sync   |

If funds are available, the Bank Account is debited, the Bank Server confirms to the ATM, and cash is dispensed to the customer. If not, the transaction fails and the customer is notified.

---

### Step 7 — Session End

| # | From | To       | Message    | Type |
|---|------|----------|------------|------|
| 16 | ATM | Customer | Eject card | Sync |

Regardless of the transaction outcome, the ATM ejects the card to end the session.

---

## Conditional Logic Summary

| ALT Block | Condition     | Success Path                      | Failure Path        |
|-----------|---------------|-----------------------------------|---------------------|
| 1         | Card valid?   | Request PIN from customer         | Eject card          |
| 2         | PIN valid?    | Request withdrawal amount         | PIN invalid, eject  |
| 3         | Funds ok?     | Dispense cash, transaction OK     | Transaction failed  |

---

## Key Design Notes

- **Solid arrows (→)** represent synchronous request messages.
- **Dashed arrows (←)** represent return/response messages.
- **Red dashed arrows** indicate error/failure responses.
- **Green dashed arrows** indicate successful return messages.
- All three `alt` blocks implement a two-branch conditional: a happy path and an `[else]` failure path.
- The card is always ejected at the end of a session — whether the transaction succeeded, failed, or was rejected at authentication.
- The Bank Account is only contacted at the funds-check stage; card and PIN validation are handled entirely by the Bank Server.

---

## Error / Failure Paths

| Failure Scenario      | Point of Detection | System Response                  |
|-----------------------|--------------------|----------------------------------|
| Invalid card          | Bank Server        | ATM ejects card immediately      |
| Invalid PIN           | Bank Server        | ATM ejects card, denies access   |
| Insufficient funds    | Bank Account       | ATM notifies customer, no cash   |

---

## Summary

This sequence diagram captures a complete ATM cash withdrawal session, from card insertion through to cash dispensing or failure notification. It clearly separates responsibilities across the four participants and shows how the system handles three distinct failure points through conditional `alt` blocks. The design ensures that the customer's card is always returned at the end of the session, providing a safe and predictable user experience.
