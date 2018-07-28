---
title: vscode 前端最佳配置
abbrlink: 6132
date: 2018-07-28 15:17:33
categories: 工具
tags:
  - vscode
comments: true
---

```javascript
{   
    // VScode主题配置
    "search.followSymlinks": false,  // 解决cpu飙到100%问题
    "git.autofetch": true,
    "editor.tabSize": 2,
    "workbench.colorTheme": "TwoStones",
    // 忽略工程打开的文件夹
    "files.exclude": {
        "**/.git": false,
        "**/.svn": true,
        "**/.DS_Store": true,
        "**/node_modules": true
    },
    // 搜索忽略的文件夹
    "search.exclude": {
        "**/node_modules": true,
        "**/bower_components": true
    },
    // 代码补全和高亮
    "emmet.triggerExpansionOnTab": true,  // 启用tab展开缩写
    "emmet.includeLanguages": {
        "vue-html": "html",
        "vue": "html",
        "javascript": "javascriptreact",
        "jsx-sublime-babel-tags": "javascriptreact"  // 在react的jsx中添加对emmet的支持
    },
    "emmet.syntaxProfiles": {
        "vue-html": "html",
        "vue": "html"
    },
    // eslint支持vue文件
    "eslint.autoFixOnSave": true,
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        "html",
        "vue",
        {
            "language": "html",
            "autoFix": true
        }, {
            "language": "vue",
            "autoFix": true
        }
    ],
    "eslint.options": {
        "plugins": ["html"]
    },
    // stylus格式化样式
    "stylusSupremacy.insertColons": false, // 是否插入冒号
    "stylusSupremacy.insertSemicolons": false, // 是否插入分好
    "stylusSupremacy.insertBraces": false, // 是否插入大括号
    "stylusSupremacy.insertNewLineAroundImports": false, // import之后是否换行
    "stylusSupremacy.insertNewLineAroundBlocks": false, // 两个选择器中是否换行
    // prettier结合vetur代码格式化
    "prettier.semi": false, // 格式化去掉行尾分号 
    "prettier.singleQuote": true, // 格式化为单引号
    "prettier.trailingComma": "all", // 尽可能控制尾随逗号的打印
    "prettier.eslintIntegration": true, // 开启 eslint 支持
    "vetur.validation.template": false, // v-for key报错
    "vetur.format.defaultFormatter.html": "js-beautify-html", // html格式化依赖
    "javascript.format.insertSpaceBeforeFunctionParenthesis": true,  //函数前加空格
    "vetur.format.defaultFormatter.js": "vscode-typescript",  //没有下边这个 上边不生效
    "vetur.format.defaultFormatterOptions": {
        "js-beautify-html": {
            "wrap_attributes": "force-aligned",  // 属性强制折行对齐
        }
    },
    "editor.quickSuggestions": {
        "strings": true  //自动显示建议
    },
    // sync配置
    "sync.gist": "4096fae8cf25eae5babfa3aa3a71a4e7",
    "sync.lastUpload": "2018-07-27T19:25:02.036Z",
    "sync.autoDownload": false,
    "sync.autoUpload": false,
    "sync.lastDownload": "",
    "sync.forceDownload": false,
    "sync.host": "",
    "sync.pathPrefix": "",
    "sync.quietSync": false,
    "sync.askGistName": false,
    "sync.removeExtensions": true,
    "sync.syncExtensions": true,
    "git.enableSmartCommit": true,
    "git.confirmSync": false,
    "explorer.confirmDelete": false,
    "explorer.confirmDragAndDrop": false,
    "[python]": {
        "editor.tabSize": 4,
    },
    "atomKeymap.promptV3Features": true,
    "editor.multiCursorModifier": "ctrlCmd",
    "editor.formatOnPaste": true,
    "liveServer.settings.donotShowInfoMsg": true,
    "liveServer.settings.donotVerifyTags": true,
    // vue代码格式，因为vetur有bug不能代码格式，所有用beautify
    "beautify.language": {
        "html": [
            "vue"
        ]
    }
}
```

### 另外介绍几款超强大的插件
`Settings Sync`
同步vscode的插件和配置，更换电脑时就可以用之前保存好的插件和配置了

`live server`
开启一个服务器，代码更新页面就会自动刷新了

`bookmarks`
代码标签，代码标记后会保存起来，查找代码就很方便了

`path Intelliense`
路径提示，妈妈再也不用担心我路径写错迷路了

`Bracket Pair Colorizer`
括号颜色标记，很好的区分各个代码块