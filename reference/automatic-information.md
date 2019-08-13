---
description: >-
  Description of each piece of information that The Adobe Experience Cloud SDK collects automatically
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases.
{% endhint %}

The Adobe Experience Cloud SDK collects a number of pieces of information automatically without any special configuration, however, they can be disabled if needed using the `configure` command [See Configuring the SDK](fundamentals/configuring-the-sdk.md). Below is a list of those pieces of information.

## Device

Information about the device. This does not include data that can be looked up server side from the user agent string.

### Screen Height

**Path in Payload:** `events[].device.screenHeight`  
**Example:** `900`

The height in pixel of the screen  

### Screen Width

**Path in Payload:** `events[].device.screenHeight`  
**Example:** `1440`  

The width of the screen (in pixels)  

### Screen Orientation

**Path in Payload:** `events[].device.screenOrientation`
**Possible Values:** `landscape` or `portrait`

The orientation of the user's screen

## Environment

Details about the browser environment.

### Viewport Height

**Path in Payload:** `events[].environment.browserDetails.viewportHeight`  
**Example:** `679`  

The height of the browser's content area (in pixels).

### Viewport Width

**Path in Payload:** `events[].environment.browserDetails.viewportWidth`  
**Example:** `642`

The width of the browser's content area (in pixels). 

### Connection Type

**Path in Payload**:`events[].environment.connectionType`  
**Example**: `4g`  

The type of connection used to access the web page.  

### Environement Typethis to Browser  

**Path in Payload:**`events[].environment.type`  
**Possible Values:** `browser`  

The type of evironment the experience was surface through. The Adobe Expereience Platform SDK for Javascript will always set Browser.

## Place Context

Information about the location of the end user.

### Local Time

**Path in Payload:** `events[].placeContext.localTime`  
**Example:** `2019-08-07T22:47:17.129Z`  

Local timestamp for the end user in simplified extended ISO format [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601). 

### Local Timezone Offset

**Path in Payload**: `events[].placeContext.localTimezoneOffset`  
**Example**: `360`

Number of minutes the user is offset from GMT  

## Web Details

Details about the page the user is on.

### Current Page URL

**Path in Payload:** `events[].web.webPageDetails.URL`  
**Example:** `https://somesite.com/somepage.html`  

The URL of the current page  

### Referrer URL

**Path in Payload:** `events[].web.webReferrer.URL`  
**Example:** `http://somereferrer.com/linkedpage.html`  

The URL of the previous page visited
