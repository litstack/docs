# Fields

## Introduction

Fields can be used to make database entries easily editable. Each field is
appended one or more columns of a database table.
[Validation Rules](https://laravel.com/docs/8.x/validation) can be added to
fields and it can be determined who can edit a field.

## Basics

When you create a field you must first specify the `attribute` that the field
refers to. Like in the following example an input field refers to `first_name`.

```php
$form->input('first_name');
```

Every field requires a title.

```php
$form->input('first_name')->title('Firstname');
```

Furthermore the width can be specified either in **12** columns as in bootstrap,
or in fractions. The fractions are converted into columns.

```php
$form->input('first_name')->title('Firstname')->width(4);
// Is the same as:
$form->input('first_name')->title('Firstname')->width(1 / 3);
```

## Authorization

With the `authorize` method it can be determined if the user has the permission
for a field. If the user does not have the required authorization, the field is
not displayed.

```php
$form->input('first_name')->authorize(function($user) {
    return $user->can('edit user-name');
});
```

## Readonly

If you want the user to see the field, but not be able to edit it, you can use
the `readOnly` function.

```php
$form->input('first_name')->readOnly(! lit_user()->can('edit user-name'));
```
