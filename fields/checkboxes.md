# Checkboxes

## Introduction

Multiple checkboxes. The selected values are stored as an array.

## Usage

The following example shows a list of fruits that can be selected using
checkboxes.

```php
$form->checkboxes('fruits')
    ->title('Fruits')
    ->options([
        'orange' => 'Orange',
        'apple' => 'Apple',
        'pineapple' => 'Pineapple',
        'grape' => 'Grape',
    ])
    ->hint('Select your fruits.')
    ->width(6);
```

### Cast

Add the `array`
[cast](https://laravel.com/docs/5.2/eloquent-mutators#attribute-casting) to your
model:

```php
protected $casts = [
    'fruits' => 'array'
];
```

## Methods

| Method                              | Description                                                            |
| ----------------------------------- | ---------------------------------------------------------------------- |
| `$field->title('Foo')`              | The title description for this field.                                  |
| `$field->hint('Foo.')`              | A short hint that should describe how to use the field.`               |
| `$field->info('...')`               | Question mark with tooltip. (Can contain longer field descriptions)     |
| `$field->width(1/2)`                | Width of the field.                                                    |
| `$field->options(['foo', 'bar'])`   | An array with checkbox values and descriptions.                       |
| `$field->stacked()`                 | Places each checkbox one over the other instead of next to each other. |
| `$field->rules('required')`         | Rules that should be applied when **updating** and **creating**.       |
| `$field->creationRules('required')` | Rules that should be applied when **creating**.                        |
| `$field->updateRules('required')`   | Rules that should be applied when **updating**.                        |
