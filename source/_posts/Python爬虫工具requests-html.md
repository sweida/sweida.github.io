---
title: Python爬虫工具requests-html
abbrlink: 47067
date: 2018-06-29 14:25:05
categories: 后端
tags:
  - Python
comments: true
---

requests 作者 kennethreitz 出了一个新库 requests-html，requests-html 是基于现有的框架 PyQuery、Requests、lxml、beautifulsoup4等库进行了二次封装，作者将Requests设计的简单强大的优点带到了该项目中。

[requests官方中文教程](http://docs.python-requests.org/zh_CN/latest/user/quickstart.html)
[requests-html官方教程](https://html.python-requests.org)

### 安装
```
pip install requests-html
```

### 开始使用
```python
from requests_html import HTMLSession
session = HTMLSession()
```

### GET和POST请求
```python
# GET请求
url = "http://kaoshi.edu.sina.com.cn/college/scorelist?tab=batch&wl=1&local=2&batch=&syear=2013"
r = session.get(url)
r.encoding='utf-8'  # 解决中文乱码问题
print(r.text)
# 获取的网页的内容存储到本地
with open('test.html','wb') as f:
    f.write(r.content)

# POST请求
url = 'https://shuju.wdzj.com/plat-info-target.html'
params = {'wdzjPlatId': 59,'type': 1, 'target1': 1, 'target2': 0}
r = session.post(url, params=params)
print(r.text)

###定制请求头
headers = {'user-agent': 'my-app/0.0.1'}
r = session.get(url, headers=headers)
```

### 获取页面链接
```python
r = session.get('http://www.w3school.com.cn')
print(r.html.links)

# 输出 (太多，中间省略部分)
{'http://www.w3ctech.com/', '/glossary/index.asp', '/html5/html5_quiz.asp', '/php/index.asp', '/asp/index.asp', '/php/php_ref_date.asp', 'http://wetest.qq.com/?from=links_w3school', '/asp/asp_ref.asp', '/tags/index.asp', '/xmldom/index.asp', '/example/csse_examples.asp', '/w.asp', '/index.html', 'http://weibo.com/w3schoolcomcn', '/ws.asp', '/b.asp', '/cssref/index.asp', '/jquerymobile/index.asp',
...
'/xsl/xsl_languages.asp',
'/example/html_examples.asp'}

# 获取绝对地址
t = r.html.absolute_links
print(t)
{'http://www.w3school.com.cn/media/index.asp',
'http://www.w3school.com.cn/glossary/index.asp', 
'http://www.w3school.com.cn/php/php_ref.asp', 
'http://www.w3school.com.cn/site/index.asp', 
...
'http://www.w3school.com.cn/asp/asp_quiz.asp'}
```

### 使用JQuery选择器
```python
# 获取3cschoool首页左侧的菜单列表  first=True表示找到的第一个‘HTML教程’
menuList = r.html.find('#navsecond > ul', first=True)
print(menuList.text)

# 输出
HTML
HTML5
XHTML
CSS
CSS3
TCP/IP

# 找出所有菜单的标题和链接
menuList = r.html.find('#navsecond > ul')
for menu in menuList:
    print(menu.text)  # 获得标题
    print(menu.absolute_links)  # 获得链接

# 输出
HTML
HTML5
XHTML
CSS
CSS3
TCP/IP
{'http://www.w3school.com.cn/html5/index.asp', 'http://www.w3school.com.cn/css/index.asp', 'http://www.w3school.com.cn/html/index.asp', 'http://www.w3school.com.cn/css3/index.asp', 'http://www.w3school.com.cn/xhtml/index.asp', 'http://www.w3school.com.cn/tcpip/index.asp'}
JavaScript
HTML DOM
jQuery
jQuery Mobile
AJAX
JSON
DHTML
E4X
WMLScript
{'http://www.w3school.com.cn/jquerymobile/index.asp', 'http://www.w3school.com.cn/js/index.asp', 'http://www.w3school.com.cn/e4x/index.asp', 'http://www.w3school.com.cn/wmlscript/index.asp', 'http://www.w3school.com.cn/jquery/index.asp', 'http://www.w3school.com.cn/htmldom/index.asp', 'http://www.w3school.com.cn/ajax/index.asp', 'http://www.w3school.com.cn/json/index.asp', 'http://www.w3school.com.cn/dhtml/index.asp'}
PHP
SQL
ASP
ADO
VBScript
{'http://www.w3school.com.cn/asp/index.asp', 'http://www.w3school.com.cn/php/index.asp', 'http://www.w3school.com.cn/ado/index.asp', 'http://www.w3school.com.cn/vbscript/index.asp', 'http://www.w3school.com.cn/sql/index.asp'}
XML
DTD
XML DOM
XSL
XSLT
XSL-FO
XPath
XQuery
XLink
XPointer
Schema
XForms
WAP
SVG
{'http://www.w3school.com.cn/schema/index.asp', 'http://www.w3school.com.cn/xquery/index.asp', 'http://www.w3school.com.cn/xml/index.asp', 'http://www.w3school.com.cn/xslfo/index.asp', 'http://www.w3school.com.cn/xlink/index.asp', 'http://www.w3school.com.cn/wap/index.asp', 'http://www.w3school.com.cn/xforms/index.asp', 'http://www.w3school.com.cn/xsl/xsl_languages.asp', 'http://www.w3school.com.cn/dtd/index.asp', 'http://www.w3school.com.cn/xpath/index.asp', 'http://www.w3school.com.cn/svg/index.asp', 'http://www.w3school.com.cn/xmldom/index.asp', 'http://www.w3school.com.cn/xsl/index.asp'}
Web Services
WSDL
SOAP
RSS
RDF
{'http://www.w3school.com.cn/rdf/index.asp', 'http://www.w3school.com.cn/soap/index.asp', 'http://www.w3school.com.cn/webservices/index.asp', 'http://www.w3school.com.cn/rss/index.asp', 'http://www.w3school.com.cn/wsdl/index.asp'}
ASP.NET
Web Pages
Razor
MVC
Web Forms
{'http://www.w3school.com.cn/aspnet/razor_intro.asp', 'http://www.w3school.com.cn/aspnet/mvc_intro.asp', 'http://www.w3school.com.cn/aspnet/index.asp', 'http://www.w3school.com.cn/aspnet/webpages_intro.asp', 'http://www.w3school.com.cn/aspnet/aspnet_intro.asp'}
网站构建
万维网联盟 (W3C)
浏览器信息
网站品质
语义网
职业规划
网站主机
网络媒体
{'http://www.w3school.com.cn/semweb/index.asp', 'http://www.w3school.com.cn/hosting/index.asp', 'http://www.w3school.com.cn/browsers/index.asp', 'http://www.w3school.com.cn/quality/index.asp', 'http://www.w3school.com.cn/site/index.asp', 'http://www.w3school.com.cn/w3c/index.asp', 'http://www.w3school.com.cn/careers/index.asp', 'http://www.w3school.com.cn/media/index.asp'}

# 获取搜索框属性 attrs
search = r.html.find('#searchui > form', first=True)
t = search.attrs
print(t)

# 输出
{'method': 'get', 'id': 'searchform', 'action': 'http://www.google.com.hk/search'}
```

### 实战
代码撸多了，让我们看会妹纸，以美桌网（ www.win4000.com ）为例，打开网站，观察到这是个列表，图片是缩略图，要想保存图片到本地，当然需要高清大图
![缩略图](47067/001.png)
```python
from requests_html import HTMLSession
import requests

session = HTMLSession()


# 背景图片地址
url = "http://www.win4000.com/wallpaper_2285_0_10_1.html"
r = session.get(url)

# 手动新建bg文件夹，保存图片到bg/目录
def save_image(url, title):
    img_response = requests.get(url)
    with open('./bg/'+title+'.jpg', 'wb') as file:
        file.write(img_response.content)

# 查找页面中图片列表，找到链接，
# 点击链接，访问查看大图，并获取大图地址pic-large
items_img = r.html.find('ul.clearfix > li > a')
for img in items_img:
    img_url = img.attrs['href']
    if "/wallpaper_detail" in img_url:
        r = session.get(img_url)          # 解析图片详情
        item_img = r.html.find('img.pic-large', first=True)
        url = item_img.attrs['src']       # 大图图片地址
        title = item_img.attrs['title']   # 图片标题
        print(url+title)
        save_image(url, title)
```
