+++
author = "Raikay"  
title = "linux系统常用基础命令"  
date = "2021-11-15"  
description = "linux系统常用基础命令 查询 手册 教程 记录"  
tags = [  
         "linux",
]  

+++

## 目录操作

```sh
#当前目录
pwd

# 创建目录
mkdir $HOME/testFolder

#切换目录
cd $HOME/testFolder

#切换到上一级目录
cd ../

# 移动
mv $HOME/testFolder /var/tmp

#删除
rm -rf /var/tmp/testFolder

#查看目录下的文件
# /etc 目录默认是 Unix 系统的软件配置文件存放位置
ls /etc
#竖着显示查询结果
ls -l /

#查看所有目录包括子目录（效果很酷）
ls -R /
```

## 文件操作
```sh
#创建
touch ~/testFile

#复制
cp ~/testFile ~/testNewFile

#删除
rm ~/testFile

#查找名称包含nginx的.conf文件
find / -name '*.conf' |grep nginx

#解压：
# tar.gz  tar  tgz
tar xzvf FileName.tar.gz

#压缩：
tar czvf FileName.tar.gz DirName
```
#### 查看文件

```sh
#一、从第3000行开始，显示1000行。即显示3000~3999行
cat filename | tail -n +3000 | head -n 1000

#二、显示1000行到3000行
cat filename| head -n 3000 | tail -n +1000

#-----------------------------
#注意两种方法的顺序
# 分解：
#    tail -n 1000：显示最后1000行
#    tail -n +1000：从1000行开始显示，显示1000行以后的
#    head -n 1000：显示前面1000行
#--------------------------------

#三、用sed命令
sed -n '5,10p' filename 这样就可以只查看文件的第5行到第10行。


#过滤出 /etc/passwd 文件中包含 root 的记录
grep 'root' /etc/passwd

#递归地过滤出 /var/log/ 目录中包含 linux 的记录
grep -r 'linux' /var/log/

#重定向
#可以使用 > 或 < 将命令的输出到一个文件中
echo 'Hello World' > ~/test.txt

#复杂查询
https://blog.51cto.com/xuqq999/774714
```

#### 远程复制文件
```
#整个文件夹
scp -r /home/administrator/test/ root@192.168.1.100:/root/

# 单个文件
scp /home/administrator/Desktop/old/driver/test/test.txt root@192.168.1.100:/root/

#下载到本地
scp -r root@192.168.62.10:/root/ /home/administrator/Desktop/new/
```

## 系统综合
```sh
#重启
reboot

#下面三个是一样的，都是当前用户主目录
cd ~
cd $HOME
cd /home/UserName

#查看发行版
cat /etc/os-release
find /etc/*-release

#查看系统版本
cat /proc/version

#查看系统信息
uname -a

#查看正在运行的进程
ps -ef | grep java

#实时查看服务器性能
top

#对 blog.raikay.com 发送 4 个 ping 包, 检查与其是否联通
ping -c 4 blog.raikay.com

#过滤得到当前系统中的 ssh 进程信息
ps -aux | grep 'ssh'

# 权限
chmod -R 777 权限
```
## 服务管理
```sh
#Centos7.5的服务管理是systemctl
systemctl start httpd.service      #启动，不加 .server也可以
systemctl stop httpd.service    #停止
systemctl status httpd.service     #查看状态
systemctl restart httpd.service    #重启
systemctl enable httpd.service     #开机启动
Systemctl enable  serviceName    #开启服务的开机自启动
systemctl disable firewalld.service  #禁止firewall开机启动
Systemctl disable serviceName  #关闭服务的开机自启动

#ububtu启动服务
#apache2
/etc/init.d/apache2 start #restart stop

#ssh
/etc/init.d/ssh start #restart stop
```

## 网络
```sh
#netstat 命令
#netstat 命令用于显示各种网络相关信息，如网络连接, 路由表, 接口状态等等

#列出所有处于监听状态的tcp端口
netstat -lt

#查看所有的端口信息, 包括 PID 和进程名称
netstat -tulpn
```

**检查网卡**
> Centos7的ifconfig不能用了，貌似需要后安装  
```sh
#查询IP 网卡信息
ip addr 
#网卡状态查询
service network status
```
**网卡关闭与激活**
```sh
ifdown eth0   #关闭网络
ifup eth0     #启动网络,eth0网卡名称
```
**网络服务启动与关闭**
```sh
#方法一:
service network stop    #关闭网络服务
service network start   #启动网络服务
service network restart #重启网络服务
#方法二：
/etc/init.d/network stop
/etc/init.d/network start
/etc/init.d/network restart 
```
**设置开机启动**：  
> 新版centos7安装后网卡默认采用ipv6，需要将ipv6修改为ipv4   

修改网卡配置：    
```sh
#vim /etc/sysconfig/network-scripts/ifcfg-ens33
#ens33 网卡名
IPV6INIT=no
ONBOOT=yes
```



## 防火墙
### CentOS7防火墙管理
> 在 CentOS 7 iptable 防火墙已经被 firewall替代

