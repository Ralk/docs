# Boolean

## Usage

The boolean field adds a switch to a boolean attribute.

```php
$form->boolean('active')
    ->title('Live')
    ->hint('Put the site online.')
    ->width(1/3);
```

Add the `boolean`
[cast](https://laravel.com/docs/5.2/eloquent-mutators#attribute-casting) to your
model:

```php
protected $casts = [
    'active' => 'boolean'
];
```

## Methods

| Method                              | Description                                                        |
| ----------------------------------- | ------------------------------------------------------------------ |
| `$field->title('Live')`             | The title description for this field.                              |
| `$field->hint('Foo.')`              | A short hint that should describe how to use the field.            |
| `$field->info('...')`               | Question mark with tooltip. (Can contain longer field descriptions) |
| `$field->width(1/2)`                | Width of the field.                                                |
| `$field->rules('required')`         | Rules that should be applied when **updating** and **creating**.   |
| `$field->creationRules('required')` | Rules that should be applied when **creating**.                    |
| `$field->updateRules('required')`   | Rules that should be applied when **updating**.                    |
