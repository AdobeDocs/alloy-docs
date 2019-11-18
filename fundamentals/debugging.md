---
description: >-
  Learn how to toggle debugging.
---

# Debugging

When debugging is enabled, the SDK will output messages to the browser console that can be helpful in debugging your implementation and understanding how the SDK is behaving. Debugging will also result in a server side synchronous validation of the data being collected against the schema you have configured.

Debugging is disabled by default, but can be toggled in three different ways: the `configure` command, the `debug` command, or a query string parameter.

## Toggling Debugging Through the Configure Command

When configuring the SDK using the `configure` command, you may enable debugging by setting the `debugEnabled` option to `true`.

```javascript
alloy("configure", {
  "configId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId":"ADB3LETTERSANDNUMBERS@AdobeOrg",
  "debugEnabled": true
});
```

{% hint style="info" %}
This will enable debugging for all users of the webpage rather than just your personal browser.
{% endhint %}

## Toggling Debugging Through the Debug Command

You may toggle debugging through a separate `debug` command as follows:

```javascript
alloy("debug", {
  "enabled": true
});
```

If you prefer not to change code on your webpage or don't want logging messages to be produced for all users of your website, this is particularly useful because you can run the `debug` command within your browser's JavaScript console at any time.

## Toggling Debugging Through a Query String Parameter

You may toggle debugging by setting an `alloy_debug` query string parameter to `true` or `false` as follows:

```HTTP
http://example.com/?alloy_debug=true
```

Similar to the `debug` command, if you prefer not to change code on your webpage or don't want logging messages to be produced for all users of your website, this is particularly useful because you can set the query string parameter when loading the webpage within your browser.

## Priority and Duration

When debugging is set through the `debug` command or query string parameter, it will override any `debug` option set in the `configure` command. In these two cases, debugging will also remain toggled for the duration of the session. In other words, if you were to enable debugging using the debug command or query string parameter, it would stay enabled until the end of your session or until you run the `debug` command or set the query string parameter again.
