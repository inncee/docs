# docs
Inncee API documentation

* [1.1 Create Account](#11-create-account)
* [1.2 Install your tracking script](#12-install-your-tracking-script)

This guide will be able to help walk you through on how to get started with installing Inncee onto your custom website. 

## 1.1 Create Account 

Create an account at https://beta.inncee.com. 

This allows our system to generate  - a unique account ID - a tracking script code to install on your store - and a secret key to allow you to authenticate the JSON data that you can send to our API endpoints.  Once generated, all this information will be sent to you via email. Note: Ignore the pop-up that asks you to connect your store to Shopify. 


## 1.2 Install your tracking script 

You will need to install our tracking script library after the opening of the <head> tag found in every page of your website. 

`<script type=”text/javascript” src=”https://shopify.inncee.com/tracker/YOUR_ACCOUNT_ID?shop=YOUR_STORE_URL”></script>`

## 1.3 Tracking Events 

For our platform to be able to analyze your data and look for information, we need to be able to record events that happened on your website. 

Some events are automatically tracked while others requires you to install code snippets manually or calling our webhook end points in our app to send data to us.
