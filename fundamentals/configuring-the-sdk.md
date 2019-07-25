---
description: >-
  Learn how to configure the SDK.
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases. 
{% endhint %}

# Configuring the SDK

Configuration for the SDK is done with the `configure` command. This should _always_ be the first command called.

```javascript
alloy("configure", {
  "propertyId": "ebebf826-a01f-4458-8cec-ef61de241c93"
});
```

The options are as follows.

* `propertyId` - \(required\) The property ID links the SDK to the appropriate accounts and configuration.
* `edgeDomain` - \(optional\) The domain that will be used to interact with Adobe Services. This is only used if you have a CNAME that proxies requests to Adobe's edge infrastructure.
* `log` - \(optional\) A boolean indicating whether debugging messages will be displayed in the browser's JavaScript console.
