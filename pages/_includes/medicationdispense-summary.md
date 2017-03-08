#### Complete Summary of the Mandatory Requirements

1.  One status in `MedicationDispense.status` which has a [required](http://build.fhir.org/terminologies.html#required) binding to:
-   [MedicationDispenseStatus] value set.
1.  One medication via `MedicationDispense.medicationCodeableConcept` or `MedicationDispense.medicationReference`   
-  `MedicationDispense.medicationCodeableConcept` has an [extensible](http://build.fhir.org/terminologies.html#extensible) binding to [Medication Clinical Drug (RxNorm)] value set.
1.  One patient reference in `MedicationDispense.subject`
1.  One date in `MedicationDispense.whenHandedOver`

#### Summary of the Must Support Requirements

1.  One or more references to a patient, practitioner, or related-person in `MedicationDispense.performer.actor`
1.  Dosage Information in `MedicationDispense.dosageInstruction`

[Medication Clinical Drug (RxNorm)]: ValueSet-us-core-medication-codes.html
[MedicationDispenseStatus]: http://build.fhir.org/valueset-medication-dispense-status.html
