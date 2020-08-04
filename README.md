# oc-seeder-plugin

Laravel Seeder integration for October CMS

This plugin integrates Laravel's Factory and Database Seeder features with October CMS.

## Defining factories

To define a new Factory for your plugin, create a `factories.php` in the plugin's root folder and define your factories [as you would in Laravel](https://laravel.com/docs/6.x/database-testing#writing-factories):

```php
<?php

/** @var $factory Illuminate\Database\Eloquent\Factory */
$factory->define(\YourVendor\YourPlugin\Models\YourModel::class, function (\OFFLINE\Seeder\Classes\Generator $faker) {
    return [
        'name' => $faker->name,
        'number' => $faker->numberBetween(0, 6),
    ];
});
```

## Defining seeders

Add a `registerSeeder` method to your `Plugin.php` in which you seed your plugin' models:

```php
public function registerSeeder()
{
    factory(\YourVendor\YourPlugin\Models\YourModel::class, 50)->create()->each(function ($model) {
        $model->image()->save(factory(\System\Model\File::class)->make());
    });
}
```

## Running seeders

Simply run `php artisan plugin:seed` to run the seeders of all plugins.

## Credits

This plugin is heavily inspired by [Inetis' oc-testing-plugin](https://github.com/inetis-ch/oc-testing-plugin).