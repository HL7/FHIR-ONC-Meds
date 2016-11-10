### Guidance

 1. Medication lists
  1. Active vs Medication History vs All medications
   -  [ONC definitions](Use-Cases.html) of active and historical lists
   -  Ability to retrieve proposed meds as well
  1. All resources can be used to construct the list
	- May only use MedicationStatement converting all MedicationResources to it.
	  - ( Discussion: may need some additional clarification surrounding the use of MedicationStatement and what if any information is lost / using `derivedFrom` element to capture that information ( # of refills/who/substitutions))
  - Guidance on who responsible for ‘deciding’ active – the server or client?
  - Guidance on How to deal with the “Not-Given” medications

### Profiles

1. [MedicationAdministration Profile](StructureDefinition-medicationadministration.html):  The MedicationAdministration resources can be used to record a patient consuming or otherwise being administered a medication.  For more information about the context for their usages, refer to the medication domains's [boundaries section].  This profile sets minimum expectations for the MedicationAdministration resource to record, search and fetch medications associated with a patient. It identifies which core elements, extensions, vocabularies and value sets **SHALL** be present in the resource when using this profile.
1. [MedicationDispense Profile](StructureDefinition-medicationdispense.html):  The MedicationDispense resources can be used to record a patient consuming or otherwise being administered a medication.  For more information about the context for their usages, refer to the medication domains's [boundaries section].  This profile sets minimum expectations for the MedicationAdministration resource to record, search and fetch medications associated with a patient. It identifies which core elements, extensions, vocabularies and value sets **SHALL** be present in the resource when using this profile.
