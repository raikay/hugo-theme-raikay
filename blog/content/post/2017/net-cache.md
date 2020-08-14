+++
author = "Raikay"
title = "DotNet Framework 类库 自带的缓存 HttpRuntime.Cache HttpContext.Cache"
date = "2017-03-11"
description = "DotNet Framework 类库 自带的缓存 HttpRuntime.Cache HttpContext.Cache"
tags = [
    "dotnet",
]

+++

###  系统自带两个cache类

在.NET运用中经常用到缓存(Cache)对象。有`HttpContext.Current.Cache`以及`HttpRuntime.Cache`，`HttpRuntime.Cache`是应用程序级别的，而`HttpContext.Current.Cache`是针对当前WEB上下文定义的。HttpRuntime下的除了WEB中可以使用外，非WEB程序也可以使用。

1. HttpRuntime.Cache 相当于就是一个缓存具体实现类，这个类虽然被放在了 System.Web 命名空间下了。但是非 Web 应用也是可以拿来用的。

2. HttpContext.Cache 是对上述缓存类的封装，由于封装到了 HttpContext ，局限于只能在知道 HttpContext 下使用，即只能用于 Web 应用。

综上所属，在可以的条件，尽量用 HttpRuntime.Cache ，而不是用 HttpContext.Cache 。

### 缓存数据规则
1. 数据可能会被频繁的被使用，这种数据可以缓存。
2. 数据的使用频率不高，但 生存周期很长，这样的数据也可以加入缓存。

### 封装Helper
```csharp
/*
 * 日期：2016-06-27
 */ 
using System;
using System.Web;
using System.Collections;

public class CacheHelper
{
    /**/
    /// <summary>
    /// 获取数据缓存
    /// </summary>
    /// <param name="CacheKey">键</param>
    public static object GetCache(string CacheKey)
    {
        System.Web.Caching.Cache objCache = HttpRuntime.Cache;
        return objCache[CacheKey];
    }

    /**/
    /// <summary>
    /// 设置数据缓存
    ///理论上永久保存,但服务器重启,内存紧张时也会丢失
    /// </summary>
    public static void SetCache(string CacheKey, object objObject)
    {
        System.Web.Caching.Cache objCache = HttpRuntime.Cache;
        objCache.Insert(CacheKey, objObject);
    }
    /// <summary>
    /// 设定绝对的过期时间
    /// </summary>
    /// <param name="CacheKey"></param>
    /// <param name="objObject"></param>
    /// <param name="seconds">超过多少秒后过期</param>
    public static void SetCacheDateTime(string CacheKey, object objObject, long Seconds)
    {
        System.Web.Caching.Cache objCache = HttpRuntime.Cache;
        objCache.Insert(CacheKey, objObject, null, System.DateTime.Now.AddSeconds(Seconds), TimeSpan.Zero);
    }


    /// <summary>
    /// 设置当前应用程序指定包含相对过期时间Cache值
    /// </summary>
    /// <param name="CacheKey"></param>
    /// <param name="objObject"></param>
    /// <param name="timeSpan">超过多少时间不调用就失效，单位是秒</param>
    public static void SetCacheTimeSpan(string CacheKey, object objObject, long timeSpan)
    {
        System.Web.Caching.Cache objCache = HttpRuntime.Cache;
        objCache.Insert(CacheKey, objObject, null, DateTime.MaxValue, TimeSpan.FromSeconds(timeSpan));
    }

    /**/
    /// <summary>
    /// 移除指定数据缓存
    /// </summary>
    public static void RemoveAllCache(string CacheKey)
    {
        System.Web.Caching.Cache _cache = HttpRuntime.Cache;
        _cache.Remove(CacheKey);
    }

    /**/
    /// <summary>
    /// 移除全部缓存
    /// </summary>
    public static void RemoveAllCache()
    {
        System.Web.Caching.Cache _cache = HttpRuntime.Cache;
        IDictionaryEnumerator CacheEnum = _cache.GetEnumerator();
        while (CacheEnum.MoveNext())
        {
            _cache.Remove(CacheEnum.Key.ToString());
        }
    }
}
```