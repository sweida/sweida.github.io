---
title: laravel的CSRF保护
abbrlink: 34454
date: 2018-10-17 01:53:25
categories: 后端
tags:
  - laravel
  - PHP
---

跨站请求伪造（CSRF）是一种通过伪装授权用户的请求来攻击授信网站的恶意漏洞。
Laravel 默认情况下，为了安全考虑，Laravel 期望所有路由都是「只读」操作的（对应请求方式是 GET、HEAD、OPTIONS），如果路由执行的是「写入」操作（对应请求方式是 POST、PUT、PATCH、DELETE），则需要传入一个隐藏的 Token 字段`_token`以避免[跨站请求伪造攻击]（CSRF）。如果请求方式是 DELETE，但是并没有传递 `_token` 字段，所以会出现异常。

避免跨站请求伪造攻击的措施就是对写入操作采用非 GET 方式请求，同时在请求数据中添加校验 Token 字段，Laravel 也是这么做的，这个 Token 值会在渲染表单页面时通过 Session 生成，然后传入页面，在每次提交表单时带上这个 Token 值即可实现安全写入，因为第三方站点是不可能拿到这个 Token 值的，所以由第三方站点提交的请求会被拒绝，从而避免 CSRF 攻击。

`X-CSRF-Token`
除了将 CSRF 令牌作为 POST 参数进行验证外，还可以通过设置 X-CSRF-Token 请求头来实现验证，VerifyCsrfToken 中间件会检查 X-CSRF-TOKEN 请求头。实现方式如下，首先创建一个 meta 标签并将令牌保存到该 meta 标签：
```
<meta name="csrf-token" content="{{ csrf_token() }}">
```

### 禁止CSRF功能
Laravel默认是开启了，需要关闭此功能有两种方法：

** 方法一 **
打开文件：`app\Http\Kernel.php`
把这行注释掉：
```
'App\Http\Middleware\VerifyCsrfToken'

```

** 方法二 **
打开文件：`app\Http\Middleware\VerifyCsrfToken.php`
修改为：
```php
namespace App\Http\Middleware;

// 引入这个
use Closure;
use Illuminate\Foundation\Http\Middleware\VerifyCsrfToken as BaseVerifier;

class VerifyCsrfToken extends BaseVerifier {

    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
     // 加入这个方法
    public function handle($request, Closure $next)
    {
        // 使用CSRF
        //return parent::handle($request, $next);
        // 禁用CSRF
        return $next($request);
    }

    // 也可以对指定的表单提交方式使用CSRF
    // 只对GET的方式提交使用CSRF，对POST方式提交表单禁用CSRF
    public function handle($request, Closure $next)
    {
        // Add this:
        if($request->method() == 'POST')
        {
            return $next($request);
        }

        if ($request->method() == 'GET' || $this->tokensMatch($request))
        {
            return $next($request);
        }
        throw new TokenMismatchException;
    }

}
```

### 使用 `CSRF`

方法一：HTML的代码中加入：
```html
<input type="hidden" name="_token" value="{{ csrf_token() }}" />

<!-- 或者可以使用 Blade 指令 @csrf -->
<form method="POST" action="/profile">
    @csrf
    ...
</form>
```

另一种是使用cookie方式。
使用cookie方式，需要把app\Http\Middleware\VerifyCsrfToken.php修改为：
```php
namespace App\Http\Middleware;

use Closure;
use Illuminate\Foundation\Http\Middleware\VerifyCsrfToken as BaseVerifier;

class VerifyCsrfToken extends BaseVerifier {

    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        return parent::addCookieToResponse($request, $next($request));
    }

}
```
