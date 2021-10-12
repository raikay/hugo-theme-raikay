+++
author = "Raikay"
title = "Net Core 3.x 禁用序列化时自动首字母小写"
date = "2019-03-11"
description = "Net Core 3.x 禁用序列化时自动首字母小写."
tags = [
    "dotnet",
]
series = [".Net Core","配置文件"]
+++


```
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers().AddJsonOptions(config =>
    {
        //去掉转小写功能
        // System.Text.Json.JsonNamingPolicy.CamelCase：转小写 
        // null:不转小写
        config.JsonSerializerOptions.PropertyNamingPolicy =null;

    });
}
```