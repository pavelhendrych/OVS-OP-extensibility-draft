# Use of SAP delivered objects
SAP delivers classes and methods that can be reused in your validation. 

### Disablement
During the validation process, you might need to check whether currently validated value has been disabled for validations. For that, you can reuse the following method. 
```
	DATA(iv_disabled) = cl_ovs_services=>is_disabled( 
      EXPORTING 
        iv_partner_type = <partner_type>
        iv_partner_type = <partner_number>
        iv_check_id = <check_id>
        iv_check_sub_id = <sub_id ).
```
If the `iv_disabled` is set to true, the result of the validation shall be a value dedicated for Disabled validations in the [Customizing](Customizing.md).



### Reusing existing validation results
The validation of one particular value can be executed several times per day. It is unlikely that the used online service would return a different result. The [Customizing](Customizing.md) even allows to reuse an existing values for a couple of days. 
An existing value shall be reused only when it is equal to 'VALID'. A dedicated Result code for this situation shall be maintained in the [Customizing](Customizing.md). 
```
	cl_ovs_services=>is_valid_in_cache(  
      EXPORTING 
        iv_partner_type = <partner_type>
        iv_partner_type = <partner_number>
        iv_check_id = <check_id>
        iv_check_sub_id = <sub_id> 
      IMPORTING 
        ev_last_check_result = DATA(lv_last_result)
        rv_is_valid = DATA(lv_valid) ).
    IF lv_valid IS NOT INITIAL AND lv_last_result = <valid_in_cache>. 
		" don't validate, assign an RC for VALID. 
	ENDIF.
```

The parameter `lv_valid` is set if the cache contains a value within the validity set in the [Customizing](Customizing.md). Only if the `lv_valid` is set, you can use the `lv_last_result`. Otherwise, it has an undefined value. 

 ### Get message type
 The decision whether or not to dislay any validation message shall be based on the [Customizing](Customizing.md) and not purely on the validation result. For this, you might need to access the customizing to get the Message type and Message text. 
```
 	DATA(ls_msg) = cl_ovs_services=>get_message(
            iv_check_id         = <check_id>
            iv_check_result     = <check_result>
            iv_integration_spot = <integration_spot> ).
	IF ls_msg-type = 'E'.
		MESSAGE ...
	ENDIF. 		
```

### Access to the customizing
In case you need to access some other values maintained in the [Customizing](Customizing.md), you can reuse one of the methods defined in the class `cl_ovs_customizing`. 

 
 ### Communication with an external service
In case a simple, xml/text payload-based communication is required, you can reuse the class `cl_ovs_comm`. The communication is synchronous. This class might not be suitable for REST-like based services where a part of the request is a part of the URL. 

| Method | Description |
| ----------- | ----------- |
| `open_connection` | Opens a connection to a online resource. `iv_comm_system` requires a name of an RFC. This name can be obtained from the customizing using the `cl_ovs_customizing` class.  | 
| `send_sync` | Sends the request in the http request body and waits for a response which is then returned.  | 
| `close_connection` | Must be called at the end of the validation process to release all the network resources.  | 
If there is a problem within the communication, the `send_sync` method throws an `cx_ovs_error`. Similarily, the method `open_connection` throws the same exception if the communication cannot be opened (wrong URL, Unauthorized etc.).

