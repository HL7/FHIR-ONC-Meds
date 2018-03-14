<table class="grid">
 <tbody><tr><td><a name="Medication"> </a><a name="medication"> </a><b>Medication</b></td><td>NewRx/MedicationPrescribed
-or-
RxFill/MedicationDispensed
-or-
RxHistoryResponse/MedicationDispensed
-or-
RxHistoryResponse/MedicationPrescribed</td></tr>
 <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;code</td><td>coding.code = //element(*,MedicationType)/DrugCoded/ProductCode

coding.system = //element(*,MedicationType)/DrugCoded/ProductCodeQualifier

coding.display = //element(*,MedicationType)/DrugDescription</td></tr>
 <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;status</td><td></td></tr>
 <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;manufacturer</td><td>no mapping</td></tr>
 <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;form</td><td>coding.code =  //element(*,DrugCodedType)/FormCode

coding.system = //element(*,DrugCodedType)/FormSourceCode</td></tr>
 <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;amount</td><td></td></tr>
 <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;ingredient</td><td></td></tr>
 <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;item[x]</td><td>coding.code = //element(*,MedicationType)/DrugCoded/ProductCode

coding.system = //element(*,MedicationType)/DrugCoded/ProductCodeQualifier

coding.display = //element(*,MedicationType)/DrugDescription</td></tr>
 <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;isActive</td><td></td></tr>
 <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;amount</td><td>//element(*,DrugCodedType)/Strength</td></tr>
 <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;batch</td><td>no mapping</td></tr>
 <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;lotNumber</td><td>no mapping</td></tr>
 <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;expirationDate</td><td>no mapping</td></tr>
 <tr><td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serialNumber</td><td></td></tr>
</tbody></table>
