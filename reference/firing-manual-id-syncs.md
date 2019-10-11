---
description: >-
  Learn how to manually synchronize the Adobe unique user ID with the unique user ID of a third-party data source.
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases.
{% endhint %}

# Firing Manual ID Syncs

The SDK can synchronize IDs with other applications using a feature called ID syncs. This happens automatically and is configured from within Adobe's tools. Sometimes, it may be useful to fire your own customized ID sync URLs manually in addition to those that come from the server.

```js
alloy("syncIdsByUrl", {
  idSyncs: [
    {
      type: "url",
      id: 500,
      spec: {
        url: "https://idsync.rlcdn.com/365868.gif?partner_uid=79653899615727305204290942296930013270",
        hideReferrer: 0,
        ttlMinutes: 10080
      }
    }
  ]
}).then(function(result) {
  console.log("syncIdsByUrl result", result);
});
```

The `syncIdsByUrl` command supports the following option:

### `idSyncs`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Array | No | none |

An array of ID sync objects.

Each ID sync object is structured as follows:

### `type`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| String | Yes | none |

"url" must be specified to distinguish it from other types such as "cookie".

### `id`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Number | Yes | none |

Data source ID (integer >= 0). This should be obtained from your consultant in order to avoid collisions with other data source IDs.

### `spec`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Object | Yes | none |

A specifications object.

### `spec.url`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| String | Yes | none |

The custom ID sync URL. It usually contains the data source unique user ID.

### `spec.hideReferrer`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Boolean | Yes | none |

`true` to make the document.referrer of the network request blank. `false` to leave it populated.

### `spec.ttlMinutes`

| **Type** | **Required** | **Default Value** |
| -- | -- | -- |
| Number | No | 10080 |

Delay (integer), in minutes, before synchronizing IDs again. Minimum is 10080 (7 days).