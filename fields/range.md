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

| Method                              | Description                                                      |
| ----------------------------------- | ---------------------------------------------------------------- |
| `$field->title('Foo')`              | The title description for this field.                            |
| `$field->hint('Foo.')`              | A short hint that should describe how to use the field.`         |
| `$field->width(1/2)`                | Width of the field.                                              |
| `$field->min(1)`                    | Minimum value.                                                   |
| `$field->max(10)`                   | Maximum value.                                                   |
| `$field->step(2)`                   | Steps. (default: 1)                                              |
| `$field->rules('required')`         | Rules that should be applied when **updating** and **creating**. |
| `$field->creationRules('required')` | Rules that should be applied when **creating**.                  |
| `$field->updateRules('required')`   | Rules that should be applied when **updating**.                  |
