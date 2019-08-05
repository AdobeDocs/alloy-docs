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

## `propertyId`

**Type:** String\
**Required:** Yes\
**Default Value:** none

Your assigned property ID, which links the SDK to the appropriate accounts and configuration.

## `imsOrgId`

**Type:** String\
**Required:** Yes\
**Default Value:** none

Your assigned Experience Cloud organization ID.

## `edgeDomain`
**Type:** String\
**Required:** No\
**Default Value: `alpha.konductor.adobedc.net`** 

The domain that will be used to interact with Adobe Services. This is only used if you have a CNAME that proxies requests to Adobe's edge infrastructure.

## `log`
**Type:** Boolean\
**Required:** No\
**Default Value:** `false`

Indicates whether debugging messages should be displayed in the browser's JavaScript console.

## `suppressErrors`
 **Type:** Boolean\
 **Required:** No\
 **Default Value:** `false`

Indicates whether errors should be suppressed. As described in [Executing Commands](executing-commands.md), _uncaught_ errors will be logged to the developer console, regardless of whether logging is enabled in Alloy. By setting `suppressErrors` to `true`, promises returned from Alloy will never be rejected, though errors will still be logged to the console if logging is enabled in Alloy. 
