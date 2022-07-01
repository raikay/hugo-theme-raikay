+++
author = "Raikay"  
title = "dotnet core 使用grpc服务"  
date = "2021-08-24"  
description = "dotnet core 使用grpc"  
tags = [  
    "dotnet ",  "grpc", 
]  

+++

# 一、GRPC服务端

引用NuGet包

```
Grpc.AspNetCore
```

`Startup.cs`  中 `ConfigureServices`  函数配置grpc

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.AddRazorPages();
    //配置grpc
    services.AddGrpc();
}
```

`Configure `  函数映射服务grpc类

```c#
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }
        else
        {
            app.UseExceptionHandler("/Error");
            app.UseHsts();
        }

        app.UseHttpsRedirection();
        app.UseStaticFiles();

        app.UseRouting();

        app.UseAuthorization();
        
		//映射grpc服务类
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapRazorPages();
            endpoints.MapGrpcService<GreeterService>();
            endpoints.MapGrpcService<LuCatService>();
        });
}
```

`Program.cs` 添加证书、设置监听端口  

```c#
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            var certificate = new X509Certificate2("https/6069857_game.raikay.com.pfx", "aE14DALS");
            webBuilder.UseKestrel(options =>
            {
                options.AddServerHeader = false;
                options.Listen(IPAddress.Any, 443, listenOptions =>
                {
                    listenOptions.UseHttps(certificate);
                });

            }).UseContentRoot(Directory.GetCurrentDirectory())
                .UseStartup<Startup>()
                .UseUrls("https://*:443");
        });
```

`appsettings.json`配置http2  

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*",
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http2"
    }
  }
}
```

添加协议文件`LuCat.proto ` 

```protobuf
syntax = "proto3";

option csharp_namespace = "AspNetCoregRpcService";

import "google/protobuf/empty.proto";
package LuCat; //定义包名

//定义服务
service LuCat{
    //定义吸猫方法
	rpc SuckingCat(ParamRequest) returns(SuckingCatResult);
}

message SuckingCatResult{
	string message=1;
}
message ParamRequest {
  int32 id = 1;
}
```

实现服务  

`LuCat.LuCatBase` 是根据  `LuCat.proto` 文件自动生成的  

```c#
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Google.Protobuf.WellKnownTypes;
using Grpc.Core;
using System.Linq;

namespace AspNetCoregRpcService
{
    public class LuCatService : LuCat.LuCatBase
    {
        private static readonly List<Cat> Cats = new List<Cat>() {
            new Cat { Id = 1, Name = "英短银渐层", Describe = "英短银渐层是由英国短毛猫与金吉拉猫繁育而来" },
            new Cat { Id = 2, Name = "英短金渐层" ,Describe="英短金渐层是由英短蓝猫改良而来"},
            new Cat { Id = 3, Name = "美短" ,Describe="美短身体强健、勇敢活泼。"}
        };
        private static readonly Random Rand = new Random(DateTime.Now.Millisecond);
        public override Task<SuckingCatResult> SuckingCat(ParamRequest param, ServerCallContext context)
        {
            Cat cat = null;
            if (Cats.Any(_ => _.Id == param.Id))
            {
                cat = Cats.FirstOrDefault(_ => _.Id == param.Id);
            }
            else
            {
                cat = Cats[Rand.Next(0, Cats.Count)];
            }
            return Task.FromResult(new SuckingCatResult()
            {

                Message = $"您获得一只{cat.Name},{cat.Describe}"
            });
        }
    }
    public class Cat
    {
        public int Id { set; get; }
        public string Name { set; get; }
        public string Describe { set; get; }
    }
}
```

源码地址  

>  https://gitee.com/raikay/demo/tree/master/Grpc/https



# 二、GRPC客户端

添加`LuCat.proto` 文件  

`LuCat.LuCatClient()`  添加  `LuCat.proto`  文件后自动生成，和服务端  `LuCat.proto`  文件一致  

```
syntax = "proto3";

option csharp_namespace = "AspNetCoregRpcService";

import "google/protobuf/empty.proto";
package LuCat; //定义包名

//定义服务
service LuCat{
    //定义方法
	rpc SuckingCat(ParamRequest) returns(SuckingCatResult);
}

message SuckingCatResult{
	string message=1;
}
message ParamRequest {
  int32 id = 1;
}
```

调用服务

```c#
static async Task Main(string[] args)
{
    var channel = GrpcChannel.ForAddress("https://game.raikay.com/");
    var catClient = new LuCat.LuCatClient(channel);
    var catReply = await catClient.SuckingCatAsync(new ParamRequest { Id=0});
    Console.WriteLine(""+ catReply.Message);
    Console.ReadKey();
}
```



# 三、调试

调试工具：BloomRPC

下载地址：

> https://github.com/uw-labs/bloomrpc

点加号导入`LuCat.proto`

点TLS导入证书，我是在阿里云申请的免费证书

![IMG](http://blogimg.raikay.com/330643629704089600.png)

