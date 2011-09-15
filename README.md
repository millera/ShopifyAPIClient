# shopify.php

Lightweight PHP (JSON) client for the [Shopify API](http://api.shopify.com/).

## Getting Started

### Download
Download the [latest version of shopify.php](https://github.com/sandeepshetty/shopify.php/archives/master):

```shell
$ curl -L http://github.com/sandeepshetty/shopify.php/tarball/master | tar xvz
$ mv shopify.php-shopify.php-* shopify.php
```

### Configure
Open up the `shopify.php` file and edit the values of the constants `SHOPIFY_APP_API_KEY` and `SHOPIFY_APP_SHARED_SECRET` to your app's **API Key** and **Shared Secret** respectively.

### Require

```php
<?php
	require 'path/to/shopify.php/shopify.php';
?>
```

### Usage
Creating the app installation URL:

```php
<?php

	$url = shopify_app_install_url($shop_domain);

?>
```

Validate the installation when Shopify redirects the shop owner to your app's **Return URL** after installation:

```php
<?php

	if (!shopify_validate_app_installation($_GET['shop'], $_GET['t'], $_GET['timestamp'], $_GET['signature']))
	{
		// Guard Clause
	}

?>
```

Making API calls:

```php
<?php

	$shopify = shopify_api_client($shops_myshopify_domain, $shops_token);

	// Get all products
	$response = $shopify('GET', '/admin/products.json', array('published_status'=>'published'));


	// Create a new recurring charge
	$charge = array
	(
		"recurring_application_charge"=>array
		(
			"price"=>10.0,
			"name"=>"Super Duper Plan",
			"return_url"=>"http://super-duper.shopifyapps.com",
			"test"=>true
		)
	);

	$response = $shopify('POST', '/admin/recurring_application_charges.json', $charge);

?>
```

The response array looks like this:

```php
<?php

	array
	(
		'error'=> false, // Indicates CURL errors.
		'body'=> array(), // The Shopify API response as an associative array
		'status_message' => 'Created', // HTTP status message
		'status_code' => '201', // HTTP status code
		'headers' => array() // HTTP headers as an associative array with lowercase keys
	);

?>
```