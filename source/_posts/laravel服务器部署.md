---
title: laravel服务器部署
abbrlink: 59853
date: 2018-10-16 16:31:58
categories: 后端
tags:
  - laravel
  - PHP
---

用宝塔新建个站点 `laravel-blog.api` ，分配一个端口，登陆服务器，进入`www/wwwroot/laravel-blog.api`目录

用命令行克隆`github`的项目
```bash
cd ..
cd www/wwwroot/laravel-blog.api

git clone https://github.com/sweida/laravel.git

cd laravel

# 把克隆的项目移动出来根目录
mv * .[^.]* ..

cd ..

# 然后删除laravel目录
rm laravel
```

**安装 Composer 并使用 Composer 安装代码依赖**
访问 [composer](https://getcomposer.org/download/) 官网 获取下面四行代码最新版，直接粘贴执行安装 Composer
```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('SHA384', 'composer-setup.php') === '669656bab3166a7aff8a7506b8cb2d1c292f042046c5a994c43155c0be6190fa0355160742ab2e1c88d40d5be660b410') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"

# composer.phar
mv composer.phar /usr/local/bin/composer

# 进入项目目录
cd /www/wwwroot/laravel-blog.api

# 执行 composer install
composer install

```

**创建 .env 文件**
```bash
cd /www/wwwroot/laravel-blog.api

cp .env.example .env

# 根据项目实际情况修改 .env 文件，修改数据库名，登录名和密码
vim .env
```

**生成 laravel key**
```
cd /www/wwwroot/laravel-blog.api

php artisan key:generate
```

**创建数据库，执行迁移**
```bash
# 首先登录 mysql 创建一个对应项目的数据库，名字应该和 .env 文件中的一致
cd /www/wwwroot/laravel-blog.api

php artisan migrate
```

**修改权限**
```bash
# 把文件夹权限设置为777
sudo chown -R www-data:www-data /www/wwwroot/laravel-blog.api

sudo chmod -R 777 /www/wwwroot/laravel-blog.api/storage
```


### 访问项目的端口号，成功进入，但是除了根'/'目录，其它目录都是404
**解决方法：修改宝塔里`laravel-blog`项目的nginx配置文件**
```
server {  
    listen 80;  

    server_name www.laravel.com;  

    root /www/wwwroot/laravel-blog.api/public/;  
    index index.html index.php index.htm;  

    # 添加下面3行
    location / {  
        try_files $uri $uri/ /index.php?$query_string;  
    }  

    location ~ \.php$ {  
        include snippets/fastcgi-php.conf;  
    #  
    #   # With php7.0-cgi alone:  
    #   fastcgi_pass 127.0.0.1:9000;  
    #   # With php7.0-fpm:  
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;  

    }  

}
```
