---
title: laravel连接数据库mysql与创建数据库表
abbrlink: 42085
date: 2018-09-18 15:50:34
categories: 后端
tags:
  - laravel
  - PHP
---

### 连接数据库
Laravel中数据库配置文件为`config/database.php`，打开该文件，默认内容如下,修改mysql对应值，

```php
<?php

return [
    //默认返回结果集为PHP对象实例
    'fetch' => PDO::FETCH_CLASS,
    //默认数据库连接为mysql，可以在.env文件中修改DB_CONNECTION的值
    'default' => env('DB_CONNECTION', 'mysql'),

    'connections' => [
        //sqlite数据库相关配置
        'sqlite' => [
            'driver' => 'sqlite',
            'database' => storage_path('database.sqlite'),
            'prefix' => '',
        ],
        //mysql数据库相关配置
        'mysql' => [
            'driver' => 'mysql',
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'laraveldb'),
            'username' => env('DB_USERNAME', 'root'),
            'password' => env('DB_PASSWORD', 'root'),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'strict' => true,
            'engine' => null,
        ],    
      ];
    ];
```

修改.env对应值
```
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:x2NKVZImZBLgeCYg+1uTcw64hxi5SeNbnI2oqww7TRw=
APP_DEBUG=true
APP_URL=http://localhost

LOG_CHANNEL=stack

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laraveldb
DB_USERNAME=root
DB_PASSWORD=root
```
* 注：执行migration时, 如果报了这个错误 `could not find driver`
需要允许pdo扩展，需要将php安装文件`php.ini`文件中将`pdo_mysql`这一行前的分号删除。

### 创建数据库表 
设计数据库 使用migration
```
php artisan make:migration create_table_users --create=users
```

migration users文件up方法 
```php
// create_table_users.php
public function up()
{
    Schema::create('users', function (Blueprint $table) {
        $table->increments('id');
        $table->string('username')->unique();
        $table->string('email')->unique()->nullable();
        $table->string('phone')->unique()->nullable();
        $table->string('password');
        $table->text('intro')->nullable();
        $table->timestamps();
    });
}
```

查看执行运行结果，如果没报错再执行添加表
```
php artisan migrate --pretend
```

如果表存在，删除数据库表
```
<!-- mysql -->
drop table users;
```

添加数据库表
```
php artisan migrate

# Migrating: 2018_09_18_024606_create_table_users
# Migrated:  2018_09_18_024606_create_table_users
```

添加表成功
![数据表](42085/table.png)