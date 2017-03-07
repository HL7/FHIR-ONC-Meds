
## Definitions, Interpretations and Requirements common to all US-Meds actors
{:.no_toc}

This section outlines the definitions and interpretations and specific guidance for the US-Meds IG use cases  The conformance verbs used are defined in [FHIR Conformance Rules]().  The [general guidance]() used in the US-Core IG apply to this guide as well.


source pages/\_include/{{page.md_filename}}.md  file

<!-- TOC -->

* Do not remove this line (it will not be displayed)
{:toc}

### Background on the FHIR Medications resources

The 5 FHIR medications resources concerned with the ordering, dispensing, administration and recording of medication are:

- [Medication]():  Represents the medication itself
- [MedicationRequest](): Represents a prescription - the instructions for the administration of a  medication.  
- [MedicationDispense](): Represents a response to a prescription - provision of a supply of a medication.
- [MedicationAdministration](): Represents the consumption or administration of a medication.
- [MedicationStatement](): Represents the record for past present and future medications taken

Details about each resource can be found in the FHIR specification.  As well, a gerneral discussion regarding the interaction between these resources is described in the FHIR [Medications Module]() and the [Guide to Resources]() Sections.

For this IG we are focused on access and updates to a patient's medications and the interaction between the medication order (*MedicationRequest*) and the medication record (*MedicationStatement*).  Therefore it is important to understand the relationships between these resources.  Figure 1 illustrates the workflow relationship between the order and the fulfillment as represented by the FHIR medication resources.

<!-- all this Bootstrap html for getting an image to fit nicely in text - 640 pix wide -->
file:///assets/images/usmed-fig1.png
<div>
<figure class="figure">
<figcaption class="figure-caption"><strong>Figure 1.</strong></figcaption>
  <img src="assets/images/usmed-fig1.png" class="figure-img img-responsive img-rounded center-block" alt="Workflow relationship between the order and the fulfillment">
</figure>

<!--
<img src="assets/images/usmed-fig1.png" class="img-responsive" alt="Workflow relationship between the order and the fulfillment">
-->
<p></p>
</div>

Figure 2 shows the information sources for MedicationStatement which include patient reported medications and medications directly derived from the system's MedicationRequest resources.  Note that patient reported medications may be directly reported by the patient or derived from variety of patient provided records including including other FHIR resources.

<!-- all this Bootstrap html for getting an image to fit nicely in text - 640 pix wide -->
file:///assets/images/usmed-fig2.png
<div>
<figure class="figure">
<figcaption class="figure-caption"><strong>Figure 2.</strong></figcaption>
  <img src="assets/images/usmed-fig2.png" class="figure-img img-responsive img-rounded center-block" alt="Workflow relationship between the order and the fulfillment">
</figure>

<!--
<img src="assets/images/usmed-fig1.png" class="img-responsive" alt="Workflow relationship between the order and the fulfillment">
-->
<p></p>
</div>


### US Meds UC-1 - Patient and Provider access to a patient’s active and historical medication list

The guidance below addressed how a client accesses a patient's medication list. This use case adopts the Use cases for the Argonaut Project and US Core specifically within the scope of accessing medication information as prescribed in Meaningful Use 2015 §?170.302(d).*Maintain active medication list. Enable a user to record, change, and access a patient’s active medication list as well as medication history: (i) Ambulatory setting. Over multiple encounters; or (ii) Inpatient setting. For the duration of an entire hospitalization.*

#### Fetching All Medications and Active Medications

"All medications" is defined as including all historical (prescribed and consumed), active, future (i.e. intended) and entered in error medications. "Active medications" is defined as including all prescribed and/or being consumed medications. They do not include past, future and entered in error medications.

A MedicationStatement resource query **SHALL** be all that is required to access the "all medications" and "all *active* medications" for a patient. The MedicationStatement **SHALL** include both patient reported medications and all medications directly derived from the system's MedicationRequest resources. For medications directly derived from the system's MedicationRequest resources:

   - The `MedicationStatement.derivedFrom` element **SHALL** reference the systems MedicationRequest resource
   - Only MedicationRequest resources with an `intent` of ‘order’ and a `status` of active will be accessible through MedicationStatement. ( i.e. MedicationStatement won’t be derived from a MedicationRequest resources with an `intent` of ‘order-instance’.)
   - For more information about the dosage etc, the client **MAY** need to query the MedicationRequest resource using the `derivedFrom` reference.

