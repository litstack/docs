# Conditional Fields

## Introduction

Field **conditions** can be used to display fields when a model attribute has a
certain value. Individual fields or field groups can be made dependent on model
values.

![radio conditions](./screens/conditions/conditions_radio.gif 'radio conditions')

## Usage

The following example shows an input field that only gets displayed if model
attribute `type` has the value `news`:

```php
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

})->when('type', 'news');
```

### Multiple Conditions

You may chain as many conditions in a row by adding `or` to the desired
condition:

```php
$form->input('title')->title('Title')
    ->when($field, 'news')
    ->orWhen($field, 'blog');
```
