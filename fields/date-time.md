# DateTime

## Introduction

A DateTime-Picker.

<center>
      <img src="./screens/datetime/example.png" style="max-width:400px">
</center>

## Examples

The following example shows the basic usage of the `datetime` field.

```php
$form->datetime('publish_at')
    ->title('Date')
    ->formatted('l')
    ->hint('Chose a date.')
    ->width(6);
```

### Shortcut

```php
$form->dt('publish_at') // dt and datetime work the same
```

### Date and Time

You may enable to pick a time as well by settings `onlyDate` to false:

```php{3}
$form->datetime('publish_at')
    ->title('Date')
    ->onlyDate(false)
    ->formatted('l')
    ->hint('Chose a date.')
    ->width(6);
```

<center>
      <img src="./screens/datetime/date_and_time.png" style="max-width:400px">
</center>

## Methods

| Method                              | Description                                                      |
| ----------------------------------- | ---------------------------------------------------------------- |
| `$field->title('Publish At')`       | The title description for this field.                            |
| `$field->formatted('LL')`           | The shown datetime format. (Useful formats listed below.)       |
| `$field->hint('Pick a date.')`      | A closure where all repeatable blocks are defined.               |
| `$field->onlyDate()`                | Pick date only (default: `true`).                                |
| `$field->onlyTime()`                | Pick time only (default: `false`).                               |
| `$field->minuteInterval(15)`        | Timepicker minute interval (default: `5`).                       |
| `$field->width(1/2)`                | Width of the field.                                              |
| `$field->rules('required')`         | Rules that should be applied when **updating** and **creating**. |
| `$field->creationRules('required')` | Rules that should be applied when **creating**.                  |
| `$field->updateRules('required')`   | Rules that should be applied when **updating**.                  |

## Formats

Useful formats:

| Key    | Example Result                   |
| ------ | -------------------------------- |
| `LT`   | 2:15 PM                          |
| `LTS`  | 2:15:52 PM                       |
| `L`    | 12/08/2019                       |
| `l`    | 12/8/2019                        |
| `LL`   | December 8, 2019                 |
| `ll`   | Dec 8, 2019                      |
| `LLL`  | December 8, 2019 2:15 PM         |
| `lll`  | Dec 8, 2019 2:15 PM              |
| `LLLL` | Sunday, December 8, 2019 2:15 PM |
| `llll` | Sun, Dec 8, 2019 2:15 PM         |
