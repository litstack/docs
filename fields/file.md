# File

## Introduction

A drag and drop file uploader using Spatie's
[medialibary](https://docs.spatie.be/laravel-medialibrary/v7/introduction/).

## Basics

The file field is closely related to the [image field](/fields/image) as it is
also based on spaties medialibary. Hence, they have several methods in common
and only the important differences are described at this place.

```php
$form->file('download')
    ->title('Download')
    ->hint('A file attachment.')
    ->maxFiles(1);
```

## Accept

By default the file upload only accepts Documents with the mime-type
`application/pdf`. You may override the accepted MIME types using the
`acccept()` method.

```php{2}
$form->file('video')->title('Video Download')->accept('video/*');
```

To allow multiple MIME types or additional subtypes you may chain them as a
comma separated string:

```php
$form->file('attachments')
    ->title('Files')
    ->accept('application/pdf,application/msword,image/*');
```

A quick list of available MIME types can be found at
[apache.org](https://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types)

## Methods

| Method                     | Description                                                        |
| -------------------------- | ------------------------------------------------------------------ |
| `$field->title('File')`    | The title for this form field.                                     |
| `$field->hint('Foo.')`     | A short hint that should describe how to use the form field.`      |
| `$field->info('...')`      | Questionmark with tooltip. (Can contain longer field descriptions) |
| `$field->width(1/2)`       | Width of the form field.                                           |
| `$field->sortable()`       | Should the files be sortable? (default: `true`)                    |
| `$field->maxFiles(1)`      | Maximum number of uploadable files. (default: `5`)                 |
| `$field->maxFileSize(100)` | Maximum file size. (default: `12`)                                 |
| `$field->accept()`         | The accpeted mime types. (default: `application/pdf` )             |
