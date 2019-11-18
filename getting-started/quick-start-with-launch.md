---
description: >-
  Quick start guide for using the Experience Platform Web SDK extension to collect data. 
---

# Prerequisites

Currently the Adobe Experience Platform Web SDK only supports sending data to the Adobe Experience Platform using XDM. You must satisfy the following prerequisites.

- Have a [1st party domain (CNAME)](https://docs.adobe.com/content/help/en/core-services/interface/ec-cookies/cookies-first-party.html) enabled
- Be entitled to the Adobe Experience Platform
- Be using the latest version of the Visitor ID service

## Prepare Platform

To be able to send data to AEP you will need to create an XDM schema and a dataset that uses that schema.

- [Create a schema](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/schema_editor_tutorial/schema_editor_tutorial.md)
- Add the Alloy Mixin to the schema you created
- [Create a dataset](https://platform.adobe.com/dataset/overview) with your schema where you would like the data to land

## Requesting a Configuration ID

You must have a configuration ID to be able to use the SDK. The configuration ID is what ensures that your data is routed to the right place. You can obtain a configuration ID either from your consultant or through client care. They will need the following information.

- Org ID: You can find this using the instructions [here](https://docs.adobe.com/content/help/en/core-services/interface/manage-users-and-products/organizations.html)
- Dataset ID: This is available in the dataset UI when you click on a dataset
- Schema ID: This is available in the URL of the schema creation screen
- Friendly Name: This will be the friendly name that will be used in future UIs for this configuration

## Install the SDK in Launch

Log into Launch and install the `AEP Web SDK` extension. As part of installing the SDK you will be prompted to configure the extension. Enter the Config ID you requested above. The extension will automatiaclly fill in your Organization ID.

For more details on different configuration options see our [Configuring the SDK](../fundamentals/configuring-the-sdk.md) section.

## Send an Event

Once the extension is installed, can start sending events by adding a "Send Beacon" action from the AEP Web SDK extension. We recommend sending at least one event every time a page is loaded with the "occurs at the start of a view" option checked.

For more details on how to track events see your [Tracking Events](../fundamentals/tracking-events.md) section.

## Send Data

You can send data along with your events that matches the schema you created earlier. If I owned a commerce site and I had added the commerce mixin to my schema I would send the following structure when someone viewed a product.

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
