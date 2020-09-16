# Select

A select field.

```php
$form->select('article_type')
    ->title('Type')
    ->options([
        1 => 'News',
        2 => 'Info',
    ])
    ->hint('Select the article type.');
```

## BelongsTo

A select can be used for `belongsTo` relations.

```php
$form->select('article_id')
    ->title('Article')
    ->options(
        Article::all()->mapWithKeys(function($item, $key){
            return [$item->id => $item->title];
        })->toArray()
    )
    ->hint('Select Article.');
```

## Methods

| Methods                   | Description                                                            |
| ------------------------- | ---------------------------------------------------------------------- |
| `$field->title()`         | The title description for this field.                                  |
| `$field->options()`       | An array with the options, can be a simple array or key => value pairs |
| `$field->hint()`          | A short hint that should describe how to use the field.`               |
| `$field->info()`          | Questionmark with tooltip. (Can contain longer field descriptions)     |
| `$field->width()`         | Width of the field.                                                    |
| `$field->rules()`         | Rules that should be applied when **updating** and **creating**.       |
| `$field->creationRules()` | Rules that should be applied when **creating**.                        |
| `$field->updateRules()`   | Rules that should be applied when **updating**.                        |
