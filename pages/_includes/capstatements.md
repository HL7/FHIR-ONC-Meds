## {{ page.title }}
{:.no_toc}

source pages/\_include/{{page.md_filename}}.md  file

This section outlines conformance requirements for each of the US-Meds actors identifying the specific profiles that need to be supported, the specific RESTful operations that need to be supported, and the search parameters that need to be supported. Note: The individual profiles identify the structural constraints, terminology bindings and invariants, however, implementers must refer to the conformance requirements for details on the RESTful operations, specific profiles and the search parameters applicable to each of the US-Meds actors.

---

<!-- TOC  the css styling for this is \pages\assets\css\project.css under 'markdown-toc'-->
**Contents**

* Do not remove this line (it will not be displayed)
{:toc}

---

<!-- end TOC -->


### Conformance requirements for the US Meds Server

Source Resource: [XML](capabilitystatement-server.xml.html)/[JSON](capabilitystatement-server.json.html)

- FHIR Version: 1.9.0
- Supported formats: xml, json
- Published: 2017-03-08
- Published by: Health Level Seven International Pharmacy Work Group

The Section describes the expected capabilities of the US Meds Server actor which is responsible for providing responses to the queries submitted by the US Med Client applications. When conforming to the US Meds IG and It is expected that it will be used in conjuction with the [US Core CapabilityStatement](todo.html). Together they describe the complete list of FHIR profiles, RESTful operations, and search parameters supported by US Meds Servers. US Meds Clients have the option of choosing from this list to access necessary data based on their local use cases and other contextual requirements.

#### Behavior

Description: The US Meds Server **SHALL**:

- Support the [US Core Patient](todo.html) resource profile.
- At a minimum, support the US Core MedicationStatement Profile.
- Implement the RESful behavior according to the FHIR specification.
- Return the following response classes:
   - (Status 200): successful operation
   - (Status 400): invalid parameter
   - (Status 401/4xx): unauthorized request
   - (Status 403): insufficient scope
   - (Status 404): unknown resource
   - (Status 410): deleted resource.
- Support *json* resource formats for all US Meds interactions.
- Declare a CapabilityStatement identifying the list of profiles, operations, search parameter supported.

The US Meds Server **SHOULD**:

- Support the following US Core and US Meds resource profiles:
   - US Core Medication   
   - US Meds MedicationAdministration
   - US Meds MedicationDispense
   - US Core MedicationRequest.
- Support *xml* resource formats for all US Meds interactions.
- Identify the US Core profile(s) and US Meds profiles supported as part of the FHIR `meta.profile` attribute for each instance.

The US Meds Server **MAY**:

- Support other US Core and US Meds resource profile.



#### Security:

US Meds Servers **SHALL**:

- Implement the security requirements documented in the US-Core IG.
- A server has ensured that every API request includes a valid Authorization token, supplied via: Authorization: Bearer {server-specific-token-here}
- A server has rejected any unauthorized requests by returning an HTTP 401 Unauthorized response code.

#### Profile Interaction Summary:

- All servers **SHALL** make available the [read](file:///Users/ehaas/Documents/FHIR/build/publish/http.html#read) and [search](file:///Users/ehaas/Documents/FHIR/build/publish/http.html#search) interactions for the Profiles the server chooses to support.
- All servers **SHOULD** make available the [vread](file:///Users/ehaas/Documents/FHIR/build/publish/http.html#vread) and [history-instance](file:///Users/ehaas/Documents/FHIR/build/publish/http.html#history) interactions for the Profiles the server chooses to support.

**Summary of US Meds search criteria**

Specific server search capabilities are described in detail below in each of the resource sections.  The MedicationAdministration, MedicationDispense, MedicationStatement and MedicationRequest resources can represent a medication using either a code or refer to the Medication resource.  When referencing a Medication resource, the resource may be contained or an external resource. The server application can choose any one way or more than one method, but *if* the an external reference to Medication is used, the server **SHALL** support the [`_include`](todo.html) parameter for searching this element. The client application must support all methods.

| Resource Type | Supported Profiles | Supported Searches | Supported \_includes |
| ------- | -------- | -------- | -------- | ------- | ------ |
| [Medication](#medication) |  US Core Medication Profile |
| [MedicationAdministration](#medicationadministration) | US Core MedicationAdministration Profile |patient | MedicationAdministration:medication |
| [MedicationDispense](#medicationdispense) | US Core MedicationDispense Profile | patient | MedicationDispense:medication |
| [MedicationRequest](#medicationrequest) | US Core MedicationRequest Profile | patient, status, patient + status | MedicationRequest:medication |
| [MedicationStatement](#medicationstatement)| US Core MedicationStatement Profile | patient, status, encounter, patient + status, patient + status + encounter | MedicationStatement:medication |
{:.grid}

#### Resource  Details:

##### 1. Medication

Supported Profiles:  [US Core Medication Profile](todo.html)

##### 2. MedicationAdministration
Supported Profiles:  [US Core MedicationAdministration Profile](todo.html)

Search Criteria:

A server **SHALL** be capable of returning a patient's administered medications using one of or both:

- `GET /MedicationAdministration?patient=[id]`
- `GET /MedicationAdministration?patient=[id]&_include=MedicationAdministration:medication`

Search Parameters:

| Conformance | Parameter | Type | \_include (see documentation) | Modifiers |
| ---|---|---|---|--- |
| **SHALL** | patient | reference |  MedicationAdministration:medication | |
{:.grid}

##### 3. MedicationDispense
Supported Profiles:  [US Core MedicationDispense Profile](todo.html)

Search Criteria:


A server **SHALL** be capable of returning a patient's dispensed medications using one of or both:

- `GET /MedicationDispense?patient=[id]`
- `GET /MedicationDispense?patient=[id]&_include=MedicationDispense:medication`

| Conformance | Parameter | Type | \_include (see documentation) | Modifiers |
| ---|---|---|---|--- |
| **SHALL** | patient | reference | MedicationDispense:medication
{:.grid}

##### 3. MedicationRequest
Supported Profiles:  [US Core MedicationRequest Profile](todo.html)

Search Criteria:


A server is capable of returning a patient’s active medications orders using one of or both:

- `GET /MedicationRequest?patient=[id]`
- `GET /MedicationRequest?patient=[id]&_include=MedicationRequest:medication`

Search Parameters:

| Conformance | Parameter | Type | \_include (see documentation) | Modifiers |
| ---|---|---|---|--- |
| **SHALL** | status + status | reference + token | MedicationRequest:medication
{:.grid}

##### 4. MedicationStatement
Supported Profiles:  [US Core MedicationStatement Profile](todo.html)

Search Criteria:

A server is capable of returning all medications for a patient using one of or both:

- `GET /MedicationStatement?patient=[id]`
- `GET /MedicationStatement?patient=[id]&_include=MedicationStatement:medication`

Search Parameters:

| Conformance | Parameter | Type |  \_include (see documentation) | Modifiers |
| ---|---|---|---|--- |
| **SHALL** | patient | reference | MedicationStatement:medication
| **SHALL** | patient + status | reference + token | MedicationStatement:medication
| **SHOULD** | patient + status + encounter | reference + token | MedicationStatement:medication  |
{:.grid}

### Conformance requirements for the US Meds Client

which is responsible for creating and initiating the queries for information about an individual patient.
