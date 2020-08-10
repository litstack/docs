# Conditional Fields

## Introduction

If fields should depend on the value of other fields, field conditions can be
used.

![radio conditions](./screens/conditions/conditions_radio.gif 'radio conditions')

## Examples

The following example shows an input field that only gets displayed if the
select field `type` has the value `news`:

```php{10}
$form->select('type')
    ->options([
        'news' => 'News',
        'blog' => 'Blog',
    ])
    ->title('Type');

$form->input('news_title')
    ->title('Title')
    ->when('type, 'news');
```

A varity of conditions are available for any field:

| Condition                                | Description                                 |
| ---------------------------------------- | ------------------------------------------- |
| `$field->when('type', 'foo')`            | Matches the exact given value.              |
| `$field->whenNot('type', 'foo')`         | When field doesnt match the given value.    |
| `$field->whenContains('type', 'foo')`    | Matches the given substring or array value. |
| `$field->whenNotContains('type', 'foo')` | When field doesnt contain the given value.  |

### Conditional Groups

Conditions can also be applied to groups:

```php{4}
$form->group(function($form) {

    $form->input('news_title')->title('Title');
    $form->input('news_text')->title('Text');

})->when($field, 'news');
```

### Multiple Conditions

You may chain as many conditions in a row by adding `or` to the desired
condition:

```php
$form->input('title')->title('Title')
    ->when($field, 'news')
    ->orWhen($field, 'blog');
```
