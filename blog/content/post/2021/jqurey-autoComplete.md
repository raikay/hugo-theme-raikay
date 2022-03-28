+++
author = "Raikay"
title = "jQuery UI 自动完成组件（Autocomplete）使用"
date = "2021-06-08"
description = "jQuery UI 自动完成组件（Autocomplete）的使用 实例教程 源码 自动补全 历史记录 联想词"
tags = [ "jQuery","UI","Autocomplete","自动完成","组件"]

+++

### JS：

```js
var shopVal="";
var shopText = "";

$(function () {
    $("#tags").autocomplete({
        source: function ( request, response ) {
            $.ajax({
                url: "shop/list", //请求数据网址
                data: {
                    AppName: request.term,
                    appId: window.parent.appId
                    //dataType: "jsonp", //也可以是jsonp形式
                    //text: $("#tags").val() // 等同于 request.term
                },
                success: function (data) {
                    //console.log(data);
                    response(data);
                    
                }
            });
        },
        minLength: 1,//输入多少字的时候触发请求事件
        select: function (e, ui) {
            //选后后事件
            shopVal = ui.item.id;
            shopText = ui.item.value;
            //alert("value:" + ui.item.value + "lable:" + ui.item.label + " id:" + ui.item.id);
        }
    });
});
```

### HTML：

```html
<div class="ui-widget">
    <label for="tags">店铺名称：</label>
    <input id="tags" />
</div>
```

### 返回数据格式：

```json
[
	{
		"id": "10da4de5-3661-4718-81bf-be8dfdd85bcd",
		"lable": "小豆包丁一般生产企业业态测试门店（北京）集团",
		"value": "小豆包丁一般生产企业业态测试门店（北京）集团"
	},
	{
		"id": "303b62d9-3488-47f2-ada7-e5b2db88d095",
		"lable": "小白龙",
		"value": "小白龙"
	},
	{
		"id": "bb3af0f0-aa3f-4320-9001-1d921a977836",
		"lable": "智慧餐饮小店",
		"value": "智慧餐饮小店"
	},
	{
		"id": "dde4a0e9-a5c8-48b7-be36-2be726279910",
		"lable": "小菜门店（电商）",
		"value": "小菜门店（电商）"
	},
	{
		"id": "da83acfc-dcd5-4f79-b380-1184e0721b14",
		"lable": "千寻小店（中央厨房）",
		"value": "千寻小店（中央厨房）"
	}
]
```



### 图例：

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/2021/09/19/20210919232808.png)



鸣谢：

> [https://www.runoob.com/jqueryui/example-autocomplete.html](https://www.runoob.com/jqueryui/example-autocomplete.html)  
> [https://jqueryui.com/autocomplete/](https://jqueryui.com/autocomplete/)   