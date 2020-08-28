# Extend With Vue

## Introduction

The admin interface can be extended with custom Vue components for numerous
purposes. This documents describes how custom Vue components can be added to the
application.

## Setup

To include your own `Vue` components in the admin application, the local npm
package `vendor/litstack/litstack` has to be installed. This can be done using
the following artisan command:

```shell
php artisan lit:extend
```

At the beginning of your `webpack.mix.js` the import of the Litstack mix config
will be added automatically. Two files are compiled:

-   `lit/resources/js/app.js` => `public/lit/js/app.js`
-   `lit/resources/sass/app.scss` => `public/lit/css/app.css`

Add them to **assets** in the config.

```php
// config/lit.php
'assets' => [
    'js' => '/lit/js/app.js',
    'css' => [
        '/public/lit/css/app.css',
        // Add more css files here ...
    ],
],
```

All javascript files can be found in `lit/resources/js`.

::: tip

Components that are created in the `components` folder are automatically
registered.

:::

Run `npm run watch` and you are good to go.

::: warning

Dont forget to compile your assets every time you **update** the package.

:::

## Register Vue Components

Vue Components that are located in the `./lit/resources/js/components` folder
are automatically globally registered. They can be used for example in Pages,
Tables or anywhere where you want to include Vue components.

## Bootstrap Vue

To make it easy to build uniform Litstack pages, the package uses
[Bootstrap Vue](https://bootstrap-vue.org/docs/components) for all frontend
components. Bootstrap Vue comes with a large number of components to cover all
the necessary areas needed to build an application.
