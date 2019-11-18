---
description: >-
  Learn how to interact with multiple properties.
---

# Interacting with Multiple Properties

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
  "configId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId": "ADB3LETTERSANDNUMBERS@AdobeOrg"
});

mycustomname1("event", {
  "data": {
    "key": "value"
  }
});

mycustomname2("configure", {
  "configId": "f46e981f-fd03-4bdd-a9d9-73ce4447f870",
  "orgId": "ADB3NUMBERSANDLETTERS2@AdobeOrg"
});

mycustomname2("event", {
  "data": {
    "key": "value"
  }
});
```

Be sure to execute the `configure` command for each instance before executing other commands on the same instance.

## Limitations

To avoid conflicts with cookies, only one instance of Alloy within a page can have a particular `configId`.  Similarly, only one instance of Alloy can have a particular `orgId`.  
