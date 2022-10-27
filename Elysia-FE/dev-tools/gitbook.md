# GitBook使用教程

## 1.环境安装

### 1.1 nodejs安装

大家可根据自己的操作系统下载对应的版本，本教程仅介绍windows系统下的nodejs安装，其它系统类似。nodejs官方下载地址：[https://nodejs.org/zh-cn/](https://nodejs.org/zh-cn/) 
> 为了避免安装过程中出现未知错误，建议不要使用高版本的nodejs，本教程使用的版本为12.16.2，并不影响使用，
安装到指定位置，一直下一步即可，没有踩坑点。

打开cmd命令提示符，执行如下命令，验证是否安装成功
```java
C:\Users\Administrator>node -v
v12.16.2
```
以上输出代表安装成功，否则会提示不是内部或外部命令

### 1.2 安装GitBook库

命令提示符中执行如下命令：
```java
C:\Users\Administrator>npm install gitbook-cli -g
```
执行如下命令，验证是否安装成功
```java
C:\Users\Administrator>gitbook -V
CLI version: 2.3.2
GitBook version: 3.2.3
```
> 注意V是大写

### 1.3 安装ebook-convert以及calibre

为了支持GitBook的导出功能，需要安装ebook-convert和calibre

#### 1.3.1 安装ebook-convert

命令提示符中执行如下命令：
```java
C:\Users\Administrator>npm install ebook-convert -g
```
执行如下命令验证ebook-convert是否安装成功
```java
C:\Users\Administrator>ebook-convert --version
ebook-convert.exe (calibre 6.7.1)
Created by: Kovid Goyal <kovid@kovidgoyal.net>
```

#### 1.3.2 安装calibre

下载对应系统的安装包，官网下载地址：https://calibre-ebook.com/download 
> 本人安装的是6.7.1版本，安装之后会自动把安装根目录加入到环境变量path中，如果大家用的低版本，务必确认安装根目录是否已经加入到环境变量path中。

## 2 GitBook使用

### 2.1 创建简单的GitBook

首先在指定位置为自己的电子书创建目录，并进入到该目录中
```java
E:\>mkdir my-gitbook
E:\>cd my-gitbook
```
执行`gitbook init`命令，初始化电子书目录，执行完后会自动创建目录文件SUMMARY.md和README.md文件
```java
E:\my-gitbook>gitbook init
warn: no summary file in this book
info: create README.md
info: create SUMMARY.md
info: initialization is finished
```
执行`gitbook serve`启动web服务，默认端口4000。
```java
E:\my-gitbook>gitbook serve
Live reload server started on port: 35729
Press CTRL+C to quit ...

info: 7 plugins are installed
info: loading plugin "livereload"... OK
info: loading plugin "highlight"... OK
info: loading plugin "search"... OK
info: loading plugin "lunr"... OK
info: loading plugin "sharing"... OK
info: loading plugin "fontsettings"... OK
info: loading plugin "theme-default"... OK
info: found 1 pages
info: found 0 asset files
info: >> generation finished with success in 0.4s !

Starting server ...
Serving book on http://localhost:4000
```
浏览器地址栏输入http://localhost:4000 ，访问该电子书


### 2.2 GitBook基本使用

#### 2.2.1 多级目录
实现多级目录，只需要修改根目录的summary.md文件即可
示例如下：

```java
bui# Summary

* [介&emsp;绍](README.md)
* [第 1 章：后端学习之路](Elysia-BE/README.md)
* [第 2 章：前端学习之路](Elysia-FE/README.md)
	* [2.1 开发工具](Elysia-FE/dev-tools/README.md)
		* [2.1.1 GitBook](Elysia-FE/dev-tools/gitbook.md)
* [第 3 章：优秀项目学习](Elysia-Projects/README.md)
* [第 4 章：好用工具分享](Elysia-Tools/README.md)
```
其中章节通过缩进进行约束，末尾的链接是相对电子书对应的文件夹的根目录
修改后登录localhost:4000查看效果如下：




#### 2.2.2 目录折叠

在根目录创建book.json文件，内容如下
```java
{
    "plugins": [ 
        "expandable-chapters"
    ]
}
```
请手动执行`gitbook install`安装刚才引入的插件
```
E:\GitBook\Elysia>gitbook  install
info: installing 1 plugins using npm@3.9.2
info:
info: installing plugin "expandable-chapters"
info: install plugin "expandable-chapters" (*) from NPM with version 0.2.0
E:\GitBook\Elysia
`-- gitbook-plugin-expandable-chapters@0.2.0

info: >> plugin "expandable-chapters" installed with success
```
从新执行`gitbook serve`命令，发现目录已经可以折叠了，效果如下

![](https://raw.githubusercontent.com/chenguo-design/images-repo/master/Elysia/image-20221023221236249.png)

#### 2.2.3 分隔符

只需要在md文件中使用`---`即可将内容分隔开，直接上例子，例如某个md文中内容如下
```java
* [第 1 章：GitBook](/Elysia-FE/dev-tools/gitbook.md)
* [第 2 章：GitBook](/Elysia-FE/dev-tools/gitbook.md)
* [第 3 章：GitBook](/Elysia-FE/dev-tools/gitbook.md)
---
* [第 4 章：GitBook](/Elysia-FE/dev-tools/gitbook.md)
* [第 5 章：GitBook](/Elysia-FE/dev-tools/gitbook.md)
* [第 6 章：GitBook](/Elysia-FE/dev-tools/gitbook.md)
---
* [第 7 章：GitBook](/Elysia-FE/dev-tools/gitbook.md)
* [第 8 章：GitBook](/Elysia-FE/dev-tools/gitbook.md)
* [第 9 章：GitBook](/Elysia-FE/dev-tools/gitbook.md)
```

效果如下

![](https://raw.githubusercontent.com/chenguo-design/images-repo/master/Elysia/image-20221023222803103.png)

不仅可以将功能相关的功能导航放在一起，同时使内容看上去不那么臃肿。

#### 2.2.4 友情链接

在book.json中增加links属性
> book.json文件中记得用英文逗号`,`跟其它属性分割
```java
{
    "links": {
        "sidebar": {
            "本书github站点": "https://github.com/chenguo-design/Elysia"
        }
    }
}
```
效果如下图：

![](https://raw.githubusercontent.com/chenguo-design/images-repo/master/Elysia/image-20221023224124661.png)

#### 2.2.5 设置书名、作者、描述、语言等

在book.json中添加如下内容
```java
{
    "title": "《Elysia》",
    "author": "ChenGuo",
    "description": "记录学习、工作期间学习到的知识，见证自己的成长😄",
    "language": "zh-hans",
    "gitbook": "3.2.3",
    "structure": {
        "readme": "README.md"
    }
}
```

#### 2.2.6 自定义样式

在book.json中添加如下内容
```java
{
    "styles":{
        "website":"./styles/elysia-common.css"
    }
}
```

## 3 插件

### 3.1 插件配置
插件的使用就不在赘述，直接在book.json中将要加入的插件添加到对应位置即可，此处介绍插件的配置。

以配置sharing默认插件为例，只需要在book.json中增加`pluginConfig`属性，并添加上我们的自定义配置

```
{
    ......
    "plugins": [ 
        "sharing"
    ],
    "pluginsConfig": {
        "sharing": {
            "facebook" : true,
            "weibo" : false
        }
    }
    ......
}
```

> 对应插件的配置项可以查看文档: https://www.npmjs.com/package/gitbook-plugin-sharing-plus , 其它插件也可以在该网页上方的搜索栏搜索，前缀加上`gitbook-plugin`通常更准确

### 3.2 插件禁用

禁用插件操作与新增操作类似，区别是在对应的插件名前面加上`-`。例如，要禁用`sharing`插件，只需要在book.json中做如下修改。

```java
{
    ......
    "plugins": [ 
        "-sharing"
    ]
    ......
}
```

### 3.3 好用插件推荐

#### 3.3.1 捐赠

插件名称：`donate`
插件地址：  https://www.npmjs.com/package/gitbook-plugin-donate
book.json配置：

```java
{
    ......
    "plugins": ["donate"],
    "pluginsConfig": {
        "donate": {
          "wechat": "微信捐赠二维码图片地址",
          "alipay": "支付宝捐赠二维码图片地址",
          "title": "捐赠",
          "button": "默认值：Donate",
          "alipayText": "默认值：支付宝捐赠",
          "wechatText": "默认值：微信捐赠"
        }
    }
    ......
}
```
> 注意，如果此前gitbook没有安装该插件，需要使用`gitbook install`命令安装该插件，否则`gitbook serve`会报错

#### 3.3.2 回到顶部

插件名称：`back-to-top-button`
插件地址：  https://www.npmjs.com/package/gitbook-plugin-back-to-top-button
book.json配置：

```java
{
    "plugins" : [ "back-to-top-button" ]
}
```
#### 3.3.3 页内导航

该插件允许在本页开始提供本页的目录，方便浏览者进行检索

插件名称：`page-treeview`
插件地址：  https://www.npmjs.com/package/gitbook-plugin-page-treeview
book.json配置：
```java
{
    "plugins" : [ "page-treeview" ],
    "pluginsConfig": {
    	"page-treeview": {
       	 	"copyright": "Copyright &#169; aleen42",
       		"minHeaderCount": "2",
            "minHeaderDeep": "2"
    	}
	}
}
```
## 4 导出

GitBook导出非常简单，只需要命令行执行`gitbook XXX`即可，其中`xxx`是你具体想要导出的格式，常见的导出格式有PDF、ePub、mobi、HTML。下面我以导出pdf为例进行演示，首先在gitbook电子书所在的目录输入`gitbook pdf`。

```java
E:\GitBook\Elysia>gitbook pdf
info: 12 plugins are installed
info: 8 explicitly listed
info: loading plugin "expandable-chapters"... OK
info: loading plugin "back-to-top-button"... OK
info: loading plugin "highlight"... OK
info: loading plugin "search"... OK
info: loading plugin "lunr"... OK
info: loading plugin "sharing"... OK
info: loading plugin "fontsettings"... OK
info: loading plugin "theme-default"... OK
info: found 6 pages
info: found 8 asset files
info: >> generation finished with success in 369.0s !
info: >> 1 file(s) generated
```
然后根目录下面会生成book.pdf，导出成功。同理，你也可以导出其它几种类型，在此就不再演示。
