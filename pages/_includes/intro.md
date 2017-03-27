## {{site.data.fhir.igName}} Implementation Guide

source pages/\_include/{{page.md_filename}}.md  file

### Introduction

The {{site.data.fhir.igName}} Implementation Guide is based upon the [FHIR STU3]({{ site.data.fhir.path }}) specification. It promotes the consistent use of the pharmacy FHIR resources in the US Realm Electronic Health Record Systems (EHRs) to provide patient and provider access to patient medications. This IG provides specific guidance on how to access patients' active and historical medications, including prescriptions, dispenses, administrations and statements.  It also proposes future areas of guidance on how to create new outpatient prescriptions and to record a dispensed medication.

### Scope

This guide applies the general [Argonaut Project scope statement](http://argonautwiki.hl7.org/images/e/ec/Argonaut_UseCasesV1-1.pdf) and adopts it specifically to enable a user to access, record, change a patient’s active medications as well as medication history.  This covers the  [Meaningful Use 2015 §?170.302(d)](https://www.healthit.gov/sites/default/files/2015Ed_CCG_a7-Medication-list.pdf) certification requirement for EHRs.

#### Actors

The following actors are defined:

- US Meds Requestor: An application that initiates a data access request to retrieve patient data. This can be thought of as the client in a client-server interaction and the user is either a provider or a patient.
- US Meds Responder: A produ that responds to the data access request providing patient data. This can be thought of as the server in a client-server interaction and typically is part of an EHRs.

#### Use Cases

The following specific uses case are defined:

1. [Patient and Provider access to a patient’s active and historical medication list](guidance.html#uc-1)
1. [Medication list updates](guidance.html#uc-2)
1. [Create new outpatient Prescription](guidance.html#uc-3)
1. [Dispense medication from Pharmacy](guidance.html#uc-4)

A detailed description and guidance for each is given in the [General Guidance](guidance.html) section.

###  Profiles

This IG uses the following US-Core Profiles from the US Core Implementation Guide:

- [US Core Medication]({{ page.us-core-base }}StructureDefinition-us-core-medication.html)
- [US Core Medication Request]({{ page.us-core-base }}StructureDefinition-us-core-medicationrequest.html)
- [US Core Medication Statement]({{ page.us-core-base }}StructureDefinition-us-core-medicationstatement.html)

In addition two profiles have been defined for this implementation guide:

{% include list-simple-profiles.xhtml %}

  Each profile defines the minimum mandatory elements, extensions and terminology requirements that MUST be present. For each profile requirements and guidance are given in a simple narrative summary. A formal hierarchical table that presents a logical view of the content in both a differential and snapshot view is also provided along with references to appropriate terminologies and examples. In addition each profile has a “Quick Start” section which is intended as an implementer friendly overview of the required search and read operations.

### Security

Refer to the US Core Implementation Guide's [General Security Considerations]({{ page.us-core-base }}security.html) page for a discussion of the security conformance requirements.
