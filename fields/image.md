# Image

## Introduction

A drag and drop image uploader using Spatie's
[medialibary](https://docs.spatie.be/laravel-medialibrary/v7/introduction/).

## Model Setup

To attach images to a model, only the media **contract** and the media **trait**
must be added to the model.

```php
use Ignite\Crud\Models\Traits\HasMedia;
use Spatie\MediaLibrary\HasMedia as HasMediaContract;

class Post extends Model implements HasMediaContract
{
    use HasMedia;
}
```

## Basics

For all images that are uploaded an input field for `alt` and `title` is
displayed, with `translatable` these two fields are made translatable.

```php
$form->image('images') // images is the corresponding media collection.
    ->translatable()
    ->title('Images')
    ->hint('Image Collection.')
    ->maxFiles(5);
```

Add the image attribute to your model:

```php
public function getImagesAttribute()
{
    return $this->getMedia('images');
}
```

## Crop

To crop the image to a desired ratio before uploading it, a ratio can be defined
using the method `crop`.

```php{3}
$form->image('images')->title('Images')->crop(16 / 9);
```

::: tip

If no ratio is passed as a parameter to the crop method, the image can be freely
cropped.

:::

## Expand

By default the images are displayed as a square. However, this view is not
suitable for example for header images. With **expand** you can display the
image at full size.

```php{2}
$form->image('header_image')->title('Header Image')->expand();
```

![Image expand](./screens/image/expand.png 'Image expand')

## Preview Image

In the case that the first image from the list should be used as a preview
image, you can use `firstBig` to display the first image bigger to show that it
has a special importance.

```php{3}
$form->image('images')
    ->title('Images')
    ->firstBig()
    ->hint('The first image is the preview image.');
```

![Image firstBig](./screens/image/first_big.png 'Image firstBig')

## Conversions

In the config `lit.mediaconversions` all conversions groups are specified. If
you would like a model to use another conversion group than `default`, the group
name can be set using the attribute `mediaconversions`.

```php
class Post extends Model implements HasMediaContract
{
    ...

    /**
     * Media conversions group.
     *
     * @var string
     */
    protected $mediaconversions = 'other';
}
```

::: warning

To display conversion urls it is important to set the env `APP_URL` to the url
you are working in.

:::

## Max. File Size

The maximum allowed file size for an image can be specified using the
`maxFileSize` method. The file size is given in **megabytes**. The default value
is 12 mb.

```php{3}
$form->image('images')->title('Images')->maxFileSize(25);
```

## Methods

| Method                     | Description                                                        |
| -------------------------- | ------------------------------------------------------------------ |
| `$field->title('Image')`   | The title for this form field.                                     |
| `$field->translatable()`   | Should the field be translatable.                                  |
| `$field->hint('Foo.')`     | A short hint that should describe how to use the form field.`      |
| `$field->info('...')`      | Question mark with tooltip. (Can contain longer field descriptions) |
| `$field->width(1/2)`       | Width of the form field.                                           |
| `$field->sortable()`       | Should the images be sortable? (default: `true`)                   |
| `$field->maxFiles(1)`      | Maximum number of uploadable images. (default: `5`)                |
| `$field->maxFileSize(100)` | Maximum file size. (default: `12`)                                 |
| `$field->expand()`         | Expand the preview image to its full width.                        |
| `$field->crop(16/9)`       | Opens a Crop-Tool before the upload. (default: `false`)            |
| `$field->showFullImage()`  | Display's the full image inside of the square box.                 |
| `$field->firstBig()`       | Display's the first image bigger.                                  |
