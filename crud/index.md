# Index Config

## Introduction

Each Crud configuration can have an index page that shows an overview of the
models. This index page is configured in the `index` method of its corresponding
config.

```php
use Ignite\Crud\CrudIndex;

public function index(CrudIndex $page)
{
    // Build your index page here.
}
```

`Ignite\Crud\CrudIndex` is an instance of `Ignite\Page\Page` so all functions
described in the [Page](../basics/page.md) documentation can be used. This
includes binding Vue components or Blade views components to a page:

```php
$page->component('foo'); // Vue component
$page->view('foo'); // Blade view
```

In the Config for a CRUD model its index table is defined.

## Table

The index table is built using the method `table` like this:

```php
use Ignite\Crud\CrudIndex;

public function index(CrudIndex $page)
{
    $page->table(function($table) {
        $table->col('Name')->value('{first_name} {last_name}');
    });
}
```

The [table config](table.md) describes how columns with **images**,
**relations** and much more can be built.

## Eager Loading

Eager loadings can be performed in the `query` method. Here you can modify the
query that is executed when loading the index table items.

```php
use Ignite\Crud\CrudIndex;

public function index(CrudIndex $page)
{
    $page->table(...)
        ->query(function($query) {
            $query->with('department')->withCount('projects_count');
        });
}
```

## Search

All attributes to be searched for are specified in `search`. You can also
specify `relations` and their attributes.

```php
use Ignite\Crud\CrudIndex;

public function index(CrudIndex $page)
{
    $page->table(...)->search('title', 'department.name');
}
```

## Sort

You can sort by all model `attributes` as well as `relations` `attributes`. The
sortBy attributes are specified as follows: `{attributes}.{desc|asc}`. The
default attribute to sort by is specified in the `sortByDefault` method.

```php
use Ignite\Crud\CrudIndex;

public function index(CrudIndex $page)
{
    $page->table(...)->sortByDefault('id.desc');
}
```

In this example you can see how the array for the sort attributes can look like
to sort by `id` or by a `relation`.

```php
use Ignite\Crud\CrudIndex;

public function index(CrudIndex $page)
{
    $page->table(...)
        ->sortBy([
            'id.desc' => 'New first',
            'id.asc' => 'Old first',
            'department.name.desc' => 'Department A-Z',
            'department.name.asc' => 'Department Z-A'
        ]);
}
```

:::tip

In case of long loading times when sorted by relation attributes it can help to
add an [index](https://laravel.com/docs/7.x/migrations#indexes) on the column
that connects the relation.

:::

## Filter

Filters are specified in groups. Laravel's model
[`scopes`](https://laravel.com/docs/7.x/eloquent#local-scopes) are used to
filter the index table as shown in the example:

```php{lit/app/Config/PostConfig.php}
use Ignite\Crud\CrudIndex;

public function index(CrudIndex $page)
{
    $page->table(...)
        ->filter([
            'Department' => [
                'development' => 'Development',
                'marketing' => 'Marketing',
            ],
        ]);
}
```

```php{app/Models/Post.php}
public function scopeDevelopment()
{
    return $this->hasOne('App\Models\Department')->where('name', 'development');
}

public function scopeMarketing()
{
    return $this->hasOne('App\Models\Department')->where('name', 'marketing');
}
```

### Default Filter

You may also specify one or more default filter that will be used after the page
is loaded:

```php{lit/app/Config/PostConfig.php}
$page->table(...)
    ->defaultFilter('development')
    ->filter([
        'Department' => [
            'development' => 'Development',
            'marketing' => 'Marketing',
        ],
    ]);
```

### Custom Filter With Form Fields

To add filters with form fields like e.g. `checkboxes` or `date` pickers you can
create custom filters. A filter is generated via the artisan command
`lit:filter`:

```shell
php artisan lit:filter MyCustomFilter
```

The following class will the be generated:

```php{lit/app/Filters/MyCustomFilter.php}

namespace Lit\Filters;

use Ignite\Crud\Filter\Filter;
use Ignite\Crud\Filter\FilterForm;
use Ignite\Support\AttributeBag;
use Illuminate\Database\Eloquent\Builder;

class MyCustomFilter extends Filter
{
    /**
     * Apply field attributes to query.
     *
     * @param Builder      $query
     * @param AttributeBag $attributes
     * @var   void
     */
    public function apply($query, AttributeBag $attributes)
    {
        //
    }

    /**
     * Add filter form fields.
     *
     * @param  FilterForm $form
     * @return void
     */
    public function form(FilterForm $form)
    {
        //
    }
}
```

#### Add Form Fields

You can now add fields like this:

```php
public function form(FilterForm $form)
{
    $form->datetime('from');
    $form->datetime('to');
}
```

Or Checkboxes:

```php
use App\Models\Tag;

public function form(FilterForm $form)
{
    $options = Tag::all()->pluck('name')->toArray();

    $form->checkboxes('tags')->options($options);
}
```

#### Apply Field Attributes To The Query

The next thing you want to do is to apply the field attributes to the query:

```php
public function apply($query, AttributeBag $attributes)
{
    if ($attributes->from) {
        $query->where('created_at', '>=', $attributes->from);
    }

    if ($attributes->to) {
        $query->where('created_at', '<', $attributes->to);
    }
}
```

#### Use Custom Filter

The custom Filter can now be used for your CRUD index:

```php{lit/app/Config/PostConfig.php}
use Lit\Filters\MyCustomFilter;

$page->table(...)
    ->filter([
        'Button Text' => MyCustomFilter::class,
    ]);
```

## Pagination

The maximum number of items to be displayed on a page is defined in `perPage`.
The default is `10`.

```php
use Ignite\Crud\CrudIndex;

public function index(CrudIndex $page)
{
    $page->table(...)->perPage(5);
}
```
