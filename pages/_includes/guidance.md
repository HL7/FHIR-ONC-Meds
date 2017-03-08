
## Definitions, Interpretations and Requirements common to all US-Meds actors
{:.no_toc}

This section outlines the definitions and interpretations and specific guidance for the US-Meds IG use cases  The conformance verbs used are defined in [FHIR Conformance Rules](todo.html).  The [general guidance](todo.html) used in the US-Core IG apply to this guide as well.


source pages/\_include/{{page.md_filename}}.md  file

<!-- TOC -->

* Do not remove this line (it will not be displayed)
{:toc}

### Background on the FHIR Medications resources

The 5 FHIR medications resources concerned with the ordering, dispensing, administration and recording of medication are:

- [Medication](todo.html):  Represents the medication itself
- [MedicationRequest](todo.html): Represents a prescription - the instructions for the administration of a  medication.  
- [MedicationDispense](todo.html): Represents a response to a prescription - provision of a supply of a medication.
- [MedicationAdministration](todo.html): Represents the consumption or administration of a medication.
- [MedicationStatement](todo.html): Represents the record for past present and future medications taken

Details about each resource can be found in the FHIR specification.  As well, a gerneral discussion regarding the interaction between these resources is described in the FHIR [Medications Module](todo.html) and the [Guide to Resources](todo.html) Sections.

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
<p></p>
</div>

Except for Medication resource itself, the medication resources represent a medication using either a code or a reference to a Medication resource. When using a code, the code  **SHALL** be [extensibly](todo.html) bound to [RxNorm](todo.html) - i.e. unless the concept is not covered by RxNorm, the RxNorm code **SHALL** be used.  More information about using codes can be found in US Core [General Guidance Section](todo.html) and the [FHIR Specification](todo.html).  When referencing the Medication resource, the resource may be a [contained](todo.html) or an external resource. These options are shown in figure 3 below.  The server application **MAY** choose any combination of these methods, but if an external reference to Medication is used, the server **SHALL** support the include parameter for searching this element. The client application **MUST** support all methods. The US Core IG provides [Examples](todo.html) that show these different methods. Additional guidance is provided below and in the [CapabilityStatement](todo.html) section.

<!-- all this Bootstrap html for getting an image to fit nicely in text - 640 pix wide -->
file:///assets/images/usmed-fig3.png
<div>
<figure class="figure">
<figcaption class="figure-caption"><strong>Figure 3.</strong></figcaption>
  <img src="assets/images/usmed-fig3.png" class="figure-img img-responsive img-rounded center-block" alt="Workflow relationship between the order and the fulfillment">
</figure>
<p></p>
</div>

### Use Case 1 - Patient and Provider access to a patient’s active and historical medication list

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

For specific guidance on how to interpret the “Not-Given” element, see the [MedicationStatement Resource Notes](todo.html) section in the FHIR Specification. An example is provided which contains an instance using this element.  

**Quick Start:**

**1\.** Get "all medications" for a patient by querying MedicationStatement using the `patient` search parameter.  This query is identical to that defined in the [US Core implementation Guide](todo.html). See that IG for further details.

Example: [Get All Medications](todo.html)

**2\.** Get "all *active* medications" for a patient by querying MedicationStatement using the `patient` and `status`='active' search parameters. This search returns a Bundle of all active MedicationStatements and if the \_include parameter is used, Medication resources for the specified patient.

`GET /MedicationStatement?patient=[id]&status=active{&_include=MedicationStatement:medication}`

*Support:* Mandatory for server and client to support search by patient and status parameter.  Mandatory for client to support the \_include parameter.  Optional for server to support the \_include parameter.

Example: [Get All *Active* Medications](todo.html)

**3\.** Get “all *active* medications” for an encounter by querying MedicationStatement using the patient and status=’active’ and encounter search parameters. This search returns a Bundle of all active MedicationStatements for an encounter and if the \_include parameter is used, Medication resources for the specified patient.

`GET /MedicationStatement?patient=[id]&encounter=[id]&status=active{&_include=MedicationStatement:medication}`

Support: Mandatory for server and client to support search by patient, encounter, and status parameter. Mandatory for client to support the \_include parameter. Optional for server to support the \_include parameter.

Example: [Get All *Active* Medications for an Encounter](todo.html)

#### Fetching Active Medications Orders

Active medication orders includes only the currently prescribed medications.  They Do not include past, on-hold, stopped, cancelled, future(draft) and entered in error or unknown medications.  A MedicationRequest resource query **SHALL** be all that is required to access the "all active medication orders" using the `patient` and `status`='active' search parameters. This search returns a Bundle of all a patient's active MedicationRequests and, if the \include parameter is used by the server, Medication resources.

**Quick Start:**

**1\.** `GET /MedicationRequest?patient=[id]&status=active`

*Support:* Mandatory for server and client to support search by patient and status parameter.  Mandatory for client to support the \_include parameter.  Optional for server to support the \_include parameter.

Example: [Get All *Active* Medication Order](todo.html)

### Use Case 2 - Medication list update

This use case adopts the Use cases for the Argonaut Project and US Core specifically within the scope of recording and changing medications as prescribed in Meaningful Use 2015 §?170.302(d).*Maintain active medication list. Enable a user to record, change, and access a patient’s active medication list as well as medication history: (i) Ambulatory setting. Over multiple encounters; or (ii) Inpatient setting. For the duration of an entire hospitalization.*  The updates would be to the MedicationStatement resource and  include both creation of new resources or changes to the existing resources.  These interactions require patient or provider requires write access to the EHR systems.   Refer to the US Core Implementation Guide's [General Security Considerations](todo.html) page for a discussion of the security and authorization requirements.

This Use Case will be the focus of future versions of this IG.

### Use Case 3 - Create new outpatient Prescription

Provider creates new outpatient Prescription using the MedicationRequest resource.  Subsequent workflow steps depend on specific pattern agreed upon by business partners. See a list of possible scenarios in the [FHIR workflow sections](todo.html) in the FHIR specification.

At the time of this publication, the US implementors have not used the MedicationRequest resource sufficiently to provide meaningful feedback on best practices and guidance.  This Use Case will be the focus of future versions of this IG.


### Use Case 4 - Dispense medication from Pharmacy

Background: Community pharmacies in the US are mandated to use [NCPDP](todo.html) Script format for representing dispensing information and hospital pharmacies are typically using HL7 V2 RXD messaging.  Possible scenarios where this resource could be used for interchange of dispensing data include:
- prescription history
- real time confirmation of benefits
- prescription drug monitoring

At the time of this publication, the US implementors have not used the MedicationDispense resource sufficiently to provide meaningful feedback on best practices and guidance.  This Use Case will be the focus of future versions of this IG.
