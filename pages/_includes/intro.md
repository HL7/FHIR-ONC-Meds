## {{site.data.fhir.igName}} Implementation Guide

source pages/\_include/{{page.md_filename}}.md  file

### Introduction

The {{site.data.fhir.igName}} Implementation Guide is based upon the [FHIR STU3](todo.html) standard and promotes consistent use of the FHIR pharmacys resources in the US Realm Electronic Health Record Systems (EHRs) for providing patient and provider access to patient medications. It provides specific guidance on accessing patients active and historical medications and medication orders. It also proposes a specific approach to creating new outpatient prescriptions and dispensing medications.

### Scope

This guide applies the general [Argonaut Project scope statement](todo.html) and adopts it specifically for enabling a user to access, record, change a patient’s active medications as well as medication history.  This covers the  Meaningful Use 2015 §?170.302(d) certification requirement for EHRs.

#### Actors

The following actors are defined:

- US Meds Requestor: An application that initiates a data access request to retrieve patient data. This can be thought of as the client in a client-server interaction and the user is either a provider or a patient.
- US Meds Responder: A product that responds to the data access request providing patient data. This can be thought of as the server in a client-server interaction and typically is part of an EHRs.

#### Use Cases

The following specific uses case are defined:

1. [Patient and Provider access to a patient’s active and historical medication list](todo.html)
1. [Medication list updates](todo.html)
1. [Create new outpatient Prescription](todo.html)
1. [Dispense medication from Pharmacy](todo.html)

A detailed description and guidance for each is given in the [General Guidance](todo.html) section.

###  Profiles

This IG uses the following US-Core medication Profiles from the  [US Core Implementation Guide](http://ig.fhir.me/Healthedata1/US-Core/):

- [US Core Medication](http://ig.fhir.me/Healthedata1/US-Core/StructureDefinition-us-core-medication.html)
- [US Core Medication Order](http://ig.fhir.me/Healthedata1/US-Core/StructureDefinition-us-core-medicationrequest.html)
- [US Core Medication Statement](http://ig.fhir.me/Healthedata1/US-Core/StructureDefinition-us-core-medicationstatement.html)

In addition these two profiles have been defined for this implementation guide:

{% include list-simple-profiles.xhtml %}

  Each profile defines the minimum mandatory elements, extensions and terminology requirements that MUST be present. For each profile requirements and guidance are given in a simple narrative summary. A formal hierarchical table that presents a logical view of the content in both a differential and snapshot view is also provided along with references to appropriate terminologies and examples. In addition each profile has a “Quick Start” section which is intended as an implementer friendly overview of the required search and read operations.

### Security

Refer to the US Core Implementation Guide's [General Security Considerations](todo.html) page for a discussion of the security conformance requirements.
