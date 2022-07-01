+++
author = "Raikay"
title = "使用BaGet搭建自己的Nuget服务器"
description = "使用BaGet搭建自己的Nuget服务器,Nuget包管理平台"
date = "2020-05-17"
tags = [
    "NuGet",
    "BaGet",
]

+++



### 一、下载部署BaGet

BaGet下载地址：`https://github.com/loic-sharma/BaGet`

可以自己编译，也可以下载作者提供的Release版本  

### 二、下载安装Nuget

Nuget.exe 下载地址：`https://dist.nuget.org/win-x86-commandline/latest/nuget.exe`

Nuget安装后还需要配置环境变量  

### 三、打包Nuget

在项目.csproj目录中打开cmd 或者powershell 并执行：nuget spec  



用文本编辑器将上述命令执行完成的.nuspec 文件进行编辑

```xml
<?xml version="1.0"?>
<package >
  <metadata>
    <id>Dongteng</id>
    <version>1.0.0</version>
    <title>ceshiceshi</title>
    <authors>Dongteng</authors>
    <owners>$author$</owners>
    <licenseUrl>http://LICENSE_URL_HERE_OR_DELETE_THIS_LINE</licenseUrl>
    <projectUrl>http://PROJECT_URL_HERE_OR_DELETE_THIS_LINE</projectUrl>
    <iconUrl>http://ICON_URL_HERE_OR_DELETE_THIS_LINE</iconUrl>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <description>dongteng test</description>
    <releaseNotes>Summary of changes made in this release of the package.</releaseNotes>
    <copyright>Copyright 2019</copyright>
    <tags>Tag1 Tag2</tags>
  </metadata>
</package>
```

根据实际的需求修改，一般修改id、`version`、`authors`、`description等

修改完以上信息后执行命令：nuget pack，进行打包.正常结果如下   

也可以使用bat脚本执行   

`nuget-publish.cmd `:

```
@echo off
echo *******************clean nupkg*******************
del /S *.nupkg
del /S *.nuspec
echo *******************building*******************
dotnet build
echo *******************pack nupkg*******************
dotnet pack
echo *******************publish nupkg*******************
dotnet nuget push **/*.nupkg -s http://nuget.raikay.com/v3/index.json package.nupkg
pause
```



### 四、发布

运行命令行，将包文件推送到本地nuget服务器中，执行命令：（如果设置了Key，则需要在包名之前添加对应的ApiKey）

```
dotnet nuget push -s http://nuget.raikay.com/v3/index.json package.nupkg Dongteng.1.0.0.nupkg
```


​    
### 遇到的错误

**部署(IIS)之后可能会出现503错误**

![IMG](http://blogimg.raikay.com/330634772944654336.png)

**解决方案：**

去掉`web.Config`中

```
hostingModel="inprocess"
```



**推送包失败，405错误**

![IMG](http://blogimg.raikay.com/330634788014788608.png)

**解决方案：**

`web.config`中添加如下代码

```xml
<modules>
   <remove name="WebDAVModule"/>
</modules>
```

最终效果：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
          <modules>
            <remove name="WebDAVModule"/>
          </modules>
      <aspNetCore processPath="dotnet" arguments=".\BaGet.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\stdout"  />
    </system.webServer>
  </location>
</configuration>
<!--ProjectGuid: 284366CB-C68F-473E-908A-50A382616AE0-->
```



