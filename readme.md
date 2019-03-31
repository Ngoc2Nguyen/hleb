# HLEB
### ![HLEB LOGO](https://phphleb.ru/logo.gif)
# 微框架 PHP Micro-Framework

爱好和非商业项目的框架。 需要PHP 7.0或更高版本。

路由>控制器>模型>页面构建器>调试面板

[Link to instructions](https://phphleb.ru/ru/v1/) (RU)

安装
-----------------------------------
启动迷你框架HLEB
1.从原始位置下载包含项目的文件夹。

使用Composer：
```html
$ composer create-project phphleb/hleb
```
2.将资源的地址分配给“public”子目录。
3.建立允许更改“存储”文件夹及其中所有文件夹和文件的所有用户的权限。

完成这些步骤后，您可以通过在浏览器的地址栏中键入先前分配的资源地址（本地或远程服务器）来验证安装。 如果安装成功，将显示带有框架徽标的停放页面。

定制
-----------------------------------
微框架HLEB中的命令字符常量在 **start.hleb.php** 文件中设置。 最初，具有此名称的文件不存在，必须从同一项目根目录中的 **default.start.hleb.php** 文件中复制。

注意！ 常量HLEB_PROJECT_DEBUG启用/禁用调试模式。 不要在公共服务器上使用调试模式。


路由
-----------------------------------
项目路由由开发人员在“/routes/main.php”文件中编译，其他具有来自“routes”文件夹的路由的文件可以插入（包含）到该文件中，这些文件一起构成路由映射。

路由由类 **Route** 方法确定，其中主要是 **get()** 。 此类的所有方法都可用，仅在路由映射中使用。

注意！ 路径文件被缓存，不应包含任何包含外部数据的代码。

```php
Route::get('/', 'Hello, world!');
```

使用 **view（)** 函数显示“/views/index.php”文件的内容（也可在控制器中使用）。
```php
Route::get('/', view('index'));
```

这是一个更复杂命名的路线的例子。 这里，$x 和 $y 值被传送到“/views/map/new.php”文件，并设置动态地址的条件（“版本”和“页面”可以采用不同的值）。 您可以使用框架的专用函数按名称调用路由。
```php
Route::get('/ru/{version}/{page}/', view('/map/new', ['x' => 59.9, 'y' => 30.3]))->where(['version' => '[a-z0-9]+', 'page' => '[a-z]+'])->name('RouteName');

```
路线组
-----------------------------------

位于路线或组之前的方法：

**type()->**, **prefix()->**, **protect()->**, **before()->**

```php
Route::prefix('/lang/')->before('AuthClassBefore')->getGroup();
  Route::get('/page/', "<h1>Page</h1>"); // /lang/page/
  Route::protect()->type('post')->get('/ajax/', '{"connect":1}'); // /lang/ajax/
Route::endGroup();
```
位于路线或团体之后的方法：

**->where()**, **->after()**

```php
Route::type(['get','post'])->before('ClassBefore')->get('/path/')->controller('ClassController')->after('ClassAfter');

```

控制器
-----------------------------------
创建具有此类内容的简单控制器：
```php
// 文件 /app/Controllers/TestController.php
namespace App\Controllers;
use App\Models\UserModel;
use Hleb\Constructor\Handlers\Request;
class TestController extends \MainController
{
    function index($value) // $value = 'friends'
    {
     $data = UserModel::getData(['id' => Request::get('id'), 'join' => $value]);
     return view('/user/profile', ['data' => $data]);
    }
}
```
您可以在路线图中使用它：

```php
Route::get('/profile/{id}/')->controller('TestController',['friends'])->where(['id' => '[0-9]+']);
```  
或

```php
Route::get('/profile/{id}/')->controller('TestController@index',['friends'])->where(['id' => '[0-9]+']);
``` 


楷模
-----------------------------------
 ```php
// 文件 /app/Models/UserModel.php
namespace App\Models;
class UserModel extends \MainModel
{
   static function getData($params)
   {
     $data = /* ... */ //对数据库的查询，返回用户数据数组。
     return $data;
   }
}
```

页面构建器
-----------------------------------
```php
Route::renderMap('index page', ['/parts/header', 'index', '/parts/footer']);
Route::get('/', render('index page'));
```

调试面板
-----------------------------------
```php
\Hleb\Main\WorkDebug::add($debug_data, 'description');
```

执照
-----------------------------------
自由 ([MIT](https://github.com/phphleb/hleb/blob/master/LICENSE) License) 


