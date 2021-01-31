# Location

<iframe src="https://github.com/sponsors/litstack/card" title="Sponsor litstack" height="100" width="100%" style="border: 0;" class="github-sponsor"></iframe>

## Introduction

The location package ships with a google maps field that let's you benefit from
the extensive places api, to search for location's or pick one from the map.

![maps](./screens/maps.gif)

## Maps Field

The maps field let's the user pick location by searching for it or clicking
somewhere on the map.

The map field requires 2 parameters, the first one is the name of the latitude
attribute, the second parameter is the longitute attribute name.

```php
$form->map('lat', 'lng');
```

### Store Place Attributes

The maps field let's you store additional information about places. This can be
achieved by passing an array as a third parameter to the maps field, containting
the desired attributes and the database column in which the attribute should be
stored. In the following example the `formatted_address` would be stored in the
`street` column.

```php
$form->map('lat', 'lng', [
	'formatted_address' => 'street',
]);
```

The following attributes are available:

| Method              | Description                   |
| ------------------- | ----------------------------- |
| `formatted_address` | The formatted address name.   |
| `street_number`     | The location's street number. |
| `street_name`       | The location's street name.   |
| `state`             | The locaiton's state.         |
| `city`              | The location's city name.     |
| `country`           | The location's country.       |
| `postal_code`       | The location's postal code.   |
