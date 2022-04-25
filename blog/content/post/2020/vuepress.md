+++
author = "Raikay"
title = "Vuepress起步"
date = "2020-03-11"
description = "Vuepress 开始 教程"
tags = [
    "markdown",
]

+++

### 1、安装

```sh
npm install -g vuepress
```

如果慢或者安装失败可以用`cnpm`替换，或者`yarn`

```sh
# yarm
yarn global add vuepress  

#cnpm
npm install -g cnpm --registry=https://registry.npm.taobao.org #如果已安装请忽略
cnpm install -g vuepress
```

### 2、创建项目目录

```
mkdir vuepress-starter && cd vuepress-starter
```

### 3、新建一个 markdown 文件

```sh
echo '# Hello VuePress!' > README.md
```

运行项目

```sh
vuepress dev .
```



### 4、访问站点

```
http://localhost:8080/
```

![20200408212023](http://blogimg.raikay.com/306330845831106560.png)





### 初始化项目

```sh
npm init -y
```

执行命令后，项目根目录会出现一个`package.json`，我们修改scripts节点

```
"scripts":{
     "dev":"vuepress dev docs",
     "build":"vuepress build docs"
}
```



修改之后：

```
{
    "name":"vuepress-starter",
    "version":"1.0.0",
    "description":"",
    "main":"index.js",
    "scripts":{
        "dev":"vuepress dev docs",
        "build":"vuepress build docs"
    },
    "keywords":[

    ],
    "author":"",
    "license":"ISC"
}
```

创建基本目录

按照[官方推荐目录结构](https://www.vuepress.cn/guide/directory-structure.html)创建一些基本目录，推荐的不用全部创建，大部分都是可选

```
vuepress-starter
├─package.json
├─docs
|  ├─README.md
|  ├─.vuepress
|  |     ├─config.js
|  |     ├─public
|  |     |   └favicon.ico
```

创建之后我们在来运行一直，看看是否正常

```
npm run dev
```

还是和刚才一样的，表示没什么问题

#### 修改配置文件

`/vuepress-starter/docs/.vuepress/config.js`

```json
module.exports = {
    title: 'Hello VuePress',
    description: 'Hello, my friend!',
    head: [
        ['link', {
            rel: 'icon',
            href: `/favicon.ico`
        }]
    ],
    dest: './docs/.vuepress/dist',
    ga: '',
    evergreen: true,
}
```



##### 修改README.md

`/vuepress-starter/docs/README.md`

VuePress 提供了对 [YAML front matter](https://links.jianshu.com/go?to=https%3A%2F%2Fjekyllrb.com%2Fdocs%2Ffrontmatter%2F) 开箱即用的支持，我们可以模仿vuepress首页修改`README.md`

```yaml
---
home: true
heroImage: /favicon.ico
actionText: 快速上手 →
actionLink: /guide/
features:
- title: 简洁至上
  details: 以 Markdown 为中心的项目结构，以最少的配置帮助你专注于写作。
- title: Vue驱动
  details: 享受 Vue + webpack 的开发体验，在 Markdown 中使用 Vue 组件，同时可以使用 Vue 来开发自定义主题。
- title: 高性能
  details: VuePress 为每个页面预渲染生成静态的 HTML，同时在页面被加载的时候，将作为 SPA 运行。
footer: MIT Licensed | Copyright © 2018-present xxxxxx
---
```

运行项目

```sh
npm run dev
```

![20200408224259](http://blogimg.raikay.com/306330875619053568.png)

现在看上去已经很像样子了

接下来在添加一些内容

创建guide文件夹

`/vuepress-starter/docs/guide`

`guide`下新建`README.md`

```markdown
## This is guide
content...

### title3
content...

### title3-01

## small title
content...
```

运行项目，点击首页 快速上手 查看效果

![20200408225105](http://blogimg.raikay.com/306330897714647040.png)



配置导航

在config文件添加themeConfig

![image-20200408225646648](http://blogimg.raikay.com/306330912075943936.png)



##### 配置侧边栏

在config.js文件下的themeConfig属性下添加：

```json
sidebarDepth: 2,
sidebar: [
  {
    title: 'Guide',
    collapsable: false,
    children: ['/guide/']
  }         
]
```

注：通过 themeConfig.sidebarDepth 来修改它的行为。默认的深度是 1，它将提取到 h2 的标题，设置成 0 将会禁用标题（headers）链接，同时，最大的深度为 2，它将同时提取 h2 和 h3 标题。

配置好后，运行项目

![image-20200408230046511](http://blogimg.raikay.com/306330929016737792.png)



构建项目

```sh
npm run build
```



如果不是网站根目录，要在config配置根目录

比如：http://blog.raikay.com/docs/

```json
module.exports = {
    title: 'Hello VuePress',
    description: 'Hello, my friend!',
	base : '/docs/',
	...
```



鸣谢：https://www.jianshu.com/p/2ac5727947cd

