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

<html>
<body>

<table>
  <tr>
    <th>Event</th>
    <th>Description</th> 
    <th>Code</th>
  </tr>
  <tr>
    <td>Required.</td>
    <td>This code block helps us to identify visitors when they are logged in. It should be included on every public page of your website when the user has logged in.
    <br /><br /> It can be omitted if the user is not logged in. 
    <br /><br /> innceeID * – Your unique account ID. 
    </td>
    <td>analytics.identify(‘USER_ID’,{ 
    <br />‘innceeID’:‘YOUR_ACCOUNT_ID’
    <br />});
    </td>
  </tr>
  <tr>
    <td>Add to cart</td>
    <td>Required. This code block records the event whenever a user clicks on the add to cart button. It should be included on every page that has an “Add To Cart” button. 
    <br /><br />innceeID * – Your unique account ID.
    <br /><br />categoryID * - The unique ID of your category.
    <br /><br />productID * – The unique ID of the product being added to cart variantID
    </td>
    <td>analytics.track(“addtocart”,{<br />‘innceeID’ : ‘YOUR_ACCOUNT_ID’,
    <br />‘categoryID’: ‘yyyyy’,
    <br />‘productID’:‘ppppp’,
    <br />‘variantID’: ‘’,
    <br />‘Price’: ’12.50’,
    <br />‘Quantity’: ‘1’,
    <br />‘Currency’: ‘$’
    <br />});
    </td>
  </tr>
  <tr>
    <td>Checkout</td>
    <td>Required. This code block records the event when a user initiates a checkout. It should be included on every page that has a checkout button.
    <br /><br />innceeID * – Your unique account ID
    <br /><br />cartID * - A unique ID of the shopping cart being checked out
    </td>
    <td>analytics.track(“Checkout”,{
    <br />‘innceeID’ : ‘YOUR_ACCOUNT_ID’,
    <br />‘cartID’ : ‘qqqqqq’
    <br />});
    </td>
  </tr>
    <tr>
    <td>Order Completed</td>
    <td>Required. Records the event when an order has been completed
    <br /><br />innceeID * – Your unique account ID
    <br /><br />orderID * - The unique order ID
    </td>
    <td>analytics.track(“OrderComplete”,{
    <br />‘innceeID’ : ‘YOUR_ACCOUNT_ID’,
    <br />‘orderID’ : ‘xxxxxx’
    <br />});
    </td>
  </tr>
</table>

</body>
</html>

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

<html>
<body>

<table>
  <tr>
    <th>Orders</th>
    <th>POST</th> 
    <th>/hooks/order/create</th>
  </tr>
  <tr>
    <td  colspan="3">Called when a new order has been created.</td>
  </tr>
  <tr>
    <th>Orders</th>
    <th>POST</th> 
    <th>/hooks/order/create</th>
  </tr>
  <tr>
    <td  colspan="3">Called when a new order has been created.</td>
  </tr>
  <tr>
    <td  colspan="3">
    <br />{"orders": [
    <br />{
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"id": 12345667,
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"created_at": "2017-02-08T14:56:07-05:00",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"updated_at": "2017-02-08T14:56:07-05:00",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"fulfilled_at": null,
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"total_price": "199.00",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"subtotal_price": "199.00",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"total_tax": "0.00",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"taxes_included": false,
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"currency": "SGD",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"total_discounts": "0.00",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"total_line_items_price": "199.00",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"order_reference": "#1002",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"order_source": "web",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"order_status": "open",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"cancelled_at": NULL,
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"cancel_reason": NULL,
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"customer_id": 1244345,
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"discount_codes": [
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;],
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"line_items": [
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"line_item_id": 1071823175,
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"product_id": 447654529,
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"variant_id": 12133424,
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"product_name": "IPod Touch 8GB",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"quantity": 1,
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"price": "199.00",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"sku": "IPOD2009BLACK",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"variant_name": "Black",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"brand": "Apple",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"fulfillable_quantity": 1,
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"total_discount": "0.00"
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;], 
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;"refunds": [
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"created_at": “”,
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"adjustment": "-3.50",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"tax_adjustment": "-1.00",
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"quantity": "2"
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;}
    <br /> &nbsp; &nbsp; &nbsp; &nbsp;]
    <br /> &nbsp; &nbsp;}
    </td>
  </tr>
  <tr>
    <th>Orders</th>
    <th>DELETE</th>
    <th>/hooks/order/{#orderid}/delete</th>
  </tr>
  <tr>
    <td  colspan="3">Called when an order has been marked for deletion</td>
  </tr>
  <tr>
    <td  colspan="3"></td>
  </tr>
  <tr>
    <td>id</td>
    <td colspan="2">The unique numeric identifier for the order. Order id should be unique across the system. No 2 orders should have the same ids</td> 
  </tr>
  <tr>
    <td>created_at</td>
    <td colspan="2">The date and time when the order was created</td> 
  </tr>
  <tr>
    <td>updated_at</td>
    <td colspan="2">The date and time when the order was updated.</td> 
  </tr>
  <tr>
    <td>fulfilled_at</td>
    <td colspan="2">The date and time when the order was fulfilled. If order is not fulfilled,
value should be NULL.</td> 
  </tr>
  <tr>
    <td>total_price</td>
    <td colspan="2">The sum of all the prices of all the items in the order, taxes and discounts
included. (value must be positive)</td> 
  </tr>
  <tr>
    <td>subtotal_price</td>
    <td colspan="2">Price of the order before shipping and taxes.</td> 
  </tr>
  <tr>
    <td>total_tax</td>
    <td colspan="2">The sum of all the taxes applied to the order. (value must be positive)</td> 
  </tr>
  <tr>
    <td>taxes_included</td>
    <td colspan="2">States whether or not taxes are included in the order subtotal. Valid values
include “true” or “false”</td> 
  </tr>
  <tr>
    <td>currency</td>
    <td colspan="2">The three letter code for the currency used for payment</td> 
  </tr>
  <tr>
    <td>total_discounts</td>
    <td colspan="2">The total amount of discounts to be applied to the price of the order</td> 
  </tr>
  <tr>
    <td>total_line_items_price</td>
    <td colspan="2">The sum of all the prices of all the items in the order</td> 
  </tr>
  <tr>
    <td>Order_reference</td>
    <td colspan="2">The order reference number that the customer sees.</td> 
  </tr>
  <tr>
    <td>order_source</td>
    <td colspan="2">Where the order originated. Valid values include “web”, “pos” or “mobile”.
Mobile refers to orders that originated from a native app</td> 
  </tr>
  <tr>
    <td>order_status</td>
    <td colspan="2">Status of the order. Valid values include
    <br />- open – For orders that have not been fulfilled
    <br />- closed – For orders that have been fulfilled and completed
    <br />- cancelled – For orders that have been cancelled</td> 
  </tr>
</table>

</body>
</html>