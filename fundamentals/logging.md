---
description: >-
  Learn how to toggle logging.
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases. 
{% endhint %}

# Logging

When logging is enabled, the SDK will output messages to the browser console that can be helpful in debugging your implementation and understanding how the SDK is behaving. Logging is disabled by default, but can be toggled in three different ways: the `configure` command, the `log` command, or a query string parameter.

## Toggling Logging Through the Configure Command

When configuring the SDK using the `configure` command, you may enable logging by setting the `log` option to `true`.

```javascript
alloy("configure", {
  "configId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "imsOrgId":"ADB3LETTERSANDNUMBERS@AdobeOrg",
  "log": true
});
```

{% hint style="info" %}
This will enable logging for all users of the webpage rather than just your personal browser.
{% endhint %}

## Toggling Logging Through the Log Command

You may toggle logging through a separate `log` command as follows:

```javascript
alloy("log", {
  "enabled": true
});
```

If you prefer not to change code on your webpage or don't want logging messages to be produced for all users of your website, this is particularly useful because you can run the `log` command within your browser's JavaScript console at any time.


## Toggling Logging Through a Query String Parameter

You may toggle logging by setting an `alloy_log` query string parameter to `true` or `false` as follows:

```
http://example.com/?alloy_log=true
```

Similar to the `log` command, if you prefer not to change code on your webpage or don't want logging messages to be produced for all users of your website, this is particularly useful because you can set the query string parameter when loading the webpage within your browser. 

## Priority and Duration

When logging is set through the `log` command or query string parameter, it will override any `log` option set in the `configure` command. In these two cases, logging will also remain toggled for the duration of the session. In other words, if you were to enable logging using the log command or query string parameter, it would stay enabled until the end of your session or until you run the `log` command or set the query string parameter again.
