# Configuration for Online Validation

## Create a Definition for Your Validation

Define the check ID, maintain the result codes, and enable selective online validation in Customizing for Cross-Application Components under SAP Business Partner -> Online validation -> Maintain Online Validation Checks (transaction OVS_CHECKS). 

Note that SAP recommends to maintain at least the following result codes: 

| Result Code | Description |
| ----------- | ----------- |
| 1 | Valid | 
| 2 | Invalid | 
| 3 | Valid, Internal result exists *@todo link cache handling* | 
| 4 | Validation Disabled. *@todo add link to the help protal* | 
| 4 | Technical error | 

For more information about how to maintain this Customizing activity, check the documentation in the system.


## Create an Remote Function Call (RFC)
You need an RFC communication for a connection with an external service. 


### Online Validation
Go to the SM59 transaction and create an RFC to the desired service. 
Make sure you create the parameters for the RFC according to the documentation of the online service. 


## Configure the Online Validation with the New RFC
Once you have created the RFC, maintain the RFC to the respective check ID in Customizing for Cross-Application Components under SAP Business Partner -> Business Partner -> Basic Settings -> Online Validation -> Activate Validation Checks (transaction OVS_CHECKACTIV). 

Note that the name of the RFC is case-sensitive and must be maintained as in transaction SM59.
Once you have maintained the RFC, this validation will be executed. To avoid any issues when running your business processes, make sure to do this step when the implementation is ready for testing. 

For more information about how to maintain this Customizing activity, check the documentation in the system.


## Offline Validation
If your validation does not communicate with an external service, maintain an entry with the Check ID and empty RFC in Customizing for Cross-Application Components under SAP Business Partner -> Business Partner -> Basic Settings -> Online Validation -> Activate Validation Checks (transaction OVS_CHECKACTIV). 