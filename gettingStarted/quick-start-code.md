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

# Install the SDK

Download the latest version of the SDK and copy the following code into the `<head>` for you html.

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="alloy.js" async></script>
```

Be sure to adjust the reference to `alloy.js` to your domain.

For more details and additional options see our section on [installing the SDK](../fundamentals/installing-the-sdk.md)

# Configure the SDK

After the library is added to the page. You will need to call the `configure` command.

```javascript
alloy("configure", {
  "configId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "imsOrgId":"ADB3LETTERSANDNUMBERS@AdobeOrg"
});
```

Be sure to change the `configId` and the `imsOrgId` out for your configuration ID (instructions above) and you OrgId (same as above).

For more details on different configuration options see our [Configuring the SDK](../fundamentals/configuring-the-sdk.md) section.

# Send an event

Once the `configure` command called you can start sending events.

```javascript
alloy("event", {
  "viewStart":true
});
```

This will send an event and will include all the details that the SDK collects automatically, including page information and browser information. `viewStart` is an options that lets the SDK know that a view has started or that a page is loading and it is safe to render.

For more details on how to track events see your [Tracking Events](../fundamentals/tracking-events.md) section.

# Send data

You can send data along with your events that matches the schema you created earlier. If I were a commerce site and I had implemented the commerce mixin I would send the following when someone viewed a product. 

```javascript
alloy("event", {
  "viewStart":true
  "xdm": {
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
});
```
