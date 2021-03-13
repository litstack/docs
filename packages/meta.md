# Location

## Introduction

A package to easily add meta for crud models and forms to your page.

## Installation

The package can be installed via composer and will autoregister.

```bash
composer require litstack/meta
```

You can now publish and migrate the migration for your meta model:

```shell
php artisan vendor:publish --provider="Litstack\Meta\MetaServiceProvider" --tag=migrations
php artisan migrate
```

## Usage

Start by perparing your Crud-Model by using the `HasMeta` Trait and implement
the `metaable` Contract:

```php
use Litstack\Meta\Metaable;
use Litstack\Meta\Traits\HasMeta;

class Post extends Model implements Metaable
{
    use HasMeta;
}
```

> Forms don't need further setup

In order to display the form in litstack edit your model-config:

```php
use Litstack\Meta\Traits\CrudHasMeta;

class PostConfig extends CrudConfig
{
    use CrudHasMeta;

    // …

    public function show(CrudShow $page)
    {
        $this->meta($page);
    }
}
```

You may disable certain fields in the meta modal:

```php
$this->meta($page, disable: [
    'description',
    'keywords'
]);
```

To display the meta-fields in your template, simply use the `<x-lit-meta />`
component and pass it the `metaFields` of your model.

```php
<head>
    ...
    <x-lit-meta :for="$post" />
    ...
</head>
```

## Default Values / Customizing / Overriding

If you want to use meta attributes directly from model attributes you can
specify them in `metaAttributes` in your config. You may as well override the
meta methods like `metaAuthor` to return dynamic meta attributes:

```php
class Post extends Model implements Metaable
{
    use HasMeta;

    protected $metaAttributes = [
        'author' => 'author.name',
        'image'  => 'header_image',
    ];

    public function getHeaderImageAttribute()
    {
        // ...
    }

    public function metaTitle(): ?string
    {
        // Return a prefix:
        return "Awesome Blog: " . parent::metaTitle();
    }
}
```

You may set default attributes by setting `defaultMetaAttributes` or add a
method `defaultMeta...` method:

```php
class Post extends Model implements Metaable
{
    use HasMeta;

    protected $defaultMetaAttribute = [
        'description' => 'description',
    ];

    public function defaultMetaTitle()
    {

    }
}
```

The same works for forms:

```php
class HomeConfig extends FormConfig
{
    // ...

    protected $metaAttributes = [
        'title' => 'foo',
    ];

    public function show(CrudShow $page)
    {
        $page->card(function($form) {
            $form->input('foo');
        });
        $this->meta($page);
    }
}
```
