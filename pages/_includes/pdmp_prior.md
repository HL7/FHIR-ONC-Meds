
## Background on Prescription Drug Monitoring Program (PDMP) on FHIR
{:.no_toc}

<!-- TOC  the css styling for this is \pages\assets\css\project.css under 'markdown-toc'-->

* Do not remove this line (it will not be displayed)
{:toc}

<!-- end TOC -->

This section outlines the definitions, interpretations, and specific guidance for using FHIR with a state PDMP. Prescription Drug Monitoring Programs are being deployed in every state in the US to address prescription drug misuse, abuse, and diversion. They provide critical health information to physicians and other health care providers about an individual’s history of controlled substance prescriptions. While there is not one single PDMP specification, the Centers for Disease Control and Prevention provide background information on [what states need to know about PDMPs](https://www.cdc.gov/drugoverdose/pdmp/states.html). The current connections to PDMPs primarily utilize NCPDP, ASAP, and NIEM. The guidance provided here is intended to supplement, not replace, existing exchanges using other standards.


States have approached the deployment of PDMPs in a variety of approaches...<br>

-ADD IMAGE from our  PPT-

In order to support a FHIR based exchange this guide includes a mapping to the key resources.


### PDMP Exchange Approaches
{:.no_toc}
The existing state PDMP exchanges primarily rely on a messaging approach. This section describes a parallel messaging approach in FHIR and outlines the initial steps for a FHIR RESTful approach.

#### PDMP Exchange using FHIR Messaging
{:.no_toc}

- Profile MessageHeader
- Build extension for fields that don't exist?

#### PDMP Exchange using FHIR API
{:.no_toc}

- Include a section and discuss what would be necessary
- Authorization server - unlikely states will want to deploy
- Give a couple example queries
- Partial section build out since uncertain if folks will use -- primarily to generate discussion.


### FHIR Mapping to Existing PDMP Exchanges

#### Mappings for NCPDP Request
{:.no_toc}

This section includes the minimal mapping for the PDMP request from an EHR to a state PDMP.

<!-- [MedicationRequest]({{ site.data.fhir.path }}medicationrequest-mappings.html#script10.6): includes a full mapping for medicationRequest resource to SCRIPT 10.6 -->


{% include NCPDP_Request_table.html %}


#### Mappings for NCPDP Response
{:.no_toc}

This section includes the minimal mapping for the PDMP response from a state PDMP to an EHR.

{% include NCPDP_Response_table.html %}

#### Mappings for PMIX Request

This section includes the minimal mapping for the PDMP request from an EHR to a state PDMP.

{% include PMIX_Request_table.html %}

#### Mappings for PMIX Response

This section includes the minimal mapping for the PDMP response from a state PDMP to an EHR.

{% include PMIX_Response_table.html %}
