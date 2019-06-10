---
description: >-
  Learn how to implement Adobe Products with the Adobe Experience Platform SDK
  for Web.
---

# Getting Started \(without Launch\)

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases.
{% endhint %}

## Basic Concepts

### Installation

The first step in implemented the Adobe Experience Platform SDK is to copy and paste the following "base code" as high as possible in the `<head>` tag of your HTML:

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="alloy.js" async></script>
```

The base code, by default, creates a global function named `alloy`. You will use this function to interact with the SDK. If you would like to name the global function something else, you may change the `alloy` name as follows:

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["mycustomname"]);
</script>
<script src="alloy.js" async></script>
```

With this change made, the global function would be named `mycustomname` instead of `alloy`.

This base code, in addition to creating a global function, also loads additional code contained within an external file \(`alloy.js`\) hosted on a server. By default, this code is loaded asynchronously to allow your webpage to be as performant as possible. This is the recommended implementation.

#### Supporting Internet Explorer

This SDK makes use of promises, which is a method of communicating the completion of asynchronous tasks. The [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) implementation used by Alloy is natively supported by all target browsers except Internet Explorer. As such, to use the SDK on Internet Explorer, you will need to have `window.Promise` [polyfilled](https://remysharp.com/2010/10/08/what-is-a-polyfill).

The easiest way to determine if you already have `window.Promise` polyfilled is to open your website in Internet Explorer, open the browser's debugging console, type `window.Promise` into the console, then hit enter. If something other than `undefined` appears, you likely have already polyfilled `window.Promise`. Another way to determine if `window.Promise` is polyfilled is by loading your website after having completed the above installation instructions. If the SDK throws an error mentioning something about a promise, you likely have not polyfilled `window.Promise`.

If you've determined you need to polyfill `window.Promise`, the easiest way to do so is by including the following script tag above the previously provided base code:

```markup
<script src="https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js"></script>
```

This will load in a script ensuring `window.Promise` is a valid Promise implementation.

### Configuration

Configuration for the SDK is done with the `configure` command. This should _always_ be the first command called.

```javascript
alloy("configure", {
  "propertyID": "ebebf826-a01f-4458-8cec-ef61de241c93"
});
```

The options are as follows.

* `propertyID` - \(required\) The property ID links the SDK to the appropriate accounts and configuration.
* `edgeDomain` - \(optional\) The domain that will be used to interact with Adobe Services. This is only used if you have a CNAME that proxies requests to Adobe's edge infrastructure.
* `log` - \(optional\) A boolean indicating whether debugging messages will be displayed in the browser's JavaScript console.

### Executing Commands

Once the base code has been implemented on your webpage, you may begin executing commands with the SDK. You do not need to wait for the external file \(`alloy.js`\) to be loaded from the server before executing commands. If the SDK has not finished loading, commands will be queued and processed by the SDK as soon as possible.

Commands are executed using the following syntax.

```javascript
alloy("commandName", options);
```

The `commandName` tells the SDK what to do, while `options` are the parameters and data you would like to pass into a command. Because the available options depend on the command, please consult the documentation for each command for more details.

Each time a command is executed, a [promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) will be returned. The promise represents the eventual completion of the command. In the example below, we can use `then` and `catch` methods to determine when the command has succeeded or failed.

```javascript
alloy("commandName", options)
  .then(function(value) {
    // The command succeeded.
    // "value" will be whatever the command returned
  })
  .catch(function(error) {
    // The command failed.
    // "error" will be an error object with additional information
  })
```

### The `event` command

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

### Starting a View

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

### Logging

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
})
```

Note that you may execute this `log` command within your browser's JavaScript console to toggle logging at any time. This can be particularly useful when it's difficult to change code on your webpage or you don't want logging messages to be produced for all users of your website.

When logging is enabled, it will remain enabled until you disable it again. You can disable logging through the same mechanisms just outlined, but by using a value of `false` instead of `true`.

## Customization for Specific Use Cases

### In-App Browsers

Within browsers embedded inside mobile applications, the SDK will behave exactly as it would within a regular browser; however, you should make sure the Visitor ID gets passed from the mobile application into the website through the URL. This process is outlined in the documentation for the [Experience SDK for Mobile](https://marketing.adobe.com/resources/help/en_US/mobile/ios/hybrid_app.html).

### Retrieving Personalization Details for Custom Rendering

If you would like to handle rendering of personalization content yourself, you can wait for the promise to be resolved after executing an `event` command with a `type` value of `viewStart` as follows:

```javascript
alloy("event", {
  type: "viewStart",
  data: {
    "key": "value"
  }
}).then(function(value) {
  // More details to come about what value holds and how to use
  // it to render personalization content.
});
```

### Interacting with Multiple Properties on the Same Page

There are certain cases where you might want to interact with two different properties on the same page. These include:

* Companies that have been acquired and are working on integrating their websites together 
* Data-sharing relationships between multiple companies
* Customers who are testing new Adobe Solutions and don't want to disrupt their existing implementation

To do this, the SDK allows you to create a separate instance for each property by adding another name to the array in the base code. In the following example, we've provided two names, `mycustomname1` and `mycustomname2`.

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["mycustomname1", "mycustomname2"]);
</script>
<script src="alloy.js" async></script>
```

As a result, the script will create two instances of the SDK. The global function for interacting with the first instance will be named `mycustomname1` and the global function for interacting with the second instance will be named `mycustomname2`.

By creating two separate instances, each can be configured for a different property. Any communication or data persistence that occurs due to interacting with `mycustomname1` will be kept isolated from `mycustomname2` and vice-versa.

Following the above example, you can now execute commands using each of the instances as follows:

```javascript
mycustomname1("configure", {
  "propertyID": "ebebf826-a01f-4458-8cec-ef61de241c93"
});

mycustomname1("event", {
  "data": {
    "key": "value"
  }
});

mycustomname2("configure", {
  "propertyID": "f46e981f-fd03-4bdd-a9d9-73ce4447f870"
});

mycustomname2("event", {
  "data": {
    "key": "value"
  }
});
```

Be sure to execute the `configure` command for each instance before executing other commands on the same instance.

### Loading the JavaScript File Synchronously

As explained previously, the base code you have copied and pasted into your website's HTML will load an external file with additional code. This additional code contains the core functionality of the SDK. Any command you attempt to execute while this file is loading will be queued and then processed once the file is loaded. This is the most performant method of installation.

Under certain circumstances, however, you may wish to load the file synchronously \(more details about these circumstances will be documented later\). Doing so will block the rest of the HTML document from being parsed and rendered by the browser until the external file has been loaded and executed. This additional delay before displaying primary content to users is typically frowned upon, but may make sense depending on the circumstances.

To load the file synchronously instead of asynchronously, simply remove the `async` attribute from the second `script` tag as shown below:

```markup
<script>
  !function(n,o){o.forEach(function(o){n[o]||((n.__alloyNS=n.__alloyNS||
  []).push(o),n[o]=function(){var u=arguments;return new Promise(
  function(i,l){n[o].q.push([i,l,u])})},n[o].q=[])})}
  (window,["alloy"]);
</script>
<script src="alloy.js"></script>
```

