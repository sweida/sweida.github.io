---
title: laravel模型工厂和数据填充
abbrlink: 37793
date: 2018-10-22 23:10:37
categories: 后端
tags:
  - laravel
  - PHP
---

### 生成工厂
使用Artisan命令创建工厂
```bash
php artisan make:factory userFactory

# 或者生成指定模型的名称的工厂
php artisan make:factory userFactory --model=user
```

### 写作工厂
您可能需要在执行测试之前将几条记录插入数据库
```php
// 生成文章
$factory->define(App\article::class, function (Faker $faker) {
    return [
        'title' => $faker->sentence,
        'content' => $faker->text,
        'classify' => $faker->randomElement($array = array ('前端', '后端', '工具', '随写')),
    ];
});
```

一些比较常用的字段
```
email 邮箱
unique()->safeEmail 唯一值的邮箱
bcrypt('123456') 生成加密密码
name 名字
word 单词
sentence 短句
text 段落
randomElement($array = array ('前端', '后端', '工具', '随写')) 数组中的随机一个
numberBetween($min = 1, $max = 10) 2个数中的随机数
date($format = 'Y-m-d') 年月日格式
url 链接
```

### 数据填充：写播种机
执行Artisan命令`make:seeder`
框架生成的所有播种机都将放在目录中：`database/seeds`
```bash
php artisan make:seeder UsersTableSeeder
```

在 DatabaseSeeder 类中，你可以使用 call 方法执行其他填充类，使用 call 方法允许你将数据库填充分解成多个文件，这样单个填充器类就不会变得无比巨大，只需简单将你想要运行的填充器类名传递过去即可：
```php
public function run(){
    $this->call(UsersTableSeeder::class);
    $this->call(PostsTableSeeder::class);
    $this->call(CommentsTableSeeder::class);
}
```

定义工厂后，可以factory在测试或种子文件中使用全局函数来生成模型实例。我们使用make方法创建模型，但不会将它们保存到数据库中：
```php
public function UsersTableSeeder()
{
    $user = factory(App\User::class)->make();
    // 或者创建多条数据
    $users = factory(App\User::class, 3)->make();

    // create方法不仅创建模型实例，还使用Eloquent的save方法将它们保存到数据库中
    $user = factory(App\User::class)->create();
}
```

覆盖属性
如果要覆盖模型的某些默认值，可以将值数组传递给make方法。只有指定的值才会被替换，而其余值仍然设置为工厂指定的默认值：
```php
$user = factory(App\User::class)->make([
    'name' => 'Abigail',
]);

$user = factory(App\User::class)->create([
    'name' => 'Abigail',
]);
```

使用Artisan命令为数据库设定数据
默认情况下，`db:seed` 命令运行 `DatabaseSeeder` 类，不过，你也可以使用 `--class` 选项来指定你想要运行的独立的填充器类
```bash
# 生成数据，叠加之前的
php artisan db:seed

# 只生成指定表的数据
php artisan db:seed --class=UsersTableSeeder

# 清空数据表，并生成数据
php artisan migrate:refresh --seed
```
