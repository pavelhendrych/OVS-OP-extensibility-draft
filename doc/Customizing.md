# Online Validation Service Customizing

## Create a definition of your validation

First of all, you need to provide definiton of your validation - the CheckID. This id must exist before your start the subsequent development steps. 

The customizing is available in the IMG on the following path (in case you obtained the OVS by a support package)
Cross-Application Components -> SAP Business Partner -> Business Partner -> Basic Settings -> Online Validation -> Maintain Online Validation Checks
or you can use the transaction OVS_CHECKS. 

### Define Check IDs
Create the main record for your Validation.

| Field Name | Description |
| ----------- | ----------- |
| Check ID | ID of the check. It must start with Z or Y. The rest of the ID can be chosen freely.  |
| Validity of Check Result| It defines how many days can be an existing result returned again without a need to validate online. |
| Check ID description | Human readable name. It will appear in F4 Helps |

### Maintain Result Codes
Create as many various result codes as required by the business. 

| Field Name | Description |
| ----------- | ----------- |
| Result Code | ID of the result. |
| Integration Spot | Name of the business process.  |
| Message Type | Type of the message shown to the business user. |
| Save | If checked, the result will be saved into the OVS_HISTORY table. | 
| Result Code Description | Text that will be displayed to the business user |

You can keep the Integration spot empty. If so, this row will be used for the Integration spots that don't have a dedicated row here in this table for given Result code. It allows to have a special text for certain Integration spot and a fallback text for other Integration spots. 

> The value in the Result Code Descripition is saved in the logon language used during maintenance of the view. If you require the text in other languages, this column must be maintained again while being logged on in all required languages. 

## Enable Selective Online Validation
Here you can enable or disable the validation execution in each supported business process. To enable the validation in certain process, this table must contain a row for given Integration spot and the checkbox must ticked. 
| Field Name | Description |
| ----------- | ----------- |
| Integration Spot | Name of the business process |
| Enable | If checked, the validation will be enabled | 


## Create a RFC

## Configure the OVS with the newly created RFC
After the RFC is created, the OVS must know which RFC shall be used for which Check ID. Open the following IMD path Cross-Application Components -> SAP Business Partner -> Business Partner -> Basic Settings -> Online Validation -> Activate Validation Checks
or use the transaction OVS_CHECKACTIV. 
