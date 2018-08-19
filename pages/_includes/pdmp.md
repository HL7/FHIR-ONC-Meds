
## US Meds Prescription Drug Monitoring Program (PDMP) FHIR Implementation Guide
{:.no_toc}

<!-- TOC  the css styling for this is \pages\assets\css\project.css under 'markdown-toc'-->

* Do not remove this line (it will not be displayed)
{:toc}

<!-- end TOC -->


### Introduction 

In the United States, every state is deploying a Prescription Drug Monitoring Program (PDMP) which track controlled substance prescriptions within the state. Overtime, these PDMP databases start to provide rich information on provider and patient behaviors with respect to prescribing and use of controlled substances (primarily opioids). Enabling a Provider to access a Patient's PDMP data during care delivery will help in avoiding potential drug misuse, abuse and diversion. In order to reduce opioid abuse, some states have implemented policies mandating Providers to check the state PDMP for the Patient's controlled substance history before prescribing controlled substances. To further address opioid abuse which is a current national priority, the US Meds Prescription Drug Monitoring Program FHIR Implementation Guide (US Meds PDMP FHIR IG) outlines how systems can access PDMP data for a patient from the state PDMP systems using the HL7 FHIR standard. For general background on state PDMPs, see the Centers for Disease Control and Prevention [what states need to know about PDMPs](https://www.cdc.gov/drugoverdose/pdmp/states.html).


### Scope 

This section outlines the transactions which are in-scope and not in-scope for the US Meds PDMP FHIR IG. As shown in the Figure 1 below, the PDMP eco-system can be simplified for the purpose of this project to consist of EHRs or Clinical Systems, Pharmacies and the State PDMP systems. 

The US Meds PDMP FHIR IG defines how an EHR or an App or other Clinical Systems can access a Patient's controlled substance prescription history from the State PDMP systems. This is identified by Interaction 3 in the diagram.
All other interactions such as Interaction 1 and 2 in the diagram below are important for the overall PDMP eco-system, they are not in-scope for this project and IG.  


{% include img.html img="simplified-pdmp.png" caption="Figure 1: Simplified View of PDMP eco-system interactions" %}



### Abstract Model, Actors and Definitions

This section defines the abstract model which is used to identify the specific actors and interactions that are in-scope for the project. The abstract model is as shown in Figure 2 below which consists of two actors namely the PDMP Requestor and the PDMP Responder. 

**PDMP Requestor**: PDMP Requestor is a health IT system that is requesting a Patient's controlled substance history. Real-world examples of such health IT systems include EHRs, SMART on FHIR Apps and other Clinical Systems used for care delivery. 

**PDMP Responder**: PDMP Responder is a health IT system that accepts a request from a PDMP Requestor and responds to the request with a controlled substance history for the patient. Real-world examples of such health IT systems include intermediary gateways such as Appriss, RxCheck, State HIEs and State PDMP systems. 

**Note**: PDMP Responders, which act as intermediaries, provide additional services such as being able to query multiple State PDMP systems for analytics which generates additional information for the Provider. The fact that a PDMP Responder is an intermediary or if it is the actual State PDMP system is transparent to the PDMP Requestor and is more of a deployment architecture. 


{% include img.html img="abstract-model.png" caption="Figure 2: PDMP Abstract Model and Actors" %}



### PDMP Data Access using FHIR

The abstract model interactions can be implemented using FHIR in multiple ways namely

* Using FHIR Search APIs
* Using FHIR Messaging APIs

These APIs are discussed further in the next few sub-sections.


#### Using FHIR Search APIs

The FHIR standard provides a rich set of [search mechanisms](http://hl7.org/fhir/search.html) by which specific FHIR resources can be accessed from a FHIR server. Typically the search parameters are specified in the RESTful URL and accessed using the HTTP GET operation. 

The following is an example of how search parameters will be used by a PDMP Requestor to retrieve PDMP data from a PDMP Responder. 

`GET [base]/MedicationDispense?subject:Patient.name.given=peter&subject:Patient.name.family=jacobs&subject:Patient.birthdate=eq1973-11-25&authorizingPrescription.dispenseRequest.validityPeriod=ge2010-01-01&authorizingPrescription.dispenseRequest.validityPeriod=le2015-12-31&_include=MedicationDispense:subject&_include:recurse=MedicationDispense:authorizingPrescription&_include=MedicationDispense:medication`

The above API will fetch all MedicationDispense resources for Patient with a given name of "peter" and family name of "jacobs" with a birthdate of "1973-11-25" with a prescription that falls within in a 5 year window starting from January 1st 2010 to December 31st 2015 and as part of the returned information will include MedicationDispense, MedicationRequest, Practitioner, Organization, Patient and Medication information as part of the returned bundle.

Also as part of the Search API one can specify to the server to include additional information such as the prescriber information, patient information. The combinations that need to be implemented by the US Meds PDMP FHIR IG actors will be described in detail as part of the Capability Statements.  


#### Using FHIR Messaging APIs

The FHIR standard also provides APIs that resemble messaging paradigm similar to HL7 v2. These are part of the [FHIR Messaging APIs](http://hl7.org/fhir/messaging.html). A PDMP Requestor can request prescription history for a patient by invoking the [$process-message](http://hl7.org/fhir/messageheader-operations.html) on the base URL as follows:

`POST [base-url]/$process-message`

**NOTE**: FHIR Messaging APIs will require the use of the POST operation even to access data.

The body of the message will be a FHIR Bundle that contains a MessageHeader resource as the first entry with the following minimum details:

* MessageHeader Resource
	* MessageHeader.id
	* MessageHeader.event (A new event type for retrieving PDMP data needs to be added to the value-set)
	* MessageHeader.timestamp
	* MessageHeader.responsible containing a reference to Practitioner resource requesting the information.
	* MessageHeader.focus containing a reference to the Patient resource for whom the prescription history is required.
	* MessageHeader.data containing a reference to Parameters resource to specify additional parameters such as the date range.

* Include the Practitioner and Patient Resources as entries in the bundle.
* Include the Parameters resource with the parameters of startDate and endDate with values for date range.

The PDMP Responder has to be able to process this message and return back a Bundle which contains all the MedicationDispense resources along with Practitioner, Organization and Patient resources related to the data set. In case of errors OperationOutcome would be returned similar to any regular FHIR API. 

**NOTE**: FHIR Messaging operations can only be invoked on FHIR Servers which conform to the FHIR Messaging operations in their capability statements and not on regular FHIR servers implementing RESTful Search APIs.


#### Final Approach for US Meds PDMP FHIR IG


For the purposes of the US Meds PDMP FHIR IG, the FHIR Search APIs will be used and will be specified in detail in the CapabilityStatements for the actors. While both the Search and Messaging APIs have their own strengths and weaknesses, the decision to use FHIR Search APIs is based on the following factors

* Argonaut program has embraced the FHIR Search APIs for conforming to the ONC 2015 Edition for the Common Clinical Data Set
* US-Core and US Meds Implementation Guides specify the search APIs as basic capabilities for FHIR servers providing access to data.
* Search APIs will be relatively easier to build on existing FHIR infrastructure developed by vendor community.
* Search APIs are limited to synchronous operations only and avoid the complexity of the various Message Exchange patterns that need to be accounted for in a Messaging system. 


#### Security Considerations


All implementers of FHIR servers and clients should pay attention to [FHIR Security](http://hl7.org/fhir/security.html) considerations. In addition to the [FHIR Security](http://hl7.org/fhir/security.html) considerations, the PDMP requests need to contain specific information about Requestor Identity and Requestor Facility information. Providing this information using FHIR Search APIs is very cumbersome and is not necessary. This kind of information can be collected by the PDMP Responder's Authorization Server during application registration and avoid repeating the information on each request. These mechanisms are outlined in great detail in the [SMART Backend Services Authorization Guide](http://docs.smarthealthit.org/authorization/backend-services/). The US Meds PDMP FHIR IG will use the SMART Backend Services Authorization Guide to collect the necessary requestor information appropriate to making the PDMP data request. In addition the authentication and authorization mechanisms will be used as part of requesting the PDMP data using the FHIR Search APIs. 


### PDMP Data Elements and Mappings

This section describes and identifies data elements that are used commonly in the PDMP data requests and responses and provides mappings of these data elements to FHIR. Based on environmental scans and prior performed by ONC across a spectrum of PDMP implementations the following data was collected:

* Most of the existing EHR implementations use NCPDP script 10.6 or ASAP web services to request and receive PDMP data from PDMP Responders (Intermediaries or State PDMP systems). 
* Most of the State PDMP systems are implemented using data elements specified by the NIEM standard and expose these data elements using PMIX APIs.
* Most of the EHRs use intermediaries to request data from one or more State PDMP systems and send NCPDP based requests and receive responses in NCPDP format. 

Based on the above findings, NCPDP Request and Response data elements have been used as a starting point to specify the FHIR APIs. Since the community understands these NCPDP data elements, a mapping of NCPDP Request and Response data elements to FHIR Resources has been created and specified below. This allows organizations already familiar with NCPDP to use the mapping provided to develop their FHIR Resources and APIs. Similarly mapping from PMIX/NIEM data elements to FHIR is also provided for systems using PMIX/NIEM to map their data to FHIR and expose them through appropriate APIs.

#### NCPDP Mappings for PDMP Request
{:.no_toc}

This section includes the minimal mapping for the PDMP request from an EHR to a state PDMP using NCPDP.

<!-- [MedicationRequest]({{ site.data.fhir.path }}/medicationrequest-mappings.html#script10.6): includes a full mapping for medicationRequest resource to SCRIPT 10.6 -->

{% include NCPDP_Request_table.html %}


#### NCPDP Mappings for PDMP Response
{:.no_toc}

This section includes the minimal mapping for the PDMP response from a state PDMP to an EHR using NCPDP.

{% include NCPDP_Response_table.html %}

#### PMIX Mappings for PDMP Request

This section includes the minimal mapping for the PDMP request from an EHR to a state PDMP using PMIX.

{% include PMIX_Request_table.html %}

#### PMIX Mappings for PDMP Response

This section includes the minimal mapping for the PDMP response from a state PDMP to an EHR using PMIX.

{% include PMIX_Response_table.html %}



  

#### FHIR profiles

This project does not create any new profiles, but will reuse the US-Core profiles and the US Meds profiles to request and receive PDMP data using FHIR resources.  


### Capability Statements

This section identifies the CapabilityStatements defined for this implementation guide. The section outlines conformance requirements for each of the US Meds PDMP FHIR IG actors which includes the specific profiles, operations, security mechanisms and search parameters that need to be supported. 

**Note**: The individual profiles identify the structural constraints, terminology bindings and invariants.

#### Conformance requirements for the PDMP Responder

The section describes the expected capabilities of the PDMP Responder actor which is responsible for providing responses to the queries submitted by the PDMP Requestor applications. 

##### Behavior 

The PDMP Responder **SHALL**:

* Support the US Core Patient, US Core Practitioner, US Core Organization resource profiles.
* Support the US Meds MedicationRequest and MedicationDispense Profile.
* Implement the RESTful behavior according to the [FHIR specification](https://www.hl7.org/fhir/http.html).
	* which includes returning the following response classes:
		* (Status 200): successful operation
		* (Status 400): invalid parameter
		* (Status 401/4xx): unauthorized request
		* (Status 403): insufficient scope
		* (Status 404): unknown resource
		* (Status 410): deleted resource.
* Support json resource formats for all PDMP interactions.
* Declare a CapabilityStatement identifying the list of profiles, operations, and search parameters supported.

The PDMP Responder **SHOULD**:

* Support the following US Core and US Meds resource profiles:
	* US Core Medication
	* US Meds MedicationAdministration

* Support xml resource formats for all US Meds interactions.
* Identify the US Core profile(s) and US Meds profiles supported as part of the FHIR meta.profile attribute for each instance.

The PDMP Responder MAY:

* Support other US Core and US Meds resource profile

##### Profile Interactions:

* The PDMP Responder **SHALL** support the FHIR Search interaction for MedicationDispense profile.

* The PDMP Responder **SHOULD** support the FHIR Read profile interaction MedicationDispense profile.

* The PDMP Responder **MAY** support other FHIR profile interactions.


##### Security:

* The PDMP Responder **SHALL** support the SMART Backend Services Authorization Guide for verifying authentication and providing authorization to PDMP Requestors.

* The PDMP Responder **SHALL** support the HTTP Header parameter X-Request-ID for request coorelation between the PDMP Requester and PDMP Responder. 

##### Search:

The PDMP Responder **SHALL** support the following search parameters and combination for the MedicationDispense resource

* Chained Search parameters
	* subject:Patient.name.given - Patient's first name
	* subject:Patient.name.family - Patient's family name
	* subject:Patient.birthdate - Patient's birth date
	* authorizingPrescription.dispenseRequest.validityPeriod - To specify the date range for the PDMP data retrieval
	
The PDMP Responder **SHALL** support the following _include parameters for the MedicationDispense Search operations

* _include=MedicationDispense:subject - Returns the Patient Resource information
* _include:recurse=MedicationDispense:authorizingPrescription - Returns the MedicationRequest, Practitioner Resource information and Organization information
* _include=MedicationDispense:medication - Returns the Medication Resource information

	
The following is an example of the query. 

`GET [base]/MedicationDispense?subject:Patient.name.given=peter&subject:Patient.name.family=jacobs&subject:Patient.birthdate=eq1973-11-25&authorizingPrescription.dispenseRequest.validityPeriod=ge2010-01-01&authorizingPrescription.dispenseRequest.validityPeriod=le2015-12-31&_include=MedicationDispense:subject&_include:recurse=MedicationDispense:authorizingPrescription&_include=MedicationDispense:medication`

The above API will fetch all MedicationDispense resources for Patient with a given name of "peter" and family name of "jacobs" with a birthdate of "1973-11-25" with a prescription that falls within in a 5 year window starting from January 1st 2010 to December 31st 2015 and as part of the returned information will include MedicationDispense, MedicationRequest, Practitioner, Organization, Patient and Medication information as part of the returned bundle.

All other information required to authenticate and authorize a PDMP Requestor is captured as part of registering the PDMP Requestor following the SMART Backend Services Authorization Guide. 


#### Conformance requirements for the PDMP Requestor

The section describes the expected capabilities of the PDMP Requestor actor which is responsible for providing responses to the queries submitted by the PDMP Requestor applications. 

##### Behavior 

The PDMP Requestor **SHALL**:

* Support the US Core Patient, US Core Practitioner, US Core Organization resource profiles.
* Support the US Meds MedicationRequest and MedicationDispense Profile.
* Consume the RESTful responses according to the FHIR specification.
	* which includes returning the following response classes:
		* (Status 200): successful operation
		* (Status 400): invalid parameter
		* (Status 401/4xx): unauthorized request
		* (Status 403): insufficient scope
		* (Status 404): unknown resource
		* (Status 410): deleted resource.
* Support json resource formats for all PDMP interactions.

The PDMP Requestor **SHOULD**:

* Support the following US Core and US Meds resource profiles:
	* US Core Medication
	* US Meds MedicationAdministration

* Support xml resource formats for all PDMP interactions.


##### Profile Interactions:

* The PDMP Requestor **SHALL** support the FHIR Search interaction for MedicationDispense profile.

* The PDMP Requestor **SHOULD** support the FHIR Read profile interaction for MedicationDispense profile.

* The PDMP Requestor **MAY** support other FHIR profile interactions.


##### Security:

* The PDMP Requestor **SHALL** support the SMART Backend Services Authorization Guide applicable to clients.

* The PDMP Requestor **SHALL** add the HTTP Header parameter X-Request-ID as part of the Search request for request coorelation between the PDMP Requester and PDMP Responder. 

##### Search:

The PDMP Requestor **SHALL** invoke the Search operation on the PDMP Responder including the following search and _include parameters when requesting PDMP data using the MedicationDispense resource

* Chained Search parameters
	* subject:Patient.name.given - Patient's first name
	* subject:Patient.name.family - Patient's family name
	* subject:Patient.birthdate - Patient's birth date
	* authorizingPrescription.dispenseRequest.validityPeriod - To specify the date range for the PDMP data retrieval
	
The PDMP Requestor **SHALL** include the following _include parameters for the MedicationDispense Search operations

* _include=MedicationDispense:subject - Returns the Patient Resource information
* _include:recurse=MedicationDispense:authorizingPrescription - Returns the MedicationRequest, Practitioner Resource information and Organization information
* _include=MedicationDispense:medication - Returns the Medication Resource information

	
The following is an example of the query. 

`GET [base]/MedicationDispense?subject:Patient.name.given=peter&subject:Patient.name.family=jacobs&subject:Patient.birthdate=eq1973-11-25&authorizingPrescription.dispenseRequest.validityPeriod=ge2010-01-01&authorizingPrescription.dispenseRequest.validityPeriod=le2015-12-31&_include=MedicationDispense:subject&_include:recurse=MedicationDispense:authorizingPrescription&_include=MedicationDispense:medication`

The above API will fetch all MedicationDispense resources for Patient with a given name of "peter" and family name of "jacobs" with a birthdate of "1973-11-25" with a prescription that falls within in a 5 year window starting from January 1st 2010 to December 31st 2015 and as part of the returned information will include MedicationDispense, MedicationRequest, Practitioner, Organization, Patient and Medication information as part of the returned bundle. 

### Patient Matching Considerations 

The US Meds PDMP FHIR IG does not add any patient matching requirements to the PDMP actors, but relies on existing practices used for patient matching based on the first name, last name and date of birth provided through the request. Some states may require requesters to include additional parameters to facilitate more accurate patient matching results, however these variations are out of scope for this IG.


### Deployment Architecture 

The following are deployment options showing how the US Meds PDMP FHIR IG can be used to implement the various actors. 

#### Deployment Option 1: 

In this deployment option, the EHRs, Apps and Clinical Systems act as the PDMP Requestors and interact with Intermediary Gateways such as Appriss, RxCheck which act as the PDMP Responders. The communication is performed using FHIR APIs. In this case the PDMP Requestors are isolated from the State PDMP Systems and the protocols they support. 

The Intermediaries may translate the incoming FHIR request for data to a PMIX/NIEM request to comply with existing state interfaces or may use other methods to get the data from the State PDMP systems. All of these interactions are isolated from the PDMP Requestor. The Intermediaries may also retrieve data from multiple State PDMP systems simultaneously.

{% include img.html img="dep-option-1.png" caption="Figure 3: Deployment Option using Intermediaries and PMIX/NIEM" %}


#### Deployment Option 2:

In this deployment option, the EHRs, Apps and Clinical Systems act as the PDMP Requestors and interact with Intermediary Gateways such as Appriss, RxCheck which act as the PDMP Responders. The communication is performed using FHIR APIs. In this case the PDMP Requestors are isolated from the State PDMP Systems and the protocols they support. 

The Intermediaries in this case will use FHIR APIs to request data from one or more State PDMP Systems. The advantage here it is the same standard end to end however an Intermediary can provide value added information such as analytics, ability to integrate data from multiple states. 

The Intermediary plays the role of PDMP Responder for Transactions 1 and 2 but plays the role of PDMP Requestor for Transactions 1a and 1b where the State PDMP Systems play the PDMP Responder role. 

{% include img.html img="dep-option-2.png" caption="Figure 4: Deployment Option using Intermediaries and only FHIR" %}


#### Deployment Option 3:

In this deployment option, the EHRs, Apps and Clinical Systems act as the PDMP Requestors and interact with the State PDMP Systems which act as the PDMP Responders using the FHIR APIs 

There are no intermediaries being used in this deployment and State PDMP Systems have to support the necessary FHIR APIs and SMART Backend Authorization protocols.

{% include img.html img="dep-option-3.png" caption="Figure 5: Deployment Option using FHIR and no Intermediaries" %}

