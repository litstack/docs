# Meta

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

Start by preparing your Model by using the `HasMeta` Trait and implement the
`metaable` Contract:

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
