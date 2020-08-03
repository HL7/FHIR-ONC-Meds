---
title: Home Page
layout: default
active: home
---


<!-- source pages/\_include/{{page.md_filename}}.md  file -->

<!-- TOC  the css styling for this is \pages\assets\css\project.css under 'markdown-toc'

* Do not remove this line (it will not be displayed)
{:toc}

end TOC -->

<!-- end TOC -->

<div markdown='1' class="highlight-note">
### September 2020 Ballot Review

Most of the original guidance published in the US Meds IG is updated and migrated to the latest version of US Core. Rather than duplicate the guidance, references to the relevant US Core sections are noted.

Beyond updating references to US Core, updates for this ballot are concentrated to the mapping on the [PDMP](pdmp.html) page. 

</div>
{: .highlight-note}

### Introduction

The US Med Implementation Guide is based upon the [FHIR Version 4.0.1]({{ site.data.fhir.path }}index.html) specification. It promotes consistent implementation of the pharmacy FHIR resources in US Realm Electronic Health Record Systems (EHRs) to provide patient and provider access to patient medications. This IG provides specific guidance on how to access a patient's active and historical medications, including prescriptions, dispenses, administrations and statements.  It also proposes future areas of guidance on how to create new outpatient prescriptions, record a dispensed medication, and mappings to support PDMP integration.

### Scope

This guide applies to the general [Argonaut Project use cases](http://argonautwiki.hl7.org/images/e/ec/Argonaut_UseCasesV1-1.pdf) and adopts it specifically to enable a user to access, record, change a patient’s active medications as well as a medication history.  This covers the  [Meaningful Use 2015 §?170.302(d)](https://www.healthit.gov/sites/default/files/2015Ed_CCG_a7-Medication-list.pdf) certification requirement for EHRs.

#### Actors

The following actors are defined:

- US Meds Requestor: An application that initiates a data access request to retrieve patient medication data. This can be thought of as the client in a client-server interaction and the user is either a patient or provider.
- US Meds Responder: A product that responds to the data access request providing patient medication data. This can be thought of as the server in a client-server interaction and typically is part of an EHRs.

#### Use Cases

The following specific uses case are defined:

1. [Patient and Provider access to a patient’s active and historical medication list](guidance.html#uc-1)
1. [Medication list updates](guidance.html#uc-2)
1. [Create new outpatient Prescription](guidance.html#uc-3)
1. [Dispense medication from Pharmacy](guidance.html#uc-4)

A detailed description and guidance for each is given in the [General Guidance](guidance.html) section.

###  Profiles

This IG uses the following US Core Profiles from the US Core Implementation Guide:

- [US Core Medication]({{ site.data.fhir.uscore }}StructureDefinition-us-core-medication.html)
- [US Core Medication Request]({{ site.data.fhir.uscore }}StructureDefinition-us-core-medicationrequest.html)

In addition, two new profiles have been defined as part of this implementation guide:

{% include list-simple-profiles.xhtml %}


  Each profile defines the minimum mandatory elements, extensions and terminology requirements that MUST be present. Requirements and guidance are given in a simple narrative summary. A formal hierarchical table that presents a logical view of the content in both a differential and snapshot view is also provided along with references to appropriate terminologies and examples. In addition each profile has a “Quick Start” section, which is intended as an implementer friendly overview of the required search and read operations.

### Security

Refer to the US Core Implementation Guide's [General Security Considerations]({{ site.data.fhir.uscore }}security.html) page for a discussion of the security conformance requirements.

----

Primary Authors: Brett Marquard, Nagesh Bashyam, Melva Peters, Eric Haas

Sponsoring HL7 Workgroup : [Pharmacy](http://www.hl7.org/Special/committees/medication/index.cfm)
