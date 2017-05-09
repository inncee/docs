# Installation and Setup
This guide will be able to help walk you through on how to get started with installing Inncee onto your custom website.

### 1.1 Create Account 
Create an account at https://beta.inncee.com. This allows our system to generate

-	a unique account ID
-	a tracking script code to install on your store
-	and a secret key to allow you to authenticate the JSON sata that you can send to our API endpoints

Once generated all this information will be sent to you via email

Note: Ignore the pop-up that asks you to connect your store to Shopify.

### 1.2 Install your tracking script
You will need to install our tracking script library after the opening of the <head> tag found in every
page of your website.


`<script type=”text/javascript”
src=”https://shopify.inncee.com/tracker/YOUR_ACCOUNT_ID?shop=YOUR_STORE_URL”></script>`


Once the script has been installed, you’ll be able to start recording events on your website.

### 1.3 Tracking Events
For our platform to be able to analyze your data and look for information, we need to be able to record
events that happened on your website. Some events are automatically tracked while others requires
you to install code snippets manually or calling our webhook end points in our app to send data to us.

#### Event Setup
Install these code snippets into your online store to begin tracking the following events.

| Event | Description | Code |
| --- | --- | ----|
| User logged in | Required. This code block helps us to identify visitors when they arelogged in. It should be included on every public page of your website when the user has logged in.  It can be omitted if the user is not logged in. innceeID * – Your unique account ID.| analytics.identify(‘USER_ID’, {‘innceeID’:‘YOUR_ACCOUNT_ID’}); 
| Add to cart` | Required. This code block records the event whenever a user clicks on the add to cart button. It should be included on every page that has an “Add To Cart” button. innceeID * – Your unique account ID.categoryID * - The unique ID of your category. productID * – The unique ID of theproduct being added to cart variantID  |  analytics.track(“addtocart”,{‘innceeID’ : ‘YOUR_ACCOUNT_ID’,‘categoryID’: ‘yyyyy’,‘productID’: ‘ppppp’, ‘variantID’: ‘’, ‘Price’: ’12.50’, ‘Quantity’: ‘1’, ‘Currency’: ‘$’}); 
| Checkout` | Checkout Required. This code block records the event when a user initiates a checkout. It should be included on every page that has a checkout button.   innceeID * – Your unique account ID   cartID * - A unique ID of the shopping cart being checked out  |  analytics.track(“Checkout”,{‘innceeID’ : ‘YOUR_ACCOUNT_ID’,‘cartID’ : ‘qqqqqq’});
| Order Completed` | Required. Records the event when an order has been completed innceeID * - Your unique account ID orderID * - The unique order ID  |  analytics.track(“OrderComplete”,{‘innceeID’ : ‘YOUR_ACCOUNT_ID’,‘orderID’ : ‘xxxxxx’});


#### Automatically Tracked Events
There are some events that are automatically tracked by us when you first install our script. No further actions will be required from you.
- Page views
- Add to cart clicks
- UTM Campaign visits

##### Backend Events
The code snippets above help us to track events that happen on your online store. We will also need to track certain events that happen in the backend of your store as well.

We’ve setup a series of API end points that ready to receive JSON data from your store when certain backend events happen.

For example, when a new order has been created, you can send a JSON response with information about the new order to our API end point and our platform can begin storing that information for analysis.

###### Prerequisites
When making API calls to our servers, you are required to first generate a HMAC and attach it to your headers along with the timestamp used to generate it.

The recommended method for generating the HMAC using PHP is as follows

```$timestamp = time(); 
$secret = YOUR_SHARED_SECRET;
$domain = "YOUR_DOMAIN.COM";
$message = "store=".$domain."&timestamp=".$timestamp;
$hmac = base64_encode(hash_hmac('sha256', $message, $secret, true));
```

When making calls to our servers, attach the timestamp as a URL parameter
```
$ch = curl_init();
$url = "https://api.inncee.com/hooks/order/
```

the timestamp should be the same one used to generate the HMAC. Now attach the HMAC as a header with the key HTTP_X_HMAC_SHA256

```
$hmac_header = "HTTP_X_HMAC_SHA256:".$hmac
curl_setopt($ch,CURLOPT_HTTPHEADER, array($hmac_header));
```

Once done, you are now good to attach your data and send to our servers.
See the next section for a list of all our API end points and when you can send data to it.

###### ORDERS API 

|Orders | POST | /hooks/order/create |
| --- | --- | ----|
|Called when a new order has been created.|||
Orders | POST | /hooks/order/create |
|Called when an order or many orders has been updated|||  
```
{
  "orders": [
    {
      "cancel_reason": null,
      "cancelled_at": null,
      "created_at": "2017-02-08T14:56:07-05:00",
      "currency": "SGD",
      "customer_id": 1244345,
      "discount_codes": [],
      "fulfilled_at": null,
      "id": 12345667,
      "line_items": [
        {
          "brand": "Apple",
          "fulfillable_quantity": 1,
          "line_item_id": 1071823175,
          "price": "199.00",
          "product_id": 447654529,
          "product_name": "IPod Touch 8GB",
          "quantity": 1,
          "sku": "IPOD2009BLACK",
          "total_discount": "0.00",
          "variant_id": 12133424,
          "variant_name": "Black"
        }
      ],
      "order_reference": "#1002",
      "order_source": "web",
      "order_status": "open",
      "refunds": [
        {
          "adjustment": "-3.50",
          "created_at": "",
          "quantity": "2",
          "tax_adjustment": "-1.00"
        }
      ],
      "subtotal_price": "199.00",
      "taxes_included": false,
      "total_discounts": "0.00",
      "total_line_items_price": "199.00",
      "total_price": "199.00",
      "total_tax": "0.00",
      "updated_at": "2017-02-08T14:56:07-05:00"
    }
  ]
}
```

|Orders | DELETE | /hooks/order/{#orderid}/delete
| --- | --- | ----|
Called when an order has been marked for deletion.|||
||||
|id|The unique numeric identifier for the order. Order id should be unique across the system. No 2 orders should have the same ids||
