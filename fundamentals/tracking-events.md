---
description: >-
  Learn how to track events. 
---

# Tracking Events

In order to send event data to the Adobe Experience Cloud, you will want to use the `event` command. The `event` command is the primary way of sending data to the Experience Cloud as well as retrieving personalized content, identities, and audience destinations.

Data being sent to Adobe Experience Cloud falls into two categories: (1) XDM data and (2) non-XDM data.

## Sending XDM Data

XDM data is an object whose content and structure matches a schema you have created within the Adobe Experience Platform. [Learn more about how to create a schema.](https://www.adobe.io/apis/experienceplatform/home/tutorials/alltutorials.html#!api-specification/markdown/narrative/tutorials/schema_editor_tutorial/schema_editor_tutorial.md)

Any XDM data you would like to be part of your analytics, personalization, audiences, or destinations should be sent using the `xdm` option.

```javascript
alloy("event", {
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

### Sending Non-XDM Data

Currently, sending data that does not match an XDM schema is unsupported. Support is planned for a future date.


### Setting `eventType`

In an XDM experience event, there is an `eventType` field. This holds the primary event type for the record. This can be passed in as part of the xdm option.

```javascript
alloy("event", {
  "xdm": {
    "eventType": "commerce.purchases",
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

Alternatively, the `eventType` can be passed into the event command using the `type` option. Behind the scenes, this will be added to the XDM data. Having the `type` as an option allows you to more easily set the `eventType` without having to modify the XDM payload.

```javascript
var myXDMData = { ... };

alloy("event", {
  "xdm": myXDMData,
  "type": "commerce.purchases"
});
```

### Starting a View

When a view has started, it is important to notify the SDK by setting `viewStart` to `true` within the `event` command. This will indicate, among other things, that the SDK should retrieve and render personalization content. Even if you are not using personalization currently, it will greatly simplify enabling personalization or other features later because you will not be required to modify on-page code. In addition, tracking views will be beneficial when viewing analytics reports after data has been collected.

The definition of a view can depend on the context.

* In a regular website, each webpage would typically be considered a unique view. In this case, an event with `viewStart` set to `true` should be executed as soon as possible at the top of the page.
* In a single page application \(SPA\), a view is less defined. It typically means that the user has navigated within the application and most of the content has changed. For those familiar with the technical foundations of single page applications, this is typically when the application loads a new route. Whenever a user moves to a new view, however you choose to define a "view", an event with `viewStart` set to `true` should be executed.

The event with `viewStart` set to `true` is the primary mechanism for sending data to the Adobe Experience Cloud and requesting content from the Adobe Experience Cloud. Here is how you start a view:

```javascript
alloy("event", {
  "viewStart": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

Once data is sent, the server will respond with personalized content, among other things. This personalized content will be automatically rendered into your view. Link handlers will also be automatically attached to the new view's content.

## Using the sendBeacon API

It can be tricky to send event data just before the web page user has navigated away. If the request takes too long, the browser may cancel the request. Some browsers have implemented a web standard API called `sendBeacon` to allow data to be more easily collected during this time. When using `sendBeacon`, the browser will make the web request in the global browsing context. This means the browser makes the beacon request in the background and does not hold up the page navigation. To tell Alloy to use `sendBeacon`, add the option `"documentUnloading": true` to the event command.  Here is an example:

```javascript
alloy("event", {
  "documentUnloading": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

Browsers have imposed limits to the amount of data that can be sent with `sendBeacon` at one time. In many browsers, the limit is 64K. If the browser rejects the event because the payload is too large, Alloy will fall back to using its normal transport method (e.g., fetch).

## Handling Responses from Events

If you want to handle a response from an event, you can be notified of a success or failure as follows:

```javascript
alloy("event", {
  "viewStart": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
}).then(function() {
    // Tracking the event succeeded.
  })
  .catch(function(error) {
    // Tracking the event failed.
  });
```

## Modifying Events Globally

If you want to add, remove, or modify fields from the event globally, you can configure an `onBeforeEventSend` callback.  This callback will be called everytime an event is sent.  This callback is passed an event object with an `xdm` field.  Modify `event.xdm` to change the data that is sent in the event.

```javascript
alloy("configure", {
  "configId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg",
  "onBeforeEventSend": function(event) {
    // Change existing values
    event.xdm.web.webPageDetails.URL = xdm.web.webPageDetails.URL.toLowerCase();
    // Remove existing values
    delete event.xdm.web.webReferrer.URL;
    // Or add new values
    event.xdm._adb3lettersandnumbers.mycustomkey = "value";
  }
});
```

`xdm` fields are set in this order:

1. Values passed in as options to the event command `alloy("event", { xdm: ... });`
2. Automatically collected values.  (see [Automatic Information](../reference/automatic-information.md))
3. The changes made in the `onBeforeEventSend` callback.

If the `onBeforeEventSend` callback throws an exception, the event will still send; however, none of the changes that were made inside the callback will be applied to the final event.

## Potential Actionable Errors

When sending an event, an error may be thrown if the data being sent is too large (over 32KB for the full request). In this case, you will need to reduce the amount of data being sent.

When debugging is enabled, the server will synchronously validate event data being sent against the configured XDM schema. If the data does not match the schema, details about the mismatch will be returned from the server and an error will be thrown. In this case, you will need to modify the data to match the schema. When debugging is not enabled, the server validates data asynchronously and therefore no corresponding error will be thrown. 
