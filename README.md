Features of Laravel User Activity
---------------------------------

*   Beautiful, responsive and easy UI.
*   Easy installation to existing or new Laravel application.
*   Monitor record edit, record delete, login and lockout activity.
*   Filtering user activity logs.
*   Configurable routes.
*   Custom middleware support.
*   Console command for activity log clear.

Installation
------------

Step 1 Run this command given below to install the laravel user activity package.

```
composer require haunv/laravel-user-activity
```


Step 2 Now run this artisan command.

```
php artisan user-activity:install
```


Installation finished!

Usages
------

By default, It will catch login activity and lockout (too many attempts) automatically. You can enable or disable it by config value. To keep track of your existing table record edit and delete, just use the `Loggable` trait in your model. That's it, super simple! If any user edits or delete your existing table record, it will automatically be logged into the `logs` table and you will get the details, what data deleted or edited by users.

```
<?php namespace App;

use Illuminate\Database\Eloquent\Model;
use Haruncpi\LaravelUserActivity\Traits\Loggable;

class Student extends Model
{
    use Loggable;

} 
```


![activity-log-details-preview.png](https://laravelarticle.com/site/images/1px.png)

To view user activity dashboard, browse the `/admin/user-activity`. By default, only logged in user can view the user activity dashboard. You can add any middleware restriction by `user-activity.php` config file.

```
http://example.com/admin/user-activity
```


**Dashboard Preview**

![user-activity-dashboard.png](https://laravelarticle.com/site/images/1px.png)

Available console command
-------------------------

To delete activity log data older than defined days value into the `user-activity.php` config file (default value: 7 days).

```
php artisan user-activity:delete
```


To delete activity log data older than n days.

```
php artisan user-activity:delete 30
```


To delete all activity log data

```
php artisan user-activity:delete all
```


N.B With the help of these console commands, you can make a laravel schedule for deleting user activity log data automatically. If you are a shared hosting (cPanel) user then you can read the [laravel schedule in the shared hosting](https://laravelarticle.com/laravel-scheduler-on-cpanel-shared-hosting) tutorial post.

Customization
-------------

Change the config value according to your need into the `user-activity.php` config file to customize activity log dashboard URL, custom middleware and activity log enable or disable. Default config values are given below.

```
<?php

return [
    'activated'        => true,
    'middleware'       => ['web', 'auth'],
    'route_path'       => 'admin/user-activity',
    'admin_panel_path' => 'admin/dashboard',
    'delete_limit'     => 7,

    'model' => [
        'user' => "App\User"
    ],

    'log_events' => [
        'on_edit'    => true,
        'on_delete'  => true,
        'on_login'   => true,
        'on_lockout' => true
    ]
];
```


Note: You can configure your user instance. For Laravel 8, use it `App\Models\User`

Hope this package will help you to monitor your application user activity with a beautiful and easy UI. The laravel user activity package is an open-source laravel package with CC 4.0 licence. Support the [Laravel User Activity package](https://github.com/haruncpi/laravel-user-activity) GitHub repository to make it better.

### Base Model Class Configuration

Suppose, you have a parent model class extends by the laravel eloquent model class and it has a lot of sub-class. In this situation, you can easily configure the Larave User Activity package with your base model class implementation.

Use Loggable trait In your base model class

```
class BaseModel extends Model
{
    use Loggable;
    
    ...
    ...
}
```


Now all the sub-class of the base model will be automatically logged! In case, If you don't want to log for any subclass of the base model just add a property to exclude logging.

```
class B extends BaseModel
{
    public $excludeLogging = true;
    ...
    ...
} 
```