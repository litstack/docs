# Radio

Radio inputs.

```php
$form->radio('type')
    ->title('Type')
    ->options([
        'article' => 'Article',
        'hint' => 'Hint',
    ]);
```

## Methods

| Method                    | Description                                                         |
| ------------------------- | ------------------------------------------------------------------- |
| `$field->title()`         | The title description for this field.                               |
| `$field->hint()`          | A short hint that should describe how to use the field.`            |
| `$field->info()`          | Questionmark with tooltip. (Can contain longer field descriptions)  |
| `$field->width()`         | Width of the field.                                                 |
| `$field->options()`       | An array with checkboxe values and descriptions.                    |
| `$field->stacked()`       | Places each radio one over the other instead of next to each other. |
| `$field->rules()`         | Rules that should be applied when **updating** and **creating**.    |
| `$field->creationRules()` | Rules that should be applied when **creating**.                     |
| `$field->updateRules()`   | Rules that should be applied when **updating**.                     |
