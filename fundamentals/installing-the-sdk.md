---
description: >-
  Learn how to install the SDK.
---

# Installing the SDK

The first step in implementing the Adobe Experience Platform SDK is to copy and paste the following "base code" as high as possible in the `<head>` tag of your HTML:

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

{% hint style="warning" %}
To avoid potential problems, please use a name containing at least one character that is not a digit and that doesn't conflict with the name of a property already found on `window`.
{% endhint %}

This base code, in addition to creating a global function, also loads additional code contained within an external file \(`alloy.js`\) hosted on a server. By default, this code is loaded asynchronously to allow your webpage to be as performant as possible. This is the recommended implementation.

## Supporting Internet Explorer

This SDK makes use of promises, which is a method of communicating the completion of asynchronous tasks. The [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) implementation used by the SDK is natively supported by all target browsers except Internet Explorer. As such, to use the SDK on Internet Explorer, you will need to have `window.Promise` [polyfilled](https://remysharp.com/2010/10/08/what-is-a-polyfill).

The easiest way to determine if you already have `window.Promise` polyfilled is to open your website in Internet Explorer, open the browser's debugging console, type `window.Promise` into the console, then hit enter. If something other than `undefined` appears, you likely have already polyfilled `window.Promise`. Another way to determine if `window.Promise` is polyfilled is by loading your website after having completed the above installation instructions. If the SDK throws an error mentioning something about a promise, you likely have not polyfilled `window.Promise`.

If you've determined you need to polyfill `window.Promise`, the easiest way to do so is by including the following script tag above the previously provided base code:

```html
<script src="https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js"></script>
```

This will load in a script ensuring `window.Promise` is a valid Promise implementation.

## Loading the JavaScript File Synchronously

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
