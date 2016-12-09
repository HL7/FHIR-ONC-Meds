The MedicationDispense resources can be used to record a medications that have been dispensed for a named person/patient.  For more information about the context for their usages, refer to the medication domains's [boundaries section].  This profile sets minimum expectations for the MedicationAdministration resource to record, search and fetch medications associated with a patient. It identifies which core elements, extensions, vocabularies and value sets **SHALL** be present in the resource when using this profile.

**Example Usage Scenarios:**

The following are example usage scenarios for the
DAF-MedicationDispense profile:

-  Query for medications that have been dispensed to a particular patient


##### Mandatory Data Elements and Terminology


The following data-elements are mandatory (i.e data MUST be present). These are presented below in a simple human-readable explanation.  Profile specific guidance and examples are provided as well.  The [**Formal Profile Definition**](#profile) below provides the  formal summary, definitions, and  terminology requirements.  

**Each MedicationDispense must have:**

1.  a status
1.  a medication
1.  a patient
1.  a date or date range

In addition, the following data-elements must be [supported](http://hl7.org/FHIR/us/daf/2016Sep/daf-core.html#mustsupport).

*If the data is present, MedicationDispense shall include:*

1. who dispensed
2. dosage information


**Profile specific implementation guidance:**

*  The MedicationDispense and MedicationOrder resources can represent a medication, using either a code or refer to a [Medication] resource.  The server application can choose one way or both methods,  but the client application must support both methods.  More specific guidance is provided in the [conformance](CapabilityStatements-tbd.html) resource for this profile


  [Medication Clinical Drug (RxNorm)]: valueset-daf-medication-codes.html
  [MedicationOrderStatus]: http://hl7.org/fhir/us/daf/valueset-medication-order-status.html
[MedicationDispenseStatus]: http://hl7.org/fhir/us/daf/valueset-medication-statement-status.html
[MedicationDispense]:http://build.fhir.org/medicationdispense.html
 [MedicationOrder]: http://build.fhir.org/medicationorder.html
 [Medication]:http://build.fhir.org/medication.html
 [Conformance]: daf-core-medicationstatement-conformance.html
 [boundaries section]: http://build.fhir.org/medicationdispense.html#bnr
