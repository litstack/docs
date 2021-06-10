# Field Masks

## Introduction

Apply input masks to your field to indicate the format of valid input values.
Litstack makes use of [Cleave.js](https://nosir.github.io/cleave.js/) to apply
masks to form fields.

## Basic Usage

To add masks to your form field you may specify the desired mask options by
passing an array to the `mask` method of your field:

```php
$form->input('credit_card')->mask([
	'creditCard': true
]);
```

Checkout some useful masks on
[nosir.github.io/cleave.js](https://nosir.github.io/cleave.js/).

## Add Addons

To make use of cleave js addons like phone number masks of your country, you
need to add the script of the desired addon to your `lit` config so it will be
loaded.

```php{config/lit.php}
	'assets' => [
		// ...
        'scripts' => [
			// ...
			// Add us phone number.
			'https://cdnjs.cloudflare.com/ajax/libs/cleave.js/1.6.0/addons/cleave-phone.us.js'
		],
	]
```

The US phone number mask can now be applied like this:

```php
$form->input('phone_number')->mask([
	'phone'           => true,
	'phoneRegionCode' => 'us',
]);
```

## Conditional Options

You may use attribute values to add conditional options by adding the attribute
in curly brackets:

```php
$form->select('country')->options([
	'de' => 'de',
	'us' => 'us',
]);

$form->input('title')->mask([
	'phone'           => true,
	'phoneRegionCode' => '{country}',
]);
```
