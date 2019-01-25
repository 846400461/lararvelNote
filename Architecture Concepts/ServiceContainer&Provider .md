## Service Container（服务容器）和 Provider (服务提供者)

![描述图](https://github.com/846400461/lararvelNote/blob/master/image/serviceContainer&Provider.png?raw=true)

服务容器是存放服务类的容器，想要实例服务类，可以通过

```php+HTML
$test=App::make('Superguy');
```

其中App是[Facades](https://laravel-china.org/docs/laravel/5.7/facades/2251)(Facades 为应用的 [服务容器](https://laravel-china.org/docs/laravel/5.7/container) 提供了一个「静态」 接口。Laravel 自带了很多 Facades，可以访问绝大部分功能。`Laravel Facades` 实际是服务容器中底层类的 「静态代理」 ，相对于传统静态方法，在使用时能够提供更加灵活、更加易于测试、更加优雅的语法)

### 构建自定义服务类的实例：

#### 服务类或接口

Superguy.php

```php
<?php
/**
 *autor：caibin
 */
namespace app\lib\Superman;

class Superguy
{

    function __construct()
    {
        # code...
    }
}
```

#### 被注入（绑定）类（已实现的）

Aman.php

```php
<?php
/**
 *
 */
/**
 *
 */
namespace app\lib\Superman;


class Aman implements bigPower
{
    protected $name;
    function __construct()
    {
        # code...
    }

    public function UserPower($powerName)
    {
        if(is_string($powerName))
        {
            echo $powerName;
        }
        else
        {
            echo "not a string\n";
        }
    }

    public function biubiuDa()
    {
        return "biubiu gun\n";
    }
}

```

接口：xPower.php

```php
<?php

namespace app\lib\Superman;

interface bigPower{
    public function biubiuDa();
}

```



####生成服务提供者

> php artisan make:provider testServiceProvider

#### 在服务提供者内绑定服务类

testServiceProvider.php

```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;

class testServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap services.
     *
     * @return void
     */
    public function boot()
    {
        //
    }

    /**
     * Register services.
     *
     * @return void
     */
    public function register()
    {
         $this->app->bind('Superguy','app\lib\Superman\Superguy');

        $this->app->bind('app\lib\Superman\Superguy','app\lib\Superman\Aman');
    }
}
```

#### 注册服务提供者

在config\app.php内注册

```php

   'providers' => [

        /*
         * Laravel Framework Service Providers...
         */
        Illuminate\Auth\AuthServiceProvider::class,
        Illuminate\Broadcasting\BroadcastServiceProvider::class,
        Illuminate\Bus\BusServiceProvider::class,
        Illuminate\Cache\CacheServiceProvider::class,
        Illuminate\Foundation\Providers\ConsoleSupportServiceProvider::class,
        Illuminate\Cookie\CookieServiceProvider::class,
        Illuminate\Database\DatabaseServiceProvider::class,
        Illuminate\Encryption\EncryptionServiceProvider::class,
        Illuminate\Filesystem\FilesystemServiceProvider::class,
        Illuminate\Foundation\Providers\FoundationServiceProvider::class,
        Illuminate\Hashing\HashServiceProvider::class,
        Illuminate\Mail\MailServiceProvider::class,
        Illuminate\Notifications\NotificationServiceProvider::class,
        Illuminate\Pagination\PaginationServiceProvider::class,
        Illuminate\Pipeline\PipelineServiceProvider::class,
        Illuminate\Queue\QueueServiceProvider::class,
        Illuminate\Redis\RedisServiceProvider::class,
        Illuminate\Auth\Passwords\PasswordResetServiceProvider::class,
        Illuminate\Session\SessionServiceProvider::class,
        Illuminate\Translation\TranslationServiceProvider::class,
        Illuminate\Validation\ValidationServiceProvider::class,
        Illuminate\View\ViewServiceProvider::class,

        /*
         * Package Service Providers...
         */

        /*
         * Application Service Providers...
         */
        App\Providers\AppServiceProvider::class,
        App\Providers\AuthServiceProvider::class,
        // App\Providers\BroadcastServiceProvider::class,
        App\Providers\EventServiceProvider::class,
        App\Providers\TelescopeServiceProvider::class,
        App\Providers\RouteServiceProvider::class,
        //本实例注册的服务提供者
        App\Providers\testServiceProvider::class,

    ],
```

#### 调用服务类

web.php

```php
<?php

/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/


Route::get('/', function () {
    $test=App::make('Superguy');
    $test=App::make('test');
    $str=$test->UserPower("caibin");
    $str=$str." ".$test2->biubiuDa();
    return $str;
});

Auth::routes();

Route::get('/home', 'HomeController@index')->name('home');

```



