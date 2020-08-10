# Password

## Introduction

A `password` input Field.

```php
$form->password('password')
    ->title('Password');
```

## Confirm Form

Update or create form requires a password confirmation.

```php{3}
$form->password('password')
    ->title('Password')
    ->confirm();
```

## Confirm Password

Password fields can be used to update and confirm user passwords using the
following `rules`.

```php
$modal->password('password')
    ->title('New Password')
    ->rules('required', 'min:5')
    ->minScore(0);

$modal->password('password_confirmation')
    ->rules('required', 'same:password')
    ->dontStore()
    ->title('New Password')
    ->noScore();
```

## Methods

| Method                               | Description                                                                    |
| ------------------------------------ | ------------------------------------------------------------------------------ |
| `$field->title('Password')`          | The title description for this field.                                          |
| `$field->placeholder('Password')`    | The placeholder for this field.                                                |
| `$field->hint('Choose a Password.')` | A short hint that should describe how to use the field.`                       |
| `$field->width(1/2)`                 | Width of the field.                                                            |
| `$field->confirm()`                  | Requires the user to type in the current passwort to confirm update or create. |
| `$field->rulesOnly()`                | The password wont be stored, only validation rules are used.                   |
| `$field->rules('min:5')`             | Rules that should be applied when **updating** and **creating**.               |
| `$field->creationRules('min:5')`     | Rules that should be applied when **creating**.                                |
| `$field->updateRules('min:5')`       | Rules that should be applied when **updating**.                                |
