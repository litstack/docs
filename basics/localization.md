# Localization

## Introduction

The application can be managed multilingual. The translation in the frontend can
be done in `php` using
[laravel's localization](https://laravel.com/docs/7.x/localization) service or
in `vue` using
[vue-i18n](https://kazupon.github.io/vue-i18n/docs/formatting.html). It uses the
syntax of **laravel**. All translation strings are formatted so they can be used
in `vue-i18n` as well.

In Laravel applications that include the package, there are **two** different
locales, one for your website and one for the admin application. So for example,
a user can manage german content in the admin application and still see the
interface in the english version.

The following examples refer to the translations which look like this:

```php
// lit/resources/lang/{locale}/messages.php

return [
    "welcome": "Welcome, :name"
];
```

## PHP

To compile for the locale of the admin interface the `__lit()` helper method is
used, just like `__()` from laravel's
[localization](https://laravel.com/docs/7.x/localization#retrieving-translation-strings).

```php
__lit('messages.welcome', ['name', 'Jannes'])
```

Pluralization can be used with the `__lit_choice` function or the short version
`__lit_c`.

```php
'apples' => '{0} There are none|[1,19] There are some|[20,*] There are many',

...

__lit_choice('apples', 10)
__lit_c('apples', 10)
```

## Vue

In `Vue` the [vue-i18n](https://kazupon.github.io/vue-i18n/introduction.html)
format is used.

```javascript
<template>
    <div>
        {{ $t('messages.welcome', { name: 'Jannes' }) }}
    </div>
</template>
```

::: tip

You may also use the Larvel localization helpers in Vue components.

-   \_\_()
-   trans()
-   trans_choice()

:::

## Determine Locale

To determine the locale the function `getLocale` can be used on the `Lit` facade
like this:

```php
use Ignite\Support\Facades\Lit;

$litLocale = Lit::getLocale();

if (Lit::isLocale('en')) {
    //
}
```

## Add Path

By default the path `lit/resources/lang` is imported for the admin translation.
You can register any number of paths with localization files in your service
providers. However, it is recommended to keep the translations for the admin
application and your website separate.

```php
use Ignite\Support\Facades\Lang;

Lang::addPath(base_path('yourpath/lang/'));
```

::: tip

All translation attributes are merged, which makes it easy to extend existing
localizations for parts like **validation** or others.

:::
