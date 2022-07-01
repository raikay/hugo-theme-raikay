+++
author = "Raikay"
title = "使用docker安装部署gitlab实例"
date = "2020-04-08"
description = "Docker部署Gitlab 环境  安装 文档 教程 镜像，以及数据库的配置"
tags = [
    "gitlab",
    "docker",
]

+++

### 安装Gitlab

1、拉取镜像  

```shell
# gitlab-ce为稳定版本，后面不填写版本则默认pull最新latest版本
docker pull gitlab/gitlab-ce
```

2、创建映射目录  

```shell
mkdir -p /home/gitlab/config   #创建config目录
mkdir -p /home/gitlab/logs    #创建logs目录
mkdir -p /home/gitlab/data    #创建data目录
```

3、运行镜像  

强烈建议80端口不要映射其他端口， http clone 那里生成的地址不带映射端口

```c
docker run -d  -p 443:443 -p 80:80 -p 222:22 \
--name gitlab --restart always \
-v /home/gitlab/config:/etc/gitlab \
-v /home/gitlab/logs:/var/log/gitlab \
-v /home/gitlab/data:/var/opt/gitlab \
gitlab/gitlab-ce
```
参数说明：  

| 参数名称       |简写| 参数说明                                                     |
| :------------- |:-------: |:------------------ |
| detach         | d|后台运行  |
| hostname       | |指定主机地址，如果有域名可以指向域名  |
| publish        | p|将容器内部端口向外映射,左边代表宿主机的端口，右边代表容器端口 |
| name           | |容器名称        |
| restart always | |总是重启            |
| volume         |v |将容器内数据文件夹或者日志、配置等文件夹挂载到宿主机指定目录  |

4、配置  

按上面的方式，gitlab容器运行没问题，但在gitlab上创建项目的时候，生成项目的URL访问地址是按容器的hostname来生成的，也就是容器的id。作为gitlab服务器，我们需要一个固定的URL访问地址，于是需要配置gitlab.rb（宿主机路径：/home/gitlab/config/gitlab.rb）。  

```shell
# gitlab.rb文件内容默认全是注释
vim /home/gitlab/config/gitlab.rb
```



```shell
# 配置http协议所使用的访问地址,不加端口号默认为80 
#(我加端口号访问就会失败，最后用默认80端口)
external_url 'http://192.168.198.130'

# 配置ssh协议所使用的访问地址和端口
gitlab_rails['gitlab_ssh_host'] = '192.168.198.130'
gitlab_rails['gitlab_shell_ssh_port'] = 222 # 此端口是run时22端口映射的222端口
:wq #保存配置文件并退出
```

> 注意事项：`external_url 和gitlab_rails`这两个ip参数建议固定操作系统的静态不变的IP或说是域名进行配置，假设IP变得的话在GitLab新建项目的时候，生成的IP还是原来的IP，此时就无法推送代码在Gitlab里面

5、重启  

```shell
# 重启gitlab容器
$ docker restart gitlab
```

6、安装成功

浏览器访问 `http://192.168.198.129:7002/`  

第一次进入需要设置root密码  

![IMG](http://blogimg.raikay.com/330634555323191296.png)





### 相关命令：

```
docker exec -it gitlab /bin/bash  #进去gitlab容器的命令
gitlab-ctl reconfigure  #重置gitlab客户端的命令


docker start gitlab #启动命令
docker restart gitlab #重启命令
docker stop gitlab #停止命令

netstat -tnl #查看本机端口状态

#gitlab常用命令
gitlab-ctl reconfigure　 #重新应用gitlab的配置
gitlab-ctl restart　　　 #重启gitlab服务
gitlab-ctl status　　　  #查看gitlab运行状态
gitlab-ctl stop　　　　  #停止gitlab服务
gitlab-ctl tail　　　　　#查看gitlab运行日志

```

### 错误解决

docker启动报错 
```
iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport
```
解决办法：重启
```
systemctl restart docker
```

鸣谢：

> https://www.cnblogs.com/zhengyazhao/p/11690189.html
> 
> https://www.cnblogs.com/root0/articles/12762789.html
> 
> https://www.jianshu.com/p/080a962c35b6
> 
> https://www.jianshu.com/p/0bc9b4755082
> 
> https://www.cnblogs.com/believepd/p/10499844.html