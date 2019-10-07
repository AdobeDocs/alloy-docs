---
description: >-
  Learn how to fire manual ID syncs to synchronize the Adobe unique user ID with the unique user ID of a third party data source.
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases.
{% endhint %}

# Firing Manual ID Syncs

It may be useful to fire your own customized ID syncs in addition to those that come from the server.

```js
      alloy("syncIdsByUrl", {
        idSyncs: [
          {
            type: "url",
            id: 500,
            spec: {
              url:
                "https://idsync.rlcdn.com/365868.gif?partner_uid=79653899615727305204290942296930013270",
              hideReferrer: 0,
              ttlMinutes: 10080
            }
          }
        ]
      }).then(function(result) {
        console.log("syncIdsByUrl result", result);
      });
```

The `syncIdsByUrl` command contains the following properties:

* `idSyncs` An array of ID sync objects.
* `type` "url" must be specified to distinguish it from other types such as "cookie".
* `id` Data source ID. This should be obtained from your consultant in order to avoid collisioms with other data source IDs.
* `url` The custom ID sync URL. It usually contains the data source unique user ID.
* `hideReferrer` 1 to make the document.referrer of the network request blank. 0 to leave it populated.
* `ttlMinutes` Delay in minutes before firing ID sync again. Minimum is 10080 (7 days).