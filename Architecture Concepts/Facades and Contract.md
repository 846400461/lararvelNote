## Facades and Contract

### Contracts

我认为Contract与 php 的接口相似，不过其中加入了laravel的服务容器。在函数的形参里面声明参数实现了什么接口，laravel就会自动实例化这个参数，这样可以减少初始化步骤，前提是这个接口要在服务容器中注册。

### Contracts实例

Superguy.php

```php
<?php
/**
 *
 */
namespace app\lib\Superman;

interface Superguy
{


}
```

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


class Aman implements Superguy
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

testServiceProvider.php

```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
//use app\lib\Superman\Aman;
//use app\lib\Superman\Superguy;

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
        $this->app->bind('Superguys','app\lib\Superman\Superguy');

        $this->app->bind('app\lib\Superman\Superguy','app\lib\Superman\Aman');




    }
}

```

provider需要在config/app.php注册 这里就省略了

调用Contracts

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
use app\lib\Superman\Superguy as Superguys;

Route::get('/', function (Superguys $test) {

    $str=$test->UserPower("caibin");
    $str=$str." ".Superman::biubiuDa();
    return $str;
});

Auth::routes();

Route::get('/home', 'HomeController@index')->name('home');

```



### Facades

```php
<?php
/**
 *
 * @authors Your Name (you@example.org)
 * @date    2019-01-25 19:40:24
 * @version $Id$
 */
namespace app\myfacades;

use Illuminate\Support\Facades\Facade;


class Superman extends Facade {

    protected static function getFacadeAccessor()
    {
        return 'Superguys';
    }
}

```

