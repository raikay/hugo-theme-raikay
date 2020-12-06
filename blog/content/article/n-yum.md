+++
author = "Raikay"  
title = "在CentOS系统中使用yum安装指定版本软件的方法"  
date = "2020-12-06"  
description = "在CentOS系统中使用yum安装指定版本软件的方法 Linux 基本命令 yum常用命令 安装软件"  

+++


yum默认都是安装最新版的软件，这样可能会出一些问题，或者我们希望yum安装指定(特定)版本(旧版本)软件包.

### 安装软件

查询prce可安装版本

```
# yum list | grep pcre
```

#### 查询结果：

```
pcre-7.8-6.el6.i686 : Perl-compatible regular expression library

pcre-7.8-6.el6.x86_64 : Perl-compatible regular expression library

pcre-7.8-6.el6.x86_64 : Perl-compatible regular expression library
```

#### 确认版本，进行安装：

```
#yum install pcre-7.8-6.el6.i686 -y
```

#### 查看安装的pcre版本

```
# rpm -qa | grep pcre
```

 

### **卸载软件：**

如果你带有yum，可以直接yum remove xxx  
如果是rpm包，rpm -e xxx  
tar包的话需要你直接删除该文件或者make uninstall xxx  