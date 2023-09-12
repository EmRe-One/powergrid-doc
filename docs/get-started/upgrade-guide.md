# Upgrade Guide

## Upgrade From V4

PowerGrid is now on version 5.x.

This page will provide you with important information to upgrade from v4.x.

### Dependency Upgrades

The following items have been updated in this release:

* PHP 8.1+
* [Laravel Framework](https://laravel.com/) 10.0+
* [Laravel Livewire](https://livewire.laravel.com/) 3.0+
* [Tailwind](https://tailwindcss.com/) v3+

---

## Solve the necessary imports

#### Rename Filter class

```php{4}
// Change:
use PowerComponents\LivewirePowerGrid\Filters\Filter;
// To:
use PowerComponents\LivewirePowerGrid\Facades\Filter;
```

---

#### Rename Rule class

```php{4}
// Change:
use PowerComponents\LivewirePowerGrid\Rules\Rule;
// To:
use PowerComponents\LivewirePowerGrid\Facades\Rule;
```

#### Change `PowerGrid::eloquent` to `PowerGrid::columns`

```php{10,14}
// Change:
use PowerComponents\LivewirePowerGrid\PowerGridEloquent;

public function addColumns(): PowerGridEloquent
{
    return PowerGrid::eloquent()
    // 
}
// To:
use PowerComponents\LivewirePowerGrid\PowerGridColumns;

public function addColumns(): PowerGridColumns
{
    return PowerGrid::columns()
    // 
}
```

#### Remove ActionButton Trait

```php{1,5}
use PowerComponents\LivewirePowerGrid\Traits\ActionButton;

final class PowerGridTable extends PowerGridComponent
{
   use ActionButton;
   // ---
}
```

#### If you previously use row action button add Column::action()

```php{5}
public function columns(): array
{
    return [
        ...
        Column::action('Action'),
    ];
}
```

---

#### Change Button caption to Button slot

```php{10}
// PowerGrid 4
   Button::add('bulk-demo')
       // 🚫 Before 
       ->caption('Bulk Action')
       ->class('...')
       
// PowerGrid 5
    Button::add('bulk-demo')
       // ✅ After 
       ->slot('Bulk Action')
       ->class('...')
```

---

#### Change Button emit, emitTo, emitSelf

* Button::emit
```php{10}
// PowerGrid 4 - Livewire v2
   Button::add('bulk-demo')
       // 🚫 Before 
       ->emit('event', [], false) // string $event, array|\Closure $params, bool $singleParam = false)
       ->class('...')
       
// PowerGrid 5 - Livewire v3 sintax
    Button::add('bulk-demo')
       // ✅ After 
       ->dispatch('event', ['dishId' = 1]) // string $event, array $params)
       ->class('...')
```

* Button::emitTo
```php{10}
// PowerGrid 4 - Livewire v2
   Button::add('bulk-demo')
       // 🚫 Before 
       ->emitTo('to', [], false) // string $to, string $event, array|\Closure $params, bool $singleParam = false)
       ->class('...')
       
// PowerGrid 5 - Livewire v3 sintax
    Button::add('bulk-demo')
       // ✅ After 
       ->dispatchTo('event', ['dishId' = 1]) // string $to, string $event, array $params)
       ->class('...')
```

### Change exportable keys on config

`config/livewire-powergrid.php`

```php{8-9,16-17}
'exportable' => [
     'default'      => 'openspout_v4',
     'openspout_v4' => [
         // 🚫 Before 
         'xlsx' => \PowerComponents\LivewirePowerGrid\Services\OpenSpout\v4\ExportToXLS::class,
         'csv'  => \PowerComponents\LivewirePowerGrid\Services\OpenSpout\v4\ExportToCsv::class,
         // ✅ After 
         'xlsx' => \PowerComponents\LivewirePowerGrid\Components\Exports\OpenSpout\v4\ExportToXLS::class,
         'csv'  => \PowerComponents\LivewirePowerGrid\Components\Exports\OpenSpout\v4\ExportToCsv::class,
     ],
     'openspout_v3' => [
         // 🚫 Before 
         'xlsx' => \PowerComponents\LivewirePowerGrid\Services\OpenSpout\v3\ExportToXLS::class,
         'csv'  => \PowerComponents\LivewirePowerGrid\Services\OpenSpout\v3\ExportToCsv::class,
         // ✅ After 
         'xlsx' => \PowerComponents\LivewirePowerGrid\Components\Exports\OpenSpout\v3\ExportToXLS::class,
         'csv'  => \PowerComponents\LivewirePowerGrid\Components\Exports\OpenSpout\v3\ExportToCsv::class,
     ],
],
```
