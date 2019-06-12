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

## Starting a View

When a view has started, you will need to notify the SDK by setting `type` to `viewStart` within the `event` command. The definition of a view can depend on the context.

* In a regular website, each webpage would typically be considered a unique view. In this case, an event of type `viewStart` should be executed as soon as possible at the top of the page.
* In a single page application \(SPA\), a view is less defined. It typically means that the user has navigated within the application and most of the content has changed. For those familiar with the technical foundations of single page applications, this is typically when the application loads a new route. Whenever a user moves to a new view, however you choose to define a "view", the event of type `viewStart` should be executed.

The event with type `viewStart` is the primary mechanism for sending data to the Adobe Experience Cloud and requesting content from the Adobe Experience Cloud. Here is how you start a view:

```javascript
alloy("event", {
  "type": "viewStart",
  "data": {
    "key": "value"
  }
});
```

Once data is sent, the server will respond with personalized content, among other things. This personalized content will be automatically rendered into your view. Link handlers will also be automatically attached to the new view's content.

## Handling Responses from Events

If you want to handle a response from an event you can promise to the event like so. 

```javascript
alloy("event", {
  "type": "viewStart",
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
 * `responseBody` - This is the body that was sent on the response from the server. This property will only exist if a response was expected and processed by Alloy (for example, when `type` is `viewStart`).
 
