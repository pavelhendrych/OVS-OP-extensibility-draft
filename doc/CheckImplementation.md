# Validation Implementation
When you created the BAdI implementaion, an Implementation class with the the IF_OVS_CHECK interface must have been created. The interface has two methods


| Field Name | Description |
| ----------- | ----------- |
| IF_OVS_CHECK~GET_DISABLEMENT_VALUES | Provides data for the Disablement. |
| IF_OVS_CHECK~VALIDATE | Handles the requests to validate certain data.  |

## Disablement
*@todo: Provide link to the help portal*
Disablement of Online Validation app enables you to disable the validation of business partners' master data, such as the validation of VAT registration numbers and bank account numbers.
The Disablement app collets all data from the selected business partners which would be normally validated and allows disabling them. This method is supposed to return ALL data from given business partners that would be validated within your implementation. It has the following parameters:
| Parameter name | Description |
| ----------- | ----------- |
| `it_partners` | Partner Type & Number pairs. See Partner Identification section bellow. |
| `ct_values`   | All the values of given partner that would be validated and can be disabled. A value for all columns must be provided in this method. |

The `ct_values` contains also the Partner identification as well as the Check ID and the values that can be disabled. 
Below you can also find more info on the SubID field. 

The method `IF_OVS_CHECK~GET_DISABLEMENT_VALUES` is called to enumerate all values that could be validated by this validation implementation. 

## Validation
The Online Validation Service routes the validation request to all BAdIs that are configured to be called in given Business process in the  [Customizing](Customizing.md).

The method `IF_OVS_CHECK~VALIDATE` is supposed to perform the validation. It has the following parameters: 
| Parameter name | Description |
| ----------- | ----------- |
| `iv_integration_spot` | ID of the business process from which the validation was requested. |
| `it_data` | Data to be validated. The format of the data is different for each integration sport. |

> See the method `ZCL_CHECKID_EXAMPLE->if_ovs_check~validate` on how to use the `iv_integration_spot` parameter and its possible values. Value of the `iv_integration_spot` determnies which data can be expected in the `it_data` parameter. The mentioned class is located in this GitHub repository, it is not a part of your S/4 HANA system. 

### Data extraction for validation
In the previous chapter, we learned how to identify the business process requesting the validation. It tells us  what is the content of `it_data`. Now we need to extract the data to be validated from the `it_data` and validate them. 
There are the following methods in the `ZCL_CHECKID_EXAMPLE` class that shows the conversion from type-free `it_data` into local variables with the correct data type. The examples show extractions of all available values in `it_data`. You can delete the variables which are not needed. 
You can use the method stub in your implementation and concentrate on the validation itself. 
| Method name | Description |
| ----------- | ----------- |
| `validate_billing_sales` | Sales Order or Billing posting. The `it_data` contains only VAT IDs toghether with the IDs of the business partners. |
| `validate_BP` | Business Partner master data maintenance. The `it_data` contains only VAT ID toghether with the ID of the business partner.  |
| `validate_mass_check` | Validate Multiple Partners program. The `it_data` contains ID of business partners to be validated. The data to be validated are expected to be read from the database. |
| `validate_payment` | Automatic payment program. The `it_data` contains data about the payment incl. all payment items. |
| `validate_fi_post` | FI document posting. The `it_data` contains data from the posted doument incl. One time account data. |
| `validate_fi_change` | FI document change. The `it_data` contains data from the posted doument incl. One time account data. |
| `validate_mm` | MM document posting (MIRO). The `it_data` contains data from the posted doument incl. One time account data.  |


### Partner identification
In both the request and the result there are 2 fields to identify the partner - partner type and partner number. 
On S/4 Hana, the partner type is usually equal to 'B' and the partner number is equal to the BUT000-PARTNER. 

The same logic is used in the Disablement function. 
| Partner Type | Partner Number and relation to the source table-column |
| ----------- | ----------- |
| B | Equal to the BUT000-PARTNER |
| C | Equal to KNA1-KUNNR |
| V | Equal to LFA1-LIFNR |

### Check SubID 
The SubID is usualy maintained in the `CHECK_SUB_ID` field, data element `OVF_D_CHECKSUBID`. It must be unique for each validated value for one partner. If the validated value fits the length of the fields (100 characters), use directly the validated value (e.g. bank account number or VAT ID). If not, it must be somehow shortened but it must stay unique for one partner. Moreover, the SubID might be visible in the Disablement application - if so, it shall be human redable. The user shall be able to say which value (e.g. which address) is being offered for Disabling. 

### Filling the table with results
Your validation implementation is supposed to create a table with results and append them into the changing parameter `ct_result` of the `IF_OVS_CHECK~VALIDATE` method.
You can reuse the existing DDIC objects like the table type `OVS_T_HISTORY` and the structure `OVS_S_HISTORY`. 
The table has the following fields: 
| Field name | Description |
| ----------- | ----------- |
| `MANDT  | Keep empty. |
| `PARTNER_TYPE`  | Partner type. See Partner identification section. |
| `PARTNER_NUMBER`  | Partner Number. See Partner identification section. |
| `CHECK_ID | ID` of your validation. Must be equal to the ID defined in the [Customizing](Customizing.md) |
| `CHECK_SUB_ID`  | ID of the validated value. See the Check SubID section |
| `TIMESTAMP`  | Keep empty. Will be filled with the current time stamp valid at the moment of writing the record to the database.  |
| `CHECK_RESULT`  | Result of the validation. It must be equal to one of the values defined in the [Customizing](Customizing.md) |
| `INTEGRATION_SPOT` | ID of the integration spot. It is delivered as a parameter to the `IF_OVS_CHECK~VALIDATE` method. |
| `REQUEST` | If needed, you can store here additional information about the request sent to the online service. | 
| `FULL_RESPONSE` | If needed, you can store here a piece or the whole response form the online service. |

The partner type & number are usualy derived from the input data. For example, when called from the Automatic Payment program, the input data contain ID of the vendor (or customer). 
The REQUEST and the FULL_RESPONSE contain a value in the xstring form. You can use the method `ZCL_CHECKID_EXAMPLE=>to_xstring` in the example class. The program Show history of online checks uses a xml viewer to display the content of those fields. Althought the value can have any form, SAP recommends to use the xml form for nice formatted view. Xml format is used by other SAP delivered validations. 

<!--
This info shall be published once the way how the BAdIs are called is changed. 

## Exceptional states
It might happen that the validation cannot be started or successfully finished. You can either return a dedicated Check Result code for a such situation or you can stop further validations. 
-->