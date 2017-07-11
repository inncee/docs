# Inncee's API Reference Guide
Below is a detailed guide on the various API end points that Inncee has published to allow users to send data into our platform. 

* [Orders](https://github.com/inncee/docs/tree/master/Developers%20Reference/API#orders-api)
* [Customers](https://github.com/inncee/docs/tree/master/Developers%20Reference/API#customers-api)
* [Products](https://github.com/inncee/docs/tree/master/Developers%20Reference/API#products-api)
* [Category](https://github.com/inncee/docs/tree/master/Developers%20Reference/API#category-api)
* [CategoryMap](https://github.com/inncee/docs/tree/master/Developers%20Reference/API#categorymap-api)

## ORDERS API 


<table width="100%">
	<tr>
		<th>Orders</th>
		<th>GET</th>
		<th>/hooks/order/#{orderid}</th>
	</tr>
	<tr>
		<td colspan="3">
			Retrieves an order by the order id
		</td>
	</tr>
	<tr>
		<td colspan="3" style="border-bottom:1px solid #ccc">
			<p><i>To see response returned by this API call, please look at the order object in the order update endpoint section of the documentation below.</i></p>
		</td>
	</tr>
	<tr>
		<th>Orders</th>
		<th>GET</th>
		<th>/hooks/order/all</th>
	</tr>
	<tr>
		<td colspan="3">
			Retrieves all the orders by default
		</td>
	</tr>
	<tr>
		<td>limit</td>
		<td colspan="2">Amount of results (default: 50) (maximum: 200)</td>
	</tr>
	<tr>
		<td>page</td>
		<td colspan="2">Page to show (default: 1)</td>
	</tr>
	<tr>
		<td colspan="3">
			<h3>List 200 order records</h3>
			<p><i>GET /hooks/order/all?limit=200</i></p>
		</td>
	</tr>
	<tr>
		<td colspan="3" style="border-bottom:1px solid #ccc">
			<p><i>To see response returned by this API call, please look at the order object in the order update endpoint section of the documentation below.</i></p>
		</td>
	</tr>
	<tr>
    	<th>Orders</th>
    	<th>POST</th> 
    	<th>/hooks/order/create</th>
	</tr>
  	<tr>
    	<td colspan="3" style="border-bottom:1px solid #ccc">Called when a new order has been created</td>
  	</tr>
  	<tr>
    	<th>Orders</th>
    	<th>PUT</th> 
    	<th>/hooks/order/update</th>
  	</tr>
  	<tr>
    	<td colspan="3">Called when a new order has been created</td>
  	</tr>
  	<tr>
    	<td colspan="3" style="border-bottom:1px solid #ccc">
    	<pre>
    		<code>
   {
     "orders": [
      {
         "id": 12345667,
         "created_at": "2017-02-08T14:56:07-05:00",
         "updated_at": "2017-02-08T14:56:07-05:00",
         "fulfilled_at": null,
         "total_price": "199.00",
         "subtotal_price": "199.00",
         "total_tax": "0.00",
         "taxes_included": false,
         "currency": "SGD",
         "total_discounts": "0.00",
         "total_line_items_price": "199.00",
         "order_reference": "#1002",
         "order_source": "web",
         "order_status": "open",
         "cancelled_at": null,
         "cancel_reason": null,
         "customer_id": 1244345,
         "discount_codes": [
         ],
         "line_items": [
         {
             "line_item_id": 1071823175,
             "product_id": 447654529,
             "variant_id": 12133424,
             "product_name": "IPod Touch 8GB",
             "quantity": 1,
             "price": "199.00",
             "sku": "IPOD2009BLACK",
             "variant_name": "Black",
             "brand": "Apple",
             "fulfillable_quantity": 1,
             "total_discount": "0.00"
         }],
         "refunds": [
         {
             "refund_id" : "1234",
             "created_at": "",
             "adjustment": "-3.50",
             "tax_adjustment": "-1.00",
             "quantity": "2"
         },
         {
             "refund_id" : "1235",
             "created_at": "",
             "adjustment": "-3.50",
             "tax_adjustment": "-1.00",
             "quantity": "2"
         }]
     }]
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
    	<td colspan="3" style="border-bottom:1px solid #ccc">Called when an order has been marked for deletion</td>
  	</tr>
  	<tr>
    	<th>Order Properties</th>
		<th colspan="2">Description</th>
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
    	<br/><br/>A Discount code will include the following fields:
    		<ul>
  				<li>amount: The amount of the discount. The type field determines the
unit of this amount</li>
  				<li>code: The discount code</li>
  				<li>type: The type of discount, valid values are:
    				<ul>
    					<li>fixed_amount: The default value. Applies a discount of amount as a unit of the store's currency. For example, if amount is 30 and the store's currency is USD, then 30 USD is deducted from the order total when the discount is applied</li>
    					<li>percentage: Applies a percentage discount of amount. For example, if amount is 30, then 30% of the order total will be deducted when the discount is applied</li>
    					<li>shipping: Applies a free shipping discount on orders that have a shipping rate less than or equal to amount. For example, if amount is 30, the discount will give the customer free shipping for any shipping rate that is less than or equal to $30</li>
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


## CUSTOMER API 


<table width="100%">
	<tr>
		<th>Customers</th>
		<th>GET</th>
		<th>/hooks/customer/{#customerid}</th>
	</tr>
	<tr>
		<td colspan="3">
			Retrieves a customer by the customer id
		</td>
	</tr>
	<tr>
		<td colspan="3" style="border-bottom:1px solid #ccc">
			<p><i>To see response returned by this API call, please look at the customer object in the customer update endpoint section of the documentation below.</i></p>
		</td>
	</tr>
	<tr>
		<th>Customers</th>
		<th>GET</th>
		<th>/hooks/customer/all</th>
	</tr>
	<tr>
		<td colspan="3">
			Retrieves all the customer records
		</td>
	</tr>
	<tr>
		<td>limit</td>
		<td colspan="2">Amount of results (default: 50) (maximum: 200)</td>
	</tr>
	<tr>
		<td>page</td>
		<td colspan="2">Page to show (default: 1)</td>
	</tr>
	<tr>
		<td colspan="3">
			<h3>List 200 customer records</h3>
			<p><i>GET /hooks/customer/all?limit=200</i></p>
		</td>
	</tr>
	<tr>
		<td colspan="3" style="border-bottom:1px solid #ccc">
			<p><i>To see response returned by this API call, please look at the customer object in the customer update endpoint section of the documentation below.</i></p>
		</td>
	</tr>
  	<tr>
    	<th>Customers</th>
    	<th>POST</th> 
    	<th>/hooks/customers/create</th>
  	</tr>
  	<tr>
    	<td colspan="3" style="border-bottom:1px solid #ccc">Called when a new customer account has been created</td>
  	</tr>
  	<tr>
    	<th>Customers</th>
    	<th>PUT</th> 
    	<th>/hooks/customers/update</th>
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
        	"id": 543213,
            "created_at":"2017-02-08T14:56:07-05:00",
            "updated_at": "2017-02-08T14:56:07-05:00",
            "accepts_marketing": true,
            "city": "Ottawa",
            "province": "Ontario",
            "country": "Canada,
            "status": "enabled"
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
    <td colspan="3" style="border-bottom:1px solid #ccc">Called when a customer account has been deleted</td>
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

## PRODUCTS API

<table  width="100%">
	<tr>
		<th>Product</th>
		<th>GET</th>
		<th>/hooks/product/{#productid}</th>
	</tr>
	<tr>
		<td colspan="3">Retrieves a product by the product id</td>
	</tr>
	<tr>
		<td colspan="3" style="border-bottom:1px solid #ccc">
			<p><i>To see response returned by this API call, please look at the product object in the product update endpoint section of the documentation below.</i></p>
		</td>
	</tr>
	<tr>
		<th>Product</th>
		<th>GET</th>
		<th>/hooks/product/all</th>
	</tr>
	<tr>
		<td colspan="3">Retrieves all product records.</td>
	</tr>
	<tr>
		<td>limit</td>
		<td colspan="2">Amount of results (default: 50) (maximum: 200)</td>
	</tr>
	<tr>
		<td>page</td>
		<td colspan="2">Page to show (default: 1)</td>
	</tr>
	<tr>
		<td colspan="3">
			<h3>List 200 product records</h3>
			<p><i>GET /hooks/product/all?limit=200</i></p>
		</td>
	</tr>
	<tr>
		<td colspan="3" style="border-bottom:1px solid #ccc">
			<p><i>To see response returned by this API call, please look at the product object in the product update endpoint section of the documentation below.</i></p>
		</td>
	</tr>
  	<tr>
    	<th>Product</th>
    	<th>POST</th> 
    	<th>/hooks/product/create</th>
  	</tr>
  	<tr>
    	<td colspan="3" style="border-bottom:1px solid #ccc">Called when a new product has been created</td>
  	</tr>
  	<tr>
    	<th>Product</th>
    	<th>PUT</th> 
    	<th>/hooks/product/update</th>
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
         "id": 3245465,
         "created_at": "2017-02-08T14:56:07-05:00",
         "updated_at": "2017-02-08T14:56:07-05:00",
         "product_name": "IPod Touch 8GB",
         "brand": "pple",
         "tags": "music, device, white",
         "path": "/product/android-phone/lenovo-a1000-hitam-/0889800170294",
         "image_url": "https://api.oktagon.co.id/core/images/product /ZTE%20Kis%203%20V8118w%20Biru.jpg",
         "variants":
         [
           {
             "id": "3245465-1",
             "name": "8G",
             "price": "128.00",
             "sku": "IPOD2008PINK",
             "inventory_quantity": null
           },
           {
             "id": "3245465-2",
             "name": "16G",
             "price": "128.00",
             "sku": "IPOD2008BLUE",
             "inventory_quantity": null
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
    	<th>Product</th>
    	<th>DELETE</th> 
    	<th>/hooks/product/{#productid}/delete</th>
  	</tr>
  	<tr>
    	<td colspan="3" style="border-bottom:1px solid #ccc">Called when a product has been deleted</td>
  	</tr>
	<tr>
		<th>Product Properties</th>
		<th colspan="2">Description</th>
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

## CATEGORY API

<table  width="100%">
	<tr>
		<th>Category</th>
		<th>GET</th>
		<th>/hooks/category/{#categoryid}</th>
	</tr>
	<tr>
		<td colspan="3">Retrieves a category by the category id</td>
	</tr>
	<tr>
		<td colspan="3" style="border-bottom:1px solid #ccc">
			<p><i>To see response returned by this API call, please look at the category object in the category update endpoint section of the documentation below.</i></p>
		</td>
	</tr>
	<tr>
		<th>Category</th>
		<th>GET</th>
		<th>/hooks/category/all</th>
	</tr>
	<tr>
		<td colspan="3">Retrieves all product records.</td>
	</tr>
	<tr>
		<td>limit</td>
		<td colspan="2">Amount of results (default: 50) (maximum: 200)</td>
	</tr>
	<tr>
		<td>page</td>
		<td colspan="2">Page to show (default: 1)</td>
	</tr>
	<tr>
		<td colspan="3">
			<h3>List 200 category records</h3>
			<p><i>GET /hooks/category/all?limit=200</i></p>
		</td>
	</tr>
	<tr>
		<td colspan="3" style="border-bottom:1px solid #ccc">
			<p><i>To see response returned by this API call, please look at the category object in the category update endpoint section of the documentation below.</i></p>
		</td>
	</tr>
  <tr>
    <th>Category</th>
    <th>POST</th> 
    <th>/hooks/category/create</th>
  </tr>
  <tr>
    <td colspan="3" style="border-bottom:1px solid #ccc">Called when a new category has been created</td>
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
        "id": 3245465,
        "created_at": "2017-02-08T14:56:07-05:00",
        "updated_at": "2017-02-08T14:56:07-05:00",
        "category_name": "Android Phones",
        "path": "/categories/android-phone/AN03",
        "parent_id": "2434435"
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
    <td colspan="3" style="border-bottom:1px solid #ccc">Called when a category has been deleted</td>
  </tr>
	<tr>
		<th>Category Properties</th>
		<th colspan="2">Description</th>
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


## CATEGORYMAP API


<table  width="100%">
	<tr>
		<th>CategoryMap</th>
		<th>GET</th>
		<th>/hooks/category/all/{#categoryid}</th>
	</tr>
	<tr>
		<td colspan="3">Retrieves all the products that are found in a category by the category id</td>
	</tr>
	<tr>
		<td>limit</td>
		<td colspan="2">Amount of results (default: 50) (maximum: 200)</td>
	</tr>
	<tr>
		<td>page</td>
		<td colspan="2">Page to show (default: 1)</td>
	</tr>
	<tr>
		<td colspan="3">
			<h3>List 200 category map records</h3>
			<p><i>GET /hooks/categorymap/all/43281?limit=200</i></p>
		</td>
	</tr>
	<tr>
		<td colspan="3" style="border-bottom:1px solid #ccc">
			<p><i>To see response returned by this API call, please look at the categorymap object in the categorymap update endpoint section of the documentation below.</i></p>
		</td>
	</tr>
	<tr>
		<th>CategoryMap</th>
		<th>GET</th>
		<th>/hooks/categorymap/count/{#categoryid}</th>
	</tr>
	<tr>
		<td colspan="3">Retrieves a count of all the products in a category by the category id</td>
	</tr>
	<tr>
		<td colspan="3">
		<pre>
		<code>
	{
		"count" : 20
	}
		</code>
		</pre>
		</td>
	</tr>
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
	    	"created_at": "2017-02-08T14:56:07-05:00",
	        "updated_at": "2017-02-08T14:56:07-05:00",
	        "category_id": 3424223,
	        "product_id": 4344542
	     }]
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
        	"category_id": 3424223,
            "product_id": 4344542
        }]
    }
    </code>
     </pre>
    </td>
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
