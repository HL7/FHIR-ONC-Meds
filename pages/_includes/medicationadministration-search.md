
`GET /MedicationAdministration?patient={id}`

*Support:* Mandatory for client to support search MedicationAdministration by patient parameter.  Optional for server to support.

*Implementation Notes:*  Used when the server application represents the medication using either an inline code or a contained Medication resource. This searches for all MedicationAdministration resources for a patient and returns a Bundle of all MedicationAdministration resources for the specified patient.  [(how to search by reference)].

Example: [Get all MedicationAdministration resources for a patient](todo.html)

-----------

`GET /MedicationAdministration?patient={id}&_include=MedicationAdministration:medication`

*Support:* Mandatory for client to support search by patient and the include parameter.  Optional for server to support.

*Implementation Notes:*  Used when the server application represents the medication with an external reference to  a Medication resource. This searches for all MedicationAdministration resources for a patient and returns a Bundle of all MedicationAdministration and Medication resources for the specified patient.  [(how to search by reference)].

Example: [Get all MedicationAdministration resources for a patient using the \_include parameter](todo.html)

-------

  [(how to search by reference)]: http://build.fhir.org/search.html#reference
  [(how to search by token)]: http://build.fhir.org/search.html#token
  [Composite Search Parameters]: http://build.fhir.org/search.html#combining
  [(how to search by date)]: http://build.fhir.org/search.html#date
