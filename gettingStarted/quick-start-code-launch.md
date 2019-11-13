---
description: >-
  Quick start guid for using the Experience Platform Web SDK to collect data. 
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases.
{% endhint %}

# Prerequisites

Currently the Adobe Experience Platform Web SDK only supports sending data to the Adobe Experience Platform using XDM. You must have the following prerequisites.

- Be entitled to the Adobe Experience Platform
- Be using the latest version of Visitor.js

## Prepare Platform

To be able to send data to AEP you will need to create an XDM Schema and a dataset that uses that schema.

- [Create a schama](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/schema_editor_tutorial/schema_editor_tutorial.md)
- [Create a dataset](https://platform.adobe.com/dataset/overview) with your schema where you would like the data to land
- Add the Alloy Mixin

## Requesting a Configuration ID

You must have a configuration ID to be able to use the SDK. The configuration ID is what ensures that your data is routed to the right place. You can obtain a configuration ID either from your consultant or through client care. They will need the following information.

- Org ID: You can find this using the instructions [here](https://docs.adobe.com/content/help/en/core-services/interface/manage-users-and-products/organizations.html)
- Dataset ID: This is available in the dataset UI when you click on a dataset
- Schema ID: This is available in the URL of the schema creation screen
- Friendly Name: This will be the friendly name that will be used in future UIs for this configuration

# Install the SDK in Launch

Log into Launch and install the `AEP Web SDK`. As part of installing the SDK you will be prompted to configure the SDK. Enter your `configId` and your `orgID`.

For more details on different configuration options see our [Configuring the SDK](../fundamentals/configuring-the-sdk.md) section.

# Send an event

Once the extension is installed can start sending events by adding an "Send Beacon" action from the AEP Web SDK. We recommend sending at least one event every time a page is loaded with the `viewStart` option checked. 

For more details on how to track events see your [Tracking Events](../fundamentals/tracking-events.md) section.

# Send data

You can send data along with your events that matches the schema you created earlier. If I were a commerce site and I had implemented the commerce mixin I would send the following when someone viewed a product. I would send an object with the following structure 

```javascript
{
  "commerce": {
    "productListAdds": {
        "value":1
    }
  },
  "productListItems":{
      "name":"Floppy Green Hat",
      "SKU":"HATFLP123",
      "product":"1234567",
      "quantity":2
  }
}
```
