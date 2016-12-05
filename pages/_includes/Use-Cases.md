

## 1.	Patient and Provider access to a patient’s active and historical medication list

1.	This use case adopts the [Use cases for the Argonaut Project](http://argonautwiki.hl7.org/images/e/ec/Argonaut_UseCasesV1-1.pdf) specifically within the scope of accessing medication lists as prescribed in [Meaningful Use 2015 §?170.302(d)](https://www.healthit.gov/sites/default/files/2015Ed_CCG_a7-Medication-list.pdf). ___Maintain active medication list. Enable a user to record, change, and access a patient's active medication list as well as medication history: (i) Ambulatory setting. Over multiple encounters; or (ii) Inpatient setting. For the duration of an entire hospitalization.___
 1. Definitions:
      - Active Medication List – A list of medications that a given patient is currently taking. 
      - Medication history - include a record of prior modifications to a patient’s medications
      - __Question for group__: do we want to add proposed medications to create a third option of "all medication" = past present and future?
 1. Data Access and Retrieval which is documented in the US-Core Profiles for [MedicationStatement](http://ig.fhir.me/Healthedata1/US-Core/StructureDefinition-us-core-medicationstatement.html) and [MedicationRequest](http://ig.fhir.me/Healthedata1/US-Core/StructureDefinition-us-core-medicationrequest.html)
 1. Record and change Medications:  Patient or Provider adding new medication to patient’s (“active”) medication list.
       - Requires write access to patient's data
 1.  Actors: Providers/Patient/EHR (See [Use cases for the Argonaut Project](http://argonautwiki.hl7.org/images/e/ec/Argonaut_UseCasesV1-1.pdf))
 1.  FHIR Resources
      - MedicationStatement  (Most common)
      - MedicationRequest   (Nee MedicationOrder)
      - Medication
      - MedicationAdministration ( less common - context: inpatient)
      - MedicationDispense (least common - is not widely adopted)
  1.  Terminology
      - [RxNorm](https://www.nlm.nih.gov/research/umls/rxnorm/)

## 2. Create new outpatient Prescription

  1.  Actors: Providers/EHR (See [Use cases for the Argonaut Project](http://argonautwiki.hl7.org/images/e/ec/Argonaut_UseCasesV1-1.pdf))
  1.  Workflow Description:  See a general discussion of FHIR workflow [here](http://build.fhir.org/workflow.html).
    - __Question for group__: Do we want to focus on a single or few patterns?
(http://build.fhir.org/workflow.html#commpatterns).
  1.  FHIR Resources
      - MedicationRequest   (Nee MedicationOrder)
      - Medication
      - [Task](http://build.fhir.org/task.html) for Workflow
  1.  Terminology
      - RxNorm

## 3. Dispense medication from Pharmacy

 1. Background:
    - Not supported by community pharmacies since they are mandated to use [NCPDP Script](https://www.ncpdp.org/NCPDP/media/pdf/EprescribingFactSheet.pdf)
    - Currently in hospital pharmacies using HL7 V2 RXD messaging.
    - Potential use for "secondary" recording of data.
      - For example:  script history, real time benefit checking or Prescription Drug Monitoring
  1. Actors: TBD
  1.  FHIR Resources
      - MedicationDispense
  1.  Terminology
      - [RxNorm](https://www.nlm.nih.gov/research/umls/rxnorm/)







