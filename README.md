# Installation and Setup
This guide will be able to help walk you through on how to get started with installing Inncee onto your custom website.

* [1.1 Create Account](https://github.com/inncee/docs#11-create-account)
* [1.2 Install your tracking script](https://github.com/inncee/docs#12-install-your-tracking-script)
* [1.3 Tracking Events](https://github.com/inncee/docs#13-tracking-events)
* [Event Setup](https://github.com/inncee/docs#event-setup)
* [Automatically Tracked Events](https://github.com/inncee/docs#automatically-tracked-events)
* [Backend Events](https://github.com/inncee/docs#backend-events)
* [Prerequisites](https://github.com/inncee/docs#prerequisites)
* [ORDERS API](https://github.com/inncee/docs#orders-api)
* [CUSTOMER API](https://github.com/inncee/docs#customer-api)
* [PRODUCTS API](https://github.com/inncee/docs#products-api)
* [CATEGORY API](https://github.com/inncee/docs#category-api)
* [CATEGORYMAP API](https://github.com/inncee/docs#categorymap-api)

### 1.1 Create Account 
Create an account at https://beta.inncee.com. This allows our system to generate:

-	a unique account ID
-	a tracking script code to install on your store
-	and a secret key to allow you to authenticate the JSON data that you can send to our API endpoints

Once generated all this information will be sent to you via email.

Note: Ignore the pop-up that asks you to connect your store to Shopify.

### 1.2 Install your tracking script
You will need to install our tracking script library after the opening of the <head> tag found in every
page of your website.

`<script type=”text/javascript”
src=”https://shopify.inncee.com/tracker/YOUR_ACCOUNT_ID?shop=YOUR_STORE_URL”></script>`

Once the script has been installed, you’ll be able to start recording events on your website.

How to install Inncee’s tracking script in your Shopify Store

To allow us to begin tracking your visitor activity in your store, we’ll need you to embed our tracking script file in your theme. 
By placing it near the top of the <head> tags in your theme file, it will help us to accuratly track when a visitor takes an action in your store.
Follow the steps below to begin your installation.

1) Log in to your shopify store’s admin panel
2) Click on the “Online Store” > “Themes” in your main menu
3) Click on the Three Dots icon to open up the menu, and click on the “Edit HTML/CSS” link

	![Tracking Image](https://shopify.inncee.com/images/script-edit-html-1.png)

4) Select the “theme.liquid” file under your “Layout” folder to load it up on your screen. In the “theme.liquid” file, you should be able see the <head> tag near the top of the file

	![Tracking Image 2](https://shopify.inncee.com/images/script-edit-html-2.png)

5) Paste the your script file codes after the the <title> tags (you should find them a couple of lines down from the <head> tag) Please make sure that you code is pasted only after the closing </title> tag.

	![Tracking Image 3](https://shopify.inncee.com/images/script-edit-html-3.png)

6) Click the “Save” button and you’re done!

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
    <td>User logged in</td>
    <td>Required. This code block helps us to identify visitors when they are logged in. It should be included on every public page of your website when the user has logged in
    <br /><br /> It can be omitted if the user is not logged in 
    <br /><br /> innceeID * – Your unique account ID
    </td>
    <td>
    <pre>
    <code>
    analytics.identify(
    '&lt;USER_ID&gt;',{'innceeID':
    '&lt;YOUR_ACCOUNT_ID&gt;'});
    </code>
    </pre>
    </td>
  </tr>
  <tr>
    <td>Add to cart</td>
    <td>Required. This code block records the event whenever a user clicks on the add to cart button. It should be included on every page that has an “Add To Cart” button
    <br /><br />innceeID * – Your unique account ID
    <br /><br />categoryID * - The unique ID of your category
    <br /><br />productID * – The unique ID of the product being added to cart variantID
    </td>
    <td>
    <pre>
    <code>
    analytics.track(
    “addtocart”,{‘innceeID’:
    ‘&lt;YOUR_ACCOUNT_ID&gt;’,
    ‘categoryID’:‘&lt;yyyyy&gt;’,
    ‘productID’:‘&lt;ppppp&gt;’,
    ‘variantID’: ‘&lt;&gt;’,
    ‘Price’: ’&lt;12.50&gt;’,
    ‘Quantity’: ‘&lt;1&gt;’,
    ‘Currency’: ‘&lt;$&gt;’}
    );
    </code>
     </pre>
    </td>
  </tr>
  <tr>
    <td>Checkout</td>
    <td>Required. This code block records the event when a user initiates a checkout. It should be included on every page that has a checkout button.
    <br /><br />innceeID * – Your unique account ID
    <br /><br />cartID * - A unique ID of the shopping cart being checked out
    </td>
    <td>
    <pre>
    <code>
    analytics.identify(
    '&lt;USER_ID&gt;',{'innceeID':
    '&lt;YOUR_ACCOUNT_ID&gt;'}
    );
    </code>
     </pre>
    </td>
  </tr>
    <tr>
    <td>Order Completed</td>
    <td>Required. Records the event when an order has been completed
    <br /><br />innceeID * – Your unique account ID
    <br /><br />orderID * - The unique order ID
    </td>
    <td>
    <pre>
    <code>
    analytics.track(“OrderComplete”,{
    ‘innceeID’:‘&lt;YOUR_ACCOUNT_ID&gt;’,
    ‘orderID’:‘&lt;xxxxxx&gt;’}
    );
    </code>
     </pre>
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

#### Backend Events
The code snippets above help us to track events that happen on your online store. We will also need to track certain events that happen in the backend of your store as well.

We’ve setup a series of API end points that ready to receive JSON data from your store when certain backend events happen.

For example, when a new order has been created, you can send a JSON response with information about the new order to our API end point and our platform can begin storing that information for analysis.

##### Prerequisites
When making API calls to our servers, you are required to first generate a HMAC and attach it to your headers along with the timestamp used to generate it.

The recommended method for generating the HMAC using PHP is as follows:

```
$timestamp = time(); 
$timestamp = time();
$secret = YOUR_SHARED_SECRET;
$domain = "YOUR_DOMAIN.COM";

$message = "store=".$domain."&timestamp=".$timestamp;
$hmac = base64_encode(hash_hmac('sha256', $message, $secret, true));
```

When making calls to our servers, attach the timestamp as a URL parameter.
```
$ch = curl_init();

$url = "https://api.inncee.com/hooks/order/create?timestamp".$timestamp
curl_setopt($ch,CURLOPT_URL, $url);
```

the timestamp should be the same one used to generate the HMAC. 

Now attach the HMAC as a header with the key HTTP_X_HMAC_SHA256.

```
$hmac_header = "HTTP_X_HMAC_SHA256:".$hmac
curl_setopt($ch,CURLOPT_HTTPHEADER, array($hmac_header));
```

Once done, you are now good to attach your data and send to our servers.
See the next section for a list of all our API end points and when you can send data to it.

###### ORDERS API 

<html>
<body>

<table width="100%">
  <tr>
    <th>Orders</th>
    <th>POST</th> 
    <th>/hooks/order/create</th>
  </tr>
  <tr>
    <td  colspan="3">Called when a new order has been created</td>
  </tr>
  <tr>
    <th>Orders</th>
    <th>PUT</th> 
    <th>/hooks/order/create</th>
  </tr>
  <tr>
    <td  colspan="3">Called when a new order has been created</td>
  </tr>
  <tr>
    <td  colspan="3">
    <pre>
    <code>{
    "orders": [
    {
    	"id": &lt;12345667&gt;,
    	"created_at": "&lt;2017-02-08T14:56:07-05:00&gt;",
    	"updated_at": "&lt;2017-02-08T14:56:07-05:00&gt;",
    	"fulfilled_at": &lt;null&gt;,
    	"total_price": "&lt;199.00&gt;",
    	"subtotal_price": "&lt;199.00&gt;",
    	"total_tax": "&lt;0.00&gt;",
    	"taxes_included": &lt;false&gt;,
    	"currency": "&lt;SGD&gt;",
    	"total_discounts": "&lt;0.00&gt;",
    	"total_line_items_price": "&lt;199.00&gt;",
    	"order_reference": "&lt;#1002&gt;",
    	"order_source": "&lt;web&gt;",
    	"order_status": "&lt;open&gt;",
    	"cancelled_at": &lt;NULL&gt;,
    	"cancel_reason": &lt;NULL&gt;,
    	"customer_id": &lt;1244345&gt;,
    	"discount_codes": [
    	],
    		"line_items": [
            {
            	"line_item_id": &lt;1071823175&gt;,
                "product_id": &lt;447654529&gt;,
                "variant_id": &lt;12133424&gt;,
                "product_name": "&lt;IPod Touch 8GB&gt;",
                "quantity": &lt;1&gt;,
                "price": "&lt;199.00&gt;",
                "sku": "&lt;IPOD2009BLACK&gt;",
                "variant_name": "&lt;Black&gt;",
                "brand": "&lt;Apple&gt;",
                "fulfillable_quantity": &lt;1&gt;,
                "total_discount": "&lt;0.00&gt;"
    		}
            ],
            "refunds": [
            {
            "created_at": “&lt;&gt;”,
            "adjustment": "&lt;-3.50&gt;",
            "tax_adjustment": "&lt;-1.00&gt;",
            "quantity": "&lt;2&gt;"
            }
            ]
    }
    ]
}
    </code>
    </pre>
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
    <td colspan="2">The date and time when the order was updated</td> 
  </tr>
  <tr>
    <td>fulfilled_at</td>
    <td colspan="2">The date and time when the order was fulfilled. If order is not fulfilled,
value should be NULL</td> 
  </tr>
  <tr>
    <td>total_price</td>
    <td colspan="2">The sum of all the prices of all the items in the order, taxes and discounts
included. (value must be positive)</td> 
  </tr>
  <tr>
    <td>subtotal_price</td>
    <td colspan="2">Price of the order before shipping and taxes</td> 
  </tr>
  <tr>
    <td>total_tax</td>
    <td colspan="2">The sum of all the taxes applied to the order (value must be positive)</td> 
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
    <td colspan="2">The order reference number that the customer sees</td> 
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
  <li>customer: The customer changed or cancelled the order</li>
  <li>fraud: The order was fraudulent.</li>
  <li>inventory: Items in the order were not in inventory</li>
  <li>other: The order was cancelled for a reason not in the list above</li>
</ul>   
  </tr>
  <tr>
    <td>customer_id</td>
    <td colspan="2">The unique numeric identifier for the customer</td> 
    </tr>
  <tr>
    <td>discount_codes</td>
    <td colspan="2">List of discount codes that can be applied to  the order. If no codes exist the value will default to blank
    <br /><br />A Discount code will include the following fields:
    <ul>
  <li>amount: The amount of the discount. The type field determines the
unit of this amount</li>
  <li>code: The discount code</li>
  <li>type: The type of discount, valid values are:
    <ul>
    <li>fixed_amount: The default value. Applies a discount of
amount as a unit of the store's currency. For example, if
amount is 30 and the store's currency is USD, then 30 USD
is deducted from the order total when the discount is
applied</li>
    <li>percentage: Applies a percentage discount of amount. For
example, if amount is 30, then 30% of the order total will
be deducted when the discount is applied</li>
    <li>shipping: Applies a free shipping discount on orders that
have a shipping rate less than or equal to amount. For
example, if amount is 30, the discount will give the
customer free shipping for any shipping rate that is less
than or equal to $30</li>
    </ul>
  </li>
</ul>
    </td> 
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
  	<li>total_discount: The total discount amount applied to this line item. This value is not subtracted in the line item price</li>
	</ul>  
    </td> 
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

<table  width="100%">
  <tr>
    <th>Customers</th>
    <th>POST</th> 
    <th>/hooks/customers/create</th>
  </tr>
  <tr>
    <td colspan="3">Called when a new customer account has been created</td>
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
    <pre>
    <code>
    {
    	"customers": [
        {
        	"id": &lt;543213&gt;,
            "created_at":"&lt;2017-02-08T14:56:07-05:00&gt;",
            "updated_at": "&lt;2017-02-08T14:56:07-05:00&gt;",
            "accepts_marketing": &lt;true&gt;,
            "city": "&lt;Ottawa&gt;",
            "province": "&lt;Ontario&gt;",
            "country": "&lt;Canada&gt;,
            "status": "&lt;enabled&gt;"
    	}
        ]
    }
    </code>
    </pre>
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
    <td colspan="2">The status of the customer’s account. Valid states are:
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

<table  width="100%">
  <tr>
    <th>Products</th>
    <th>POST</th> 
    <th>/hooks/products/create</th>
  </tr>
  <tr>
    <td colspan="3">Called when a new product has been created</td>
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
    <pre>
    <code>
    {
    	"products": [
        {
        	"id": &lt;3245465,
            "created_at": "&lt;2017-02-08T14:56:07-05:00&gt;",
            "updated_at": "&lt;2017-02-08T14:56:07-05:00&gt;",
            "product_name": "&lt;IPod Touch 8GB&gt;",
            "brand": "&lt;Apple&gt;",
            "tags": "&lt;music, device, white&gt;",
            "path": "&lt;product/android-phone/lenovo-a1000-hitam-/0889800170294&gt;",
            "image_url": "&lt;https://api.oktagon.co.id/core/images/product /ZTE%20Kis%203%20V8118w%20Biru.jpg&gt;",
            "variants": 
            	[
                	{
                    	"id": &lt;123454534&gt;,
                        "product_id": &lt;3245465&gt;,
                        "name": "&lt;8G&gt;",
                        "price": "&lt;128.00&gt;",
                        "sku": "&lt;IPOD2008PINK&gt;",
                        "inventory_quantity": &lt;null&gt;
            		},
                    {
                    	"id": &lt;123454534&gt;,
                        "product_id": &lt;3245465&gt;,
                        "name": "&lt;16G&gt;",
                        "price": "&lt;128.00&gt;",
                        "sku": "&lt;IPOD2008PINK&gt;",
                        "inventory_quantity": &lt;null&gt;
     				}
     			]
     	}
     	]
     }
     </code>
     </pre>
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
    <td colspan="2">Name of the product</td>
  </tr>
  <tr>
    <td>brand</td>
    <td colspan="2">Supplier or brand name of the product</td>
  </tr>
  <tr>
    <td>tags</td>
    <td colspan="2">A categorization that a product can be tagged with, commonly used for filtering and searching. Each comma-separated tag has a character limit of 255</td>
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
    <br /><br />If your product does not have a variant, then simply create 1 variant record with the name “Default” to record the product’s SKU, price and other details
    <br /><br />
    <ul>
  <li>id: The unique numerical identifier to identify the variant</li>
  <li>product_id: The unique product id that the variant belongs to</li>
  <li>name: Name of the variant</li>
  <li>price: Price of the variant</li>
  <li>sku: SKU number of the variant</li>
  <li>inventory_quantity: Total available units of inventory available for sale</li>
</ul>
    </td>
  </tr>
</table>

</body>
</html>

###### CATEGORY API

<html>
<body>

<table  width="100%">
  <tr>
    <th>Category</th>
    <th>POST</th> 
    <th>/hooks/category/create</th>
  </tr>
  <tr>
    <td colspan="3">Called when a new category has been created</td>
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
    <pre>
    <code>
    {
    	"categories": [
        {
        "id": &lt;3245465&gt;,
        "created_at": "&lt;2017-02-08T14:56:07-05:00"&gt;,
        "updated_at": "&lt;2017-02-08T14:56:07-05:00"&gt;,
        "category_name": "&lt;Android Phones"&gt;,
        "path": "&lt;/products/android-phone/AN03"&gt;,
        "parent_id": "&lt;2434435&gt;"
        }
        ]
    }
    </code>
    </pre>
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
then this value is set to NULL
	</td>
  </tr>
</table>

</body>
</html>

###### CATEGORYMAP API

<html>
<body>

<table  width="100%">
  <tr>
    <th>CategoryMap</th>
    <th>POST</th> 
    <th width="70%">/hooks/categorymap/create</th>
  </tr>
  <tr>
    <td colspan="3">Called when you add a product to a category</td>
  </tr>
    <tr>
    <td colspan="3">
    <pre>
    <code>
    {
    "categorymap": [
    {
    	"id": &lt; 3245465&gt;
        "created_at": "&lt;2017-02-08T14:56:07-05:00&gt;",
        "updated_at": "&lt;2017-02-08T14:56:07-05:00&gt;",
        "category_id": &lt; 3424223&gt;,
        "product_id": &lt;4344542&gt;
        }
        ]
     }
     </code>
     </pre>
    </td>
  </tr>
  <tr>
    <th>CategoryMap</th>
    <th>DELETE</th> 
    <th>/hooks/categorymap/delete</th>
  </tr>
    <tr>
    <td colspan="3">Called when a product has been removed from a category</td>
  </tr>
  <tr>
    <td colspan="3">
    <pre>
    <code>
    {
    	"categorymap": [
        {
        	"id": &lt;3245465&gt;
            "category_id": &lt;3424223&gt;,
            "product_id": &lt;4344542&gt;
            }
            ]
    }
    </code>
     </pre>
    </td>
  </tr>
  <tr>
    <td colspan="3"></td>
  </tr>
  <tr>
    <td>id</td>
    <td colspan="2">The unique numeric identifier for the mapping</td>
  </tr>
  <tr>
    <td>created_at</td>
    <td colspan="2">The date and time stamp that the product was added to the category</td>
  </tr>
  <tr>
    <td>updated_at</td>
    <td colspan="2">The date and timestamp that the product mapping was updated</td>
  </tr>
  <tr>
    <td>category_id</td>
    <td colspan="2">The unique numeric identifier of the category that the product is mapped to</td>
  </tr>
  <tr>
    <td>product_id</td>
    <td colspan="2">The unique numeric identifier of the product that is being mapped</td>
  </tr>
</table>

</body>
</html>