# Car Insurance System - Class Diagram

## 📌 Overview

This system represents an insurance management system handling policies, claims, payments, and vehicles.

---

## 🧩 Classes Description

### Customer

* Attributes: customerID, name, address, drivingLicence
* Methods: getDetails(), updateContact(), fileClaim()

### InsurancePolicy

* Attributes: policyNo, startDate, endDate, premiumAmount, coverageType
* Methods: renewPolicy(), cancelPolicy(), getCoverage()

### Vehicle

* Attributes: regNo, make, model, year, value
* Methods: getDetails(), calcPremium(), updateValue()

### Claim

* Attributes: claimID, incidentDate, claimAmount, status
* Methods: submitClaim(), approveClaim(), rejectClaim()

### Payment

* Attributes: paymentID, amount, paymentDate, method
* Methods: processPayment(), getReceipt(), refund()

### ComprehensivePlan (inherits InsurancePolicy)

* Attributes: accidentCover, theftCover
* Methods: getFullCoverage(), addRider()

### ThirdPartyPlan (inherits InsurancePolicy)

* Attributes: liabilityLimit, legalCover
* Methods: getLiabilityCover(), claimThirdParty()

---

## 🔗 Relationships

* Customer → InsurancePolicy: Composition
* InsurancePolicy → Vehicle: Aggregation
* Customer → Claim: Association (1 to many)
* Claim → Payment: Association
* InsurancePolicy → Payment: Association (1 to many)
* InsurancePolicy → Plans: Inheritance

---

## 🔄 System Flow

1. Customer purchases policy
2. Policy is linked to a vehicle
3. Customer pays premium (Payment)
4. In case of incident, customer files claim
5. Claim is processed and payment is issued

---

## 🔑 Key Concepts

* Composition: Policy depends on Customer
* Aggregation: Policy covers Vehicle
* Inheritance: Policy types (Comprehensive, ThirdParty)
* Association: Claims and Payments
