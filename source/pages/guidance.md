---
title: Definitions, Interpretations and Requirements common to all US-Meds actors
layout: default
active: guidance
topofpage: true
f: http://build.fhir.org/
---

* Do not remove this line (it will not be displayed)
{:toc}

<!-- end TOC -->

This section outlines the definitions and interpretations and specific guidance for the US Meds IG use cases.  Most of the original guidance published in the US Meds IG is updated and migrated to the latest version of US Core. Rather than duplicate the guidance, references to the relevant US Core sections are noted.

<!-- source pages/\_include/{{page.md_filename}}.md  file -->

### Background on the FHIR Medications resources

Guidance now incorporated in the [US Core Medication Background](http://hl7.org/fhir/us/core/all-meds.html#background-on-the-fhir-medications-resources).

### Use Case 1 - Patient and Provider access to a patients' medications
{: #uc-1}

The guidance below addresses how a patient or a provider can access a patients' active, historical and future (planned) medications list.     

#### Fetching All Medications, Active Medications, and All Medications for an Encounter

Guidance now incorporated in the [US Core Medication Fetching Guidance](http://hl7.org/fhir/us/core/all-meds.html#fetching-all-medications-active-medications-and-all-medications-for-an-encounter).

**Deduplication of Data**

Guidance now incorporated in the [US Core Medication De-duplication Guidance](http://hl7.org/fhir/us/core/all-meds.html#de-duplication-of-data).

**“Not Taken” Medications**

For specific guidance on how to determine if the patient has taken the medication, see the [MedicationStatement Resource Notes]({{ site.data.fhir.path }}medicationstatement.html#11.4.3.3) section in the FHIR Specification.

#### Fetching Active Medications Orders

Guidance now incorporated in the [US Core Medication All Active List Guidance](http://hl7.org/fhir/us/core/all-meds.html#get-all-active-medications).

### Use Case 2 - Medication list update
{: #uc-2}

This use case adopts the use cases for the Argonaut Project and US Core specifically within the scope of recording and changing medications as prescribed in Meaningful Use 2015 §?170.302(d) - *Maintain active medication list. Enable a user to record, change, and access a patient’s active medication list as well as medication history: (i) Ambulatory setting. Over multiple encounters; or (ii) Inpatient setting. For the duration of an entire hospitalization.*  These interactions require "write access" to the EHR systems. US Core and Argonaut have minimal experience writing medications to a FHIR server.   

This Use Case will be the focus of future versions of this IG.

### Use Case 3 - Create new outpatient Prescription
{: #uc-3}

This use case involves the provider creating a new outpatient prescription using the MedicationRequest, MedicationStatement and Medication resources.  Subsequent workflow steps depend on specific pattern agreed upon by business partners. See a list of possible scenarios in the [FHIR workflow sections]({{ site.data.fhir.path }}workflow-communications.html) in the FHIR specification.

At the time of this publication, the US implementers have not used the MedicationRequest, MedicationStatement and Medication resources resource sufficiently to provide meaningful feedback on best practices and guidance.  This Use Case will be the focus of future versions of this IG.


### Use Case 4 - Dispense medication from Pharmacy
{: #uc-4}

Background: Community pharmacies in the US are mandated to use [NCPDP](https://www.ncpdp.org/) Script standard to send dispensed medications for adjudication and hospital pharmacies are typically using HL7 V2 RXD messaging.  Possible scenarios where this resource could be used for interchange of dispensing data include:

- Dispense Histories
- Real-time Confirmation of Prescription Drug Benefits
- Prescription Drug Monitoring Programs (PDMPs)

At the time of this publication, the US implementers have not used the MedicationDispense resource sufficiently to provide meaningful feedback on best practices and guidance.  This Use Case will be the focus of future versions of this IG.
