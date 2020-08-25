# Helpers

## Introduction

The package includes a variety of global helper **PHP** `functions`.

::: tip

The package includes useful **Vue** `mixins` as well. Read the
[Vue mixins](../frontend/vue.md#mixins) section to learn more about the existing
`mixins`.

:::

## Lit Facade

The `Lit` singleton contains some helpers which are related to the application.

### `installed()`

The `installed` method checks if the package has been installed. This can be
useful in service providers.

```php
use Ignite\Support\Facades\Lit;

if(! Lit::installed()) {
    return;
}
```

### `route($name)`

If a route is added to the route file `lit/routes/web.php`, the prefix `lit` is
added to the name. The helper route also adds this prefix as well. Depending on
your preferences you can use the laravel helper `route` or the Litstack helper
to call up a route.

```php
// lit/routes/web.php

Route::get('dashboard', DashboardController::class)->name('dashboard');
```

```php
<a href="{{ Lit::route('dashboard') }}">Dashboard</a>
// Is the same as:
<a href="{{ route('lit.dashboard') }}">Dashboard</a>
```

### `url($url)`

If you don't want to use the route name to call a route but directly specify the
url, you can use the `url` helper which prepends the `route_prefix` from the
config **lit.php** to your url.

```php
<a href="{{ Lit::url('dashboard') }}">Dashboard</a>
```

## Miscellaneous

### `__lit($key, $replace)`

The `__lit` method returns the translation using the application locale for the
authenticated user.

```php
__lit('messages.welcome', ['name' => 'Spatie'])
```

### `fa($group, $icon)`

The `fa` helper makes it easy to include
[Fontawesome](https://fontawesome.com/icons?d=gallery) icons.

```php
fa('home') // <i class="fas fa-home"></i>
fa('fal', 'abacus') // <i class="fal fa-abacus"></i>
```

### `lit()`

The `lit` method returns the `Ignite\Support\Facades\Lit` singelton.

```php
lit()->installed();

// Is the same as:

use Ignite\Support\Singletons\Lit;
Lit::installed();
```

### `lit_user()`

The `lit_user` method returns the authenticated user model.

```php
<span>{{ lit_user()->email }}</span>
```
