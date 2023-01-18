# Quick start

We use `users` table come with `Laravel` for example,the structure of table is:

```sql
CREATE TABLE `users` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
  `email` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
  `password` varchar(60) COLLATE utf8_unicode_ci NOT NULL,
  `remember_token` varchar(100) COLLATE utf8_unicode_ci DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
  `updated_at` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00',
  PRIMARY KEY (`id`),
  UNIQUE KEY `users_email_unique` (`email`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci
```

And the model for this table is `App\User.php` (< Laravel 7) or `App\Models\User.php` (>= Laravel 8)

You can follow these steps to setup `CRUD` interfaces of table `users`:

## Add controller

Use the following command to create a controller for `App\User` model

```console
php artisan admin:make UserController --model=App\\User

// under windows use:
php artisan admin:make UserController --model=App\User
```

Or

```console
php artisan admin:controller App\\User
```

The above command will create the controller in `app/Admin/Controllers/UserController.php`.

## Add route

Add a route in `app/Admin/routes.php`：

```php
$router->resource('demo/users', UserController::class);
```

## Add menu item

Open http://localhost:8000/admin/auth/menu, add menu link and refresh the page, then you can find a link item in left menu bar.

> Where `uri` fills in the path part that does not contain the prefix of the route, such as the full path http://localhost:8000/admin/demo/users, just input `demo/users`, If you want to add an external link, just fill in the full url, such as `http://laravel-admin.org/`.

## Write CURD page logic

The controller `app/Admin/Controllers/UserController.php` created by the `admin:make` command is as follows:

```php
<?php

namespace App\Admin\Controllers;

use App\Models\User;
use Encore\Admin\Controllers\AdminController;
use Encore\Admin\Form;
use Encore\Admin\Grid;
use Encore\Admin\Show;

class UserController extends AdminController
{
    protected $title ='Users';

    protected function grid()
    {
        $grid = new Grid(new User());

        $grid->column('id', __('Id'));
        $grid->column('name', __('Name'));
        $grid->column('email', __('Email'));
        $grid->column('password', __('Password'));
        $grid->column('created_at', __('Created at'));
        $grid->column('updated_at', __('Updated at'));

        return $grid;
    }

    protected function detail($id)
    {
        $show = new Show(User::findOrFail($id));

        $show->field('id', __('Id'));
        $show->field('name', __('Name'));
        $show->field('email', __('Email'));
        $show->field('password', __('Password'));
        $show->field('created_at', __('Created at'));
        $show->field('updated_at', __('Updated at'));

        return $show;
    }

    protected function form()
    {
        $form = new Form(new User());

        $form->textarea('name', __('Name'));
        $form->textarea('email', __('Email'));
        $form->textarea('password', __('Password'));

        return $form;
    }
}
```

The `$title` attribute is used to set the title of this CURD module, which can be modified to any other string.

The `grid` method corresponds to the `list` page of the data. Refer to [model-grid documentation](/en/model-grid.md) to implement the related functional logic of the list page.

The `detail` method corresponds to the `details` page of the data, click on the `detail display` button in the operation column of the list page, and refer to [model-show documentation](/en/model-show.md) to realize the details page related functional logic.

The `form` method corresponds to the `create` and `edit` pages of the data. Refer to [model-form documentation](/en/model-form.md) to implement the relevant functional logic of the data creation and edit pages.

### Menu translations

Append menu titles in `menu_titles` index at your language files.

For example 'Work Units' title:

In `resources/lang/es/admin.php`

```php
...
// lowercase and replace spaces with _
'menu_titles' => [
    'work_units' => 'Unidades de trabajo'
],
```
