---
description: >-
  Learn how to support opt-out.
---

# Supporting Opt-Out

To respect your user's privacy, you may wish to allow the user to opt out of all Adobe Experience Cloud functionality. Opting out of purposes through Adobe Experience Platform Web SDK not only affects behavior of the Adobe Experience Platform Web SDK, but also the entire Adobe Experience Platform. Currently, the SDK only allows users to opt out of all purposes or no purposes, but in the future we hope to provide more granular control over specific purposes. Opting out of all purposes always takes precendence over any configured opt-in preferences.

If the user opts out of all purposes, the SDK will not be allowed to perform the following tasks:

* Send data to and from Adobe's servers.
* Write privacy-sensitive cookies or web storage items.

By default, the user is opted out of no purposes.

## Communicating User Preferences

If the user opts out of all purposes, execute the `optOut` command with the `purposes` option set to `all` as follows:

```javascript
alloy("optOut", {
  "purposes": "all"
});
```

Since the user has now opted out of all purposes, future privacy-sensitive commands will be rejected. Once a user has opted out of all purposes, there is no way to reverse this such that the user has opted out of no purposes. For more information on handling or suppressing errors, please refer to [Executing Commands](executing-commands.md).

## Persistence of User Preferences

Once you have communicated user preferences to the SDK using the `optOut` command, the user's preferences will be saved to a cookie. The next time the user loads your website in the browser, the SDK will retrieve and use these persisted preferences. There is no need to execute the `optOut` command again.
