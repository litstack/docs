# Forms

## Introduction

Forms provide a convenient way to store, organize and maintain data of many
kinds, such as your page content. You may create as many `Forms` as you like.

Think of a `form` as a model with **dynamic** attributes. Its structure may
change at any time. `Forms` are being managed on a crud-page just like
[crud models](model.md).

Forms are grouped into form `collections` to keep the overview. For example, the
forms **home** and **faq**, which contain the page content for the pages
**home** and **faq**, could be included in the `collection` **pages**.

## Generate Form

A `form` can be created using the following artisan command:

```shell
php artisan lit:form
```

A wizard will take you through all required steps. The corresponding `config`
and the `controller` is created afterwards.

A config file and a controller will be generated. Checkout the [model](model.md)
documentation to learn how to apply **permissions** on forms.

## Navigation

Add the navigation entry by adding the namespace of the newly generated config
to your navigation.

```php{lit/app/Config/NavigationConfig.php}
use Lit\Config\Pages\HomeConfig;

$nav->preset(HomeConfig::class, ['icon' => fa('home')]),
```

## Configuration

Define the form config in the created config file:
`Config/Form/{collection}/{form}Config.php`. First the controller must be
specified in the config:

```php
use Lit\Controllers\Form\Pages\HomeController;

/**
 * Controller class.
 *
 * @var string
 */
public $controller = HomeController::class;
```

### Form Fields

All form fields for the form are specified in the `show` method:

```php
public function show($page)
{
    $page->card(function($form) {
        $form->input('title');
    });
}
```

Read the [Crud Show](show.md) documentation to learn more on how to customize
your forms.

## Retrieve Data

In order to retrieve the form data, you have to add the **Form Facade** to your
controller. Data can now be easily retrieved with the `load` function like this:

```php
use Ignite\Support\Facades\Form;

$form = Form::load('pages', 'home');
```

This allows the data to be passed directly to a
[View](https://laravel.com/docs/7.x/blade#displaying-data).

```php
use Ignite\Support\Facades\Form;

return view('home')->with([
    'home' => Form::load('pages', 'home')
]);
```

and be used in a Blade template:

```php
<h1>{{ $home->title }}</h1>
```

### Load Using Namespace

If you want to load the form data of a single config, you can also use the
static load function of the config. This allows you to "click" directly into the
class in your editor.

```php
use Lit\Config\Form\Pages\Home;

$home = HomeConfig::load();

echo $home->title;
```

### Retrive A Full Collection

It is also possible to load all data for a collection as shown in the example:

```php
use Ignite\Support\Facades\Form;

$settings = Form::load('settings');

echo $settings->main->title;
```
