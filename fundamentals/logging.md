---
description: >-
  Learn how to toggle logging.
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases. 
{% endhint %}

# Logging

The `configure` command allows you to enable logging. If you set the `log` option to `true`, the SDK will log messages to the console that are helpful in understanding exactly what the SDK is doing.

```javascript
alloy("configure", {
  "propertyID": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "log": true
});
```

For your convenience, you can also toggle logging through a separate `log` command as follows:

```javascript
alloy("log", {
  "enabled": true
});
```

{% hint style="info" %}
Note that you may execute this `log` command within your browser's JavaScript console to toggle logging at any time. This can be particularly useful when it's difficult to change code on your webpage or you don't want logging messages to be produced for all users of your website.
{% endhint %}

Additionally logging can be enabled by adding `alloy_log=true` as query parameter in the URL to enable logging to enabled it just on your browser.

```http
https://www.example.com?alloy_log=true
````

When logging is enabled, it will remain enabled until you disable it again. You can disable logging through the same mechanisms just outlined, but by using a value of `false` instead of `true`.
