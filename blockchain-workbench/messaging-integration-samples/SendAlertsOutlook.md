Create a Smart Contract in Azure Blockchain Workbench via Messaging 
====================================================================

Overview

This sample provides step by step instructions for reacting to events from the blockchain and sending an alert.



Create the Logic App
--------------------

![](media/82ed233953daa1bf6971180cfd1c3379.png)

Click the + symbol in the upper left corner of the screen to add a new resource

Search for and select Logic App and then click Create.

![](media/7f9bfaaebcf5a38fa305e958b5bbb538.png)

A logic app is initiated by a trigger. In this scenario, the trigger will be an event from Azure Blockchain Workbench delivered via the Event Grid.



![](media/e9eb985cbf4ef55ff95c1675f184cf15.png)

Click “New Step” and then select “New Action”

Select the SQL Connector and then select the “Execute Stored Procedure” action.

![](media/86d9cff5ef3e8f9a6b135777522e4dcb.png)

Select the appropriate Azure Workbench SQL DB server from the list.

Next, select the database for your Azure Blockchain Workbench deployment and
enter your database credentials.

![](media/f2d6160b0808057be9009f2dcb095d9f.png)

Select the stored procedure named “LogicAppGetContractCreationDetails”

Within the SQL connector, provide the name of the application and workflow to be created, for example
“AssetTransfer”, “AssetTransfer.” Also provide the email address for the user on
whose behalf this transaction will be sent.

Note – the name of the application and contract is the “Name” from the
configuration file. It is not the “DisplayName”

![](media/ef7bc4d025f9bfa747b75123cac778f1.png)

Click “New Step” and select “New Action”

Select the Variable connector and then select the “Initialize Variable” action.

![](media/8de7e521679a4e6e0d3009c657a97066.png)

Name the field “RequestId”

Select the type of “String”

Provide a value of “guid()”

![](media/4ca67ffb0e784381e78cbb8bda26cdc2.png)

Click “New Step” and select “New Action”

Select the Service Bus connector and then select the “Send Message” action.

![](media/420924cad452e61c78855ac8edc48102.png)

![](media/f4679f0e5391e5792fd4f790645c0f82.png)

Select “activityhub”

In the Session Id field, select RequestId from the Dynamic content dialog

In the content field, enter the below –

{

"ContractId": null,

"WorkflowName": "",

"UserChainIdentifier": "",

"ContractCodeArtifactBlobStorageURL": "",

"ConnectionId": ,

"Parameters": [

{

"name": "description",

"value": "ITEM DESCRIPTION GOES HERE"

},

{

"name": "price",

"value": "50000"

}

],

"OperationName": "CreateContract",

"RequestId": ""

}

Using the dynamic content dialog to insert the values that were generated from
the stored procedures. The result should resemble the image below.

![](media/82d5931b3dbd9bb564985e900cabcf38.png)


Note - This example is designed to work with a deployed version of [the Asset Transfer application](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer)whose constructor includes the description and price of an item that is being offered for sale.

For other contracts, you would alter the parameters section to include the appropriate name and value values for each parameter to that constructor for that contract.

Click Save

Click Run

![](media/70e528c75e320b794260fa6044709795.png)

If you check Azure Blockchain Workbench you will see that a new contract has
been added.