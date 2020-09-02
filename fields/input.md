# Input

A basic text input field.

```php
$form->input('title')
    ->title('Title')
    ->placeholder('Title')
    ->hint('The Title is shown at the top of the page.')
    ->width(1/2);
```

## Prepend & Append

For units or similar, values can be appended or prepended to the input field.

```php
$form->input('length')
    ->title('Length')
    ->type('number')
    ->placeholder('The length in cm')
    ->hint('Enter the length in cm.')
    ->append('cm')
    ->width(12);
```

## Methods

| Method                    | Description                                                                                                                                                                                                                                    |
| ------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$field->title()`         | The title description for this form field.                                                                                                                                                                                                     |
| `$field->placeholder()`   | The placeholder for this form field.                                                                                                                                                                                                           |
| `$field->type()`          | Sets an input type for the input. [Available types](https://bootstrap-vue.js.org/docs/components/form-input#input-type): **text**, **number**, **email**, **password**, **search**, **url**, **tel**, **date**, **time**, **range**, **color** |
| `$field->hint()`          | A short hint that should describe how to use the form field.                                                                                                                                                                                   |
| `$field->width()`         | Width of the field.                                                                                                                                                                                                                            |
| `$field->translatable()`  | Should the form field be translatable? For translatable crud models, the translatable fields are automatically recognized.                                                                                                                     |
| `$field->max()`           | Max characters.                                                                                                                                                                                                                                |
| `$field->prepend()`       | Prepend the field.                                                                                                                                                                                                                             |
| `$field->append()`        | Append the field.                                                                                                                                                                                                                              |
| `$field->rules()`         | Rules that should be applied when **updating** and **creating**.                                                                                                                                                                               |
| `$field->creationRules()` | Rules that should be applied when **creating**.                                                                                                                                                                                                |
| `$field->updateRules()`   | Rules that should be applied when **updating**.                                                                                                                                                                                                |
