+++
author = "Raikay"
title = "MarkDown基本语法以及相关工具笔记简单介绍"
date = "2017-03-11"
description = "MarkDown基本语法以及相关工具笔记简单介绍"
tags = [
    "markdown",
]

+++

### 1、标题

部分工具可能只支持到三级标题  

示例：
```
# 这是一级标题
## 这是二级标题
### 这是三级标题
#### 这是四级标题
##### 这是五级标题
###### 这是六级标题
```
效果：
# 这是一级标题
## 这是二级标题
### 这是三级标题
#### 这是四级标题
##### 这是五级标题
###### 这是六级标题


### 2、引用内容

示例：  

```
> 这是引用的内容  
> > 换行请按`空格+空格+Enter` 
> > > > > 这是引用的内容
```
效果：  

> 这是引用的内容  
> > 换行请按`空格+空格+Enter` 
> >
> > > > > 这是引用的内容


### 3、超级链接
title 是鼠标放到上面显示的内容，可不填  

示例：  

```
[raikay博客1](http://blog.raikay.com "title")  

[raikay博客2](http://blog.raikay.com)
```

效果：  

[raikay博客1](http://blog.raikay.com "title")  

[raikay博客2](http://blog.raikay.com)




### 4、插入图片  
示例：  

```
![image](https://gitee.com/imgrep001/m1/raw/master/20200811133858.png)
```
效果： 

![image](https://gitee.com/imgrep001/m1/raw/master/20200811133858.png)


### 5、行间代码
示例： 

```
这是行间代码`This is inline code`
```
效果：

这是行间代码`This is inline code`




### 6、段落代码

三个点是横排数字键 1 前面那个点 ，  
三个点后面标明语言，会按照标明语言高亮，不标会自动识别
示例：  

![图片](https://gitee.com/imgrep001/m1/raw/master/20200811134014.png)

效果：   

```csharp
public class int fuc(string str)
{
   int myint = 6;
   //标注csharp就有c#的高亮效果
   
   /*注意 三个点[`]  是数字键 1 前面那个点 */
}
```

### 7、有序列表列表
点和内容之间有一个空格  
示例：
```
1. 这是有序列表
2. 这是有序列表
3. 这是有序列表
```
效果：

1. 这是有序列表
2. 这是有序列表
3. 这是有序列表

### 8、无需列表
[-] [+] [*] 都可以  

示例：
```
- 这是无序列表
- 这是无序列表
    - 这是无序列表
    - 这是无序列表
- 这是无序列表
- 这是无序列表
```
效果：  

- 这是无序列表
- 这是无序列表
    - 这是无序列表
    - 这是无序列表
- 这是无序列表
- 这是无序列表


### 9、强调
示例：  

```
**这是强调**
```
效果：  

**这是强调**



### 10、斜体
示例：  

```
*这是斜体*
```
效果：  

*这是斜体*


### 11、表格
[:]冒号位置决定是左对齐、右对齐还是居中
示例：  

```

| 姓名| 性别 |家乡|
|----:|:-----:|:---|
| 张三三 | 保密  | 北京朝阳 |
| 李四 | 男  | 上海 |
| 王五 | 男  | 广州 |
```
效果：  

| 姓名| 性别 |家乡|
|----:|:-----:|:---|
| 张三三 | 保密  | 北京朝阳 |
| 李四 | 男  | 上海 |
| 王五 | 男  | 广州 |




### 12、多选框  
示例：
```
- [ ] 这个未选中checkbox
- [x] 这个是选中checkbox
```
效果：  

- [ ] 这个未选中checkbox
- [x] 这个是选中checkbox



### 13、分割线  

[*]或者[-]都可以

示例：  

```
---
```
效果：  

---


# Markdown工具 

#### Typora (推荐)
- Markdown神器
- 桌面客户端
- 编辑区预览区不是左右分开，是一个
- 免费
- 支持导出HTML、MD
- 下载：[https://www.typora.io/](https://www.typora.io/)

#### 马克飞象
- 在线
- 有桌面客户端
- 支持语法多  
- 可以同步到印象笔
- 收费
- 可以到处HTML、MD、PDF
- 地址： [https://maxiang.io](https://maxiang.io)

#### Haroopad
- 桌面客户端
- 免费
- top目录太多的时候影响左右同步对齐，影响编辑观看
- 支持导出 Html、MD
- 下载：[http://pad.haroopress.com/user.html](http://pad.haroopress.com/user.html)



#### MdNice微信排版工具
- 在线
- 地址：[https://mdnice.com/](https://mdnice.com/)

---

# 支持MarkDown的笔记
### 有道云
- 目前我在用
- 免费空间大
- 比较人性化，用着顺手
- 有广告

#### 印象笔记
- 界面太难看
- 自己没有Markdown编辑器，要依赖第三方，第三方同步过来的还不能编辑，必须通过第三方编辑
- 客户端用着不人性化
- 安装包很大，貌似100M
### 为知笔记
- 不能时时观看Markdown
- 收费

### 支持Markdown的平台
掘金、博客园、简书(不推荐)、知乎、GitHub、SegmentFault、V2EX、Gitee

---
# 不常用语法  

  

下面这些语法，只有部分工具支持  

###  脚注

示例：  

```
Markdown [^mark]
[^mark]: 《Markdown让文字更加精致》
```
效果： 

Markdown [^mark]

[^mark]: 《Markdown让文字更加精致》


### 高亮标记  
示例:  

```
==这是加亮度标记==
```

### 内容导航

示例：  

```
[TOC]
```

### 插入视频
示例：  
```
<video id="video" controls="" preload="none" poster="http://media.w3.org/2010/05/sintel/poster.png">
      <source id="mp4" src="http://media.w3.org/2010/05/sintel/trailer.mp4" type="video/mp4">
</video>
```
{{< bilibili BV18t41187Bx 3 >}}
### 插入ifram
示例：  

```
<iframe 
    width="800" 
    height="450" 
    src="https://www.baidu.com/"
    frameborder="0" 
    allowfullscreen>
</iframe>
```
### HTML标签
示例：  
```
<div>
有一些支持丰富的站点/工具，可以直接嵌入html标签
</div>
```
