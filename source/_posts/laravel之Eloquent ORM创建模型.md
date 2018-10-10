---
title: laravel之Eloquent ORM创建模型
abbrlink: 61614
date: 2018-09-18 16:58:58
categories: 后端
tags:
  - laravel
  - PHP
---

### 创建模型 Eloquent ORM
首先，让我们创建一个Eloquent模型，创建模型实例的最简单方法是使用`Artisan`命令：`make:model`
```bash
php artisan make:model article

# 如果要在生成模型时生成数据库迁移，可以使用--migration或-m选项：
php artisan make:model article --migration
# 或者
php artisan make:model article -m

# 将自动生成一个create_aritcle_table.php的数据库迁移文件
```

### 表名
请注意，我们没有告诉Eloquent我们的`article`模型使用哪个表。按照惯例，类的复数名称将用作表名，除非明确指定了其他名称。因此，在这种情况下，Eloquent将假设`article`模型将记录存储在`articles`表中。
你可以通过table在模型上定义属性来指定自定义表：
```php
namespace App;
use Illuminate\Database\Eloquent\Model;
class article extends Model
{
    protected $table = 'my_flights';
}
```

### mysql添加数据库
```
insert into users (username, `password`) value('佟丽娅', '123456')
```

### 模型工厂
修改`database`目录下的`factories`目录的`UserFactory.php`文件，添加一个模型工厂
```php
// User是模型model
$factory->define(App\User::class, function (Faker $faker) {
    return [
        // 需要填充的字段
        'username' => $faker->name,
        'email' => $faker->unique()->safeEmail,
        'password' => bcrypt('123456'), // secret
    ];
});
```

### 然后填充数据
修改`database`目录下的`seeds`目录的`DatabaseSeeder.php`文件
```php
public function run()
{
    // 往User填充20条随机数据
    factory(\App\User::class, 20)->create();
    // 将第一条数据修改为下面
    $user = \App\User::find(1);
    $user->username = '佟丽娅';
    $user->email = '123@qq.com';
    $user->password = bcrypt('123456');
    $user->save();
}
```

执行Artisan命令,填充数据
```bash
php artisan db:seed

# 或者：使用 --class 选项来指定一个特定的 seeder 类
php artisan db:seed --class=UsersTableSeeder

# 或者：清空数据库，再填充数据
php artisan migrate:refresh --seed
```

### 数据库查找
使用find或检索单个记录first。这些方法返回单个模型实例，而不是返回模型集合：
```
// 查找索引为1的
$user = App\User::find(1);
// 或者
$user = App\User::where('id', 1)->first();

$builder->where('age', '>', 200);
```

也可以find使用主键数组调用该方法，这将返回匹配记录的集合：
```
$user = App\User::find([1, 2, 3]);
```

** 更新数据 **
```
$user = App\User::find(1);

$user->name = '佟丽娅';

$user->save();
```

** 删除数据 **
在delete模型实例上调用该方法：
```
$user = App\User::find(1);

$user->delete();
```

按键删除现有模型
如果知道模型的主键，则可以通过调用destroy方法删除模型而不检索它，destroy方法还将接受多个主键，主键数组或主键集合：
```
App\User::destroy(1);

App\User::destroy(1, 2, 3);

App\User::destroy([1, 2, 3]);

App\User::destroy(collect([1, 2, 3]));
```

按查询删除模型
```
$deletedRows = App\User::where('id', 1)->delete();
```

### 软删除
除了实际从数据库中删除记录外，Eloquent还可以“软删除”模型。模型被软删除后，实际上不会从数据库中删除它们。而是deleted_at在模型上设置属性并将其插入到数据库中。如果模型具有非空deleted_at值，则模型已被软删除。

1.将deleted_at列添加到数据库表中。Laravel 架构构建器包含一个用于创建此列的帮助器方法：
```
Schema::table('flights', function ($table) {
    $table->softDeletes();
});
```

2.使用模型上的特征并将列添加到属性中：`Illuminate\Database\Eloquent\SoftDeletes`、`deleted_at`、`$dates`
```php
namespace App;

use Illuminate\Database\Eloquent\Model;
// 添加这行
use Illuminate\Database\Eloquent\SoftDeletes;

class Flight extends Model
{
    // 添加下面2行
    use SoftDeletes;
    protected $dates = ['deleted_at'];
}
```

** 恢复软删除 **
```
return $this::withTrashed()->find(1)->restore() ?
  ['status' => 1, 'msg' => '删除成功'] :
  ['status' => 0, 'msg' => 'db delete failed'];
```

** 永久删除软删除的模型 **
有时您可能需要从数据库中真正删除模型。要从数据库中永久删除软删除的模型，使用forceDelete方法：
```
$article->forceDelete();
```
