
## Background on Prescription Drug Monitoring Program (PDMP) on FHIR
{:.no_toc}

<!-- TOC  the css styling for this is \pages\assets\css\project.css under 'markdown-toc'-->

* Do not remove this line (it will not be displayed)
{:toc}

<!-- end TOC -->

This section outlines the definitions, interpretations, and specific guidance for using FHIR with a state PDMP. Prescription Drug Monitoring Programs are being deployed in every state in the US to address prescription drug misuse, abuse, and diversion. They provide critical health information to physicians and other health care providers about an individual’s history of controlled substance prescriptions.  The current connections from PDMPs primarily utilize NCPDP, ASAP, and NIEM. The guidance provided here is intended to supplement, not replace, existing exchanges using other standards.


States have approached the deployment of PDMPs in a variety of approaches...<br>

-ADD IMAGE from our PPT-

In order to support a FHIR based exchange this guide includes a mapping to the key resources.


### FHIR Mapping to Existing PDMP Exchanges

#### Mappings for NCPDP Request
{:.no_toc}

This section includes the minimal mapping for the PDMP request.

- [MedicationRequest]({{ site.data.fhir.path }}/medicationrequest-mappings.html#script10.6): includes a full mapping for medicationRequest resource to SCRIPT 10.6
{% include table.md %}


{% include img.html img="message_request.png" caption="Figure 1: Medication Request Mapping to NCPDP and FHIR" %}

#### Mappings for NCPDP Response
{:.no_toc}

{% include img.html img="message_response1.png" caption="Figure 1: Medication Response Mapping to NCPDP and FHIR" %}
{% include img.html img="message_response2.png"  %}

- [Medication]({{ site.data.fhir.path }}/medication-mappings.html#script10.6):  includes a mapping for medication resource to SCRIPT 10.6
- [MedicationDispense]({{ site.data.fhir.path }}/medicationdispense-mappings.html): NCPDP mapping does not exist
- [MedicationAdministration]({{ site.data.fhir.path }}/medicationadministration-mappings.html): NCPDP mapping does not exist
- [MedicationStatement]({{ site.data.fhir.path }}/medicationstatement.html): NCPDP mapping does not exist



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



