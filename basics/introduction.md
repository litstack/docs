# Basics

## Introduction

This document describes the main components that help to understand how a
litstack application is built.

## The `./lit` Folder

After [installing](../installation.md) Litstack the `./lit` folder is created in
your laravel application. Think of it as an its own **isolated** laravel
application in your laravel application. It contains the `./lit/resources`
folder containing assets and views, an `./lit/app` folder with the php classes
for your **Litstack** app and other folders that are also present in the root
folder.

::: tip

Isolating your administration application from your app provides a clear
structure of your actual buisness logic.

:::

## Creator Commands

For almost every creator commands provided by laravel, there is a similar one
that creates the corresponding files into the `./lit` folder. The commands use
the `lit` prefix instead of `make`.

```shell
php artisan lit:controller
php artisan lit:request
php artisan lit:job
```

### Livewire

There is a creator command for livewire as well:

```php
php artisan lit:livewire Counter
```
