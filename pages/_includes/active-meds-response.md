A Server returns a search Bundle resource containing all the MedicationStatements with a status of "active" for the patient Brian Z


    HTTP/1.1 200 OK
    [other headers]
    {
    "resourceType" : "Bundle",
    "id" : "get-all-meds",
    "meta" : {
      "lastUpdated" : "2017-03-28T14:29:23Z"
    },
    "type" : "searchset",
    "total" : 4,
    "entry" : [
      {
        "fullUrl" : "http://fhir3.healthintersections.com.au/open/MedicationStatement/derivedfrom-mr-test2-1",
        "resource" : {
          "resourceType" : "MedicationStatement",
          "id" : "derivedfrom-mr-test2-1",
    ...snip...
          "status" : "active",
          "medicationCodeableConcept" : {
            "coding" : [
              {
                "system" : "http://www.nlm.nih.gov/research/umls/rxnorm",
                "code" : "104491",
                "display" : "Simvastatin 20 MG Oral Tablet [Zocor]",
                "userSelected" : false
              }
            ],
            "text" : "Zocor (simvastatin) 20mg Tablet"
          },
    ...snip...
      },
      {
        "fullUrl" : "http://fhir3.healthintersections.com.au/open/MedicationStatement/derivedfrom-mr-test2-2",
        "resource" : {
          "resourceType" : "MedicationStatement",
          "id" : "derivedfrom-mr-test2-2",
    ...snip...
          "status" : "active",
          "medicationCodeableConcept" : {
            "coding" : [
              {
                "system" : "http://www.nlm.nih.gov/research/umls/rxnorm",
                "code" : "311036",
                "display" : "Humulin R (insulin regular, human) U100  100units/ml inj solution",
                "userSelected" : false
              }
            ],
            "text" : "Humulin R (insulin regular, human) U100  100units/ml inj solution"
          },
    ...snip...
      },
      {
        "fullUrl" : "http://fhir3.healthintersections.com.au/open/MedicationStatement/test2-2",
        "resource" : {
          "resourceType" : "MedicationStatement",
          "id" : "test2-2",
    ...snip...
          "status" : "active",
          "medicationCodeableConcept" : {
            "coding" : [
              {
                "system" : "http://www.nlm.nih.gov/research/umls/rxnorm",
                "code" : "835829",
                "display" : "testosterone cypionate 100 MG/ML Injectable Solution",
                "userSelected" : false
              }
            ],
            "text" : "testosterone cypionate 100mg/ml inj"
          },
    ...snip...
        }
      }
     ]
    }
