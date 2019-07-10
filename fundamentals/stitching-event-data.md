---
description: >-
  Learn how to stitch event data.
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases. 
{% endhint %}

# Stitching Event Data

Sometimes, not all data is available when an event occurs. You may want to capture the data you _do_ have so it isn't lost if, for example, the user closes browser. On the other hand, you also want to include data that will be made available later.

In such cases, you may stitch data to prior events by passing `stitchId` as an option to `event` commands.

```javascript
alloy("event", {
  "data": {
    "key": "value"
  },
  "stitchId": "ABC123"
});

// Time passes and more data becomes available

alloy("event", {
  "data": {
    "key2": "value2"
  },
  "stitchId": "ABC123"
});
```

By passing the same stitch ID value to both event commands in this example, the data in the second event command will be augmented to data previously sent on the first event command. A record for each event command will be created in the Experience Data Platform, but during reporting the records will be joined together using the stitch ID and appear as a single event.

If you are sending data about a particular event to third-party providers, you may also wish to include the same stitch ID with that data as well. Later, if you choose to import the third-party data into the Adobe Experience Platform, the stitch ID will be used to stitch together all data that was collected as a result of the discrete event that occurred on your webpage.

## Generating a Stitch ID

The stitch ID value can be any string you choose, but remember that all events sent using the same ID will be reported as a single event, so be cognizant of enforcing uniqueness when events should not be stitched. If you would like the SDK to generate a unique stitch ID on your behalf (following the widely-adopted [UUID v4 specification](https://www.ietf.org/rfc/rfc4122.txt)), you may use the `getStitchId` command to do so.

As with all commands, a promise will be returned because you may be executing the command before the SDK has finished loading. The promise will be resolved with a unique stitch ID as soon as possible. For your convenience, you may pass the promise as the `stitchId` option when executing event commands and the SDK will appropriately wait for the promise to be resolved before sending data to the server. This is demonstrated as follows:

```javascript
var stitchIdPromise = alloy("getStitchId");

alloy("viewStart", {
  "data": {
    "key": "value"
  },
  "stitchId": stitchIdPromise
});

// Time passes and more data becomes available

alloy("event", {
  "data": {
    "key2": "value2"
  },
  "stitchId": stitchIdPromise
});
```

If you'd like to access the stitch ID (for example, to send to a third-party provider), you can explicitly wait for the promise to be resolved:

```javascript
var stitchIdPromise = alloy("createCorrelationID");

stitchIdPromise.then(function(stitchId) {
  console.log(stitchId);
});
```
