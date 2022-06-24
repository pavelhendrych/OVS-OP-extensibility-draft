# Business Add-In implementation

Each validation must be located in a standalone BAdI implementation. 
&nbsp
Go to the transaction SE19, check 'New BAdI' in the Create implementation secion and provide OVS_ENH_CHECK as the name of the Enhancement spot. 
You will be prompted for the name of the Enhancement spot and the underlying BAdI, choose anything in customer namespace. 

## Implementation class
This Git repository contains an example class
```
ZCL_CHECKID_EXAMPLE
```
You can either 
- Checkout and copy this class to your system. Then it can be used as the Implementation class during the BAdI creation. 
- Create a brand new implementation class. 

When the BAdI implementation is done, you have to provide a filter
```
	CHECK_ID = <your_check_id>
```

Example
![BAdI filter](doc/BAdI_Filter.png)