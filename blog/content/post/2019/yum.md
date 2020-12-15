+++
author = "Raikay"
title = "yum的基本使用以及替换国内源"
date = "2019-05-16"
description = "yum的基本使用 Centos yum 安装指定版本 软件 更换yum源头 yum替换国内加速源"
tags = [
    "linux",
]

+++

## yum源管理

1.备份原有的yum源，以防出错时还可以还原

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

2.下载同时改名

```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

#其他源
wget  http://mirrors.163.com/.help/CentOS7-Base-163.repo  #centos7系统的
wget  http://mirrors.163.com/.help/CentOS6-Base-163.repo  #centos6系统的
wget  http://mirrors.163.com/.help/CentOS5-Base-163.repo  #centos5系统的
（注：如果下载出现错误，可能是变更了下载的地址，可以到：http://mirrors.163.com/.help/centos.html 找新的下载地址。没有安装wget的，要先安装。）
```

3.产生新的缓存。

```
yum clean all
yum makecache
```

至此源更新完成，可以 yum -y update 测试一下更新的速度。

## 安装软件

以安装Docker为例

####  安装最新版

```bash
yum install -y docker-ce docker-ce-cli containerd.io
```

####  安装指定版本

列出可用版本

```bash
$ yum list docker-ce --showduplicates | sort -r

docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable
docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable
docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable
```

安装指定版本

<VERSION_STRING>需要替换为第二列的版本号，如：18.06.0.ce-3.el7

```bash
$ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
```

### 其他

源目录：

```
cd /etc/yum.repos.d/
```



鸣谢：

https://www.cnblogs.com/wujinghua/p/8552785.html