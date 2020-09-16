# Modal

Form fields inside a `modal`.

```php
$form->modal('update_email')
    ->title('E-Mail')
    ->name('Update E-Mail')
    ->form(function($form) {
        $form->input('email')->title('E-Mail');
    });
```

## Confirm with Password

Appends a password field to the end of the form that is required to confirm the
modal form using the current users password.

```php{4}
$form->modal('update_email')
    ->title('E-Mail')
    ->name('Update E-Mail')
    ->confirmWithPassword()
    ->form(function($form) {
        $form->input('email')->title('E-Mail');
    });
```

## Methods

| Method                          | Description                                                                                                                  |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `$field->title()`               | The title description for this field.                                                                                        |
| `$field->placeholder()`         | The placeholder for this field.                                                                                              |
| `$field->hint()`                | A short hint that should describe how to use the field.`                                                                     |
| `$field->info()`                | Questionmark with tooltip. (Can contain longer field descriptions)                                                           |
| `$field->width()`               | Width of the field.                                                                                                          |
| `$field->confirmWithPassword()` | Appends a password field to the end of the form that is required to confirm the modal form using the current users password. |
