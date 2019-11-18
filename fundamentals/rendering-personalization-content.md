---
description: >-
  Learn how to render personalization content.
---

# Rendering Personalization Content

The SDK will automatically take care of rendering personalization content when you send an event to the server and set `viewStart` to `true` as an option on the event.

```javascript
alloy("event", {
  "viewStart": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

The rendering of personalization content is asynchronous, so there should not be any assumption around when a particular piece content will be part of the page.

## Managing flicker

When trying to render personalization content, the SDK has to ensure there is no flicker. Flicker, also called FOOC (Flash of Original Content), is when an original content is briefly displayed before the alternative appears during testing/personalization. The SDK will try to apply CSS styles to elements of the page to ensure those elements are hidden until the personalization content is rendered successfully.

The flicker management functionality has a few phases: prehiding, preprocessing, rendering.

### Prehiding

During the prehiding phase, the SDK will use the `prehidingStyle` configuration option to create an HTML style tag and append it to the DOM to make sure that big portions of the page are hidden. If you are unsure which portions of the page will be personalized, it is recommended to set `prehidingStyle` to `body { opacity: 0 !important }`. This will ensure that the whole page is hidden. This, however has the downside of leading to worse page rendering performance reported by tools like Lighthouse, Web Page Tests, etc. To have the best page rendering performance, it is recommended to set `prehidingStyle` to a list of container elements that would contain the portions of the page that will be personalized.

Assuming we have an HTML page like the one below and we know that only `bar` and `bazz` container elements will be ever personalized:

```html
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

### Preprocessing

The preprocessing phase kicks in once the SDK has received the personalized content from the server. During this phase, the response is preprocessed - making sure that elements that have to contain personalized content are hidden. Once these elements are hidden, the HTML style tag that has been created based on the `prehidingStyle` configuration option is removed and the HTML body or the hidden container elements are shown.

### Rendering

Once all the personalization content has been rendered successfully or if there was any error all previously hidden elements are shown to make sure that there are no hidden elements on the page that were hidden by the SDK.

## Managing flicker when SDK is loaded asynchronously

The recommendation is to always load the SDK asynchronously to get the best page rendering performance. However, this has some implications for the rendering of personalization content. When the SDK is loaded asynchronously, it is required to use the prehiding snippet. The prehiding snippet has to be added before the SDK in the HTML page. Here is an example snippet that hides the entire body:

```html
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

To make sure that we do not hide the HTML body or the container elements for an extended period of time, the prehiding snippet uses a timer that by default will remove the snippet after `3000` milliseconds. The `3000` milliseconds is the maximum wait time. If the response from the server has been received and processed sooner, then the prehiding HTML style tag will be removed as soon as possible.
