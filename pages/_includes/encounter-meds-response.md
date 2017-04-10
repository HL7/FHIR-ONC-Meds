A Server returns a search Bundle resource containing all the MedicationStatements for the patient Brian Z during inpatient Encounter A:


    HTTP/1.1 200 OK
    [other headers]
    "id": "get-encounter-meds",
    "meta": {
      "versionId": "1",
      "lastUpdated": "2017-04-10T12:49:37.665-04:00"
    },
    "type": "searchset",
    "total": 6,
     ...snip...
     "entry": [
       {
         "fullUrl": "http://wildfhir.aegis.net/fhir3-0-0/MedicationStatement/derivedfrom-mr-enca-1",
         "resource": {
           "resourceType": "MedicationStatement",
           "id": "derivedfrom-mr-enca-1",
      ...snip...
      },
      "context": {
        "reference": "Encounter/A",
        "display": "Inpatient Encounter A"
      },
      "status": "completed",
      "medicationCodeableConcept": {
        "coding": [
          {
            "system": "http://www.nlm.nih.gov/research/umls/rxnorm",
            "code": "1656313",
            "display": "Cefotaxime 1000 MG Injection"
          }
        ],
        "text": "Cefotaxime 1000Mg Powder For Solution For Injection"
      },
      ...snip...
      "fullUrl": "http://wildfhir.aegis.net/fhir3-0-0/MedicationStatement/derivedfrom-mr-enca-2",
      "resource": {
        "resourceType": "MedicationStatement",
        "id": "derivedfrom-mr-enca-2",
        ...snip...
        },
        "context": {
          "reference": "Encounter/A",
          "display": "Inpatient Encounter A"
        },
        "status": "completed",
        "medicationCodeableConcept": {
          "coding": [
            {
              "system": "http://www.nlm.nih.gov/research/umls/rxnorm",
              "code": "860092",
              "display": "1 ML Ketorolac Tromethamine 15 MG/ML Injection"
            }
          ],
          "text": "Ketorolac Tromethamine 15 MG/ML For Injection"
        },
        ...snip...
        "fullUrl": "http://wildfhir.aegis.net/fhir3-0-0/MedicationStatement/derivedfrom-mr-enca-3",
        "resource": {
          "resourceType": "MedicationStatement",
          "id": "derivedfrom-mr-enca-3",
          ...snip...
          "contained": [
            {
              "resourceType": "Medication",
              "id": "med-1",
              "meta": {
                "profile": [
                  "http://hl7.org/fhir/us/core/StructureDefinition/us-core-medication"
                ]
              },
              "code": {
                "text": "Azithromycin 500mg in 250 ml Normal Saline"
              },
              ...snip...
              },
            "context": {
              "reference": "Encounter/A",
              "display": "Inpatient Encounter A"
            },
            "status": "completed",
            "medicationReference": {
                "reference": "#med-1"
              },
              ...snip...
          "fullUrl": "http://wildfhir.aegis.net/fhir3-0-0/MedicationStatement/derivedfrom-mr-enca-4",
          "resource": {
            "resourceType": "MedicationStatement",
            "id": "derivedfrom-mr-enca-4",
          ...snip...
          "context": {
            "reference": "Encounter/A",
            "display": "Inpatient Encounter A"
          },
          "status": "completed",
          "medicationCodeableConcept": {
            "coding": [
              {
                "system": "http://www.nlm.nih.gov/research/umls/rxnorm",
                "code": "860601",
                "display": "Calcium Chloride 0.0014 MEQ/ML / Potassium Chloride 0.024 MEQ/ML / Sodium Chloride 0.103 MEQ/ML / Sodium Lactate 0.028 MEQ/ML Injectable Solution"
              }
            ],
            "text": "LRS"
            ...snip...
          "fullUrl": "http://wildfhir.aegis.net/fhir3-0-0/MedicationStatement/derivedfrom-mr-enca-5",
          "resource": {
            "resourceType": "MedicationStatement",
            "id": "derivedfrom-mr-enca-5",
          ...snip...
          "context": {
            "reference": "Encounter/A",
            "display": "Inpatient Encounter A"
          },
          "status": "active",
          "medicationCodeableConcept": {
            "coding": [
              {
                "system": "http://www.nlm.nih.gov/research/umls/rxnorm",
                "code": "212446",
                "display": "Zithromax 250 MG Oral Tablet"
              }
            ],
            "text": "Zithromax 500 MG Oral Tablet"
            ...snip...
          "fullUrl": "http://wildfhir.aegis.net/fhir3-0-0/MedicationStatement/derivedfrom-mr-enca-6",
          "resource": {
            "resourceType": "MedicationStatement",
            "id": "derivedfrom-mr-enca-6",
          ...snip...
          },
          "context": {
            "reference": "Encounter/A",
            "display": "Inpatient Encounter A"
          },
          "status": "active",
          "medicationCodeableConcept": {
            "coding": [
              {
                "system": "http://www.nlm.nih.gov/research/umls/rxnorm",
                "code": "308191",
                "display": "Amoxicillin 500 MG Oral Capsule"
              }
            ],
            "text": "Amoxicillin 500 MG"

     ]
    }
