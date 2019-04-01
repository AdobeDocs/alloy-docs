---
description: Learn how to implement Adobe Products with the Experience SDK for Web.
---

# Getting Started \(without Launch\)

## Basic Concepts

### Installing the Experience SDK for Web

The Experience SDK for Web has two parts to make it work. First is the on page code which is what allows the library to be full asynchronous \(to increase render performance\). 

```markup
<script>
      (function(a,b){if(!a[b]){a.__adobeNS=a.__adobeNS||[];
      a.__adobeNS.push(b);a[b]=function(){a[b].q.push(arguments)};a[b].q=[]}})
      (window,'adbe');
</script>
```

The second is the include of the `atag.js` on the page

```markup
<script src="atag.js" async></script>
```

Both of these should be included in the head of your HTML. 

### Making Calls to the Experience SDK for Web

Calls are then made to the SDK using the following syntax. 

```javascript
<script>
    adbe('command',parameters_obj)
</script>
```

The `command` tells the SDK what to do. Here is a list of the possible commands. 

* `configure` - Sets configuration parameters for the SDK
* `viewStart` - Indicates that a new view has started and the SDK can reder content and attach link handlers
* `collect` - There is additional data that you would like to send. This also will fetch personalization content for caching

### Configuring the Experience SDK for Web

Configuration for the SDK is done with the `configure` command. This ALWAYS be the first command called. 

```javascript
adbe("configure", {
    propertyId: "ebebf826-a01f-4458-8cec-ef61de241c93"
})
```

The options are as follows. 

* `propertyId` - The property id links the SDK to the appropriate accounts and configuration. 

### Starting a View

A new view can be a few different things in different contexts. 

* In a regular web page it means the webpage has started loading and should be called at the top of the page
* In SPA it means that the user has moved into a new view where most of the page has changed. This is typically when you load a new route. It should be called when you render new content. 

A new view is the primary mechanism for sending data to the Adobe Experience Cloud and requesting content from the Adobe Experience Cloud. Here is how you start a view. 

```javascript
adbe("viewStart",{
    "data":{
        "event":"productPageView"
        "key":"value"
    }
})
```

Any data that you would like to be part of your analytics, personalization, audiences or destinations should be sent using the data key. 

The only key that is required is the `event` key, it will accept any string as a value

{% hint style="info" %}
event is more like an event type and is used to group similar events together so you can apply processing to different events
{% endhint %}

The `data` key will accept any XDM keys and any arbitrary key value pairs that you would like to send and can be used in any of the use cases \(analytics personalization, audiences, destinations, etc\). 

### Non View Events

Many times your events don't correspond to a view change. In these cases you will want to use the `collect` call. The collect call has the same signature as the `viewStart` call but won't attach link handlers or allow personalization experiences to be updated automatically. 

```javascript
adobe('collect',{
    "data":{
        "event":"buttonClick"
        "key":"value"
    },
})
```

### Adding Additional Data

Sometimes not all data is available at the top of the page. The Experience SDK for Web and the Experience Edge can handle this by calling `collect` and using `correlationId` 



```javascript
adobe('viewStart',{
    "data":{
        "event":"loadSite"
        "key":"value"
    },
    "correlationId":123456
})

//Page loads or time goes by

adobe('collect',{
    "data":{
        "key2":"value2"
    },
    "correlationId":123456
})
```

This append the data together even if the data has already been sent to the server. 

#### Data collisions

If `event` is sent in both calls the first one will be used

---- Need Details on Merging ----

## Customization for Specific Use-Cases

### In-App Browsers

In app browsers will be have exactly the same way except you will want to make sure the Visitor ID gets passed across in the URL. This is outlined in the documents for the [Experience SDK for Mobile](https://marketing.adobe.com/resources/help/en_US/mobile/ios/hybrid_app.html). 

### Retrieving Personalization Details for Custom Render

If you would like to handle rendering of personalization content your self you can register a callback to the `viewState` or `collect` calls. 

------ What is the parameter for the callback? ------

### Interacting with Multiple Properties on the Same Page

There are certain cases where you might want to interact with two different properties on the same page these include. 

* Companies that have been acquired and are working on integrating their websites together 
* Data sharing relationships between two companies
* Customers who are testing new Adobe Solutions and don't want to disrupt their existing implementation

To do this you will include two versions of the initialization code

```javascript
<script>
      (function(a,b){if(!a[b]){a.__adobeNS=a.__adobeNS||[];
      a.__adobeNS.push(b);a[b]=function(){a[b].q.push(arguments)};a[b].q=[]}})
      (window,'adbe');
      (function(a,b){if(!a[b]){a.__adobeNS=a.__adobeNS||[];
      a.__adobeNS.push(b);a[b]=function(){a[b].q.push(arguments)};a[b].q=[]}})
      (window,'example');
</script>
```

Make sure you change the name in the `(window, 'adobe')` to be what you want the new object name to be. The above script will let you make calls to 

```javascript
adbe("viewStart",{
    "data":{
        "event":"productPageView"
        "key":"value"
    }
})
```

as well as 

```javascript
example("viewStart",{
    "data":{
        "event":"productPageView"
        "key":"value"
    }
})
```

This lets you have two different configurations on the same page which allows you to access different properties. 

When doing this you will need to ensure that `configure` gets called for both objects before other calls are made. 

