# Validation Implementation
When you created the BAdI implementaion, an Implementation class with the the IF_OVS_CHECK interface must have been created. The interface has two methods


| Field Name | Description |
| ----------- | ----------- |
| IF_OVS_CHECK~GET_DISABLEMENT_VALUES | Provides data for the Disablement. |
| IF_OVS_CHECK~VALIDATE | Handles the requests to validate certain data.  |

## Disablement
*@todo: Provide link to the help portal*
Disablement of Online Validation app enables you to disable the validation of business partners' master data, such as the validation of VAT registration numbers and bank account numbers.
The Disablement app collets all data from the selected business partners which would be normally validated and allows disabling them. This method is supposed to return ALL data from given business partners that would be validated within your implementation. 

## Validation
This method is called from the Online Validation framework when the validation is requested. The service itself checks the [customizing](Customizing.md) whether your validation is enabled in given Integration spot. 

The method IF_OVS_CHECK~VALIDATE imports the paramter *iv_integration_spot* which 
