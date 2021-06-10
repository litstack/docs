# Show Config

## Introduction

In the `show` method of a **Form** or a **CRUD** config all components and
fields are configured for editing the data.

```php
use Ignite\Crud\CrudShow;

public function show(CrudShow $page)
{
    // Define the page here.
}
```

`Ignite\Crud\CrudShow` is an instance of `Ignite\Page\Page` so all functions
described in the [Page](../basics/page.md) documentation can be used. This
includes binding Vue components or Blade views to a page:

```php
$page->component('foo'); // Vue component
$page->view('foo'); // Blade view
```

## Card

Form fields must be grouped into `cards`. You are free to create only one or any
number of cards for a form.

```php
use Ignite\Crud\CrudShow;

public function show(CrudShow $page)
{
    $page->card(function ($form) {

        // Build your form in here.

        $form->input('first_name')->title('First Name');

    });
}
```

All available fields can be found in the documentation under
[Fields](../fields/introduction.md).

### Customize Card

You may customize your card with the following options:

| Method                | Description                                                         |
| --------------------- | ------------------------------------------------------------------- |
| `$card->title('Foo')` | The title description for the card.                                 |
| `$card->width(1/2)`   | Width of the card.                                                  |
| `$card->secondary()`  | Gives your card a secondary background, for less important content |

## Group

With `group` fields can be grouped in a column. This is useful to organize form
elements of different heights side by side.

```php
$form->group(function($form) {
    // Build your form inside the col.
})->width(6);
```

## Info

A good content administration interface includes **descriptions** that help the
user to quickly understand what is happening on the interface. Such information
can be created outside and inside of cards like this:

```php
use Ignite\Crud\CrudShow;

public function show(CrudShow $page)
{
    $page->info('Address')
        ->width(4)
        ->text('This address appears on your <a href="'.route('invoices').'">invoices</a>.')
        ->text(...);

    // ...
}
```

## Preview

![preview](./screens/preview.gif 'preview')

You may want to provide a direct **preview** of the crud's associated page. This
can be done using the `preview` method in your crud or form like this:

```php
use Ignite\Crud\CrudShow;

public function show(CrudShow $page)
{
    $page->preview(function($post = null, $locale) {
        return route('posts.show', [
            'slug' => $post->slug ?? ''
        ]);
    });
}
```

You may even provide a route for every language, so the user gets a preview for
all languages:

```php
$page->preview(function($post = null, $locale) {
    return route($locale.'.posts.show', [
        'slug' => $post->translate($locale)->slug ?? ''
    ]);
});
```

### Default Device

The default device can be changed in the config `lit.php` under
'crud.preview.default_device'.

```php
'crud' => [
    'preview' => [
        // Available devices: mobile, tablet, desktop
        'default_device' => 'desktop'
    ]
],
```
