# Models

## Introduction

The main task of an admin panel is to manage data. Litstack offers easy editing
and managing of
[Laravel Eloquent Models](https://laravel.com/docs/7.x/eloquent). It lets you
attach a variety of **form fields** to model attributes. There are **relation
fields** that to link any available models in any laravel relation, and the
ability to attach fields to attributes from pivot tables.

The administrative views of a model include two views. An index view with a
table and a detail view in which information for a single model can be displayed
or edited. These are configured in litstack in the so-called configs. The
`./lit/app/Config/Crud` directory contains all configuration files for your
CRUD-Models. Each model may have one or more config giles.

::: tip

Configurations can be created for existing Models.

:::

Litstack also uses the following Open-Source packages to extend the
functionality of Models:

-   [Astronomic Translatable](https://docs.astrotomic.info/laravel-translatable/)
    Used for Model translations.
-   [Spatie Medialibrary](https://docs.spatie.be/laravel-medialibrary/v8/introduction/)
    Used for media management.
-   [Cviebrock Sluggable](https://github.com/cviebrock/eloquent-sluggable) Used
    to apply slugs to model attributes.

## Generate Config

Assuming you want to generate a crud-config for the Model with the name `Post`,
you can simply generate a crud-config for the model using the `lit:crud`
command.

```shell
php artisan lit:crud Post
```

Executing the command will create all necessary files, unless they already
exist:

-   The migration for the `posts` Table
-   The model `App\Models\Post`
-   The config `./lit/app/Config/Crud/PostConfig`
-   The controller `./lit/app/Http/Controllers/Crud/PostController`

Now the config can be added to the navigation so it can be reached via your
admin panel.

```php{lit/app/Config/NavigationConfig.php}
use Lit\Config\Crud\PostConfig;

$nav->preset(PostConfig::class, ['icon' => fa('newspaper')]),
```

### Config Name

Since you are able to create **multiple** configurations for the same model, in
some cases you may want to use a config name that does not begin with the class
name of the model. The name of the config can be given as the second argument to
the `lit:crud` command:

```shell
php artisan lit:crud User StargazerConfig
```

## Media

If your model should have media like pictures or pdfs, it needs to implement the
`Spatie\MediaLibrary\HasMedia` **interface** and use the **trait**
`Ignite\Crud\Models\Traits\HasMedia`, as shown in the following example:

```php{app/Models/Post.php}
namespace App\Models;

use Ignite\Crud\Models\Traits\HasMedia;
use Illuminate\Database\Eloquent\Model;
use Spatie\MediaLibrary\HasMedia as HasMediaContract;

class Test extends Model implements HasMediaContract
{
    use HasMedia;

    // ...
}
```

Litstack generates the a Model with the needed requirements by adding the
`--media` (or short `-m`) options to the `lit:crud` command:

```shell
php artisan lit:crud Post -m
```

Now you can add images to your model via the [image field](../fields/image.md).

## Translatable

The litstack form fields make managing translatable content easier than ever.
Even setting up translatable models is easy.

## Sluggable

Models that should have one or more slugs need the trait
`Ignite\Crud\Models\Traits\Sluggable`. The trait is automatically added to the
mode when it is created by the command `lit:crud` with the option `--slug`
(short `-s`):

```shell
php artisan lit:crud Post -s
```

The `sluggable` method of the model specifies which attributes contain slugs and
from which attributes the slugs are built. In the following example the column
`slug` contains the slug that is built from the value of `title`.

```php
namespace App\Models;

use Ignite\Crud\Models\Traits\Sluggable;
use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    use Sluggable;

    protected $fillable = ['title'];

    public function sluggable()
    {
        return [
            'slug' => [
                'source' => 'title',
            ],
        ];
    }
}
```

::: tip

Read more about the sluggable options in the
[package documentation](https://github.com/cviebrock/eloquent-sluggable).

:::

::: warning

Make sure the **Sluggable** trait and configuration are in the correct Model
when using **translatable** Models.

:::

## Migrate

Edit the newly created migration and add all table fields you need. For the
translation of models
[laravel-translatable](https://docs.astrotomic.info/laravel-translatable/installation#migrations)
from `astronomic` is used. Pay attention to translatable and non-translatable
fields.

In the migration all **permissions** for the corresponding model are created in
the `permissions` array. It's possible that the permissions are used by another
model. For example, it makes sense not to give extra permissions for
`article_states` but to use the permissions from `articles`. In this case the
array can simply be left empty.

```php
class CreateArticlesTable extends Migration
{
    ...

    /**
     * Permissions that should be created for this crud.
     *
     * @var array
     */
    protected $permissions = [
        'create articles',
        'read articles',
        'update articles',
        'delete articles',
    ];

    ...
}
```

After all fields and permissions have been defined, the migration can be
executed.

```shell
php artisan migrate
```

## Controller (authorization)

A controller has been created in which the authorization for all operations is
specified. Operations can be `create`, `read`, `update` and `delete`.

```php
public function authorize(User $user, string $operation): bool
{
    return $user->can("{$operation} articles");
}
```

### Authorize individual Models

You may want to authorize individual models. This can be achieved by adding the
initial query in the `query` method. The following example shows how models can
be hidden by users who did not create them:

```php
public function query()
{
    return $this->model::where('created_by', lit_user()->id);
}
```

## Navigation

Add the navigation entry by adding the `crud.{table_name}` preset to your
navigation.

```php
$nav->preset('crud.articles', [
    'icon' => fa('newspaper')
]),
```

## Configuration

Define the CRUD-Config in the created config file:
`Config/Crud/{model}Config.php`. First the model and the controller must be
specified in the config:

```php
use App\Models\Article;
use Lit\Controllers\Crud\ArticleController;
use Ignite\Crud\Config\CrudConfig;

class ArticleConfig extends CrudConfig
{
    /**
     * Model class.
     *
     * @var string
     */
    public $model = Article::class;

    /**
     * Controller class.
     *
     * @var string
     */
    public $controller = ArticleController::class;
}
```

### Index Table, Create & Update Form

Next, the configuration for the [Index Table](index.md) and the **create** and
**update** [Form](show.md) can be adjusted.
