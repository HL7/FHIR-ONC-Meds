## Definitions, Interpretations and Requirements common to all US-Meds actors
{:.no_toc}

This section outlines the definitions and interpretations and specific guidance for the US-Meds IG use cases  The conformance verbs used are defined in [FHIR Conformance Rules](capstatements.html).  The [general guidance](http://hl7.org/fhir/us/core/guidance.html) used in the US-Core IG apply to this guide as well.


source pages/\_include/{{page.md_filename}}.md  file


---

<!-- TOC  the css styling for this is \pages\assets\css\project.css under 'markdown-toc'-->
**Contents**

* Do not remove this line (it will not be displayed)
{:toc}

---

<!-- end TOC -->

### Background on the FHIR Medications resources

The 5 FHIR medications resources concerned with the ordering, dispensing, administration and recording of medication are:

- [Medication]({{ site.data.fhir.path }}/medication.html):  Represents the medication itself
- [MedicationRequest]({{ site.data.fhir.path }}/medicationrequest.html): Represents a prescription - the instructions for the administration of a  medication.  
- [MedicationDispense]({{ site.data.fhir.path }}/medicationdispense.html): Represents a response to a prescription - provision of a supply of a medication.
- [MedicationAdministration]({{ site.data.fhir.path }}/medicationadministration.html): Represents the consumption or administration of a medication.
- [MedicationStatement]({{ site.data.fhir.path }}/medicationstatement.html): Represents the record for past present and future medications taken

Details about each resource can be found in the FHIR specification.  As well, a gerneral discussion regarding the interaction between these resources is described in the FHIR [Medications Module]({{ site.data.fhir.path }}/medications-module.html) and the [Guide to Resources]({{ site.data.fhir.path }}/resourceguide.html) Sections.

For this IG we are focused on access and updates to a patient's medications and the interaction between the medication order (*MedicationRequest*) and the medication record (*MedicationStatement*).  Therefore it is important to understand the relationships between these resources.  Figure 1 illustrates the workflow relationship between the order and the fulfillment as represented by the FHIR medication resources.

{% include img.html img="usmed-fig1.png" caption="Figure 1" %}

Figure 2 shows the information sources for MedicationStatement which include patient reported medications and medications directly derived from the system's MedicationRequest resources.  Note that patient reported medications may be directly reported by the patient or derived from variety of patient provided records including including other FHIR resources.

{% include img.html img="usmed-fig2.png" caption="Figure 2" %}

Except for Medication resource itself, the medication resources represent a medication using either a code or a reference to a Medication resource. When using a code, the code  **SHALL** be [extensibly]({{ site.data.fhir.path }}/extensibility.html) bound to [RxNorm]({{ site.data.fhir.path }}/rxnorm.html) - i.e. unless the concept is not covered by RxNorm, the RxNorm code **SHALL** be used.  More information about using codes can be found in US Core [General Guidance Section](http://hl7.org/fhir/us/core/guidance.html) and the [FHIR Specification]({{ site.data.fhir.path }}/terminologies-systems.html).  When referencing the Medication resource, the resource may be a [contained]({{ site.data.fhir.path }}/references.html#contained) or an external resource. These options are shown in figure 3 below.  The server application **MAY** choose any combination of these methods, but if an external reference to Medication is used, the server **SHALL** support the include parameter for searching this element. The client application **MUST** support all methods. The US Core IG provides [Examples](http://hl7.org/fhir/us/core/StructureDefinition-us-core-medicationstatement.html#examples) that show these different methods. Additional guidance is provided below and in the [CapabilityStatement]({{ site.data.fhir.path }}/CapabilityStatement.html) section.

{% include img.html img="usmed-fig3.png" caption="Figure 3" %}

### Use Case 1 - Patient and Provider access to a patient’s active and historical medication list
{: #uc-1}

The guidance below addressed how a client accesses a patient's medication list. This use case adopts the Use cases for the Argonaut Project and US Core specifically within the scope of accessing medication information as prescribed in Meaningful Use 2015 §?170.302(d).*Maintain active medication list. Enable a user to record, change, and access a patient’s active medication list as well as medication history: (i) Ambulatory setting. Over multiple encounters; or (ii) Inpatient setting. For the duration of an entire hospitalization.*

#### Fetching All Medications, Active Medications, and Active Medications for an Encounter

"All medications" is defined as including all historical (prescribed and consumed), active, future (i.e. intended) and entered in error medications. "Active medications" can be limited to individual encounters and is defined as including all prescribed and/or being consumed medications. They do not include past, future and entered in error medications. The `context` element should be supported to enable querying for medications administered during an encounter.


A MedicationStatement resource query **SHALL** be all that is required to access the "all medications" and "all *active* medications" for a patient. The MedicationStatement **SHALL** include both patient reported medications and all medications directly derived from the system's orders. For medications directly derived from the system's orders:

   - The `MedicationStatement.derivedFrom` element **SHALL** reference the systems MedicationRequest resource for that order.
   - Only MedicationRequest resources with an `intent` of ‘order’ and any status in `status` will be accessible through MedicationStatement ( i.e., MedicationStatement won’t be derived from an order-instance, draft or proposed order).
   - Additional information **MAY** be added (e.g., whether the medication was taken). When this is the case, the `MedicationStatement.informationSource`	**SHALL** reference the individual providing the additional information.
   - The `MedicationStatement.effectiveDate` or `effectivePeriod` **SHOULD** be derived from an actual date if available. For example, if the medication was administered during a patient visit. If the actual administration date(s) are unknown the orders authoring date **MAY** be used as an approximate date. However, in this case, the `.taken` element in this case **SHOULD** be `unk`, Unknown.
   - For more information about the dosage etc., the client **MAY** need to query the MedicationRequest resource directly using the `derivedFrom` reference.


**Deduplication of Data**

Duplication of medications may exist between the sources of information.  The deduplication activity **MAY** be provided by the server but **SHOULD** be provided by the client.

**“Not Taken” Medications**

For specific guidance on how to determine if the patient has taken the medication, see the [MedicationStatement Resource Notes](http://build.fhir.org/medicationstatement.html#11.4.3.3) section in the FHIR Specification. To illustrate this scenario, the examples listed below contain an instance in which the patient has not taken a medication.

**Quick Start:**

Below is an overview of the required search and read operations for this use case. See the [Conformance requirements](capstatements.html) for a complete list of supported RESTful operations and search parameters for this IG.

**1\.** Get "all medications" for a patient by querying MedicationStatement using the `patient` search parameter.  This query is identical to that defined in the [US Core implementation Guide](todo.html). See that IG for further details.

Example: [Get All Medications](http://hl7.org/fhir/us/core/StructureDefinition-us-core-medicationstatement.html#search)

**2\.** Get "all *active* medications" for a patient by querying MedicationStatement using the `patient` and `status`='active' search parameters. This search returns a Bundle of all active MedicationStatements and if the \_include parameter is used, Medication resources for the specified patient.

`GET /MedicationStatement?patient=[id]&status=active{&_include=MedicationStatement:medication}`

*Support:* Mandatory for server and client to support search by patient and status parameter.  Mandatory for client to support the \_include parameter.  Optional for server to support the \_include parameter.

Example: [Get All *Active* Medications](get-all-meds.html)

**3\.** Get “all *active* medications” for an encounter by querying MedicationStatement using the patient and status=’active’ and encounter search parameters. This search returns a Bundle of all active MedicationStatements for an encounter and if the \_include parameter is used, Medication resources for the specified patient.

`GET /MedicationStatement?patient=[id]&encounter=[id]&status=active{&_include=MedicationStatement:medication}`

Support: Mandatory for server and client to support search by patient, encounter, and status parameter. Mandatory for client to support the \_include parameter. Optional for server to support the \_include parameter.

Example: [Get All *Active* Medications for an Encounter](get-all-active-meds.html)

#### Fetching Active Medications Orders

Active medication orders includes only the currently prescribed medications.  They Do not include past, on-hold, stopped, cancelled, future(draft) and entered in error or unknown medications.  A MedicationRequest resource query **SHALL** be all that is required to access the "all active medication orders" using the `patient` and `status`='active' search parameters. This search returns a Bundle of all a patient's active MedicationRequests and, if the \include parameter is used by the server, Medication resources.

**Quick Start:**

**1\.** `GET /MedicationRequest?patient=[id]&status=active{&_include=MedicationStatement:medication}`

*Support:* Mandatory for server and client to support search by patient and status parameter.  Mandatory for client to support the \_include parameter.  Optional for server to support the \_include parameter.

Example: [Get All *Active* Medication Order](get-all-enc-meds.html)

### Use Case 2 - Medication list update
{: #uc-2}

This use case adopts the Use cases for the Argonaut Project and US Core specifically within the scope of recording and changing medications as prescribed in Meaningful Use 2015 §?170.302(d).*Maintain active medication list. Enable a user to record, change, and access a patient’s active medication list as well as medication history: (i) Ambulatory setting. Over multiple encounters; or (ii) Inpatient setting. For the duration of an entire hospitalization.*  The updates would be to the MedicationStatement resource and  include both creation of new resources or changes to the existing resources.  These interactions require patient or provider requires write access to the EHR systems.   Refer to the US Core Implementation Guide's [General Security Considerations](todo.html) page for a discussion of the security and authorization requirements.

This Use Case will be the focus of future versions of this IG.

### Use Case 3 - Create new outpatient Prescription
{: #uc-3}

Provider creates new outpatient Prescription using the MedicationRequest resource.  Subsequent workflow steps depend on specific pattern agreed upon by business partners. See a list of possible scenarios in the [FHIR workflow sections](get-all-active-med-order.html) in the FHIR specification.

At the time of this publication, the US implementors have not used the MedicationRequest resource sufficiently to provide meaningful feedback on best practices and guidance.  This Use Case will be the focus of future versions of this IG.


### Use Case 4 - Dispense medication from Pharmacy
{: #uc-4}

Background: Community pharmacies in the US are mandated to use [NCPDP](https://www.ncpdp.org/) Script format for representing dispensing information and hospital pharmacies are typically using HL7 V2 RXD messaging.  Possible scenarios where this resource could be used for interchange of dispensing data include:
- prescription history
- real time confirmation of benefits
- prescription drug monitoring

At the time of this publication, the US implementors have not used the MedicationDispense resource sufficiently to provide meaningful feedback on best practices and guidance.  This Use Case will be the focus of future versions of this IG.
