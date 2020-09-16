# WYSIWYG

A **W**hat-**Y**ou-**S**ee-**I**s-**W**hat-You-**G**et editor using
[tiptap](https://github.com/scrumpy/tiptap).

```php
$form->wysiwyg('text')
    ->translatable()
    ->colors([
        '#4951f2', '#f67693', '#f6ed76', '#9ff2ae', '#83c2ff'
    ])
    ->title('Description')
    ->hint('What you see is what you get field.');
```

## Methods

| Method                     | Description                                                                                                           |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `$field->title()`          | The title description for this field.                                                                                 |
| `$field->hint()`           | A short hint that should describe how to use the field.`                                                              |
| `$field->info()`           | Questionmark with tooltip. (Can contain longer field descriptions)                                                    |
| `$field->width()`          | Width of the field.                                                                                                   |
| `$field->translatable()`   | Should the field be translatable? For translatable crud models, the translatable fields are automatically recognized. |
| `$field->max()`            | Max characters.                                                                                                       |
| `$field->colors()`         | Array of colors the the text can be painted in.                                                                       |
| `$field->tableHasHeader()` | Determines if wysiwyg tables should have an header.                                                                   |
| `$field->rules()`          | Rules that should be applied when **updating** and **creating**.                                                      |
| `$field->creationRules()`  | Rules that should be applied when **creating**.                                                                       |
| `$field->updateRules()`    | Rules that should be applied when **updating**.                                                                       |
