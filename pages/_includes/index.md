# Pharmacy FHIR Maturity Project

## Introduction
This project supports the Pharmacy Workgroups efforts in moving the Pharmacy FHIR resources to a higher [FHIR maturity level](http://build.fhir.org/resource.html#maturity).  The key objective for this project is to promote consistent use of the FHIR pharmacy resources in the US for providing patient and provider access to patient medications. To achieve this the IG builds on prior work from the Argonaut Project, HL7 work groups and industry implementers to examine, implement, and test the following FHIR Medication related [resources](http://build.fhir.org/overview-clinical.html#2.4.1) and the corresponding US-Core medication Profiles:

- [Medication](http://build.fhir.org/medication.html) and [US Core Medication](http://ig.fhir.me/Healthedata1/US-Core/StructureDefinition-us-core-medication.html)
- [MedicationRequest](http://build.fhir.org/medicationrequest.html) and [US Core Medication Order](http://ig.fhir.me/Healthedata1/US-Core/StructureDefinition-us-core-medicationrequest.html)
- [MedicationStatement](http://build.fhir.org/medicationstatement.html) and [US Core Medication Statement](http://ig.fhir.me/Healthedata1/US-Core/StructureDefinition-us-core-medicationstatement.html)
- [MedicationAdministration](http://build.fhir.org/medicationadministration.html)
- [MedicationDispense](http://build.fhir.org/medicationdispense.html)

In addition to implementing the existing US-Core medication Profiles, the DAF DSTU2 profiles for `MedicationAdministration` and `MedicationDispense` have been refined into two new proposed US-Realm profiles.

Contents:

1. [Use Cases](Use-Cases.html):
1. [Guidance and Profiles](Guidance-and-Profiles.html) for
   1. MedicationAdministration
   1. MedicationDispense
   1. Medication Lists
      - Active
      - Medication History
      - All medications
1. [Lifecycle Diagrams](Lifecycle-Diagrams.html) for Medication Resources
