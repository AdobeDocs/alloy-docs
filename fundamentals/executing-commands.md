---
description: >-
  Learn how to execute commands.
---

# Executing Commands

Once the base code has been implemented on your webpage, you may begin executing commands with the SDK. You do not need to wait for the external file \(`alloy.js`\) to be loaded from the server before executing commands. If the SDK has not finished loading, commands will be queued and processed by the SDK as soon as possible.

Commands are executed using the following syntax.

```javascript
alloy("commandName", options);
```

The `commandName` tells the SDK what to do, while `options` are the parameters and data you would like to pass into a command. Because the available options depend on the command, please consult the documentation for each command for more details.

## A Note On Promises

[Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) are fundamental to how the SDK communicates with the code on your webpage. A promise is a common programming structure and is not specific to this SDK or even JavaScript. A promise acts as a proxy for a value that is not known when the promise is created. Once the value is known, the promise is "resolved" with the value. Handler functions can be associated with a promise, so that you can be notified when the promise has been resolved or when an error has occurred in the process of resolving the promise. To learn more about promises, please read [this tutorial](https://javascript.info/promise-basics) or any of the other great resources on the web.

## Handling Success or Failure

Each time a command is executed, a promise will be returned. The promise represents the eventual completion of the command. In the example below, we can use `then` and `catch` methods to determine when the command has succeeded or failed.

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

If knowing when the command succeeds is not important to you, you may remove the `then` call.

```javascript
alloy("commandName", options)
  .catch(function(error) {
    // The command failed.
    // "error" will be an error object with additional information
  })
```

Likewise, if knowing when the command fails is not important to you, you may remove the `catch` call.

```javascript
alloy("commandName", options)
  .then(function(value) {
    // The command succeeded.
    // "value" will be whatever the command returned
  })
```

## Suppressing Errors

If the promise is rejected and you have not added a `catch` call, the error will "bubble up" and be logged in your browser's developer console, regardless of whether logging in Alloy is enabled. If this is a concern for you, you may set the `suppressErrors` configuration option to `true` as outlined in [Configuring the SDK](configuring-the-sdk.md).
