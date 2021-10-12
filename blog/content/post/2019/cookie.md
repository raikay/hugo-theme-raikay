+++
author = "Raikay"
title = "Cookie、session、token、jwt简单说明"
date = "2019-09-15"
description = "Cookie、session、token、jwt简单说明"
tags = [
    "http",
]

+++

【Cookie】：是由服务器端生成,发送给客户端的文本文件，下次请求时自动发送给服务端。

【session】：在服务端保存用户的状态，并用一个cookie标识进行存取

【token】：服务端把用户信息和密钥通过加密算法生成一个字符串

【jwt】：一种基于 JSON 的开放标准,一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源。比如用在用户登录上。

可以理解是一种特殊的token