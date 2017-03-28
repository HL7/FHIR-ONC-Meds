## Definitions, Interpretations and Requirements common to all US-Meds actors
{:.no_toc}

This section outlines the definitions and interpretations and specific guidance for the US-Meds IG use cases  The conformance verbs used are defined in [FHIR Conformance Rules](capstatements.html).  The [general guidance]({{ page.us-core-base }}guidance.html) used in the US-Core IG apply to this guide as well.


source pages/\_include/{{page.md_filename}}.md  file


---

<!-- TOC  the css styling for this is \pages\assets\css\project.css under 'markdown-toc'-->
**Contents**

* Do not remove this line (it will not be displayed)
{:toc}

---

<!-- end TOC -->

### Background on the FHIR Medications resources

#### Pharmacy FHIR Resources
{:.no_toc}

The 5 FHIR pharmacy resources are concerned with the ordering, dispensing, administration and recording of medication are:

- [Medication]({{ site.data.fhir.path }}/medication.html):  Represents the medication itself
- [MedicationRequest]({{ site.data.fhir.path }}/medicationrequest.html): Represents a prescription - the instructions for the administration of a  medication.  
- [MedicationDispense]({{ site.data.fhir.path }}/medicationdispense.html): Represents a response to a prescription - provision of a supply of a medication.
- [MedicationAdministration]({{ site.data.fhir.path }}/medicationadministration.html): Represents the consumption or administration of a medication.
- [MedicationStatement]({{ site.data.fhir.path }}/medicationstatement.html): Represents the record for past present and future medications taken

Details about each resource can be found in the FHIR specification.  As well, a general discussion regarding the interaction between these resources is described in the FHIR [Medications Module]({{ site.data.fhir.path }}/medications-module.html) and the [Guide to Resources]({{ site.data.fhir.path }}/resourceguide.html) Sections.

#### Relationships Between the Pharmacy FHIR Resources
{:.no_toc}

This IG we focuses on access and updates to a patient's medications and the interaction between the medication order/prescription (*MedicationRequest*) and the medication record (*MedicationStatement*).  It is therefore important to understand the relationships between these resources.  Figure 1 illustrates the workflow relationship between the medication order and the fulfillment as represented by the FHIR medication resources.  Note that in the inpatient setting the outpatient setting the administration is done by the patient.

{% include img.html img="usmed-fig1.png" caption="Figure 1: FHIR Medication Order and Fulfillment" %}

 Several sources of medication information may be used to create a MedicationStatement as is shown in Figure 2 below. The information may come from a patient, a related person (such as a family member), a provider, or the system's medication orders.  A patient or related person might directly report the information or provide some type of record including other FHIR resources.  Typically, the medication information from a system's orders is the same as is used to create a MedicationRequest resource. Any combination of these sources may be used. For example, the patient is the *only* source of information when the patient reports an OTC medication.  Alternatively, when the patient reports actual medication usage for a known prescription, then the MedicationStatement is a combination of the order and patient stated compliance or usage.  If the patient or provider provided no information, the entire MedicationStatement is derived from the order.

{% include img.html img="usmed-fig2.png" caption="Figure 2: MedicationStatement Information Sources" %}

