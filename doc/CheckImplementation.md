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

The method IF_OVS_CHECK~VALIDATE is supposed to perform the validation. It has the following parameters: 
| Parameter name | Description |
| ----------- | ----------- |
| `iv_integration_spot` | ID of the business process from which the validation was requested. |
| `it_data` | Data to be validated. The format of the data is different for each integration sport. |

> See the method `ZCL_CHECKID_EXAMPLE->if_ovs_check~validate` on how to use the *iv_integration_spot* parameter and its possible values. Value of the *iv_integration_spot* determnies which data can be expected in the *it_data* parameter. The mentioned class is located in this GitHub repository, it is not a part of your S/4 HANA system. 

### Data extraction for validation
In the previous chapter, we learned how to identify the business process requesting the validation. It tells us  what is the content of `it_data`. Now we need to extract the data to be validated from the `it_data` and validate them. 
There are the following methods in the `ZCL_CHECKID_EXAMPLE` class that shows the conversion from type-free `it_data` into local variables with the correct data type. The examples show extractions of all available values in 'it_data'. You can delete the variables which are not needed. 
You can use the method stub in your implementation and concentrate on the validation itself. 
| Method name | Description |
| ----------- | ----------- |
| `validate_billing_sales` | Sales Order or Billing posting. The it_data contains only VAT IDs toghether with the IDs of the business partners. |
| ----------- | ----------- |
| `validate_BP` | Business Partner master data maintenance. The it_data contains only VAT ID toghether with the ID of the business partner.  |
| `validate_mass_check` | Validate Multiple Partners program. The it_data contains ID of business partners to be validated. The data to be validated are expected to be read from the database. |
| `validate_payment` | Automatic payment program. The it_data contains data about the payment incl. all payment items. |

In both the request and the result there are 2 fields to identify the partner - partner type and partner number. On S/4 Hana, the partner type is always equal to 'B'  and the partner number is equal to the BUT000-PARTNER. 


## How to populate the results table
ovs_t_history
to_xstring
 cl_ovs_services=>is_disabled
 cl_ovs_services=>is_valid_in_cache
 comm class