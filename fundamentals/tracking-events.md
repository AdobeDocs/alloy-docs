---
description: >-
  Learn how to track events. 
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases. 
{% endhint %}

# Tracking Events

In order to send event data to the Adobe Experience Cloud, you will want to use the `event` command.

Any data you would like to be part of your analytics, personalization, audiences, or destinations should be sent using the `data` key.

The `data` key will accept any XDM keys and any arbitrary key value pairs you would like to send and can be used in any of the use cases \(analytics personalization, audiences, destinations, etc\).

```javascript
alloy("event", {
  "data": {
    "key":"value"
  },
});
```

{% hint style="warning" %}
Be sure to understand how to track different types of events as outlined below. Failing to do so may result in a loss of functionality.
{% endhint %}

## Event Types

For each event, you may pass in a `type` property indicating the type of event that occurred as follows:

```javascript
alloy("event", {
  "data": {
    "type": "somethingOccurred",
    "key":"value"
  },
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
  "data": {
    "key": "value"
  }
});
```

Once data is sent, the server will respond with personalized content, among other things. This personalized content will be automatically rendered into your view. Link handlers will also be automatically attached to the new view's content.

## Using the sendBeacon API

It can be tricky to send event data just before the web page user has navigated away. If the request takes too long, the browser may cancel the request. Some browsers have implemented a web standard API called `sendBeacon` to allow data to be more easily collected during this time. When using `sendBeacon`, the browser will make the web request in the global browsing context. This means the browser makes the beacon request in the background and does not hold up the page navigation. To tell Alloy to use `sendBeacon`, add the option `"documentUnloading": true` to the event command.  Here is an example:

```javascript
alloy("event", {
  "documentUnloading": true,
  "data": {
    "key": "value"
  }
});
```

Browsers have imposed limits to the amount of data that can be sent with `sendBeacon` at one time. In many browsers the limit is 64K. If the browser rejects the event because the payload is too large, Alloy will fall back to using its normal transport method (e.g. fetch).

## Handling Responses from Events

If you want to handle a response from an event you can promise to the event like so. 

```javascript
alloy("event", {
  "viewStart": true,
  "data": {
    "key": "value"
  }
}).then(function(result) {
    // Tracking the event succeeded.
  })
  .catch(function(error) {
    // Tracking the event failed.
  })
;
```

When tracking an event succeeds, a `result` object is provided. This object has the following properties:

 * `requestBody` - This the body that was sent on the request to the server.
 * `responseBody` - This is the body that was sent on the response from the server. This property will only exist if a response was expected and processed by the SDK (for example, when `viewStart` is set to `true`).
 