The pharmacy resources represent a medication using either a code or a reference to a Medication resource. Typically, a code will be used to represent either a branded (for example, Crestor 10mg tablet) or a generic (for example, Rosuvastatin 10mg tablet) medication.  When using a code, the code  **SHALL** be [extensibly]({{ site.data.fhir.path }}/extensibility.html) bound to [RxNorm]({{ site.data.fhir.path }}/rxnorm.html) - i.e. unless the concept is not covered by RxNorm, the RxNorm code **SHALL** be used.  More information about using codes can be found in US Core [General Guidance Section]({{ page.us-core-base }}guidance.html) and the [FHIR Specification]({{ site.data.fhir.path }}/terminologies-systems.html).  A medication resource is typically used when information that is not included as part of the RxNorm code is required.  For example, the Medication resource is the only way to correctly represent compounded or extemporaneously prepared medication.  When referencing the Medication resource, the resource may be a [contained]({{ site.data.fhir.path }}/references.html#contained) or an external resource. These options are shown in figure 3 below.  The server application **MAY** choose any combination of these methods, but if an external reference to Medication is used, the server **SHALL** support the include parameter for searching this element. The client application **MUST** support all methods. The US Core IG provides [examples]({{ page.us-core-base }}StructureDefinition-us-core-medicationstatement.html#examples) that show these different methods. Additional guidance is provided below and in the [CapabilityStatement](capstatements.html) section.

{% include img.html img="usmed-fig3.png" caption="Figure 3: How A Medication is Represented in Pharmacy Resources" %}

### Use Case 1 - Patient and Provider access to a patients' active and historical medication list
{: #uc-1}

The guidance below addresses how a patient or a provider accesses a patients' medication list. This use case adopts the use cases defined as part of the Argonaut Project and US Core, specifically within the scope of accessing medication information as prescribed in Meaningful Use 2015 §?170.302(d) - *Maintain active medication list. Enable a user to record, change, and access a patient’s active medication list as well as medication history: (i) Ambulatory setting. Over multiple encounters; or (ii) Inpatient setting. For the duration of an entire hospitalization.*

#### Fetching All Medications, Active Medications, and Active Medications for an Encounter

**Definitions**

- "All medications" is defined to include all historical (prescribed and consumed), active (prescribed and consumed), future (i.e. intended) and medications that are entered in error and whose status is unknown.
- "Active medications" is defined to include all prescribed and/or being consumed medications. They do not include past, future, unknown status, and entered in error medications.

**Requirements**

A MedicationStatement resource query **SHALL** be all that is required to access "all medications" and "all *active* medications" for a patient. The MedicationStatement **SHALL** include both patient reported medications and all medications directly derived from the system's orders. For medications directly derived from the system's orders:

   - The `MedicationStatement.derivedFrom` element **SHALL** reference the systems MedicationRequest resource for that order.
   - Only MedicationRequest resources with an `intent` of ‘order’ and any status in `status` will be accessible through MedicationStatement ( i.e., MedicationStatement will not be derived from an order-instance, draft or proposed order).
   - Additional information **MAY** be added (e.g., whether the medication was taken). When this is the case, the `MedicationStatement.informationSource`	**SHALL** reference the individual providing the additional information.
   - The `MedicationStatement.effectiveDate` or `effectivePeriod` **SHOULD** be derived from an actual date if available. For example, if the medication was administered during a patient visit. If the actual administration date(s) are unknown the orders authoring date **MAY** be used as an approximate date. However, in this case, the `.taken` element in this case **SHOULD** be `unk`, Unknown.
   - For more information about the dosage etc., the client **MAY** need to query the MedicationRequest resource directly using the `derivedFrom` reference.

The 'context' element **SHOULD** be supported to enable querying for medications administered during an encounter. Searching by context (i.e., for a given inpatient encounter) will return all medications ordered during that encounter, which can include both medications administered in hospital as well as prescribed or discharge medications which are intended to be taken at home. The `category` and `context`  elements **MAY** be used together to get the intersection of medications for a given encounter (i.e., the context) that were administered during as an inpatient (i.e., the category).


**Deduplication of Data**

Duplication of medications may exist between the sources of information.  To provide a list of a patients’ medications, it may be necessary to “de-duplicate” the list. The deduplication activity **MAY** be provided by the server but **SHOULD** be provided by the client.

**“Not Taken” Medications**

For specific guidance on how to determine if the patient has taken the medication, see the [MedicationStatement Resource Notes]({{ site.data.fhir.path }}/medicationstatement.html#11.4.3.3) section in the FHIR Specification. To illustrate this scenario, the examples listed below contain an instance in which the patient has not taken a medication.

##### Quick Start


Below is an overview of the required search and read operations for this use case. See the [Conformance requirements](capstatements.html) for a complete list of supported RESTful operations and search parameters for this IG.

**1\.** Get "all medications" for a patient by querying MedicationStatement using the `patient` search parameter.  This query is identical to that defined in the [US Core implementation Guide]({{ page.us-core-base }}StructureDefinition-us-core-medicationstatement.html#search). See that IG for further details.

Example: [Get All Medications](get-all-meds.html)

**2\.** Get "all *active* medications" for a patient by querying MedicationStatement using the `patient` and `status`='active' search parameters. This search returns a Bundle of all active MedicationStatements and if the \_include parameter is used, Medication resources for the specified patient.

`GET /MedicationStatement?patient=[id]&status=active{&_include=MedicationStatement:medication}`

*Support:* Mandatory for server and client to support search by patient and status parameter.  Mandatory for client to support the \_include parameter.  Optional for server to support the \_include parameter.

Example: [Get All *Active* Medications](get-all-active-meds.html)

**3\.** Get “all *active* medications” for an encounter by querying MedicationStatement using the patient and status=’active’ and encounter search parameters. This search returns a Bundle of all active MedicationStatements for an encounter and if the \_include parameter is used, Medication resources for the specified patient.

`GET /MedicationStatement?patient=[id]&encounter=[id]&status=active{&_include=MedicationStatement:medication}`

Support: Mandatory for server and client to support search by patient, encounter, and status parameter. Mandatory for client to support the \_include parameter. Optional for server to support the \_include parameter.

Example: [Get All *Active* Medications for an Encounter](todo.html)

#### Fetching Active Medications Orders

Active medication orders include only the currently prescribed medications.  They Do not include order statuses of past, on-hold, stopped, cancelled, future(draft), entered in error or unknown.  A MedicationRequest resource query **SHALL** be all that is required to access the "all active medication orders" using the `patient` and `status`='active' search parameters. This search returns a Bundle of all a patient's active MedicationRequests and, if the \_include parameter is used by the server, Medication resources.

##### Quick Start


**1\.** `GET /MedicationRequest?patient=[id]&status=active{&_include=MedicationRequest:medication}`

*Support:* Mandatory for server and client to support search by patient and status parameter.  Mandatory for client to support the \_include parameter.  Optional for server to support the \_include parameter.

Example: [Get All *Active* Medication Orders](get-all-active-med-order.html)

### Use Case 2 - Medication list update
{: #uc-2}

This use case adopts the use cases for the Argonaut Project and US Core specifically within the scope of recording and changing medications as prescribed in Meaningful Use 2015 §?170.302(d) - *Maintain active medication list. Enable a user to record, change, and access a patient’s active medication list as well as medication history: (i) Ambulatory setting. Over multiple encounters; or (ii) Inpatient setting. For the duration of an entire hospitalization.*  The updates include both the creation of new MedicationStatement resources and changes to existing MedicationStatement resources.  These interactions require "write acces" access to the EHR systems.   Refer to the US Core Implementation Guide's page for a discussion of the security and authorization requirements.

This Use Case will be the focus of future versions of this IG.

### Use Case 3 - Create new outpatient Prescription
{: #uc-3}

This use case involves the provider creating a new outpatient Prescription using the MedicationRequest resource.  Subsequent workflow steps depend on specific pattern agreed upon by business partners. See a list of possible scenarios in the [FHIR workflow sections]({{ site.data.fhir.path }}/workflow-communications.html)) in the FHIR specification.

At the time of this publication, the US implementers have not used the MedicationRequest resource sufficiently to provide meaningful feedback on best practices and guidance.  This Use Case will be the focus of future versions of this IG.


### Use Case 4 - Dispense medication from Pharmacy
{: #uc-4}

Background: Community pharmacies in the US are mandated to use [NCPDP](https://www.ncpdp.org/) Script standard to send dispensed medications for adjudication and hospital pharmacies are typically using HL7 V2 RXD messaging.  Possible scenarios where this resource could be used for interchange of dispensing data include:

- Dispense Historys
- Real-time Confirmation of Prescription Drug Benefits
- Prescription Drug Monitoring Programs (PDMPs)

At the time of this publication, the US implementers have not used the MedicationDispense resource sufficiently to provide meaningful feedback on best practices and guidance.  This Use Case will be the focus of future versions of this IG.