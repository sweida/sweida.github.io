---
title: laravel上传图片、预览和删除
abbrlink: 47459
date: 2018-10-24 01:26:57
categories: 后端
tags:
  - laravel
  - PHP
---

### 上传图片
```php
public function img_upload(Request $request){
    // store 图片存放的地址
    $file = $request->file('file')->store('/public/ad/' . date('Y-m-d'));
    return ['status' => 1, 'data' => $file];
}

// 成功后返回data
// data: 'public/ad/2018-10-24/bUGSlt805MoL4VozkEDIxtq8VoIsoK46rvLsJf7c.jpeg'
```
`storage\app\public` 目录下就会多了一个文件

>注：如果上传报错`Unable to guess the mime type as no guessers are available`
打开php配置文件`php.ini`，添加
```
extension=php_fileinfo.dll
```

### 预览图片
想要预览上传的图片
执行Artisan生成可以预览的目录
```
php artisan storage:link
```
然后根目录下的`public`里就会生成一个文件夹`storage`，里面的目录和 `storage\app\public`一致的
就可以在浏览器预览刚才上传的图片，但是要注意的是预览的地址`public`要换成`storage`
```javascript
// 前端javascript替换
url = res.data.replace('public', 'storage')
```

```
# 预览地址
http://localhost:7000/storage/ad/2018-10-24/bUGSlt805MoL4VozkEDIxtq8VoIsoK46rvLsJf7c.jpeg
```

### 删除上传的文件

```php
// 需要引入Storage
use Illuminate\Support\Facades\Storage;

public function delete_upload() {
    $url = 'public/ad/2018-10-24/bUGSlt805MoL4VozkEDIxtq8VoIsoK46rvLsJf7c.jpeg'
    $file = Storage::delete(rq('url'));
    return $file ?
        ['status' => 1, 'msg' => '删除成功']:
        ['status' => 0, 'msg' => '删除失败'];
}
```
