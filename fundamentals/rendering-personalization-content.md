---
description: >-
  Learn how to render personalization content. 
---

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases. 
{% endhint %}

# Rendering Personalization Content

By default the SDK will automatically take care of rendering personalization content. To enable this capability you have to ensure that an `event` command has a special option named `viewStart` that is set to `true` as follows:

```javascript
alloy("event", {
  viewStart: true,
  data: {
    "type": "page-view",
    "key": "value"
  }
});
```

The rendering of personalization content is asynchronous, so there should not be any assumption around when a particular piece content will be part of the page.

# Managing flicker

When trying to render personalization content the SDK has to ensure that there is no flicker. The SDK will try to apply CSS styles to elements of the page to ensure that those elements are hidden until the personalization content is rendered successfully.

The flicker management functionality has a few phases.

## First phase

During first phase the SDK will use `prehidingStyle` configuration option to create an HTML STYLE tag and append it to the DOM, to make sure that big portions of the page are hidden. If you are unsure which portions of the page will be personalized it is recommended to set `prehidingStyle` to `body { opacity: 0 !important }`, this will ensure that the whole page is hidden. This however has the downside of leading to worse page rendering performance reported by tools like Lighthouse, Web Page Tests, etc. To have the best page rendering performance it is recommended to set `prehidingStyle` to a list of container elements that would contain the portions of the page that will be personalized.

Assuming we have an HTML page like the one below and we know that only `bar` and `bazz` container elements will be ever personalized:
```markup
<html>
  <head>
  </head>
  <body>
    <div id="foo">
      Foo foo foo
    </div>

    <div id="bar">
      Bar bar bar
    </div>

    <div id="bazz">
      Bazz bazz bazz
    </div>
  </body>
</html>
```
Then the `prehidingStyle` would have to be set to something like `#bar, #bazz { opacity: 0 !important }`.

## Second phase

Second phase kicks in once the SDK has received the personalized content from the server. During this phase the response is preprocessed making sure that elements that have to contain personalized content are hidden. Once these elements are hidden, the HTML STYLE tag that has been created based on `prehidingStyle` configuration option is removed and the HTML BODY or the hidden container elements are shown.

## Third phase

Once all the personalization content has been rendered successfully or if there was any error all previously hidden elements are shown to make sure that there are no hidden elements on the page that were hiddedn by the SDK. 

# Managing flicker when SDK is loaded asynchronously

The recommendation is to always load the SDK asynchronously to get the best page rendering performance. However this has some implications for the rendering of personalization content. When SDK is loaded asynchronously, it is required to use the prehiding snippet. The prehiding snippet has to be added before the SDK in the HTML page. The snippet itself looks soemthing like this:
```markup
<script>
      !function(e,a,n,t){
        if (a) return;
        var i=e.head;if(i){
        var o=e.createElement("style");
        o.id="alloy-prehiding",o.innerText=n,i.appendChild(o),
        setTimeout(function(){o.parentNode&&o.parentNode.removeChild(o)},t)}}
        (document, document.location.href.indexOf("mboxEdit") !== -1, "body { opacity: 0 !important }", 3000);
    </script>
```
To make sure that we do not hide HTML body or the container elements for an extended period of time the prehiding snippet uses a timer that by default will remove the snippet after `3000` milliseconds. The `3000` milliseconds is the maximum wait time, if the response from Adobe edge has been received and preprocessed sooner, then the prehiding HTML STYLE tag will be removed as soon as possible.
