# Medical clinic System - class diagram

## 📌 Overview

This system models medical operations including patients, doctors, appointments, and medical records.

---

## 🧩 Classes Description

### Person (Abstract)

* Attributes: personID, name, dateOfBirth
* Methods: getDetails(), updateContact()

### Doctor (inherits Person)

* Attributes: licenseNo, specialization, department
* Methods: diagnose(), prescribe(), viewSchedule()

### Patient (inherits Person)

* Attributes: patientID, bloodType, insuranceNo
* Methods: bookAppointment(), viewRecords(), cancelAppointment()

### Nurse

* Attributes: nurseID, ward
* Methods: assistDoctor(), takeVitals(), administerMeds()

### Appointment

* Attributes: appointmentID, dateTime, status
* Methods: confirm(), cancel(), reschedule()

### MedicalRecord

* Attributes: recordID, diagnosis, visitDate
* Methods: addEntry(), getHistory(), updateRecord()

---

## 🔗 Relationships

* Person → Doctor, Patient: Inheritance
* Doctor → Appointment: Association (1 to many)
* Patient → Appointment: Association (1 to many)
* Doctor → Nurse: Aggregation
* Patient → MedicalRecord: Composition

---

## 🔄 System Flow

1. Patient books appointment
2. Appointment is assigned to doctor
3. Doctor diagnoses patient
4. Medical record is updated
5. Nurses assist doctors

---

## 🔑 Key Concepts

* Abstract Class: Person
* Composition: Patient owns MedicalRecord
* Aggregation: Doctor works with Nurses
* Association: Appointment connects Doctor & Patient
