# Navigation

## Introduction

Your admin app by default contains two navigations: the **topbar** and your
**main** navigation.

Both navigation instances are configured in
`lit/app/Config/NavigationConfig.php` which looks as follows:

```php{lit/app/Config/NavigationConfig.php}
class NavigationConfig extends Config
{
    public function topbar(Navigation $nav)
    {
        // Build your topbar navigation in here.
    }

    public function main(Navigation $nav)
    {
        // Build your main navigation in here.
    }
}
```

![navigation](./screens/navigation.jpg 'navigation')

### Topbar

The **topbar** navigation is meant for managing less important part sof your
application, such as language and/or permissions.

### Main

The **main** navigation is inteded to provide quick access to the important
components of your application. These could be for example Models or page
content. Entries of the main navigation can be divided into
[sections](#sections), additionally [groups](#groups) can be created which are
displayed as dropdown. More than one depth is not possible here.

## Building the Navigation

The following explains how the navigations are built. The procedure is the same
for the **topbar** and the **main** navigation. With a difference that only the
**main** navigation can contain groups.

### Sections

A section can contain a list of entries. An optional title can be prepended to
the entries like shown in the example:

```php
$nav->section([

    // The Section title describes the following set of navigation entries.
    $nav->title('Bookings'),

    ...
]);
```

### Entry

Simple entries have a `title`, a `link`, and an `icon`. The
[Font Awesome](https://fontawesome.com/icons?d=gallery&m=free) icons are
included by default.

```php
$nav->entry('Home', [
    'link' => route('your.route'), // Route
    'icon' => '<i class="fas fa-home"></i>' // Font Awesome icon
]),
```

### Groups

Create nested navigation entries in groups.

```php
$nav->group([
    'title' => 'Pages',
    'icon' => '<i class="fas fa-file"></i>',
], [

    $nav->entry(...),
    ...

]),
```

### Authorization

To hide navigation entries from users without the necessary permission, an
`authorize` closure can be specified in which permissions for the logged in
litstack user can be queried.

```php
use Ignite\User\Models\User;

$nav->entry('Home', [
    // ...

    'authorize' => function(User $user) {
        return $user->can('read page-home');
    }
]),
```

### Presets

To build navigation entries, for example for Crud models, you can use navigation
presets in which the corresponding `route` and `authorization` have already been
defined. This is useful in most cases, especially to maintain the correct
`authorization`. To edit the entry further you can specify an array with the
navigation entry elements in a second parameter.

```php
$nav->preset('crud.departments', [
    'icon' => fa('building')
]),
```

::: tip

A list of all registered navigation presets can be displayed with
`php artisan lit:nav`.

:::
