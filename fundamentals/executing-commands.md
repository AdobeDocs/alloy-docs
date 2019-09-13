---
description: >-
  Learn how to execute commands.
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases. 
{% endhint %}

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

## Providing Promises as Options

{% hint style="warning" %}
Using promises as options will delay command execution until the promises are resolved. If the user navigates away from the page or closes the browser during this time, the command will not have been executed and may result in corresponding data being lost. Also, if a command's execution would result in personalized content being retrieved from the server, the content retrieval and rendering would likewise be delayed. 
{% endhint %}

If you are waiting on a piece of data to become available before executing a command, you may instead provide a promise within the `options` object you pass to the SDK. The promise will represent the eventually-resolved value. When a promise is provided, the SDK will internally wait for your promise to be resolved and the promise's resolved value will be used.

For example, if you would like to record an ecommerce transaction but are still waiting for payment details, you could execute the `event` command (covered in more detail in [Tracking Events](tracking-events.md)) as follows:

```javascript
var paymentsPromise = new Promise(function(resolve) {
  // Resolve the promise some time later when payment 
  // details become available.
  resolve([
    {
      "transactionID": "TR426941",
      "paymentAmount": 999.98,
      "paymentType": "credit_card",
      "currencyCode": "USD"
    }
  ]);
});

alloy("event", {
  "viewStart": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98,
        "payments": paymentsPromise
      }
    }
  }
});
```

In this case, the SDK will internally wait for `paymentsPromise` to be resolved. Once the promise is resolved, the finalized options that will be used to execute the command will be as follows:

```json
{
  "viewStart": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98,
        "payments": [
          {
            "transactionID": "TR426941",
            "paymentAmount": 999.98,
            "paymentType": "credit_card",
            "currencyCode": "USD"
          }
        ]
      }
    }
  }
}
```

Note that this approach will not send any of the commerce data until the payment details are available.

Another approach for solving this problem is to send the data you have immediately available (`purchaseId`, `purchaseOrderNumber`, `currencyCode`, `priceTotal`) right away so it's not lost if the user closes the browser. Then, once payment details become available, send them separately. You can then merge the collected data during reporting. See [Merging Event Data](merging-event-data.md) for more information on this approach. 

Remember, promises can be used anywhere inside the options object; this feature is not specific to the `event` command.
