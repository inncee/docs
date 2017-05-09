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
    <th>PUT</th> 
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
    <ul>
  <li>open – For orders that have not been fulfilled</li>
  <li>closed – For orders that have been fulfilled and completed</li>
  <li>cancelled – For orders that have been cancelled</li>
</ul>  
    </td> 
  </tr>
  <tr>
    <td>cancelled_at</td>
    <td colspan="2">The date and time when the order was cancelled. Value is NULL if order is
not cancelled</td> 
  </tr>
  <tr>
    <td>cancel_reason</td>
    <td colspan="2">The reason why the order was  cancelled. If the order was not cancelled, this value is "null." If the order was cancelled, the value will be one of the
following:
<ul>
  <li>customer: The customer changed or cancelled the order.</li>
  <li>fraud: The order was fraudulent.</li>
  <li>inventory: Items in the order were not in inventory.</li>
  <li>other: The order was cancelled for a reason not in the list above.</li>
</ul>   
  </tr>
  <tr>
    <td>customer_id</td>
    <td colspan="2">The unique numeric identifier for the customer.</td> 
  </tr>
  </tr>
  <tr>
    <td>discount_codes</td>
    <td colspan="2">List of discount codes that can be applied to  the order. If no codes exist the value will default to blank. 
    <br /><br />A Discount code will include the following fields:
    <ul>
  <li>amount: The amount of the discount. The type field determines the
unit of this amount.</li>
  <li>code: The discount code.</li>
  <li>type: The type of discount. Valid values are:
    <ul>
    <li>fixed_amount: The default value. Applies a discount of
amount as a unit of the store's currency. For example, if
amount is 30 and the store's currency is USD, then 30 USD
is deducted from the order total when the discount is
applied.</li>
    <li>percentage: Applies a percentage discount of amount. For
example, if amount is 30, then 30% of the order total will
be deducted when the discount is applied.</li>
    <li>shipping: Applies a free shipping discount on orders that
have a shipping rate less than or equal to amount. For
example, if amount is 30, the discount will give the
customer free shipping for any shipping rate that is less
than or equal to $30.</li>
    </ul>
  </li>
</ul>
    </td> 
  </tr>
  </tr>
  <tr>
    <td>line_items</td>
    <td colspan="2">A list of line item objects, each one containing information about an item in the order. Each line_item object has the following properties:
    <ul>
  	<li>line_item_id: the unique numerical identifier of the line item</li>
  	<li>product_id: the unique numerical identifier of the product</li>
  	<li>variant_id: the unique numerical identifier of the product variant</li>
  	<li>product_name: the name of the product</li>
  	<li>quantity: the quantity ordered by the customer</li>
  	<li>price: the price of the product ordered</li>
  	<li>sku: A unique identifier of the product for fulfillment</li>
  	<li>variant_name: the name of the product variant</li>
  	<li>brand: the name of the supplier or brand of the product</li>
  	<li>fulfillable_quantity:</li>
  	<li>total_discount: The total discount amount applied to this line item. This value is not subtracted in the line item price.</li>
	</ul>  
    </td> 
  </tr>
  </tr>
  <tr>
    <td>refunds</td>
    <td colspan="2">List of refunds applied to the order. If no refunds exists, the value will default to blank
    <br /><br />Each refund object has the following properties:
    <ul>
  		<li>created_at: the date and time that the refund was created</li>
  		<li>adjustment:</li>
  		<li>tax_adjustment:</li>
	</ul>  
    </td> 
  </tr>
</table>

</body>
</html>

###### CUSTOMER API 

<html>
<body>

