# oc-seeder-plugin

Laravel Seeder integration for October CMS.

This plugin integrates Laravel's Factory and Database Seeder features with October CMS.

## Defining factories

To define a new Factory for your plugin, create a `factories.php` in the plugin's root folder and define your factories [as you would in Laravel](https://laravel.com/docs/6.x/database-testing#writing-factories):

```php
<?php
// plugins/yourvendor/yourplugin/factories.php

/** @var $factory Illuminate\Database\Eloquent\Factory */
$factory->define(\YourVendor\YourPlugin\Models\YourModel::class, function (\OFFLINE\Seeder\Classes\Generator $faker) {
    return [
        'name' => $faker->name,
        'number' => $faker->numberBetween(0, 6),
    ];
});
```

## Defining seeders

Add a `registerSeeder` method to your `Plugin.php` in which you seed your plugin's models:

```php
public function registerSeeder()
{
    factory(\YourVendor\YourPlugin\Models\YourModel::class, 50)->create();
}
```

## Running seeders

Simply run `php artisan plugin:seed` to run the seeders of all plugins. 

Use the `--fresh` option to refresh all seeded plugins before popuplating them with new data. Be aware that this will rollback and reinstall a plugin completely, so any plugin data will be lost.

## Included factories

This plugin includes factories for the following models:

### `\System\Models\File::class`

`factory(\System\Models\File::class)->make()` returns a `File` model with a random image from [unsplash.com](https://source.unsplash.com/). You can use it in any seeder to attach a file to a created model:

```php
// Create a model
$myModel = factory(\YourVendor\YourPlugin\Models\YourModel::class)->create();

// Attach an image
$image = factory(\System\Models\File::class)->make();
$myModel->image()->save($image);
```

Use the `portrait` state to get an image in portrait orientation:

```php
$portrait = factory(\System\Models\File::class)->state('portrait')->make();
```

There are also different size states available: `tiny` returns a `90x90` image. `huge` returns a `6000x4000` image.

For more control over the generated image, you can directly use the `ocImage` method in your factory. It returns the path to a locally stored image file. You can optionally pass in a `width` and `height` attribute.

### `\Backend\Models\User::class`

`factory(\Backend\Models\User::class)->make()` returns a Backend `User` model. You can use the `superuser`, `role:publisher` or `role:developer` states to generate a specific user type.

```php
// Build a simple backend user.
factory(\Backend\Models\User::class)->make();

// Build a superuser backend user.
factory(\Backend\Models\User::class)->states('superuser')->make();

// Build a backend user with the publisher role attached.
factory(\Backend\Models\User::class)->states('role:publisher')->make();
```

### `\RainLab\User\Models\User::class`

`factory(\RainLab\User\Models\User::class)->make()` returns a RainLab `User` model.




## Credits

This plugin is heavily inspired by [Inetis' oc-testing-plugin](https://github.com/inetis-ch/oc-testing-plugin).
