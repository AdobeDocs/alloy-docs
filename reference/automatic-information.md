---
description: >-
  Description of each piece of information that The Adobe Experience Cloud SDK collects automatically
---
# Information Automatically Collected

The Adobe Experience Cloud SDK collects a number of pieces of information automatically without any special configuration, however, they can be disabled if needed using the `context` option in the `configure` command [See Configuring the SDK](../fundamentals/configuring-the-sdk.md). Below is a list of those pieces of information. The name in parentheses indicates the string to use when configuring the context.

## Device (`device`)

Information about the device. This does not include data that can be looked up server side from the user agent string.

### Screen Height

| **Path in Payload:**               | **Example:** |
| ---------------------------------- | ------------ |
| `events[].xdm.device.screenHeight` | `900`        |

The height in pixel of the screen  

### Screen Width

| **Path in Payload:**              | **Example:** |
| --------------------------------- | ------------ |
| `events[].xdm.device.screenWidth` | `1440`       |

The width of the screen (in pixels)  

### Screen Orientation

| **Path in Payload:**                    | **Possible Values:**      |
| --------------------------------------- | ------------------------- |
| `events[].xdm.device.screenOrientation` | `landscape` or `portrait` |

The orientation of the user's screen

## Environment (`environment`)

Details about the browser environment.

### Viewport Height

| **Path in Payload:**                                     | **Example:** |
| -------------------------------------------------------- | ------------ |
| `events[].xdm.environment.browserDetails.viewportHeight` | `679`        |

The height of the browser's content area (in pixels).

### Viewport Width

| **Path in Payload:**                                    | **Example:** |
| ------------------------------------------------------- | ------------ |
| `events[].xdm.environment.browserDetails.viewportWidth` | `642`        |

The width of the browser's content area (in pixels).

### Environement Type

Browser

| **Path in Payload:**            | **Example:** |
| ------------------------------- | ------------ |
| `events[].xdm.environment.type` | `browser`    |

The type of evironment the experience was surfaced through. The Adobe Experience Platform SDK for Javascript will always set Browser.

## Place Context (`placeContext`)

Information about the location of the end user.

### Local Time

| **Path in Payload:**                  | **Example:**                    |
| ------------------------------------- | ------------------------------- |
| `events[].xdm.placeContext.localTime` | `2019-08-07T15:47:17.129-07:00` |

Local timestamp for the end user in simplified extended ISO format [ISO 8601](https://tools.ietf.org/html/rfc3339#section-5.6).

### Local Timezone Offset

| **Path in Payload:**                            | **Example:** |
| ----------------------------------------------- | ------------ |
| `events[].xdm.placeContext.localTimezoneOffset` | `360`        |

Number of minutes the user is offset from GMT  

## Web Details (`web`)

Details about the page the user is on.

### Current Page URL

| **Path in Payload:**                  | **Example:**                         |
| ------------------------------------- | ------------------------------------ |
| `events[].xdm.web.webPageDetails.URL` | `https://somesite.com/somepage.html` |

The URL of the current page  

### Referrer URL

| **Path in Payload:**               | **Example:**                              |
| ---------------------------------- | ----------------------------------------- |
| `events[].xdm.web.webReferrer.URL` | `http://somereferrer.com/linkedpage.html` |

The URL of the previous page visited

## Timestamp

| **Path in Payload:**     | **Example:**               |
| ------------------------ | -------------------------- |
| `events[].xdm.timestamp` | `2019-08-07T22:47:17.129Z` |

The timestamp of the event.  This part of context cannot be removed.

UTC timestamp for the end user in simplified extended ISO format [ISO 8601](https://tools.ietf.org/html/rfc3339#section-5.6)

## Implementation Details

Information about the SDK used to collect the event

### Name

| **Path in Payload:**                      | **Example:**                            |
| ----------------------------------------- | --------------------------------------- |
| `events[].xdm.implementationDetails.name` | `https://ns.adobe.com/experience/alloy` |

The software development kit (SDK) identifier.  This field uses a URI to improve uniqueness among identifiers provided by different software libraries.

### Version

| **Path in Payload:**                         | **Example:** |
| -------------------------------------------- | ------------ |
| `events[].xdm.implementationDetails.version` | `0.11.0`     |