<table>
  <tr>
    <th>Customers</th>
    <th>POST</th> 
    <th>/hooks/customers/create</th>
  </tr>
  <tr>
    <td colspan="3">Called when a new customer account has been created.</td>
  </tr>
  <tr>
    <th>Customers</th>
    <th>PUT</th> 
    <th>/hooks/customers/create</th>
  </tr>
  <tr>
    <td colspan="3">Called when a customer account has been updated</td>
  </tr>
    <tr>
    <td colspan="3">
    {
    <br />&nbsp;&nbsp;&nbsp;&nbsp;"customers": [
    <br />&nbsp;&nbsp;&nbsp;&nbsp;{
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id": 543213,
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"created_at":
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"2017-02-08T14:56:07-05:00",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"updated_at": "2017-02-08T14:56:07-05:00",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"accepts_marketing": true,
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"city": "Ottawa",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"province": "Ontario",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"country": "Canada",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"status": "enabled"
    <br />&nbsp;&nbsp;&nbsp;&nbsp;}]
    <br />}
    </td>
  </tr>
  <tr>
    <th>Customers</th>
    <th>DELETE</th> 
    <th>/hooks/customers/{#customerid}/delete</th>
  </tr>
  <tr>
    <td colspan="3">Called when a customer account has been deleted</td>
  </tr>
  <tr>
    <td colspan="3"></td>
  </tr>
  <tr>
    <td>id</td>
    <td colspan="2">The unique numerical identifier for the customer</td>
  </tr>
  <tr>
    <td>created_at</td>
    <td colspan="2">The date and time that the customer was created</td>
  </tr>
  <tr>
    <td>updated_at</td>
    <td colspan="2">Date and time that the customer’s information was updated</td>
  </tr>
  <tr>
    <td>accepts_marketing</td>
    <td colspan="2">Indicates whether the customer has consented to be sent marketing materials
via emails. Valid values are “true” and “false”</td>
  </tr>
  <tr>
    <td>city</td>
    <td colspan="2">The customer’s city</td>
  </tr>
  <tr>
    <td>province</td>
    <td colspan="2">The customer’s province</td>
  </tr>
  <tr>
    <td>country</td>
    <td colspan="2">The customer’s country</td>
  </tr>
  <tr>
    <td>status</td>
    <td colspan="2">The status of the customer’s account. Valid states are
    <ul>
  		<li>enabled : The customer has verified their email address and created an active account</li>
  		<li>disabled: The customer has not verified their email addresses and logged into their account yet. Account is considered not active</li>
    </ul>
    </td>
  </tr>
</table>

</body>
</html>

###### PRODUCTS API

<html>
<body>

<table>
  <tr>
    <th>Products</th>
    <th>POST</th> 
    <th>/hooks/products/create</th>
  </tr>
  <tr>
    <td colspan="3">Called when a new product has been created.</td>
  </tr>
  <tr>
    <th>Products</th>
    <th>PUT</th> 
    <th>/hooks/products/update</th>
  </tr>
  <tr>
    <td colspan="3">Called when a customer account has been updated</td>
  </tr>
    <tr>
    <td colspan="3">
    {
    <br />&nbsp;&nbsp;&nbsp;&nbsp;"products": [
    <br />&nbsp;&nbsp;&nbsp;&nbsp;{
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id": 3245465,
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"created_at": "2017-02-08T14:56:07-05:00",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"updated_at": "2017-02-08T14:56:07-05:00",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"product_name": "IPod Touch 8GB",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"brand": "Apple",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"tags": "music, device, white",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"path": "product/android-phone/lenovo-a1000-hitam-/0889800170294",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"image_url": "https://api.oktagon.co.id/core/images/product /ZTE%20Kis%203%20V8118w%20Biru.jpg",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"variants": [
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id": 123454534,
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"product_id": 3245465,
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "8G",
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"price": "128.00",
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"sku": "IPOD2008PINK",
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"inventory_quantity": null
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; },
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id": 123454534,
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"product_id": 3245465,
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"name": "16G",
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"price": "128.00",
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"sku": "IPOD2008PINK",
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"inventory_quantity": null
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
     <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]
     <br />}
    </td>
  </tr>
  <tr>
    <th>Products</th>
    <th>DELETE</th> 
    <th>/hooks/products/{#productid}/delete</th>
  </tr>
  <tr>
    <td colspan="3">Called when a product has been deleted</td>
  </tr>
  <tr>
    <td colspan="3"></td>
  </tr>
  <tr>
    <td>id</td>
    <td colspan="2">The unique numerical identifier for the product</td>
  </tr>
  <tr>
    <td>created_at</td>
    <td colspan="2">The date and time that the product was created</td>
  </tr>
  <tr>
    <td>updated_at</td>
    <td colspan="2">The date and time that the product was updated</td>
  </tr>
  <tr>
    <td>product_name</td>
    <td colspan="2">Supplier or brand name of the product</td>
  </tr>
  <tr>
    <td>brand</td>
    <td colspan="2">Supplier or brand name of the product</td>
  </tr>
  <tr>
    <td>tags</td>
    <td colspan="2">A categorization that a product can be tagged with, commonly used for filtering and searching. Each comma-separated tag has a character limit of 255.</td>
  </tr>
  <tr>
    <td>path</td>
    <td colspan="2">The unique url path visitors use to view the product</td>
  </tr>
  <tr>
    <td>image_url</td>
    <td colspan="2">The url to the main image of the product
    </td>
  </tr>
  <tr>
    <td>variants</td>
    <td colspan="2">A list of variant objects, each representing a slightly different version of the product. For example, if a product comes in different sizes and colors, each size and color permutation (such as "small black", "medium black", "large blue"), would be a variant. 
    <br /><br />If your product does not have a variant, then simply create 1 variant record with the name “Default” to record the product’s SKU, price and other details.
    <br /><br />
    <ul>
  <li>id: The unique numerical identifier to identify the variant.</li>
  <li>product_id: The unique product id that the variant belongs to.</li>
  <li>name: Name of the variant</li>
  <li>price: Price of the variant</li>
  <li>sku: SKU number of the variant</li>
  <li>inventory_quantity: Total available units of inventory available for sale.</li>
</ul>
    </td>
  </tr>
</table>

</body>
</html>

###### CATEGORY API

<html>
<body>

<table>
  <tr>
    <th>Category</th>
    <th>POST</th> 
    <th>/hooks/category/create</th>
  </tr>
  <tr>
    <td colspan="3">Called when a new category has been created.</td>
  </tr>
  <tr>
    <th>Category</th>
    <th>PUT</th> 
    <th>/hooks/category/update</th>
  </tr>
  <tr>
    <td colspan="3">Called when a category’s information has been updated</td>
  </tr>
    <tr>
    <td colspan="3">
    {
    <br />&nbsp;&nbsp;&nbsp;&nbsp;"categories": [
    <br />&nbsp;&nbsp;&nbsp;&nbsp;{
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"id": 3245465,
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"created_at": "2017-02-08T14:56:07-05:00",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"updated_at": "2017-02-08T14:56:07-05:00",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"category_name": "Android Phones",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"path": "/products/android-phone/AN03",
    <br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"parent_id": "2434435"
     <br />&nbsp;&nbsp;&nbsp;}
     <br />&nbsp;&nbsp;&nbsp;]
     <br />}
    </td>
  </tr>
  <tr>
    <th>Category</th>
    <th>DELETE</th> 
    <th>/hooks/category/{#categoryid}/delete</th>
  </tr>
  <tr>
    <td colspan="3">Called when a category has been deleted</td>
  </tr>
  <tr>
    <td colspan="3"></td>
  </tr>
  <tr>
    <td>id</td>
    <td colspan="2">The unique numerical identifier for the category</td>
  </tr>
  <tr>
    <td>created_at</td>
    <td colspan="2">The date and time that the category was created</td>
  </tr>
  <tr>
    <td>updated_at</td>
    <td colspan="2">The date and time that the category information was updated</td>
  </tr>
  <tr>
    <td>Category_name</td>
    <td colspan="2">Name of the category displayed to the visitors</td>
  </tr>
  <tr>
    <td>Path</td>
    <td colspan="2">The unique url path that visitors use to view the category</td>
  </tr>
  <tr>
    <td>Parent_id</td>
    <td colspan="2">The unique identifier of the parent category. If there is no parent category,
then this value is set to NULL.
	</td>
  </tr>
</table>

</body>
</html>

