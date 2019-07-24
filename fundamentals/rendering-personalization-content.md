---
description: >-
  Learn how to render personalization content. 
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases. 
{% endhint %}

### Rendering Personalization Content

By default, the SDK will automatically take care of rendering personalization content. If you would like to handle rendering of personalization content yourself, you can wait for the promise to be resolved after executing an `event` command with a `type` value of `viewStart` as follows:

```javascript
alloy("event", {
  type: "viewStart",
  data: {
    "key": "value"
  }
}).then(function(result) {
  // More details to come about what value holds and how to use
  // it to render personalization content.
});
```
