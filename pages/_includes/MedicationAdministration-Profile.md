The [MedicationAdministration] resources can be used to record a patient consuming or otherwise being administered a medication.  For more information about the context for their usages, refer to the medication domains's [boundaries section].  This profile sets minimum expectations for the MedicationAdministration resource to record, search and fetch medications associated with a patient. It identifies which core elements, extensions, vocabularies and value sets **SHALL** be present in the resource when using this profile.

**Example Usage Scenarios:**

The following are example usage scenarios for the
DAF-MedicationAdministration profile:

-   Query for medications that have been administered to a particular patient
- Query for all medications administered during an encounter
- Query for all medications which were not administered during an encounter


##### Mandatory Data Elements and Terminology


The following data-elements are mandatory (i.e data MUST be present). These are presented below in a simple human-readable explanation.  Profile specific guidance and an [example](#example) are provided as well.  The [**Formal Profile Definition**](#profile) below provides the  formal summary, definitions, and  terminology requirements.  

**Each MedicationAdministration must have:**

1.  a status
1.  a medication
1.  a patient
1.  a date or date range

In addition, the following data-elements must be [supported](http://hl7.org/FHIR/us/daf/2016Sep/daf-core.html#mustsupport).

*’If the data is present, MedicationAdministration shall include:**

1. who administrated
2. dosage information


**Profile specific implementation guidance:**

*  The MedicationAdministration can represent a medication, using either a code or refer to a [Medication] resource.  The server application can choose one way or both methods,  but the client application must support both methods.  More specific guidance is provided in the [conformance](conformance.html) resource for this profile


  [Medication Clinical Drug (RxNorm)]: valueset-daf-medication-codes.html
  [MedicationOrderStatus]: http://hl7.org/fhir/us/daf/valueset-medication-order-status.html
[MedicationAdministrationStatus]: http://hl7.org/fhir/us/daf/valueset-medication-Administration-status.html
[MedicationStatement]:http://build.fhir.org/medicationstatement.html
[MedicationAdministration]:http://build.fhir.org/medicationadministration.html
 [MedicationOrder]: http://build.fhir.org/medicationorder.html
 [Medication]:http://build.fhir.org/medication.html
 [Conformance]: daf-core-medicationAdministration-conformance.html
 [boundaries section]: http://build.fhir.org/medicationorder.html#bnr
