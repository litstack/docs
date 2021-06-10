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

The litstack form fields make managing translatable content easier than ever:

```php
$form->input('title')->translatable();
```

To use translatable fields in models the correct configuration is required,
which is done from within the **migration** and the **model**, as described
below.

::: tip

The required configuration is generated automatically by adding the
`--translatable` (or short `-t`) option to the `lit:crud` command.

:::

### Migration

In the migration a second table must be created, which contains the translatable
fields. For `posts` this would be `post_translations`. All required **columns**
can be added to the translations table by using `translates` with the name of
the table to be translated. In the following example the column title is
translatable for the posts table.

```php{database/migrations/*create_posts_table.php}
Schema::create('posts', function (Blueprint $table) {
    $table->bigIncrements('id');
    $table->timestamps();
});
Schema::create('post_translations', function (Blueprint $table) {
    $table->translates('posts');
    $table->string('title');
});
```

### Model

Your translation model, in our case post, needs the **interface**
`Astrotomic\Translatable\Contracts\Translatable` and the **trait**
`Ignite\Crud\Models\Traits\Translatable`. Furthermore, all translatable
attributes are specified in the `translatedAttributes` property.

```php{app/Models/Post.php}
<?php

namespace App\Models;

use Astrotomic\Translatable\Contracts\Translatable as TranslatableContract;
use Ignite\Crud\Models\Traits\Translatable;
use Illuminate\Database\Eloquent\Model;

class Post extends Model implements TranslatableContract
{
    use Translatable;

    /**
     * The attributes to be translated.
     *
     * @var array
     */
    public $translatedAttributes = ['title'];

    /**
     * The relationships that should always be loaded.
     *
     * @var array
     */
    protected $with = ['translations'];
}
```

::: tip

It is recommended to **always** load the `translations` relation.

:::

Furthermore, a model for the translation table is needed. Translation Models are
generated into `app/Models/Translations`. In the translation model all mass
assignable attributes of the translation table are specified:

```php{app/Models/Translations/PostTranslation.php}
<?php

namespace App\Models\Translations;

use Illuminate\Database\Eloquent\Model;

class PostTranslation extends Model
{
    /**
     * Wether the model uses the default timestamp columns.
     *
     * @var bool
     */
    public $timestamps = false;

    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = ['title'];
}
```

#### Displaying Translated Attributes

The attribute title can now be output via the model post, the value is displayed
depending on the set **locale**:

```php
app()->setLocale('en');
echo $post->title; // Echo's english value.

app()->setLocale('de');
echo $post->title; // Echo's german value.
```

::: tip

Read more about translatable models in the
[package documentation](https://docs.astrotomic.info/laravel-translatable/).

:::

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

    public function sluggable(): array
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

## Fill Model On Create & Update

You may want fill attributes to the model before it gets created or updated from
the user via a crud form. This can be achieved by modifying the Model in either
`fillOnStore` or `fillOnUpdate` the crud controller.

The following example show's how to assign an author to a post by setting the
`author_id` to the authenticated litstack user id.

```php{lit/app/Http/Controllers/Crud/PostController.php}
namespace Lit\Http\Controllers\Crud;

use Ignite\Crud\Controllers\CrudController;
use Lit\Models\User;

class PostController extends CrudController
{
    public function fillOnStore($post)
    {
        $post->author_id = lit_user()->id;
    }

    public function fillOnUpdate($post)
    {
        // ...
    }
}
```

## Permissions

Litstack brings not only a super simple **permission manager**, but also a
simple way to create permissions and bind them to crud models. The desired
permissions just need to be added to the migration `*_make_permissions.php` and
will be added to the database by executing the `lit:permissions` command.

### Create Permissions

So let's create crud permissions for our posts example. So we add `posts` to the
`crudPermissionGroups` property. This will create the following permissions:

-   `create posts`
-   `read posts`
-   `update posts`
-   `delete posts`

```php{database/migrations/______________________make_permissions.php}
protected $crudPermissionGroups = [
    // ...

    'posts'
];
```

and execute the command:

```shell
php artisan lit:permissions
```

### Apply Permissions

The controller that comes with the crud contains the authorize method. The
second parameter is the operation to be executed. So now we can check if the
authenticated user has the permission to operate on the group `posts` by
returning `$user->can("{$operation} posts")`:

```php{lit/app/Http/Controllers/Crud/PostController.php}
<?php

namespace Lit\Http\Controllers\Crud;

use Ignite\Crud\Controllers\CrudController;
use Lit\Models\User;

class PostController extends CrudController
{
    /**
     * Authorize request for authenticated lit-user and permission operation.
     * Operations: create, read, update, delete.
     *
     * @param  User   $user
     * @param  string $operation
     * @param  int    $id
     * @return bool
     */
    public function authorize(User $user, string $operation, $id = null): bool
    {
        return $user->can("{$operation} posts");
    }
}
```

### Authorize individual Models

You may want to authorize individual models. This can be achieved by adding the
initial query in the `query` method. The following example shows how models can
be hidden by users who did not create them:

```php{lit/app/Http/Controllers/Crud/PostController.php}
class PostController
{
    public function query($query)
    {
        $query->where('created_by', lit_user()->id);
    }
}
```

## Navigation

Add the navigation entry by adding the config namespace preset to your
navigation.

```php
$nav->preset(PostConfig::class, ['icon' => fa('newspaper')]),
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
