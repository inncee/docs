# Installation and Setup For Your Shopify Store

This guide will help walk you through installing the Inncee application on your Shopify Store.

## 1. Installing the Inncee App in Shopify
The Inncee app allows us to connect to your store's system, allowing us to be automatically notified when you make update your store catalog or receive orders.

### 1.1 Create Account
Create your Inncee account at <https://beta.inncee.com/auth/register>. Be sure to select "Shopify" as your platform in step 2 of the registration process.

![](https://beta.inncee.com/images/screenshots/registration-step-1.png)

### 1.2 Enter your myshopify domain to begin installing the app
In step 3 of the registration process, you'll be prompted to enter your store's myshopify domain. Once you've filled in your domain, click on the "Begin installation" button and you'll be directed back to your Shopify store where you'll need to approve our app install request.

![](https://beta.inncee.com/images/screenshots/registration-step-3.png)

### 1.3 Approve the permissions Inncee is requesting on your Shopify store

Login to your Shopify store and you'll be navigated to a page in your store that shows our app requesting for a few READ permissions. You'll need to approve our permission request in order to continue.

![App install successful](https://shopify.inncee.com/images/inncee-shopify-install-success.png)

### 1.4 Install your Inncee tracking script in your theme files

Once you've finished approving the permissions, you'll be redirected back to your Inncee dashboard and there will be just 1 final step to complete the installation. Embedding the javascript tracker in your Shopify theme files. See the next section for the detailed steps.

## 2. Installing the Inncee tracker into your Shopify store.
The Inncee tracking library helps us to track the browsing behaviour of your visitors and the specific actions they're taking on your online store. This allows us to measure whether they are engaging well with your store's content. 

The tracking script library must be installed after the opening of the <head> tag found in every
page of your website. The page that you're on after you've successfully installed the Inncee app will display the tracking script that you need to install. If you are no longer able to access that page, you can also find your tracking script by logging in to your Inncee dashboard and navigating to the Settings > Integrations dashboard.

```html
<script type="text/javascript"
src="https://api.inncee.com/scripts/shopify/tracker/YOUR_ACCOUNT_ID"></script>
```

Once the script has been installed, we’ll be able to start recording events on your website.

### 2.1 Log in to your Shopify store
You'll need to login to your Shopify store and access the admin panel.

### 2.2 Navigate to your Shopify theme
Click on the “Online Store” > “Themes” in your main menu to get to your Theme page.

### 2.3 Open up your theme editor
Click on the Actions button to open up the menu, and click on the “Edit code” link

![Shopify Themes Page](https://shopify.inncee.com/images/script-edit-html-5.png)

### 2.4 Select your theme file
Select the “theme.liquid” file under your “Layout” folder to load it up on your screen. In the “theme.liquid” file, you should be able see the <head> tag near the top of the file

![Shopify theme.liquid file](https://shopify.inncee.com/images/script-edit-html-6.png)

### 2.5 Paste your tracking library script in the theme file
Paste the your script file codes after the the <title> tags (you should find them a couple of lines down from the <head> tag) Please make sure that you code is pasted only after the closing </title> tag.

![Pasting the script into the theme file](https://shopify.inncee.com/images/script-edit-html-7.png)

### 2.6 Click "Save"
Click the “Save” button and you’re done!
