---
description: How to add data if you have products or a shopping cart
---

# Products

{% hint style="warning" %}
This documentation is for a library and a service that is in Alpha and should not be used for production use-cases. 
{% endhint %}

If you have products on your site then this is a default set of things the you will want to send to enable the most capabilities from Adobe. Though this is a suggestion it will provide you a very strong set of data right from the start. 

## Actions Related to Products

There are certain actions that make sense to capture in relation to the purchase cycle. Here is the list of those events. 

* `Product View` -- When a customer views a product
* `Product Quick View` -- When a customer sees a preview of a product, that allows them to add the product to the cart or purchase the product. Not applicable to all companies. 
* `Add to Cart` -- When a product was added to cart regardless where it was added from. 
* `View Cart` -- When a customer views the contents of their cart. All products in the cart should be passed in. 
* `Remove from Cart` -- When a customer removes a product from the cart. 
* `Begin Checkout` - When a customer starts the checkout process
* `Purchase` - When a customer completes a purchase \(usually the Thank You or Confirmation View\). 
* `Return` - When a customer returns one or more products. 
* `Cancel Order` - When a customer cancels an order before it has been shipped

These will be all be represented in the `type` parameter of an event. 

```javascript
adbe('event', {
	"data": {
		"type": "Add to Cart",		
		"pageName": "PDP: Air Shaq",
		"pageType": "Product Detail Page",
	},
});
```

## Adobe Recommended Product Information

### General Product Attributes

You will want to include the following information in your each of the events above. 

* `productId` - The product ID, this is usually how you refer to the product internally. Customers will usually add product catalog data to this ID. 
* `productName` - The display name of the product. 
* `productImage` - A URL for the image. This is used when you want to display an image in the product. \(e.g. recommendations use cases\)
* `productPrice` - The full list price of the product. Should be sent when a product is added to cart as well as when a product is purchased. 
* `productCategory` - The category that was used to find the product \(if a product can be listed in multiple categories\). 
* `productSubCategory` - The subcategory \(if any\) that was used to find a product. \(additional keys may be added to indicate further levels of sub categorization if you have them, remember to add them to your XDM\). 
* `starRating` - The customer rating of the product on display
* `productSku` - The Sku of the product after it is available \(e.g. when someone adds it to the cart\)

Here is an example of how this data would be sent in. 

```javascript
adbe('event', {
  "isViewStart": true,
	"data": {
		"type": "Add to Cart",
		"products": [
			{
				"productId":12345,
				"productName": "Air Shaq",
				"productImage":"https://cdn.customer.com/pid/12345",
				"productPrice": 27.99,
				"productSalePrice": 25.99,
				"productCategory": "Mens",
				"productSubCategory":"Shoes",
				"productSku":"s12345",
				"starRating": 4.9

			}
		],
		"pageName": "PDP: Air Shaq",
		"pageType": "Product Detail Page"
	},
});
```

### Event Specific Attributes

#### Purchase

Purchase events add the following attributes

* `purchaseId` - This is the id for the order or the order number. 
* `discounts` - This is an array are order level discounts that are applied 
  * `discountName` - The friendly name of the discount
  * `discountType` - The type of discount \(E.g. % off, BOGO, Freeshipping, etc\)
  * `discountAmount` - The amount of this discount removes from the order. 

```javascript
adbe('event', {
  "isViewStart": true,
	"data": {
		"type": "Add to Cart",
		"products": [
			{
				"productId":12345,
				"productName": "Air Shaq",
				"productImage":"https://cdn.customer.com/pid/12345",
				"productPrice": 27.99,
				"productSalePrice": 25.99,
				"productCategory": "Mens",
				"productSubCategory":"Shoes",
				"productSku":"s12345",
			}
		],
		"discounts":[
			{
				"discountName":"Easter Blowout",
				"discountType":"Bogo",
				"disountAmount":3.75
			},
			{
				"discountName":"Free Shipping for Big Spenders",
				"discountType":"Free Shipping",
				"discountAmount":9.99
			}
				
		],
		"purchaseId": "98765453421",
		"pageName": "Thank You Page",
		"pageType": "Thank You Page"
	},
});
```

## Additional Use Case Specific Recommendations

### Subscriptions

If your products include a subscription or are a subscription you also want to add these events and fields. 

#### Events

* `subscribe` - This will be used instead of purchase
* `updateSubscription` - Used when a customer moves between service tiers
* `cancelSubscription` - When a customer cancels their subscription.

#### Attributes 

* `subscriptionInterval` - This is how often a customer is billed. This is usually values like yearly, monthly, weekly. 



