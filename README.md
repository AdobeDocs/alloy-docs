---
description: >-
  Learn what Alloy is and how it can be used.
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases.
{% endhint %}

# What is Alloy
Alloy is a client-side JavaScript library that allows customers of the Adobe Experience Cloud to interact with the various services in the Experience Cloud. It is meant to replace the following SDKs.
- Visitor.js
- AppMeasurement.js
- AT.js
- DIL.js

This is not just a wrapper around existing libraries; it is a complete rewrite. Its purpose is to end challenges with tags firing in the right order, inconsistency with library versioning challenges and better dependency management. It is a new way to implement the Experience Cloud.

In addition to a new library, there will be a new endpoint that streamlines the HTTP requests to Adobe solutions. Before, Visitor.js sent a blocking call to the visitor ID service, then AT.js sent a call to Adobe Target, Then DIL.js sent a call to Adobe Audience Manager and finally AppMeasurement.js sent a call to Adobe Analytics. This new library and endpoint can retrieve an ID, fetch a Target experience, send data to Audience Manager and pass the data into the Adobe Experience Platform in a single call.

## Requesting Write Access (Adobe Employees)

For Adobe Employees that would like write access to this repository, please follow the instructions found in the [GitHub Adobe Org Management document](https://git.corp.adobe.com/OpenSourceAdvisoryBoard/handbook/blob/master/GitHub-Adobe-Org-Management.md#request-access-to-our-adobe-github-org). When you must indicate a team to which your user should be added, please enter `AdobeDocs|UnifiedJS`.
