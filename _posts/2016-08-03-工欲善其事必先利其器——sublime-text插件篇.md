---
layout: post
title: "工欲善其事必先利其器——sublime text插件篇"
description: ""
date: 2016-08-03
tags: [sublime]
comments: true
share: true
---

## 常用配置

系统设置  
* 设置空格代替tab  
* 设置unix换行

```js
{
    "color_scheme": "Packages/User/SublimeLinter/Monokai Extended Bright (SL).tmTheme",
    "default_line_ending": "unix",
    "ignored_packages":
    [
        ""
    ],
    "translate_tabs_to_spaces": true
}

```
快捷键设置

```js
[
    {
        "command": "navigate_to_definition",
        "keys": ["ctrl+]"]
    },
    {
        "command": "jump_prev",
        "keys": ["ctrl+b"]
    },
    { "keys": ["ctrl+e"], "command": "reindent" } ,
    { "keys": ["alt+m"], "command": "markdown_preview", "args": { "target": "browser"} },
    ]

```

## 软件包管理

```
import urllib.request,os,hashlib; h = 'eb2297e1a458f27d836c04bb0cbaf282' + 'd0e7a3098092775ccb37ca9d6b2e4b7d'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

## 前端利器-Emmet

## 代码注释-DocBlockr

## 阅读源代码利器-CTags
下载ctags.exe，下载ctag插件，配置路径

```
{
    "command": "F:/wamp/www/ctags58/ctags.exe",
    "recursive" : true,
}
```

## ConverToUTF8

## Evernote
配置秘钥

```js
{
    "code_highlighting_style": "monokai",
    "enable_table_editor": true,
    "inline_css":
    {
        "code": "color: black; font-family: Menlo, Monaco, Consolas, Courier New, monospace; font-size: 14px;",
        "pre": "color: #000000; font-family: Menlo, Monaco, Consolas, Courier New, monospace; font-size: 14px; white-space: pre-wrap; word-wrap: break-word; background-color: #f8f8f8; border: 1px solid #cccccc; border-radius: 3px; overflow: auto; padding: 6px 10px; margin-bottom: 10px;"
    },
    "noteStoreUrl": "https://www.evernote.com/shard/s68/notestore",
    "token": "S=s68:U=762a09:E=15b763eb9ca:C=1541e8d89e8:P=1cd:A=en-devtoken:V=2:H=c024eb7b17de8fbb4a01e95bc59d8a06"
}
```

## linter
下载sublimeLinter,下载sublimeLinter-php[^2]
## Markdown
参看[这篇博文](http://frank19900731.github.io/blog/2015/04/13/zai-sublime-zhong-pei-zhi-markdown-huan-jing/)
### markdownEdit

### tableEdit

### markdownTOC

```js
{
  "default_autolink": true,
  "default_bracket": "round",
  "default_depth": 0
}
```

### markdownExtended ,monokai Extended

### markdown preview

```js
{
    "parser": "github",
    "build_action": "browser",
    "enable_mathjax": true,
    "enable_uml": true,
    "enable_highlight": true,
    "enable_pygments": true,
    "enabled_extensions": "github",
    "enabled_parsers": ["github"],
    "github_mode": "markdown",
    "github_inject_header_ids": true,
    "enable_autoreload": false
}
```

### pandoc
安装pandoc.msi ,下载sublime插件= =，[参看这里](http://www.yangzhiping.com/tech/pandoc.html)
配置文件：

[http://www.jianshu.com/p/e60cc2f76f12]: http://www.jianshu.com/p/e60cc2f76f12  
