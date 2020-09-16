# Textarea

Basic textarea field.

```php
$form->textarea('text')
    ->translatable()
    ->title('Description')
    ->placeholder('Type something...')
    ->hint('The Description for some Object.')
    ->width(1/2);
```

## Methods

| Method                    | Description                                                                                                           |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `$field->title()`         | The title description for this field.                                                                                 |
| `$field->placeholder()`   | The placeholder for this field.                                                                                       |
| `$field->hint()`          | A short hint that should describe how to use the field.`                                                              |
| `$field->info()`          | Questionmark with tooltip. (Can contain longer field descriptions)                                                    |
| `$field->width()`         | Width of the field.                                                                                                   |
| `$field->translatable()`  | Should the field be translatable? For translatable crud models, the translatable fields are automatically recognized. |
| `$field->max()`           | Max characters.                                                                                                       |
| `$field->rows()`          | Number of rows.                                                                                                       |
| `$field->maxRows()`       | Maximum number of rows.                                                                                               |
| `$field->rules()`         | Rules that should be applied when **updating** and **creating**.                                                      |
| `$field->creationRules()` | Rules that should be applied when **creating**.                                                                       |
| `$field->updateRules()`   | Rules that should be applied when **updating**.                                                                       |
