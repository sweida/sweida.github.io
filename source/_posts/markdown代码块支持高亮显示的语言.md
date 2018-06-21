---
title: markdown代码块支持高亮显示的语言
abbrlink: 633
date: 2018-06-20 23:59:04
categories: 工具
tags:
- markdown
---

markdown里对代码块的引用语法是三个撇[ \`\`\` ],在其后可增加代码名称,比如java,js等标记该部分代码的类型.之后在页面展现的时候就可以高亮显示关键字了.

比如下面这段代码:
>\`\`\`javascript
var xiaoming = {
&nbsp;&nbsp;name: '小明',
&nbsp;&nbsp;birth: 1990,
&nbsp;&nbsp;age: function () {
&nbsp;&nbsp;&nbsp;&nbsp;var y = new Date().getFullYear();
&nbsp;&nbsp;&nbsp;&nbsp;return y - this.birth;
&nbsp;&nbsp;}
};
\`\`\`

```javascript
var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var y = new Date().getFullYear();
        return y - this.birth;
    }
};
```

整理了一下表格,看看markdown到底支持多少种语言高亮显示.

 名称        |     关键字     
 -------     |     :-----:  
 JavaScript  | js , jscript , javascript
 Java        | java          
 PHP         | php           
 text        | text , plain  
 Python      | py , python   
 Shell       | bash , shell  
 C           | cpp , c       
 C#          | c# , c-sharp , csharp  
 CSS         | css           
 XML         | xml , xhtml , xslt , html
 SASS&SCSS   | sass , scss   
 Scala       | scala         
 SQL         | sql           
 Visual Basic| vb , vbnet    
 Objective C | objc , obj-c  
 Ruby        | ruby , rails , ror , rb
 AppleScript | applescript   
 ColdFusion  | coldfusion , cf  
 Delphi      | delphi , pascal , pas  
 diff&patch  | diff patch    
 Erlang      | erl , erlang  
 Groovy      | groovy        
 JavaFX      | jfx , javafx  
 Perl        | perl , pl , Perl    
 F#          | f# f-sharp , fsharp  
 R           | r , s , splus
 matlab      | matlab        
 swift       | swift         
 GO          | go , golang   
