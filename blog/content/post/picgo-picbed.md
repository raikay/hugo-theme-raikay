+++
author = "Raikay"
title = "Picgo图床插件介绍"
date = "2018-04-08"
description = "Picgo图床插件介绍 Picgo 图床 插件"
tags = [
    "Picgo",
]

+++

```json
{
  "uploaded": [],
  "picBed": {
    "current": "blog-uploader",
    "list": [
      {
        "name": "SM.MS图床",
        "type": "smms",
        "visible": false
      },
      {
        "name": "腾讯云COS",
        "type": "tcyun",
        "visible": false
      },
      {
        "name": "微博图床",
        "type": "weibo",
        "visible": false
      },
      {
        "name": "GitHub图床",
        "type": "github",
        "visible": false
      },
      {
        "name": "七牛图床",
        "type": "qiniu",
        "visible": false
      },
      {
        "name": "Imgur图床",
        "type": "imgur",
        "visible": false
      },
      {
        "name": "阿里云OSS",
        "type": "aliyun",
        "visible": false
      },
      {
        "name": "又拍云图床",
        "type": "upyun",
        "visible": false
      },
      {
        "name": "SM.MS-登录用户",
        "type": "smms-user",
        "visible": true
      },
      {
        "name": "自定义Web图床",
        "type": "web-uploader",
        "visible": true
      },
      {
        "name": "博客图床",
        "type": "blog-uploader",
        "visible": true
      }
    ],
    "smms": true,
    "smms-user": {
      "Authorization": "m4npdghkuL90eKZxLwjqu3ZEGNmrg8xX"
    },
    "web-uploader": {
      "customBody": "{\"fileId\": \"2160_angle-left.png\",\"apiType\": \"ali\"}",
      "customHeader": "{\"apiType\":\"SouGou,xiaomi,juejin,toutiao,BaiDu\"}",
      "jsonPath": "data.url.ali",
      "paramName": "image",
      "url": "https://img.rruu.net/api/upload/upload"
    },
    "web-uploader1": {
      "customBody": "{\"type\":\"qiantu\"}",
      "customHeader": "{\"type\":\"qiantu\"}",
      "jsonPath": "imgurl",
      "paramName": "file",
      "url": "https://img.oioweb.cn/api.php"
    },
    "web-uploader2": {
      "customBody": null,
      "customHeader": null,
      "jsonPath": "url",
      "paramName": "file",
      "url": "https://imgurl.org/upload/ftp"
    },
    "blog-uploader": {
      "cookie": "",
      "uploadTo": "掘金"
    }
  },
  "settings": {
    "shortKey": {
      "picgo:upload": {
        "enable": true,
        "key": "CommandOrControl+Shift+P",
        "name": "upload",
        "label": "快捷上传"
      }
    },
    "server": {
      "enable": true,
      "host": "127.0.0.1",
      "port": "36677"
    },
    "showUpdateTip": true,
    "pasteStyle": "markdown",
    "autoStart": true,
    "uploadNotification": true
  },
  "picgoPlugins": {
    "picgo-plugin-smms-user": true,
    "picgo-plugin-web-uploader": true,
    "picgo-plugin-blog-uploader": true
  },
  "debug": true,
  "PICGO_ENV": "GUI",
  "needReload": false
}
```