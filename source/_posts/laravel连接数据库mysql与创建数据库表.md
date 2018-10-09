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
Laravel中数据库配置文件为`config/database.php`，打开该文件，默认内容如下,不需要修改这里的:

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
            'database' => env('DB_DATABASE', 'forge'),
            'username' => env('DB_USERNAME', 'forge'),
            'password' => env('DB_PASSWORD', ''),
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
DB_PORT=3306           // 端口号
DB_DATABASE=laraveldb  // 数据库名
DB_USERNAME=root       // 用户名
DB_PASSWORD=root       // 数据库密码
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

### 可用的列类型
模式构建器包含您在构建表时可能指定的各种列类型：

命令    |   描述
-------      |     -------
`$table->boolean('confirmed');`   |   	BOOLEAN等效列。
`$table->char('name', 100);`    |   	CHAR等效列，可选长度。
`$table->date('created_at');`   |   	DATE等效列。
`$table->float('amount', 8, 2);`    |   	FLOAT等效列，具有精度（总位数）和刻度（十进制数字）。
`$table->increments('id');`   |   	自动递增UNSIGNED INTEGER（主键）等效列。
`$table->ipAddress('visitor');`   |   	IP地址等效列。
`$table->json('options');`    |   	JSON等效列。
`$table->longText('description');`    |   	LONGTEXT等效列。
`$table->string('name', 100);`    |   	VARCHAR等效列，可选长度。
`$table->text('description');`    |   	TEXT等效列。
`$table->time('sunrise');`    |   	TIME等效列。
`$table->timestamps();`   |   	添加`created_at`和`updated_at`等效列。
`$table->year('birth_year');`   |   	YEAR等效列。

### 列修饰符
在向数据库表添加列时可以使用多个列“修饰符”。例如，要使列“可为空”，您可以使用以下nullable方法：
下面是所有可用列修饰符的列表。此列表不包括索引修饰符：

修改  |    描述
-------      |     -----
`->unique()`          |   唯一值
`->nullable()`        | 	可以为空
`->nullable($value = true)`   | 	允许（默认情况下）将`null`值插入列中
`->default($value)`   | 	为列指定“默认”值
`->after('column')`	  |   将列放在“另一列”之后（MySQL）
`->autoIncrement()`   | 	将INTEGER列设置为自动增量（主键）
`->charset('utf8')`   | 	为列指定字符集（MySQL）
`->collation('utf8_unicode_ci')`    | 	为列指定排序规则（MySQL / SQL Server）
`->comment('my comment')`   | 	在列中添加注释（MySQL）
`->first()`   | 	将列“first”放在表中（MySQL）
`->storedAs($expression)`   | 	创建存储生成的列（MySQL）
`->unsigned()`    | 	将INTEGER列设置为UNSIGNED（MySQL）
`->useCurrent()`    | 	设置TIMESTAMP列以使用CURRENT_TIMESTAMP作为默认值
`->virtualAs($expression)`    | 	创建虚拟生成列（MySQL）


查看数据库执行运行结果，如果没报错再执行添加表
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

回滚迁移
```
php artisan migrate:rollback

# 回滚最后五次迁移
php artisan migrate:rollback --step=5
```

往表格新添字段
```bash
# 往表users新增votes字段
php artisan make:migration add_votes_to_users_table --table=users
```
