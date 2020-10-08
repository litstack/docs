# Table

## Introduction

`tables` can be easily configured in the backend. You can easily display
attributes, relationships or include your own `Vue` components to adjust the
table as needed. The following explains how to customize the tables to your
needs.

## Column Value

### Text

Casual text columns are added with the function `col($label)`. Attached are all
methods for configuring the column.

```php
$table->col('Name');
```

The `value` of the column is indicated by the function value. You can specify
model attributes in curly brackets to include them in the text flow.

```php
$table->col('Name')->value('{first_name} {last_name}');
```

#### Display Relationship Attributes

It is also possible to display the attribute of relationships. In this case
attributes must be separated with a dot like this:

```php
$table->col('Product')->value('{product.name}');
```

#### Display Options

Maybe you want to display a value representative for a state, this can be
achived by passing the attribute name as the first and an array of options as
the second parameter to the value method:

```php
use App\Models\Product;

$table->col('State')->value('state', [
    Product::AVAILABLE    => 'available',
    Product::OUT_OF_STOCK => 'out of stock',
]);
```

### Text Alignment

You may set the text align to right like shown in the following examples:

```php
$table->col('amount')->value('{amount} €')->right();
$table->col('state')->value('{state}')->center();
```

### Strip Html

For example, if you want to display the value of a `wysiwyg` field, it makes
sense to strip the **html tags** and specify a maximum number of characters like
this:

```php
$table->col('Text')
    ->value('{text}')
    ->stripHtml()
    ->maxChars(50);
```

### Regex

Perform a regular expression search and replace:

```php
$table->col('Fruit')
    ->value('orange')
    ->regex('/\b(\w*orange\w*)\b/im', 'apple'); // Replaces orange with apple.
```

### Money

Let's assume that you want to display an amount of money in a column. For this
the `money` method with the desired attribute can be used. The amount is
formatted and the column can be sorted by the attribute.

```php
$table->money('amount');
```

#### Currency

For formatting the
[formatCurrnency](https://www.php.net/manual/de/numberformatter.formatcurrency.php)
method of the PHP
[NumberFormatter](https://www.php.net/manual/de/class.numberformatter.php) is
used. This makes it possible to choose the 3-letter ISO 4217 currency code. The
default currency code is `EUR`.

```php
$table->money('amount', 'USD');
```

Additionally, the language can be specified as a third parameter. However, the
locale of the authenticated user is used by default.

```php
$table->money('amount', 'USD', 'en_US');
```

Usefull **ISO 4217** codes:

| Code    | Currency          | Example (`de_DE`) |
| ------- | ----------------- | ----------------- |
| `"EUR"` | Euro              | `"10,00 €"`       |
| `"USD"` | US Dollar         | `"10,00 $"`       |
| `"AUD"` | Australian Dollar | `"10,00 AU$"`     |
| `"CHF"` | Swiss Franc       | `"10,00 CHF"`     |
| `"GGP"` | Pound             | `"10,00 £"`       |

## Reduce Width

With the function `small` the column is reduced to the minimum width.

```php
$table->col('Icon')->value('{icon}')->small();
```

## Sortable

A table column can be sorted directly by clicking on the column in the table
head. To achieve this, you simply have to specify the name of the attribute you
want to sort by.

```php
$table->col('Name')->value('{first_name} {last_name}')->sortBy('first_name');
```

You may even sort by related column.

```php
$table->col('Product')->value('{product.name}')->sortBy('product.name');
```

:::tip

In case of long loading times when sorted by relation attributes it can help to
add an [index](https://laravel.com/docs/7.x/migrations#indexes) on the column
that connects the relation.

:::

## Action

You may make [actions](actions.md) available directly in a table column:

```php
use Lit\Actions\SendMailAction;

$table->action('Send Mail', SendMailAction::class)->label('Mail');
```

### Multiple Actions

You can also place multiple actions in one column. These are then accessible via
a dropdown button

```php
use Lit\Actions\FooAction;
use Lit\Actions\BarAction;

$table->actions([
    'Foo' => FooAction::class,
    'Bar' => BarAction::class,
])->label('Actions');
```

## Image

If an image is to be displayed in a table, the image url must also be specified
using the `src` method. If the image was uploaded via the `Image` Field, the
conversions specified in the config file **lit.php** can be displayed.

```php
$table->image('Image')->src('{image.url}');
```

::: tip

Display **media conversion** for images that where uploaded via the `Image`
field.

Example: `$column->src('{image.conversion_urls.sm}')`

:::

### Avatar

You may use avatar to display a rounded image.

```php
$table->avatar('Image')->src('{image.conversion_urls.sm}');
```

### maxWidth, maxHeight

Furthermore, a `maxWidth` and a `maxHeight` can be defined.

```php
$table->image('Image')
    ->src('{image.conversion_urls.sm}')
    ->maxWidth('50px')
    ->maxHeight('50px');
```

### Square

Set a width and a height if the image should simply be displayed as a square:

```php
$table->image('Image')
    ->src('{image.conversion_urls.sm}')
    ->square('50px'); // Displays the image as 50 x 50 px square using object-fit: cover
```

## Progress

Often you want to display the progress of a model directly in a table column.
Litstack provides the progress column for this purpose:

```php
$table->progress('value', $max = 100)->label('Progress');
```

## Relation

In some cases, you may want to link a relation directly in your table. This can
be achieved by using the `relation` method. The related `name` of the relation
and `routePrefix` of the corresponding CRUD config must be specified.

```php
use Ignite\Support\Facades\Config;
use App\Models\Product;

$table->relation('Product')
    ->related('product') // Relation name.
    ->value('{name}') // Related attribute to be displayed.
    ->routePrefix($this->routePrefix())
    ->sortBy('product.name');
```

## Toggle

To edit the boolean state of a moel directly in a table, a **switch** can be
displayed in a column using `toggle`. The name of the corresponding attribute
must be specified as the first parameter. In addition, the `routePrefix` for the
update route must be specified, if the table is built in a CRUD or form config,
simply use the config function `routePrefix`.

```php
$table->toggle('active')
    ->label('Live')
    ->routePrefix($this->routePrefix())
    ->sortBy('active');
```

## Blade View

With the `view` method you can easily add Blad Views to your table column:

```php
$table->view('lit::columns.hello')->label('Hello');
```

```html{lit/resouces/views/columns/hello.blade.php}
<div class="badge badge-secondary">
	Hello World!
</div>
```

### Vue in Blade components

You can use Vue components in your blade component:

```html{lit/resouces/views/columns/hello.blade.php}
<b-badge>
	Hello World!
</b-badge>
```

Use the **prop** `item` to display model attributes:

```html{lit/resouces/views/columns/hello.blade.php}
<b-badge v-html="item.state" />
```

## Vue Component

You can also integrate your own Vue components into columns. The component
**name** is specified as the first parameter, the label must be specified
separately. Additionally props can be defined.

```php
$table->component('my-table-component')
    ->label('State')
    ->prop('variants' => [
        'active' => 'success',
        'complete' => 'primary',
        'failed' => 'danger'
    ])
    ->sortBy('state');
```

Your component could look like this:

```javascript
<template>
    <b-badge :variant="variants[item.state]">
        {{ item.state }}
    </b-badge>
</template>
<script>
export default {
    name: 'MyTableComponent',
    props: {
        item: {
            required: true,
            type: Object
        },
        vairants: {
            type: Array,
            required: true
        }
    }
}
</script>
```

Read the [Extend Vue](../basics/vue.md#bootstrap-vue) section to learn how to
register your own Vue components.
