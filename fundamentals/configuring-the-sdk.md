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

There are many different options that can be set during configuration. All options can be found below, grouped by category.

## General Options

### `propertyId`

**Type:** String\
**Required:** Yes\
**Default Value:** none

Your assigned property ID, which links the SDK to the appropriate accounts and configuration.

### `imsOrgId`

**Type:** String\
**Required:** Yes\
**Default Value:** none

Your assigned Experience Cloud organization ID.

### `edgeDomain`
**Type:** String\
**Required:** No\
**Default Value: `alpha.konductor.adobedc.net`** 

The domain that will be used to interact with Adobe Services. This is only used if you have a first party domain (CNAME) that proxies requests to Adobe's edge infrastructure.

### `log`
**Type:** Boolean\
**Required:** No\
**Default Value:** `false`

Indicates whether debugging messages should be displayed in the browser's JavaScript console. This setting is sticky and will persist until you change it. 

### `suppressErrors`
 **Type:** Boolean\
 **Required:** No\
 **Default Value:** `false`

Indicates whether errors should be suppressed. As described in [Executing Commands](executing-commands.md), _uncaught_ errors will be logged to the developer console, regardless of whether logging is enabled in Alloy. By setting `suppressErrors` to `true`, promises returned from Alloy will never be rejected, though errors will still be logged to the console if logging is enabled in Alloy. 

## Privacy Options

### `optInEnabled`
 **Type:** Boolean\
 **Required:** No\
 **Default Value:** `false`
 
Enables the opt-in feature, which allows work to be queued until the user provides his/her opt-in preferences. Once the user's preferences have been provided, work will either proceed or be aborted based on the user's preferences. See [Supporting Opt-In](supporting-opt-in.md) for more information.

# Personalization Options

### `prehidingId`
 **Type:** String\
 **Required:** No\
 **Default Value:** `alloy-prehiding`

Used to set the DOM node ID of the style tag that is created by personalization component to make sure the is no flicker.

### `prehidingStyle`
 **Type:** String\
 **Required:** No\
 **Default Value:** none
 
Used to customize the CSS style definition that would be used by personalization component to create a style tag that will ensure that there is no flicker.
If this option is not provided the personalization component won't try to apply any flicker management logic.

### `authoringMode`
 **Type:** Boolean\
 **Required:** No\
 **Default Value:** `false`
 
Enables personalization component authoring mode, which will ensure that no personalization content will be retrieved from the backend and no data collection requests will be initiated from within the personalization component.
For Adobe Target Visual Experience Composer this option should be intialized to something like:
```javascript
  authoringMode: document.location.href.indexOf("mboxEdit") !== -1
```
If other WYSIWYG tools are used to create personalized experiences and content then this option should be intialized accordingly.