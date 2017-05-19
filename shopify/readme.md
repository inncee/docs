# Installation and Setup For Your Shopify Store

This guide will help walk you through installing the Inncee application on your Shopify Store.

## 1. Installing the Inncee App in Shopify
The Inncee app allows us to connect to your store's system, allowing us to be automatically notified when you make update your store catalog or receive orders.

### 1.1 Create Account
Create your Inncee account at https://beta.inncee.com. Be sure to select "Shopify" as your platform in the dropdown list.

### 1.2 Click on the "Shopify" button to begin the app installation
Once you've completed registering for your Inncee account, a pop-up will appear asking you to "Connect your Store". Click on the Shopify icon in the modal to begin the installation.

### 1.3 Click on "Approve" to approve the permissions Inncee is requesting for on your Shopify store page.
After you've clicked on the button to begin the installation, you may be asked to log into your Shopify store if you are not logged in. After you're logged in, you'll be directed to a permissions page that shows you all the permissions Inncee requires to analyze your store's performance. Click on the "Approve" button to complete the app installation.

Once successful, you'll be directed to the next page with further instructions on how to install the Inncee tracking library.

## 2. Installing the Inncee tracker into your Shopify store.
The Inncee tracking library helps us to track the browsing behaviour of your visitors and the specific actions they're taking on your online store. This allows us to measure whether they are engaging well with your store's content. 

The tracking script library must be installed after the opening of the <head> tag found in every
page of your website. The page that you're on after you've successfully installed the Inncee app will display the tracking script that you need to install. If you are no longer able to access that page, you can also find your tracking script by logging in to your Inncee dashboard and navigating to the Settings > Integrations dashboard.

`<script type=”text/javascript”
src=”https://shopify.inncee.com/tracker/YOUR_ACCOUNT_ID?shop=YOUR_STORE_URL”></script>`

Once the script has been installed, we’ll be able to start recording events on your website.

### 2.1 Log in to your shopify store’s admin panel
### 2.2 Click on the “Online Store” > “Themes” in your main menu
### 2.3 Click on the Three Dots icon to open up the menu, and click on the “Edit HTML/CSS” link

	![Tracking Image](https://shopify.inncee.com/images/script-edit-html-1.png)

### 2.4 Select the “theme.liquid” file under your “Layout” folder to load it up on your screen. In the “theme.liquid” file, you should be able see the <head> tag near the top of the file

	![Tracking Image 2](https://shopify.inncee.com/images/script-edit-html-2.png)

### 2.5 Paste the your script file codes after the the <title> tags (you should find them a couple of lines down from the <head> tag) Please make sure that you code is pasted only after the closing </title> tag.

	![Tracking Image 3](https://shopify.inncee.com/images/script-edit-html-3.png)

### 2.6 Click the “Save” button and you’re done!
