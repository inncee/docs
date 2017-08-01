# Installation and Setup
This guide will be able to help walk you through on how to get started with installing Inncee onto your custom website.

* [1.1 Create Account](https://github.com/inncee/docs/tree/master/Installation%20Guides/Custom#11-create-account)
* [1.2 Install your tracking script](https://github.com/inncee/docs/tree/master/Installation%20Guides/Custom#12-install-your-tracking-script)
* [1.3 Tracking Events](https://github.com/inncee/docs/tree/master/Installation%20Guides/Custom#13-tracking-events)
	* [Event Setup](https://github.com/inncee/docs/tree/master/Installation%20Guides/Custom#event-setup)
	* [Automatically Tracked Events](https://github.com/inncee/docs#automatically-tracked-events)
	* [Backend Events](https://github.com/inncee/docs#backend-events)
* [1.4 Sending Backend Data via API](https://github.com/inncee/docs/Installation%20Guides/Custom#14-sending-backend-data-to-inncee)
	* [Prerequisites](https://github.com/inncee/docs/Installation%20Guides/Custom#prerequisites)

### 1.1 Create Account 
Create an account at https://beta.inncee.com. This allows our system to generate:

-	a unique account ID
-	a tracking script code to install on your store
-	and a secret key to allow you to authenticate the JSON data that you can send to our API endpoints

Once generated all this information will be sent to you via email.

Note: Ignore the pop-up that asks you to connect your store to Shopify.

### 1.2 Install your tracking script
The Inncee tracking library helps us to track the browsing behaviour of your visitors and the specific actions they're taking on your online store. This allows us to measure whether they are engaging well with your store's content.

The tracking script library must be installed after the opening of the tag found in every page of your website. The page that you're on after you've successfully installed the Inncee app will display the tracking script that you need to install. If you are no longer able to access that page, you can also find your tracking script by logging in to your Inncee dashboard and navigating to the Settings > Integrations dashboard.

```html
<script type="text/javascript"
src="https://api.inncee.com/scripts/shopify/tracker/YOUR_ACCOUNT_ID"></script>
```
Do note that you tracking script should be inserted as high up as possible in your head tag
```html
<head>
    <script type="text/javascript"
    src="https://api.inncee.com/scripts/shopify/tracker/YOUR_ACCOUNT_ID"></script>
    ...other codes here ...
</head>
```

Once the script has been installed, we’ll be able to start recording events on your website.

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
    <td>Add to cart</td>
    <td>
		Required. This code block records the event whenever a user clicks on the add to cart button. It should be included on every page that has an “Add To Cart” button
		<ul>
			<li><strong>innceeID *</strong> - Your unique account ID</li>
			<li><strong>categoryID *</strong> - The unique ID of your category</li>
			<li><strong>productID *</strong> - The unique ID of the product being added to cart</li>
			<li><strong>variantID *</strong> - The unique ID of th variant being added to cart</li>
			<li><strong>Price</strong> - The unit price of the item added to cart</li>
			<li><strong>Quantity</strong> - Quantity of the item added to cart</li>
			<li><strong>Currency</strong> - Currency of the item that was added to cart</li>
		</ul>
    </td>
    <td>
    <pre>
    <code>
    analytics.track("addtocart", {
    	'innceeID': '&lt;YOUR_ACCOUNT_ID&gt;',
    	'categoryID': 'yyyyy',
    	'productID': 'ppppp',
    	'variantID': 'xxxx',
    	'Price': '12.50',
    	'Quantity': '1',
    	'Currency': '$'
	});
    </code>
     </pre>
    </td>
  </tr>
  <tr>
    <td>Checkout</td>
    <td>
		Required. This code block records the event when a user initiates a checkout. It should be included on the first page of the checkout workflow.
		<ul>
			<li><strong>innceeID *</strong> – Your unique account ID</li>
			<li><strong>created_at</strong> - Date the checkout was created</li>
			<li><strong>currency</strong> - The currency the customer is paying in</li>
			<li><strong>customer_id</strong> - The customer's unique identifier</li>
			<li><strong>discount_code</strong> - The discount codes applied to the checkout</li>
			<li><strong>email</strong> - The customer's email</li>
			<li><strong>subtotal_price</strong> - Price of the checkout before shipping and taxes</li>
			<li><strong>total_price</strong> - Sum of all the items in the checkout including shipping, taxes and discounts.</li>
			<li><strong>total_tax</strong> - Sum of all the taxes applied to the checkout</li>
			<li><strong>line_items</strong> - A list of all the line item objects, each containing info about each item in the checkout.</li>
			<li><strong>shipping_info</strong> - Information about the shipping rates applied to the checkout</li>
			<li><strong>applied_discount</strong> - The total discounts applied to the checkout.</li>
		</ul>
    </td>
    <td>
    <pre>
    <code>
    analytics.track("checkout", {
    	'innceeID': '&lt;YOUR_ACCOUNT_ID&gt;',
    	"created_at": "2012-10-12T07:05:27-04:00",
	    "currency": "USD",
	    "customer_id": 207119551,
	    "discount_code": null,
	    "email": "bob.norman@hostmail.com",
	    "subtotal_price": "398.00",
	    "total_price": "408.99",
	    "total_tax": "21.49",
	    "line_items": [
	      {
	        "id": "84b57c8bf61fa614",
	        "product_id": 632910392,
	        "variant_id": 808950810,
	        "sku": "IPOD2008PINK",
	        "vendor": "Apple",
	        "title": "IPod Nano - 8GB",
	        "price": "199.00",
	        "quantity": 1,
	        "applied_discount": "10.00"
	      },
	      {
	        "id": "84b57c8bf61fa615",
	        "product_id": 632910393,
	        "variant_id": 808950811,
	        "sku": "IPOD2008GOLD",
	        "vendor": "Apple",
	        "title": "IPod Nano - 8GB",
	        "price": "199.00",
	        "quantity": 1,
	        "applied_discount": "2.00"
	      }
	    ],
	    "shipping_info": {
	      "price": "10.99",
	      "title": "Standard Shipping"
	    },
	    "applied_discount": "12.00"
	  }
	});
    </code>
     </pre>
    </td>
  </tr>
    <tr>
    <td>Order Completed</td>
    <td>
		Required. Records the event when an order has been completed
		<ul>
			<li><strong>innceeID *</strong> - Your unique account ID</li>
			<li><strong>orderID *</strong> - The unique order ID</li>
		</ul>
    </td>
    <td>
    <pre>
    <code>
    analytics.track(“ordercomplete”,{
    	'innceeID': '&lt;YOUR_ACCOUNT_ID&gt;',
    	'orderID': ‘xxxxxx’
    });
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

### 1.4 Sending Backend Data To Inncee
The code snippets above help us to track events that happen on your online store. But we will also need to track certain events that happen in the backend of your store as well.

We’ve setup a series of API end points that are ready to receive JSON data from your store when certain backend events happen.

For example, when a new order has been created, you can send a JSON response with information about the new order to our API end point and our platform can begin storing that information for analysis.

##### Prerequisites
When making API calls to our servers, you are required to first generate a HMAC and attach it to your headers along with the timestamp used to generate it.

The recommended method for generating the HMAC using PHP is as follows:

```php
$timestamp = time(); 
$timestamp = time();
$secret = <YOUR_SHARED_SECRET>;
$domain = "<YOUR_DOMAIN.COM>";

$message = "store=".$domain."&timestamp=".$timestamp;
$hmac = base64_encode(hash_hmac('sha256', $message, $secret, true));
```

When making calls to our servers, attach the timestamp as a URL parameter.
```php
$ch = curl_init();

$url = "https://api.inncee.com/hooks/order/create?timestamp".$timestamp
curl_setopt($ch,CURLOPT_URL, $url);
```

the timestamp should be the same one used to generate the HMAC. 

Now attach the HMAC as a header with the key HTTP_X_HMAC_SHA256.

```php
$hmac_header = "HTTP_X_HMAC_SHA256:".$hmac
curl_setopt($ch,CURLOPT_HTTPHEADER, array($hmac_header));
```

Once done, you are now good to attach your data and send to our servers.
See the next section for a list of all our API end points and when you can send data to it.


##### Sending Data
We will be using the customer data as a sample to post.

First, create the customer class.

```php
class customer{
   public $id;
   public $created_at;
   public $update_at;
   public $accepts_marketing;
   public $city;
   public $province;
   public $status;


   public function __construct ($id, $created_at...){
       $this->id = $id;
       $this->created_at = $created_at
       ...
   }
}
```
Once the class is created, you can now populate the customers and convert into an array
```php
$customer_array =  array();

$customer = new customer('123', '2017-02-08T14:56:07-05:00'...);

//you can also assign manually
$customer->city = 'Singapore';

//once customer is created, you can push it into an array
array_push($customer_array , $customer);
```

When the array is completed, you can encode the array into a json and attach it to a curl call
```
$data = array("customer" => $customer_array);

$jsonData = json_encode($data);

curl_setopt($ch, CURLOPT_POSTFIELDS, $jsonData);
```
With the curl set up, let's just prepare a few more headers and you are ready to send us the data.

```php
curl_setopt(
    $ch, 
    CURLOPT_HTTPHEADER, 
    array(
        'Content-Type: application/json',
        'HTTP_X_HMAC_SHA256: ' . $hmac,
        'Content-Length: ' . strlen($data_string)
    )
);
```
Now that everything is done, all you need to do is send the data over
```php
$output = curl_exec($ch);
```
