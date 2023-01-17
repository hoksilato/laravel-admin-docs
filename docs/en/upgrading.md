# Upgrading

## View current version

```console
composer show encore/laravel-admin
```

## Update to the latest version

```console
composer require encore/laravel-admin -vvv
```

## Update to development version

```console
composer require encore/laravel-admin:dev-master -vvv
```

## Update the specified version

```console
composer require encore/laravel-admin: 1.6.15 -vvv
```

## Precautions

Since the front-end resources of each version may be modified, you need to run the following commands after the update.

```console
// Force to publish release assets files
php artisan vendor:publish --tag=laravel-admin-assets --force

// Force to publish release lang files
php artisan vendor:publish --tag=laravel-admin-lang --force

// Clean up the view cache
php artisan view:clear
```

Finally, don't forget to clean up the browser cache.