**Deduplication of Data**

Duplication of medications may exist between the sources of information.  The deduplication activity **MAY** be provided by the server but **SHOULD** be provided by the client.

**“Not-Given” Medications**

For specific guidance on how to interpret the “Not-Given” element, see the [MedicationStatement Resource Notes]() section in the FHIR Specification. An example is provided which contains an instance using this element.  

**Quick Start:**

**1\.** Get "all medications" for a patient by querying MedicationStatement using the `patient` search parameter.  This query is identical to that defined in the [US Core implementation Guide](). See that IG for further details.

**2\.** Get "all *active* medications" for a patient by querying MedicationStatement using the `patient` and `status`='active' search parameters. This search returns a Bundle of all active MedicationStatements and if the \_include parameter is used, Medication resources for the specified patient.

`GET /MedicationStatement?patient=[id]&status=active{&_include=MedicationStatement:medication}`

Example:

 - GET [base]/MedicationStatement?patient=14676&status=active
 - GET [base]/MedicationStatement?patient=14676&status=active&\_include=MedicationStatement:medication

*Support:* Mandatory for server and client to support search by patient and status parameter.  Mandatory for client to support the \_include parameter.  Optional for server to support the \_include parameter.


**3\.** Get “all *active* medications” for an encounter by querying MedicationStatement using the patient and status=’active’ and encounter search parameters. This search returns a Bundle of all active MedicationStatements for an encounter and if the \_include parameter is used, Medication resources for the specified patient.

`GET /MedicationStatement?patient=[id]&encounter=[id]&status=active{&_include=MedicationStatement:medication}`

Example:

- GET [base]/MedicationStatement?patient=14676&encounter=563478status=active
- GET [base]/MedicationStatement?patient=14676&encounter=563478&status=active&\_include=MedicationStatement:medication

Support: Mandatory for server and client to support search by patient, encounter, and status parameter. Mandatory for client to support the \_include parameter. Optional for server to support the \_include parameter.

#### Fetching Active Medications Orders

Active medication orders includes only the currently prescribed medications.  They Do not include past, on-hold, stopped, cancelled, future(draft) and entered in error or unknown medications.  A MedicationRequest resource query **SHALL** be all that is required to access the "all active medication orders" using the `patient` and `status`='active' search parameters. This search returns a Bundle of all a patient's active MedicationRequests and, if the \include parameter is used by the server, Medication resources.

**Quick Start:**

**1\.** `GET /MedicationRequest?patient=[id]&status=active`

Example:

- GET [base]/MedicationRequest?patient=14676&status=active
- GET [base]/MedicationRequest?patient=14676&status=active&\_include=MedicationStatement:medication

*Support:* Mandatory for server and client to support search by patient and status parameter.  Mandatory for client to support the \_include parameter.  Optional for server to support the \_include parameter.

### US Meds UC-2 - Create new outpatient Prescription

This use case adopts the Use cases for the Argonaut Project and US Core specifically within the scope of recording and changing medications as prescribed in Meaningful Use 2015 §?170.302(d).*Maintain active medication list. Enable a user to record, change, and access a patient’s active medication list as well as medication history: (i) Ambulatory setting. Over multiple encounters; or (ii) Inpatient setting. For the duration of an entire hospitalization.*

- Record and change Medications: Patient or Provider adding new medication to patient’s (“active”) medication list.
- Requires write access to patient’s data

- Actors: Providers/Patient/EHR (See Use cases for the Argonaut Project)
- FHIR Resources


1.  Actors: Providers/EHR (See Use cases for the Argonaut Project)
1. Workflow Description: See a general discussion of FHIR workflow here. - Question for group: Do we want to focus on a single or few patterns? (http://hl7.org/fhir/2017Jan/workflow.html#commpatterns).
1. FHIR Resources
MedicationRequest (Nee MedicationOrder)
Medication
Task for Workflow
Terminology
1. RxNorm

### US Meds UC-3 - Dispense medication from Pharmacy

1. Background:
Not supported by community pharmacies since they are mandated to use NCPDP Script
Currently in hospital pharmacies using HL7 V2 RXD messaging.
Potential use for “secondary” recording of data.
For example: script history, real time benefit checking or Prescription Drug Monitoring
1.  Actors: TBD
1. FHIR Resources
MedicationDispense
Terminology
1. RxNorm
