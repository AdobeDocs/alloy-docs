---
description: >-
  Description of each piece of information that Alloy collects automatically
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases.
{% endhint %}

Alloy collects a number of pieces of information automatically without any special configuration. Below is a list of those pieces of information. 

## Device 

Information about the device. Does not include data that can be looked up server side from the user agent string  

__Screen Height__ - The height in pixel of the screen  
_Path in Payload_: `events.device.screenHeight`  
_Example_: `900`

__Screen Width__ - The width in pixels of the screen  
_Path in Payload_: `events.device.screenHeight`  
_Example_: `1440`  

__Screen Orientation__ - The orientation of the user's screen  
_Path in Payload_: `events.device.screenOrientation`  
_Possible Values_: `landscape` or `portrait`

## Environment

__View Port Height__ - The hieght of the view port that the experience is being rendered in. Often it is browser height.  
_Path in Payload_: `events.environment.browserDetails.viewportHeight`
_Example_: `679`

__View Port Width__ - The width of the view port that the experience is being rendered in. Often it is browser width.  
_Path in Payload_: `events.environment.browserDetails.viewportWidth`  
_Example_: `642`

__Connection Type__- The type of connection that is servicing the web page.  
_Path in Payload_:`events.environment.connectionType`  
_Example_: `4g`  

__Environement Type__ - The type of evironment the experience was surface through. Alloy always set this to Browser  
_Path in Payload_:`events.environment.type`  
_Possible Values_: `browser`  

## Place Context

Information about the location of the end user.

__Local Time__ - Local timestamp  
_Path in Payload_: `events.placeContext.localTime`  
_Example_: `2019-08-07T22:47:17.129Z`  

__Local Timezone Offset__: Number of minutes the user is offset from GMT  
_Path in Payload_: `events.placeContext.localTimezoneOffset`  
_Example_: `360`

## Web Details

Details about the page the user is on.

__Current Page URL__: The URL of the current page  
_Path in Payload_: `events.web.webPageDetails.URL`  
_Example_: `https://somesite.com/somepage.html`  

__Referrer__ : the referrer for the page  
_Path in Payload_: `events.web.webReferrer.URL`  
_Example_: `http://somereferrer.com/linkedpage.html`  
