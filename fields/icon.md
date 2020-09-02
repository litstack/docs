# Icon

An icon picker field.

```php
$form->icon('icon')
    ->title('Icon')
    ->icons([
        '<i class="fas fa-address-book"></i>',
        '<i class="fas fa-address-card"></i>',
        // ...
    ])
    ->hint('Choose your icon.')
    ->width(2);
```

If the number of icons is high, it is recommended to `require` the icons from a
file.

```php
->icon(require('.../icons.php'));
```

## Add Your Own Icons

To import your own icons you have to specify the corresponding `css` file in the
config `lit.php`.

```php
'assets' => [
    // ...
    'css' => [
        '/icons/icons.css', // Add your icon's css file.
        // ...
    ],
],
```

## Methods

| Method                                       | Description                                                                 |
| -------------------------------------------- | --------------------------------------------------------------------------- |
| `$field->title('Foo')`                       | The title description for this field.                                       |
| `$field->hint('Foo.')`                       | A short hint that should describe how to use the field.                     |
| `$field->width(1/2)`                         | Width of the field.                                                         |
| `$field->icons(['<i class="my-icon"></i>'])` | List of selectable icons. (By default all fontawesome icons are selectable) |
| `$field->rules('required')`                  | Rules that should be applied when **updating** and **creating**.            |
| `$field->creationRules('required')`          | Rules that should be applied when **creating**.                             |
| `$field->updateRules('required')`            | Rules that should be applied when **updating**.                             |
