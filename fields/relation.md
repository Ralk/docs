# Relation

## Introduction

A relation picker. Relation pickers can be created for any relation of your
model.

![relation picker](./screens/relation/picker.jpg 'relation picker')

The relation field requires the name of the relation that should be edited. The
type of the relation is automatically recognized and displayed accordingly.

The relations Field can only be used for **Crud Models** and not in **Forms** or
**Blocks**. For Forms or Blocks the [oneRelation](#onerelation-and-manyrelation)
or [manyRelation](#onerelation-and-manyrelation) field can be used.

```php
$form->relation('articles')
    ->title('Articles')
     ->preview(function ($table) {
        // Build the preview table in here.
        $table->col('title'); // In this case we are showing the article title.
    });;
```

In the Model:

```php
public function articles()
{
    return $this->hasMany('App/Models/Article');
}
```

## Preview

The table preview is configured in preview.

```php
$form->relation('articles')
    ->title('Articles')
    ->preview(function ($table) {
        // Build the preview table in here.
        $table->col('title'); // In this case we are showing the article title.
    });
```

However you don't need to define a new preview table every time you create a new
relation field. You can simply display the index table of the related Model by
setting settings its Crud config class in the `use` method list this:

```php
use Lit\Config\Crud\ArticleConfig;

$form->relation('articles')
    ->title('Articles')
    ->use(ArticleConfig::class);
```

### Display Pivot Data

You may load and display data from the pivot table of a many to many relation.

In the following example we want to display when a subscription of a user
expires:

First load the pivot data by specifying the desired columns in the `withPivot`
method:

```php
$index->query(function($query) {
    $query->withPivot('expires_at');
});
```

Now we can display the `expires_at` attribute like this:

```php
$field->preview(function ($table) {
    $table->col('subscriptions.pivot.expires_at');
});
```

## Sortable

If the relation should be sortable, the related query in your Model must be
sorted by an `orderColumn`.

```php
public function articles()
{
    return $this->hasMany('App/Models/Article')->orderBy('order_column');
}
```

Now the sortable attribute can be added:

```php
$form->relation('articles')
    ->title('Articles')
    ->sortable()
    ->preview(function ($table) {
        $table->col('Title')->value('{title}');
    });
```

## Filter

With a **filter** you can specify which models can be selected for a relation.

```php
$form->relation('articles')
    ->title('Articles')
    ->filter(function($query) {
        $query->where('created_by', lit_user()->id);
    });
```

## Forms

You can edit the **related attributes** in a modal by configuring fields in the
form method:

```php
$form->relation('articles')
    ->form(function($form) {
        $form->wysiwyg('text')->title('text');
    });
```

### Create A New Relationship Model

You may also add a create form so the user can create new relationship models
directly:

```php
$form->relation('articles')
    ->create(function($form) {
        $form->wysiwyg('text')->title('text');
    });
```

::: tip

Set a create and edit form using the `createAndUpdateForm`.

:::

### Pivot Fields

Sometimes you may want to edit pivot data such as the `expired_at` date of a
**subscription**. Therefore the `pivot` method must be prepended to the field.

```php
$field->form(function($form) {
    $form->pivot()->datetime('expires_at')->title('Expires At');
});
```

## Inline Fields

You may even add inline fields to your relationship table. The following example
show's how to add an relationship to opening hours that can be created and
edited directly:

![Relation Inline Fields](./screens/relation/inline.png 'Relation Inline Fields')

```php
$form->relation('openingHours')
	->title('Opening Hours')
	->showTableHead()
	->deleteUnlinked()
	->hideRelationLink()
	->names([
		'singular' => 'Opening Hour',
		'plural'   => 'Opening Hours',
	])
	->preview(function ($preview) {
		$preview->col('Week Day')->value('{week_day}');

		$preview->field('Opening Time')
			->datetime('opening_time')
			->onlyTime()
			->minuteInterval(15);

		$preview->field('Closing Time')
			->datetime('closing_time')
			->onlyTime()
			->minuteInterval(15);
	})
	->create(function ($form) {
		$form->select('week_day')
			->title('Wochentag')
			->options([
				'monday' => 'Monday',
				'tuesday' => 'Tuesday',
				'wednesday' => 'Wednesday',
				'thursday' => 'Thursday',
				'friday' => 'Friday',
				'saturday' => 'Saturday',
				'sunday' => 'Sunday',
			]);
		$form->datetime('opening_time')
			->onlyTime()
			->minuteInterval(15)
			->width(1 / 2);

		$form->datetime('closing_time')
			->onlyTime()
			->minuteInterval(15)
			->width(1 / 2);
	});
```

## Eager Loading & Appending Accessors

With query, **relationships** & **accessors** that should be displayed can be
**eager loaded** or **appended**.

```php
$form->relation('articles')
    ->title('Articles')
    ->query(function($query) {
        $query->with('author')->append('comments_count');
    })
    ->preview(function ($table) {
        $table->col('Author')->value('{author.first_name} {author.last_name}');
        $table->col('Comments')->value('{comments_count} comments');
    });
```

## Allow Direct Unlink

To switch off the modal in which unlinking a relation is confirmed, `confirm`
must be set to false.

```php
$form->relation('articles')
    ->confirm(false);
```

## Delete Unlinked Model

You may wish to delete a relation when it has been unlinked:

```php
$form->relation('articles')
    ->deleteUnlinked();
```

## Tags

For **many relations** a `tag` preview can be generated by passing `tags` to the
`type` method. Therefore the **attribute** that should be displayed in the tag
must be specified using `tagValue`. Furthermore the variant can be specified
with `tagVariant`.

```php
$form->relation('tags')
    ->title('Tags')
    ->type('tags')
    ->tagValue('{value}');
```

![Relation Tags](./screens/relation/tags.png 'Relation Tags')

## oneRelation And manyRelation

Every Laravel relation needs its corresponding database setup. For example the
belongsTo relation needs an extra column that stores the id of the related
Model. However you migth want to create relations without going through the
process of creating a new migration to add the required columns/tables. For this
there is the `oneRelation` and `manyRelation`. They can simply be added to your
Models or Forms.

The following example shows the simple setup of a the relation field that works
for both:

```php
use App\Models\Article;

$form->manyRelation('articles')
    ->title('Articles')
    ->model(Article::class)
    ->preview(function($preview) {
        //...
    });
```

In the Model:

```php
public function articles()
{
    return $this->manyRelation('App/Models/Article', 'articles');
}
```

:::tip

`oneRelation` and `manyRelation` can be used in Forms.

:::

## Methods

| Method                          | Description                                                                                               |
| ------------------------------- | --------------------------------------------------------------------------------------------------------- |
| `$field->title(')`              | The title description for this field.                                                                     |
| `$field->hint(')`               | A short hint that should describe how to use the field.`                                                  |
| `$field->width()`               | Width of the field.                                                                                       |
| `$field->filter()`              | Initial query builder for the selectable relations.                                                       |
| `$field->query()`               | Modify preview query with eager loads and accessors that should be displayed.                             |
| `$field->preview()`             | A **closure** to define the table preview of the corresponding relation.                                  |
| `$field->confirm()`             | Modal pops when unlinking the relation and asks to confirm. (default: `true`)                              |
| `$field->deleteUnlinked()`      | Deletes the relation Model after unlinking the relation. (default: `false`)                               |
| `$field->sortable()`            | Sortable relation (only works for `many` relations).                                                      |
| `$field->small()`               | Small table column height.                                                                                |
| `$field->maxItems()`            | Set a maximum number of selectable items (only works for `many` relations).                               |
| `$field->create()`              | Define form fields for a create form so users can crete new relationship models directly.                 |
| `$field->form()`                | Define form fields so the user can update relation ship attributes.                                       |
| `$field->createAndUpdateForm()` | Define the same form fields for `create` and `form`.                                                      |
| `$field->showTableHead()`       | Whether the table head should be shown. (default: `false`)                                                |
| `$field->type()`                | The preview type (default: `table`) can be `tags` for **many relations** and `link` for **one relations** |
| `$field->tagValue()`            | The attribute that should be displayed in the tag.                                                        |
| `$field->tagVariant()`          | The bootstrap variant of the tag. (default: `info`)                                                       |
| `$field->linkValue()`           | The attributes that should be displayed as the link.                                                      |
