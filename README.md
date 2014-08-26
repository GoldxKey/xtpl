# xtpl

xtemplate for nodejs (easier in express)

[![xtpl](https://nodei.co/npm/xtpl.png)](https://npmjs.org/package/xtpl)
[![NPM downloads](http://img.shields.io/npm/dm/xtpl.svg)](https://npmjs.org/package/xtpl)
[![Build Status](https://secure.travis-ci.org/kissyteam/xtpl.png?branch=master)](https://travis-ci.org/kissyteam/xtpl)
[![Coverage Status](https://coveralls.io/repos/kissyteam/xtpl/badge.png?branch=master)](https://coveralls.io/r/kissyteam/xtpl?branch=master)
[![Dependency Status](https://gemnasium.com/kissyteam/xtpl.png)](https://gemnasium.com/kissyteam/xtpl)

## docs

### syntax

refer: https://github.com/kissyteam/xtemplate

### api

#### methods

##### config or get xtpl global option:
```javascript
Object config(option:Object)
```

option details:
<table class="table table-bordered table-striped">
    <thead>
    <tr>
        <th style="width: 100px;">name</th>
        <th style="width: 50px;">type</th>
        <th style="width: 50px;">default</th>
        <th>description</th>
    </tr>
    </thead>
    <tbody>
      <tr>
          <td>encoding</td>
          <td>String</td>
          <td>utf-8</td>
          <td>encoding for inEncoding and outEncoding</td>
      </tr>
      <tr>
          <td>inEncoding</td>
          <td>String</td>
          <td>utf-8</td>
          <td>encoding for read xtpl files</td>
      </tr>
      <tr>
          <td>outEncoding</td>
          <td>String</td>
          <td>utf-8</td>
          <td>encoding for convert rendered content into buffer</td>
      </tr>
      <tr>
          <td>XTemplate</td>
          <td>Object</td>
          <td></td>
          <td>xtemplate module value</td>
      </tr>
    </tbody>
</table>

if options is undefined, then this method will return global config.

##### render file
```javascript
void renderFile(path:String, options:Object, callback:function)
```
parameter details:
<table class="table table-bordered table-striped">
    <thead>
    <tr>
        <th style="width: 100px;">name</th>
        <th style="width: 50px;">type</th>
        <th style="width: 50px;">default</th>
        <th>description</th>
    </tr>
    </thead>
    <tbody>
      <tr>
          <td>path</td>
          <td>String</td>
          <td></td>
          <td>xtpl template file</td>
      </tr>
      <tr>
          <td>option</td>
          <td>Object</td>
          <td></td>
          <td>
          data to be rendered. the following properties will be used for control.
          <table class="table table-bordered table-striped">
              <thead>
              <tr>
                  <th style="width: 100px;">name</th>
                  <th style="width: 50px;">type</th>
                  <th style="width: 50px;">default</th>
                  <th>description</th>
              </tr>
              </thead>
              <tbody>
                <tr>
                    <td>cache</td>
                    <td>Boolean</td>
                    <td>false</td>
                    <td>whether cache xtpl by path</td>
                </tr>
                <tr>
                    <td>setting['view encoding']</td>
                    <td>String</td>
                    <td>global encoding</td>
                    <td>encoding for read xtpl files</td>
                </tr>
              </tbody>
          </table>
          </td>
      </tr>
      <tr>
          <td>callback</td>
          <td>function</td>
          <td></td>
          <td>callback</td>
      </tr>
    </tbody>
</table>

```javascript
var xtpl = require('xtpl');
xtpl.renderFile('./x.xtpl',{
	x:1
},function(error,content){

});
```

##### express adaptor

```javascript
xtpl.__express = xtpl.renderFile
```

##### clear cache

clear xtemplate cache cached by xtpl file path

```javascript
void clearCache(path:String);
```

## Example

    ├── footer.xtpl
    ├── header.xtpl
    ├── index.xtpl
    ├── layout.xtpl
    ├── layout1.xtpl
    └── sub
        └── header.xtpl

index.xtpl

    {{extend ("./layout1")}}

    {{#block ("head")}}
    <!--index head block-->
    <link type="text/css" href="test.css" rev="stylesheet" rel="stylesheet"  />
    {{/block}}

    {{#block ("body")}}
    <!--index body block-->
    <h2>{{title}}</h2>
    {{/block}}

layout1.xtpl

    <!doctype html>
    <html>
    <head>
    <meta name="charset" content="utf-8" />
    <title>{{title}}</title>
    {{{block ("head")}}}
    </head>
    <body>
    {{{include ("./header")}}}
    {{{block ("body")}}}
    {{{include ("./footer")}}}
    </body>
    </html>


render

    res.render("index", {title: "xtpl engine!"})

output

    <!doctype html>
    <html>
    <head>
    <meta name="charset" content="utf-8" />
    <title>xtpl engine!</title>

    <!--index head block-->
    <link type="text/css" href="test.css" rev="stylesheet" rel="stylesheet"  />

    </head>
    <body>
    <h1>header</h1>
    <h2>sub header</h2>

    <!--index body block-->
    <h2>xtpl engine!</h2>

    <h1>footer</h1>

    </body>
    </html>


## changelog

### 1.1.2

* support global inEncoding for read xtpl file and outEncoding for convert rendered content into buffer
* support specify xtemplate module
* support get global config through xtpl.config()

### 1.0.0

* use xtemplate 1.1.1: https://www.npmjs.org/package/xtemplate

### 0.17.0

* support map array: https://github.com/kissyteam/kissy/issues/614
* support elseif: https://github.com/kissyteam/kissy/issues/664

### 0.16.0

* read file synchronously

### 0.12.1

* support different encoding output

### 0.11.0

* bug fix
* tutorial https://github.com/kissyteam/kissy/issues/645

### 0.10.0

* 升级 kissy5.0.0-alpha.3
* xtemplate
  * 支持 include 非 xtpl 文件（不解析）： https://github.com/kissyteam/kissy/issues/646
  * 支持渲染时动态设置命令：https://github.com/kissyteam/kissy/issues/637
  * 支持宏的默认参数值：https://github.com/kissyteam/kissy/issues/647

### 0.9.3

* 优化性能
* 直接使用 kissy5.0.0-alpha.1

### 0.8.0

* change syntax：https://github.com/kissyteam/kissy/issues/570
* 支持异步命令：https://github.com/kissyteam/kissy/issues/587
* 修复 {{1}}} 渲染问题：https://github.com/kissyteam/kissy/issues/596
* 支持模型内命令带参数调用：https://github.com/kissyteam/kissy/issues/616
* 支持继承: https://github.com/kissyteam/kissy/issues/564