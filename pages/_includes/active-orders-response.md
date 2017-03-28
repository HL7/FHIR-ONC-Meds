A Server returns a search Bundle resource containing all the MedicationRequests with a status of "active" for the patient Brian Z.


      HTTP/1.1 200 OK
      [other headers]
      {
     "resourceType": "Bundle",
      "id": "get-all-active-orders",
      "meta": {
        "versionId": "1",
        "lastUpdated": "2017-03-28T14:06:11.808-04:00"
      },
      "type": "searchset",
      "total": 2,
      ...snip...
      "entry": [
        {
          "fullUrl": "http://wildfhir.aegis.net/fhir3-0-0/MedicationRequest/test2-1",
          "resource": {
            "resourceType": "MedicationRequest",
            "id": "test2-1",
        ...snip...
            "status": "active",
            "intent": "order",
            "medicationCodeableConcept": {
              "coding": [
                {
                  "system": "http://www.nlm.nih.gov/research/umls/rxnorm",
                  "code": "104491",
                  "display": "Simvastatin 20 MG Oral Tablet [Zocor]",
                  "userSelected": false
                }
              ],
              "text": "Zocor (simvastatin) 20mg Tablet"
        ...snip...
        },
        {
          "fullUrl": "http://wildfhir.aegis.net/fhir3-0-0/MedicationRequest/test2-2",
          "resource": {
            "resourceType": "MedicationRequest",
            "id": "test2-2",
        ...snip...
            "status": "active",
            "intent": "order",
            "medicationCodeableConcept": {
              "coding": [
                {
                  "system": "http://www.nlm.nih.gov/research/umls/rxnorm",
                  "code": "311036",
                  "display": "Humulin R (insulin regular, human) U100  100units/ml inj solution",
                  "userSelected": false
                }
              ],
              "text": "Humulin R (insulin regular, human) U100  100units/ml inj solution"
            },
        ...snip...
      ]
    }
