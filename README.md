############################################################
########## S2CS - Supply Source Candidate service ##########
############################################################
owned by SILC(Seller Inventory and Location Capability) Team
https://w.amazon.com/bin/view/SILC/
Low level design
https://w.amazon.com/bin/view/SILC/Services/S2CS/LowLevelDesign/
->For a high level design please refer to Node Modeling - High Level Design. https://w.amazon.com/bin/view/SILC/Projects/ISPU/Design/NodeManagement/
S2CS is meant to help 3P seller manage their multiple locations (stores, warehouses) operational configurations/capabilities.
**'Node' has been renamed to 'Supply-Source' == 'S2'**
Input:
Seller via 
- Spectrum API
- Seller Central UI
Support:
- CRUD Retail Stores/Warehouses
- Sellers can support varied fulfillment experiences (delivery, pickup)
- Buyers can shop, decide ISPU/delivery depending on availabitity
Output:
Notifications of changes
SupplySourceCapabilityService is a Coral Service.
Detailed Components:
    - S2CS: ECS/Fargate Coral Service, it will perform CRUD Operations on sellers S2s.
    - DynamoDB: Main data storage. Used to keep a versioned state of a S2 configurations and capabilities.
    - Fanout Lambda + SNS: Lambda will distribute the changes coming from DynamoDB Streams into an SNS -Simple Notification Service- Topic, where downstream services can subscribe to get the latest changes.
https://w.amazon.com/bin/view/SILC/Services/S2CS/LowLevelDesign/#HArchitecturalDiagram
-> diagram that I have seen modified in ROAR docs
####################
Notification
####################
Notification payload:
    - Event Name: INSERT when a new S2 is created. MODIFY when an S2 is updated. ARCHIVE when an S2 is archived.
    - Namespace: Only available value is MFN, but it can be expanded in the future to support other namespaces.
    - Status: Status of the S2, in case I'm not interested on Inactive ones, or have a different.
    - Regional Visibility: Currently not a required field in S2CS, but for backwards compatibility can help filter for a given marketplace/realm.
SNS Message Body: will look exactly the same as the the output from GetSupplySource API.
Follow up research:
- High Level Design. https://w.amazon.com/bin/view/SILC/Projects/ISPU/Design/NodeManagement/
- Spectrum API
- Seller Central UI