```sh
#停止firewall
systemctl stop firewalld.service 

#禁止firewall开机启动
systemctl disable firewalld.service 

#查看防火墙状态
systemctl status firewalld.service 

# 1、暂时开放FTP服务
firewall-cmd --add-service=ftp

# 2、永久开放FTP服务
firewall-cmd --add-service=ftp --permanent
# 3、永久关闭FTP服务
firewall-cmd --remove-service=ftp --permanent

# 4、让设定生效 重启restart(stop start)防火墙
systemctl restart firewalld

# 5、暂时 开放3306端口
firewall-cmd --add-port=3306/tcp

# 6、永久开放3306 加 --permanent
#加--permanent 表示永久开放
firewall-cmd --add-port=3306/tcp --permanent 

#### 7、检查设定是否生效
iptables -L -n | grep 21
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:21 ctstate NEW

# 8、检查防火墙状态
# runing not run firewall和-cmd之间不能有空格
firewall-cmd --state 

# 9、查询服务的启动状态 （ftp ssh samba http）
firewall-cmd --query-service ftp  #yes开启  no关闭
firewall-cmd --query-service ssh

# 10、列出防火墙全部的启用
firewall-cmd --list-all
```
### Ubuntu/Debian防火墙管理
```sh
#开启 并在系统启动时自动开启
sudo ufw enable

#关闭
sudo ufw disable 

#关闭所有外部对本机的访问，但本机访问外部正常
sudo ufw default deny

#查看防火墙状态
sudo ufw status 

sudo ufw allow 80  # 允许外部访问80端口

sudo ufw delete allow 80 # 禁止外部访问80端口

sudo ufw allow from 192.168.1.1  # 允许此IP访问所有的本机端口

sudo ufw deny smtp  # 禁止外部访问smtp服务

sudo ufw delete allow/deny smtp  # 删除上面建立的smtp的规则，上面建立的规则为allow，这里就删除allow；为deny，这里就删除deny

sudo ufw deny proto tcp from 10.0.0.0/8 to 192.168.0.1 port 22  # 要拒绝所有的TCP流量从10.0.0.0/8 到 192.168.0.1地址的22端口

# 可以允许所有RFC1918网络（局域网/无线局域网）访问这个主机（/8, /12, /16是一种网络分级）
sudo ufw allow from 10.0.0.0/8

sudo ufw allow from 172.16.0.0/12

sudo ufw allow from 192.168.0.0/16
```

### selinux管理

```sh
#关闭
vim /etc/selinux/config

#SELINUX=enforcing #注释掉

#SELINUXTYPE=targeted #注释掉

SELINUX=disabled #增加

:wq! #保存退出

setenforce 0 #使配置立即生效(临时关闭selinux)
# 查看
##如果SELinux status参数为enabled即为开启状态
/usr/sbin/sestatus -v      
#或者
getenforce
```

## 控制台
```sh
#清屏
clear

reset
#这个命令将完全刷新终端屏幕，之前的终端输入操作信息将都会被清空，这样虽然比较清爽，但整个命令过程速度有点慢，使用较少。
```

### screen  
Screen是Linux后台运行工具。提供了 ANSI/VT100 的终端模拟器，运行多个全屏的伪终端，每个伪终端我们称之为一个session。

**常用screen参数**
```sh
screen -S yourname -> 新建一个叫yourname的session
screen -ls -> 列出当前所有的session
screen -r yourname -> 回到yourname这个session
screen -d yourname -> 远程detach某个session
screen -d -r yourname -> 结束当前session并回到yourname这个session
```



### nohup

```sh
# 后台执行任务
nohup  <命令>  &
# 不加nohup 只有&，退出当前控制台后进程结束

# 查看nohup任务列表
jobs 

# 杀死第一个任务
fg1

# ...
bg
```




## 软件管理


### 装机必备
```sh
#安装wget vim  gcc、c++编译器以及内核文件
yum -y install gcc gcc-c++ kernel-devel wget vim gi
```


```sh
#查看软件是否安装
whereis software_name
```

### rpm包管理
```sh
#查看已安装软件包数量：
rpm -qa | wc -l
#查询系统中已经安装的软件
rpm -qa 
#查看已安装软件包名称 排序显示： 
rpm -qa | sort
#查询已经安装的文件属于哪个软件包；
rpm -qf 文件名的绝对路径
#查询已安装软件包都安装到何处；
rpm -ql 软件名
#查询已安装软件包的信息
rpm  -qi 软件名
#查看已安装软件的配置文件；
rpm -qc 软件名
#查看已经安装软件的文档安装位置：
rpm -qd 软件名
#查看已安装软件所依赖的软件包及文件；
rpm -qR 软件名
#查看是否已经安装vsftpd
rpm -qa | grep vsftpd
```
### 安装mariadb
```sh
yum install mariadb mariadb-server
# ==> 启动mariadb
systemctl start mariadb
# ==> 开机自启动
systemctl enable mariadb
# ==> 设置 root密码等相关
mysql_secure_installation
#测试登录！
mysql -uroot -p123456
```

## 其他
### curl post请求
```sh
# curl 命令如下：
curl -H "Accept: application/json" -H "Content-type: application/json" -X POST -d '{"phone": "18000011005","password": "xxxxx", "status":40,"order_no":"1998708","config":{"loading":true},"data": "123", "appVersion": "1.2.3","CHEN_ZHE_TEST_ONE_TWO_THREE": 1}'  http://192.168.57.80/mjyx-mall-gateway2/web/index.php/auth/login

# 设置POST Header
-H "Accept: application/json" -H "Content-type: application/json" -X POST -d 
# 请求参数 parames
'{"phone": "18000011005","password": "xxxxx", "status":4,"order_no":"1998708","config":{"loading":true},"data": "123", "appVersion": "1.2.3","CHEN_ZHE_TEST_ONE_TWO_THREE": 1}' 
# 请求路径 URL
http://192.168.57.80/gateway/login
```