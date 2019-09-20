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
  "propertyId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "imsOrgId":"ADB3LETTERSANDNUMBERS@AdobeOrg"
});
```

There are many different options that can be set during configuration. All options can be found below, grouped by category.

## General Options

### `propertyId`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| String | Yes | none |

Your assigned property ID, which links the SDK to the appropriate accounts and configuration.

### `imsOrgId`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| String | Yes | none |

Your assigned Experience Cloud organization ID.

### `edgeDomain`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| String | No | `alpha.konductor.adobedc.net` |

The domain that will be used to interact with Adobe Services. This is only used if you have a first party domain (CNAME) that proxies requests to Adobe's edge infrastructure.

### `logEnabled`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Boolean | No | `false` |

Indicates whether debugging messages should be displayed in the browser's JavaScript console. This setting is sticky and will persist until you change it.

### `errorsEnabled`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Boolean | No | `true` |

Indicates whether errors should be suppressed. As described in [Executing Commands](executing-commands.md), _uncaught_ errors will be logged to the developer console, regardless of whether logging is enabled in Alloy. By setting `errorsEnabled` to `false`, promises returned from Alloy will never be rejected, though errors will still be logged to the console if logging is enabled in Alloy.

### `context`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Array of Strings | No | `["web", "device", "environment", "placeContext"]` |

Indicates which context categories to collect automatically as described in [Automatic Information](../reference/automatic-information.md).  If this configuration is not specified, all of the categories will be used by default.

### `clickCollectionEnabled`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Boolean | No | `true` |

Indicates whether data associated with link clicks should be collected or not. For clicks that qualify as link clicks the following [Web Interaction](https://github.com/adobe/xdm/blob/master/docs/reference/context/webinteraction.schema.md) data is collected:

| **Property** |  |
| -- | -- |
| Link Name | Name determined by the link context |
| Link URL | Normalized URL |
| Link Type | Either set to download, exit, or other |

## Privacy Options

### `optInEnabled`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Boolean | No | `false` |

Enables the opt-in feature, which allows work to be queued until the user provides his/her opt-in preferences. Once the user's preferences have been provided, work will either proceed or be aborted based on the user's preferences. See [Supporting Opt-In](supporting-opt-in.md) for more information.

## Personalization Options

### `prehidingStyle`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| String | No | none |

Used to create a CSS style definition that will hide content areas of your web page while personalized content is loaded from the server. If this option is not provided, the SDK will not attempt to hide any content areas while personalized content is loaded, potentially resulting in "flicker".

For example, if you had an element on your web page with an ID of `container` whose default content you would like to hide while personalized content is being loaded from the server, an example of a prehiding style would be as follows:

```javascript
  prehidingStyle: "#container { opacity: 0 !important }"
```


### `authoringModeEnabled`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Boolean | No | `false` |

Enables the personalization component authoring mode, which will ensure that no personalization content will be retrieved from the server and no data collection requests will be initiated from within the personalization component.
For Adobe Target Visual Experience Composer this option should be initialized to something like:
```javascript
  authoringModeEnabled: document.location.href.indexOf("mboxEdit") !== -1
```
If other WYSIWYG tools are used to create personalized experiences and content, then this option should be initialized accordingly.

## Audiences Options

### `destinationsEnabled`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Boolean | No | `true` |

Enables the destinations feature, which allows the firing of URLs and setting of cookies based on segment qualification.

## Identity Options

### `idSyncsEnabled`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Boolean | No | `true` |

Enables the ID syncs feature, which allows the firing of URLs to synchronize the Adobe unique user ID with the unique user ID of a third party data source.

### `idSyncContainerId`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Number | No | none |

The container ID that specifies which ID syncs will be fired. This is a nonnegative integer that can be obtained from your consultant.
