+++
author = "Raikay"
title = "Dapper连接MySql(Core读取配置)"
date = "2018-03-11"
description = "git 常用命令 笔记 学习 教程 分支 差异."
tags = [
    "dotnet",
]
series = ["配置文件","helper","Dapper"]
aliases = ["migrate-from-jekyl"]
+++

### 1、NuGet:
```
MySql.Data
Dapper
```

### 2、Config:
```
 constr = ConfigHelper.GetSectionValue("constr");
```
### 3、Code:
```
IDbConnection connection = new MySqlConnection(constr);
var optionList = connection.Query<dynamic>("select * from basicoption").ToList();
connection.Close();
connection.Dispose();
```

### Helper:
```

public static class ConfigHelper
{
    private static IConfiguration _configuration;

    static ConfigHelper()
    {
        //在当前目录或者根目录中寻找appsettings.json文件
        var fileName = "appsettings.json";

        var directory = AppContext.BaseDirectory;
        directory = directory.Replace("\\", "/");

        var filePath = $"{directory}/{fileName}";
        if (!File.Exists(filePath))
        {
            var length = directory.IndexOf("/bin");
            filePath = $"{directory.Substring(0, length)}/{fileName}";
        }

        var builder = new ConfigurationBuilder()
            .AddJsonFile(filePath, false, true);

        _configuration = builder.Build();
    }

    public static string GetSectionValue(string key)
    {
        return _configuration.GetSection(key).Value;
    }
}

```
