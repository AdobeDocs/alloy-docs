---
description: How to add data if you have products or a shopping cart
---

# Products

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use cases.
{% endhint %}

If you have products on your site, then this is a default set of things you may want to send to enable the most capabilities from Adobe. Though this is a suggestion, it will provide a very strong set of data right from the start.

We are going to use the [ExperienceEvent Commerce Details](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/experienceevent-commerce.schema.md) mixin. This mixin is broken into two parts. The `commerce` object and the `productListItems`. The `commerce` lets you indicate which actions are happening to the `productListItems`

{% hint style="info" %}
If you are familiar with Adobe Analytics the `commerce` is most closely related to the `events` variable. The `productListItems` is more closely related to the `products` variable
{% endhint %}

## Actions Related to Products

Below is a list of `Measures` available in the `commerce` object.

{% hint style="info" %}
A measure has two fields `id` and `value`. Most of the time you will use the `value` field only (e.g. `'value':1`). The `id` field allows you to set a unique identifier that you can use to keep track of when the measure was sent. See the XDM documentation for [Measure](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/measure.schema.md)
{% hint %}

* [productViews](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductviews) (_Highly Recommended_) -- a view of the product occured. Be sure to set the product viewed in the `productListItems`
* [cartAbandons](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmcartabandons) (_Optional_) -- A cart is no longer accessible or purchasable by the user.
* [checkouts](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmcheckouts) (_Highly Recommended_) -- Set when a user is no longer browsing for products but is in the process of purchasing a product
* [productListAdds](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistadds) (_Highly Recommended_) -- When a products is added to the list. Be sure to set the product in the `productListItems` at the same time.
* [productListOpens](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistopens) (_Optional_) -- When a new product list is created. E.g. a new shopping cart is created.
* [productListRemovals](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistremovals) (_Highly Recommended_)
* [productListReopens](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistreopens) (_Optional_)-- When a custoemr is re-activated by the user. This often happens via re-marketing campaigns
* [productListViews](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductlistviews) (_Highly Recommended_)-- When a list of products are viewed.
* [productViews](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmproductviews) (_Highly Recommended_)-- A single product is viewed
* [purchases](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmpurchases) (_Highly Recommended_)-- An order is accepted. Must have a product list
* [saveForLaters](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/context/commerce.schema.md#xdmsaveforlaters) (_Optional_) -- When a product is saved for future use.

Here is an example of how you would set these `Measures` in the SDK.

```javascript
alloy("event", {
  "xdm": {
    "commerce":{
      "poductViews":{
        "value":1
      }
    }
  }
});
```

The commerce object also has a special field for collecting order details. This is called `order` and it is used to collect information about an order.

* [currencyCode](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmcurrencycode) -- the [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217) currency for the order total
* [payments[paymentItems]](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmpayments) -- The list of payments on an order. A [paymentItem](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#payment-item-schema) includes the following.
  * [currencyCode](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmcurrencycode) (_optional_)-- the [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217) currency for this payment method.
  * [paymentAmount](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#xdmpaymentamount) (_Highly Recommended_) -- The value of the payment in the currency code specified.
  * [paymentType](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#xdmpaymenttype) (_Highly Recommended_) -- The type of payment (e.g. `credit_card`, `gift_card`, `paypal`). See the list of [known values](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#xdmpaymenttype-known-values) for details
  * [transactionID](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/paymentitem.schema.md#xdmtransactionid) (_optional_) -- A unique ID for this payment transaction.
* [priceTotal](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmpricetotal) (_Highly Recommended_) -- The total for this order after all discounts and taxes have been applied.
* [pruchaseID](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/data/order.schema.md#xdmpurchaseid) (_Highly Recomended_) -- The unique identifier assigned by the seller for this purchase.
* [purcahseOrderNumer](_optional_) -- A unique identifier assigned by the purchaser for this purchase.

Here is an example of a typical purchase in Alloy.

```javascript
alloy("event", {
  "xdm": {
    "commerce":{
      "order":{
        "purchaseID":"123456789",
        "currenceCode":"USD",
        "priceTotal":39.98,
        "payments":[
          {
            "transactionID":"amx12345",
            "paymentAmount":39.98,
            "paymentType":"credit_card",
            "currencyCode":"USD"
          }
        ]
      }
    },
    "productListItems":[
      {
        "SKU":"HT105",
        "name":"The Big Floppy Hat",
        "priceTotal":29.99,
        "quantity":1
      },
      {
        "SKU":"HT104",
        "name":"The Small Floppy Hat",
        "priceTotal":9.99,
        "qauntity":1
      }
    ]
  }
});
```

## Lists of Products

The product list intended to be sent whenever the above items are set to indicate what products the actions are happening to. It is a list of [productListItems](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md). Each product has a number of optional fields.

* [SKU](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md) (_Highly Recommended_) -- Store Keeping Unit. It is the unique identifier for the product.
* [currencyCode](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmcurrencycode) (_optional_) -- the [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217) currency for for the product. This is only useful when you can have products with different currency codes and when it applies like when there is a purcahse or add to cart.
* [productName](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmname) (_Highly Recommended_) -- This is set to the display name or human readable name of the product.
* [priceTotal](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmpricetotal) (_Highly Recommended_) -- Should only be set when applicable. For example, It may not be possible to set on `productView` since different variations of the product can have different prices but on an `productListAdds`
* [product](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmproduct) (_Highly Recommended_) -- The XDM id for the product.
* [productAddMethod](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmproductaddmethod) (_Highly Recommended_) -- The method that was used to add a product item to the list by the visitor. Set with product list add metrics. Should only be used when a product is added to the list.
* [quantity](https://github.com/adobe/xdm/blob/1c22180490558e3c13352fe3e0540cb7e93c69ca/docs/reference/content/productlistitem.schema.md#xdmquantity) (_Highly Recommended) -- The number of units the customer has indicated they require of the product. Should be set on `productListAdds`, `productListRemoves`, `purchases`, `saveForLaters`, etc.

## Examples
