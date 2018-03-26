**Note**: Not open for May 2018 ballot

The [MedicationDispense] resource can be used to record a medication that has been dispensed for a named person/patient.  For more information about the context and usage, refer to the resource page in the FHIR specification.  This profile sets minimum expectations for recording, searching and fetching a patient's MedicationAdministration resource. It identifies which core elements, extensions, vocabularies and value sets **SHALL** be present in the resource when using this profile.

**Example Usage Scenarios:**

The following are example usage scenarios:

-  Query for medications that have been dispensed to a particular patient


##### Mandatory Data Elements and Terminology


The following data-elements are mandatory (i.e data MUST be present). These are presented below in a simple human-readable explanation.  Profile specific guidance and examples are provided as well.  The [**Formal Profile Definition**](#profile) below provides the  formal summary, definitions, and  terminology requirements.  

**Each MedicationDispense must have:**

1.  a status
1.  a medication
1.  a patient
1.  a date or date range

In addition, a system [*Must Support*](http://hl7.org/FHIR/us/daf/2016Sep/daf-core.html#mustsupport).

1. who dispensed
2. dosage information


**Profile specific implementation guidance:**

*  None

##### Examples

- [Med Dispense 1](MedicationDispense-meddispense-1.html)

  [Medication Clinical Drug (RxNorm)]: valueset-daf-medication-codes.html
 [MedicationDispense]:{{ site.data.fhir.path }}/medicationdispense.html
