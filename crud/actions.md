# Actions

## Introduction

Actions are a convenient feature to create reusable tasks for models. Each
action can be placed in different locations. For example in your index table or
on a detail page of a model.

## Create

Create an action via artisan:

```shell
php artisan lit:action MyAction
```

The created actions are located in `./lit/app/Actions`.

```php
namespace Lit\Actions;

use Illuminate\Support\Collection;

class MyAction
{
    public function run(Collection $models)
    {
        // Do Something.
    }
}
```

## Responses

When a `message` is returned via a json response, a toast is displayed in the
lower right corner of the page containing the message. This can be displayed in
four variants: `success`, `info`, `warning`, `danger`. The default is `success`.

```php
public function run(Collection $models)
{
    return response()->json(['message' => 'Hello there!', 'variant' => 'info']);
}
```

There is a helper for each variant:

```php
return response()->success('My message.');
return response()->info('My message.');
return response()->warning('My message.');
return response()->danger('My message.');
```

<center>
  <img src="./screens/message_success.png" alt="message success" width="350"/>
  <img src="./screens/message_info.png" alt="message info" width="350"/>
  <img src="./screens/message_warning.png" alt="message warning" width="350"/>
  <img src="./screens/message_danger.png" alt="message danger" width="350"/>
</center>

### Redirects

In some cases you may want to redirect the user to another page after the action
has been executed. This can be done as usual by returning a
[redirect](https://laravel.com/docs/7.x/redirects).

```php
public function run(Collection $models)
{
    // Do Something.

    return redirect('your/redirect/url');
}
```

## Confirm Modal

If your action has the function modal, the execution of the action must be
confirmed via a modal.

```php
namespace Lit\Actions;

use Illuminate\Support\Collection;
use Ignite\Page\Actions\ActionModal;

class MyAction
{
    public function modal(ActionModal $modal)
    {
        $modal->confirmVariant('primary')
            ->confirmText('Run')
            ->message('Do you want to run the Action?');
    }

    public function run(Collection $models)
    {
        // Do Something.
    }
}
```

## Fields

Form fields can be added to the confirm modal of an action. The values of these
fields are passed to the action. This allows you, for example, to let the user
type in a message to be sent by mail, as shown the following example.

```php
use Illuminate\Support\Collection;
use Ignite\Page\Actions\ActionModal;
use Ignite\Page\Actions\AttributeBag;

public function modal(ActionModal $modal)
{
    $modal->form(function($form) {
        $form->text('message')->title('Message');
    });
}

public function run(Collection $models, AttributeBag $attributes)
{
    foreach($models as $model) {
        Mail::to($model)->send(new ExampleEmail($attributes->message));
    }
}
```

<center>
  <img src="./screens/action_modal_fields.png" alt="action fields" height="250"/>
</center>

## Display the Action

### In a Table

There are multiple places where an `action` can be displayed in a table, first
of all an action can be executed for all selected items:

```php
// CrudConfig

use Lit\Actions\MyAction;

public function index(CrudIndex $page)
{
    $page->table(...)
        ->action('My Action', MyAction::class);
}
```

Additionally actions can be displayed in table columns to make important actions
more accessible. You can add either one action as a button or several actions as
dropdown to a column:

```php
use Lit\Actions\MyAction;
use Lit\Actions\OtherAction;

$page->table(function($table) {
    // Single action as button:
    $table->action('My Action', MyAction::class);

    // Multiple actions as dropdown:
    $table->actions([
        'My Action' => MyAction::class,
        'Other Action' => OtherAction::class,
    ]);
});
```

### On a Page

Actions can also be displayed on the show page of a crud, even in the slots
`navigationLeft`, `navigationRight`, `navigationControls`, `headerLeft` and
`headerRight`:

![navigation](./../basics/screens/page_slots.jpg 'navigation')

```php
// CrudConfig

use Lit\Actions\MyAction;

public function show(CrudShow $page)
{
    $page->navigationLeft()->action('My Action', MyAction::class);
    $page->navigationRight()->action('My Action', MyAction::class);
    $page->navigationControls()->action('My Action', MyAction::class);
    $page->headerLeft()->action('My Action', MyAction::class);
    $page->headerRight()->action('My Action', MyAction::class);
}
```

You may even place it somewhere on your page:

```php
$page->card(function($page) {
    $page->action('My Action', MyAction::class);
});
```

The `variant` of the actions that are displayed as buttons can be adjusted:

```php
$page->headerLeft()
    ->action('My Action', MyAction::class)
    ->variant('outline-primary');
```

## Conditional Actions

You may want to display actions depending on the value of a model attribute.
This can be achieved by applying any of the existing
[field dependencies](../fields/conditions.md#usage) to your action like this:

```php
$page->headerLeft()
    ->action('cancel', CancelBookingAction::class)
    ->whenNot('state', 'canceled');
```
