# Block

## Introduction

Block fields are used to map repetitive content. They can contain any number of
so called **repeatables**, which you can string together in any order. A
**repeatable** can be regarded as a separate individual form. As well a preview
table can be set for every block. If field values are to be displayed as
preview, they must be written in **curly brackets**.

```php
$form->block('content')
    ->title('Content')
    ->repeatables(function($repeatables) {

        // Add as many repeatables as you want.
        $repeatables->add('text', function($form, $preview) {
            // The block preview.
            $preview->col('{text}');

            // Containing as many form fields as you want.
            $form->input('text')
                ->title('Text');
        });

        $repeatables->add('image', function($form, $preview) {
            $preview->col('{}');

            $form->image('image')
                ->title('Image');
        });
    });
```

### Preparing the Model

Block fields can be added to CRUD-Models as well as to forms. Blocks don't need
a dedicated column in the model, as they are stored in a database table
centrally.

The repeatables can be loaded to your model using the `repeatables` **relation**
method.

```php
public function content()
{
    return $this->repeatables('content');
}
```

You can now receive the block data like this:

```php
Post::find($id)->content;
```

## Binding Views

You can directly bind a view to a repeatable to use it in the frontend where you
display your block:

```php{3}
$repeatables->add('text', function($form, $preview) {
    // ...
})->view('repeatables.text');
```

You may even bind Blade x components using the `x` method:

```php{3}
$repeatables->add('text', function($form, $preview) {
    // ...
})->x('repeatables.text');
```

## Reusable Repeatables

Sometimes you want to use repeatables in different places. Resuable repeatables
can be created using the `lit:repeatable` command. The **preview** and **form**
can then be configured in the newly generated class in the `lit/app/Repeatables`
directory.

```shell
php artisan lit:repeatable text
```

The generated class can be added to a block by passing the **namespace** as the
first parameter to the `add` method:

```php
use Lit\Repeatables\TextRepeatable;

$repeatables->add(TextRepeatable::class);
```

## Frontend

To use the repeatables of a block in the frontend you can loop over them and
display the content depending on the repeatable `type`, like this:

```php
@foreach($model->content as $repeatable)
    @if($repeatable->type == 'text')
        {{ $repeatable->text }}
    @elseif($repeatable->type == 'image')
        {{-- ... --}}
    @endif
@endforeach
```

However you can simply use the blade directive `block`, which does exactly that
for you.

```php
@block($model->content)
```

::: warning

The `block` directive needs a View or a Blade x component bound to the
repeatable in order to be displayed.

:::

## Methods

| Method                  | Description                                        |
| ----------------------- | -------------------------------------------------- |
| `$field->title()`       | The title description for this field.              |
| `$field->repeatables()` | A closure where all repeatable blocks are defined. |
| `$field->width()`       | Width of the field.                                |
| `$field->blockWidth()`  | Width of a block.                                  |

## Repeatable Methods

| Method                  | Description                                      |
| ----------------------- | ------------------------------------------------ |
| `$repeatable->button()` | The title for the repeatable.                    |
| `$repeatable->view()`   | The View describing the repeatable.              |
| `$repeatable->x()`      | The Blade x component describing the repeatable. |
