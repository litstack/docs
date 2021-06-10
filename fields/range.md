# Range

## Introduction

A range slider.

![Range slider](./screens/range/example.png 'Range slide')

## Example

```php
 $form->range('range')
    ->title('Range')
    ->min(1)
    ->max(4)
    ->step(1)
    ->hint('Range.')
    ->width(1/2);
```

## Methods

| Method                    | Description                                                        |
| ------------------------- | ------------------------------------------------------------------ |
| `$field->title()`         | The title description for this field.                              |
| `$field->hint()`          | A short hint that should describe how to use the field.`           |
| `$field->info()`          | Question mark with tooltip. (Can contain longer field descriptions) |
| `$field->width()`         | Width of the field.                                                |
| `$field->min()`           | Minimum value.                                                     |
| `$field->max()`           | Maximum value.                                                     |
| `$field->step()`          | Steps. (default: 1)                                                |
| `$field->rules()`         | Rules that should be applied when **updating** and **creating**.   |
| `$field->creationRules()` | Rules that should be applied when **creating**.                    |
| `$field->updateRules()`   | Rules that should be applied when **updating**.                    |
