# ATM System - Class diagram

## 📌 Overview

This system models the working of an Automated Teller Machine (ATM), including account handling, transactions, and hardware components.

---

## 🧩 Classes Description

### ATM

* Attributes: atmID, location, cashAvailable
* Methods: authenticate(), dispenseCash(), showMenu()
* Acts as the central controller.

### BankAccount

* Attributes: accountNo, balance, pin
* Methods: getBalance(), validatePIN(), updateBalance()

### Transaction

* Attributes: transactionID, amount, timestamp
* Methods: execute(), getReceipt(), rollback()

### Withdrawal (inherits Transaction)

* Methods: execute(), checkLimit(), dispenseCash()

### Deposit (inherits Transaction)

* Methods: execute(), verifyCash(), creditAccount()

### CardReader

* Attributes: readerID, status
* Methods: readCard(), ejectCard(), validateCard()

---

## 🔗 Relationships

* ATM → BankAccount: Association (1 to many)
* ATM → Transaction: Aggregation (1 to many)
* ATM → CardReader: Composition (1 to 1)
* Transaction → Withdrawal, Deposit: Inheritance

---

## 🔄 System Flow

1. User inserts card (CardReader)
2. ATM authenticates user
3. User selects transaction
4. Transaction is processed (Withdrawal/Deposit)
5. Account is updated

---

## 🔑 Key Concepts

* Composition: Strong ownership (CardReader)
* Aggregation: Weak ownership (Transaction)
* Inheritance: Reusability (Transaction subclasses)
