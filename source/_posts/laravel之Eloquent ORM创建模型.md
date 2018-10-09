---
title: laravel之Eloquent ORM创建模型
abbrlink: 61614
date: 2018-09-18 16:58:58
categories: 后端
tags:
  - laravel
  - PHP
---

### 修改laravel默认时区
laravel 和 php 一样 默认的是英国的格林尼治时间 和我们相差大概8小时
laravel 框架其实 内置了设置时区的方式
打开 `config` 下的 `app.php` 找到 `timezone`

```
'timezone' => 'UTC',
# 修改成
'timezone' => 'PRC',
```

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
