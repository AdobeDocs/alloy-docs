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
  "configId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "imsOrgId":"ADB3LETTERSANDNUMBERS@AdobeOrg"
});
```

There are many different options that can be set during configuration. All options can be found below, grouped by category.

## General Options

### `configId`

| **Type** | **Required** | **Default Value** |
| -------- | ------------ | ----------------- |
| String   | Yes          | none              |

Your assigned configuration ID, which links the SDK to the appropriate accounts and configuration.

### `imsOrgId`

| **Type** | **Required** | **Default Value** |
| -------- | ------------ | ----------------- |
| String   | Yes          | none              |

Your assigned Experience Cloud organization ID.

### `edgeDomain`

| **Type** | **Required** | **Default Value**             |
| -------- | ------------ | ----------------------------- |
| String   | No           | `beta.adobedc.net` |

The domain that will be used to interact with Adobe Services. This is only used if you have a first party domain (CNAME) that proxies requests to Adobe's edge infrastructure.

### `debugEnabled`

| **Type** | **Required** | **Default Value** |
| -------- | ------------ | ----------------- |
| Boolean  | No           | `false`           |

Indicates whether debugging should be enabled. Setting this config to `true` enables the following features:

| **Feature** |                                     |
| ------------ | ----------------------------------- |
| Synchronous validation    | Validates the data being collected against the schema and return an error in the response under the following label: `collect:error OR success` |
| Console logging    | Enables debugging messages to be displayed in the browser's JavaScript console |

### `errorsEnabled`

| **Type** | **Required** | **Default Value** |
| -------- | ------------ | ----------------- |
| Boolean  | No           | `true`            |

Indicates whether errors should be suppressed. As described in [Executing Commands](executing-commands.md), _uncaught_ errors will be logged to the developer console, regardless of whether debugging is enabled in Alloy. By setting `errorsEnabled` to `false`, promises returned from Alloy will never be rejected, though errors will still be logged to the console if logging is enabled in Alloy.

### `context`

| **Type**         | **Required** | **Default Value**                                  |
| ---------------- | ------------ | -------------------------------------------------- |
| Array of Strings | No           | `["web", "device", "environment", "placeContext"]` |

Indicates which context categories to collect automatically as described in [Automatic Information](../reference/automatic-information.md).  If this configuration is not specified, all of the categories will be used by default.

## Data Collection

### `clickCollectionEnabled`

| **Type** | **Required** | **Default Value** |
| -------------------- | -------------------------------- | ----------------------------------------------- |
| Boolean    | No                               | `true`                                  |

Indicates whether data associated with link clicks should be automatically collected or not. For clicks that qualify as link clicks, the following [Web Interaction](https://github.com/adobe/xdm/blob/master/docs/reference/context/webinteraction.schema.md) data is collected:

| **Property** |                                     |
| ------------ | ----------------------------------- |
| Link Name    | Name determined by the link context |
| Link URL     | Normalized URL                      |
| Link Type    | Set to download, exit, or other     |

## Privacy Options

### `optInEnabled`

| **Type** | **Required** | **Default Value** |
| -------------- | ---------------------- | -------------------------------- |
| Boolean   | No                     | `false`                     |

Enables the opt-in feature, which allows work to be queued until the user provides his/her opt-in preferences. Once the user's preferences have been provided, work will either proceed or be aborted based on the user's preferences. See [Supporting Opt-In](supporting-opt-in.md) for more information.

## Personalization Options

### `prehidingStyle`

| **Type** | **Required** | **Default Value** |
| -------- | ------------ | ----------------- |
| String   | No           | none              |

Used to create a CSS style definition that will hide content areas of your web page while personalized content is loaded from the server. If this option is not provided, the SDK will not attempt to hide any content areas while personalized content is loaded, potentially resulting in "flicker".

For example, if you had an element on your web page with an ID of `container` whose default content you would like to hide while personalized content is being loaded from the server, an example of a prehiding style would be as follows:

```javascript
  prehidingStyle: "#container { opacity: 0 !important }"
```

## Audiences Options

### `urlDestinationsEnabled`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Boolean | No | `true` |

Enables URL destinations, which allows the firing of URLs based on segment qualification.

### `cookieDestinationsEnabled`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Boolean | No | `true` |

Enables cookie destinations, which allows the setting of cookies based on segment qualification.

## Identity Options

### `idMigrationEnabled`

| **Type** | **Required** | **Default Value** |
| -------------- | ---------------------- | -------------------------------- |
| Boolean   | No                     | `true`                     |

Enables the AEP Web SDK to preserve the ECID by reading/writing the AMCV cookie. Use this config until users are fully migrated to the Alloy cookie and in situations where you have mixed pages on your website.

### `idSyncEnabled`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Boolean | No | `true` |

Enables the ID sync feature, which allows the firing of URLs to synchronize the Adobe unique user ID with the unique user ID of a third party data source.

### `idSyncContainerId`

| **Type** | **Required** | **Default Value** |
| -------- | ------------ | ----------------- |
| Number   | No           | none              |

The container ID that specifies which ID syncs will be fired. This is a nonnegative integer that can be obtained from your consultant.

### `thirdPartyCookiesEnabled`

| **Type** | **Required** | **Default Value** |
| -------- | ------------ | ----------------- |
| Boolean  | No           | true              |

Enables the setting of Adobe third-party cookies. The SDK has the ability to persist the visitor ID in a third party context to enable the same visitor ID to be used across site. This is useful if you have multiple site or you want to share data with partners; however, sometimes this is not desired for privacy reasons.
