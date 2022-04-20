+++
author = "Raikay"  
title = "正则表达式在C#中的使用"  
date = "2019-08-06"  
description = "正则表达式在C#中的使用 正则表达式元字符简介 基本元字符 c# .net core 正则表达式"  
tags = [
    "正则",
]  

+++

## 正则表达式简介

1. 正则表达式是对字符串的操作
2. 正则表达式是一个用来描述字符串特征的表达式。
特征：必须出现的内容、可能出现的内容、不能出现的内容。  
观察字符串规律，根据规律总结特征，然后根据特定字符串的特征来编写正则表达式。  

**常用函数：**

```c#
Regex.IsMatch(); //判断是否匹配
Regex.Match();//提取某个（一个）匹配
Regex.Matches();//提取所有的匹配
Regex.Replace();//替换
Regex.Split();//分割
```

### Regex.IsMatch()

**练习1**：判断是否是合法的邮政编码（6位数字）
```c#
lRegex.IsMatch("100830","^[0-9]{6}$")
Regex.IsMatch("119", @"^\d{6}$");
```
解释：由元字符定义得知“[0-9]”表示0到9的任意字符，“{6}”表示前面的字符匹配6次，因此“[0-9]{6}”中的{6}表示对数字匹配6次(000000123)。简写表达式得知“[0-9]”可以被“\d”代替，所以第二种写法“\d{6}”也是正确的。

**练习2**：判断一个字符串是不是身份证号码。

1. 长度为15位或者18位的字符串，首位不能是0。

2. 如果是15位，则全部是数字。

3. 如果是18位，则前17位都是数字，末位可能是数字也可能是X。

写法一：`^[1-9][0-9]{14}([0-9]{2}[0-9Xx])?$`

写法二：` ^([1-9][0-9]{14}|[1-9][0-9]{16}[0-9X])$`

 **练习3**：判断字符串是否为正确的国内电话号码，不考虑分机。

•010-8888888或010-88888880或010xxxxxxx

•0335-8888888或0335-88888888（区号-电话号）03358888888

•10086、10010、95595、95599、95588（5位）

•13888888888（11位都是数字）



```c#
Regex.IsMatch(phoneNumber, @“^((\d{3,4}\-?\d{7,8})|(\d{5})|(\d{11}))$”);
//或者
Regex.IsMatch(phoneNumber, @“^((\d{3,4}\-?\d{7,8})|(\d{5}))$”);
```
按照要求一个一个写，都用|连起来。注意：由于区号有时为010-xxxxxxx有时为010xxxxxxx，-可有可无，所以需要?,由于-表示一个区间，所以这里要转义\-。最后不要忘记在所有|的最外层加一对()

### Regex.Match() / Matches()

```
string msg = "大家好呀，hello,2010年10月10日是个好日子。恩，9494.吼吼！886.";
Match match1 = Regex.Match(msg, @"\d", RegexOptions.ECMAScript); //提取得到 2  （逐个提取）
Match match2 = Regex.Match(msg, @"\d+", RegexOptions.ECMAScript); //提取得到2010
```

```
 //提取所有的匹配的字符串
 MatchCollection matches = Regex.Matches(msg, @"\d+", RegexOptions.ECMAScript);
 for (int i = 0; i < matches.Count; i++)
 {
      Console.WriteLine(matches[i].Value);
 }
 Console.ReadLine();
```



打印结果：

```
2010
10
10
9494
886

```


 大文件的时候可以这样逐个提取：

```
            Match match = regex.Match(msg);
            while (match.Value.Length != 0)
            {
                Console.WriteLine(match.Value);
                match = regex.Match(msg, match.Index + match.Value.Length);//找到当前的索引+下一个的长度
            }
//结果2010 10 10 9494 886  如上图       //注：字符串提取不能加 开始和结束 匹配得时候可以 
```



 提取组：在提取到的字符串中，再提取其中的某部分

 

## 替换Regex.Replace

```
temp = System.Text.RegularExpressions.Regex.Replace(temp, @"R=\d{1,3},", "");
```

 

`\d`既能匹配1,2...等ASCII数字,也能匹配全角数字“１,２,３...”

`\w`既能匹配[a-zA-Z0-9_]也能匹配中文

`\s`既能匹配“英文空格”、制表符等，也能匹配“全角空格”。

如果要想让只匹配ASCII字符，则需要指定RegexOptions.ECMAScript选项。


# 下面是很乱的 练习代码：

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;

namespace _06正则表达式
{
    class Program
    {
        static void Main(string[] args)
        {
            //1.正则表达式是对字符串的操作

            //2.正则表达式是一个用来描述字符串特征的表达式。
            //特征：必须出现的内容、可能出现的内容、不能出现的内容。


            //观察字符串规律，根据规律总结特征，然后根据特定字符串的特征来编写正则表达式。


            #region >10 and <=20 的所有数字字符串
    
            //Regex.IsMatch(); //判断是否匹配
            //Regex.Match();//提取某个（一个）匹配
            //Regex.Matches();//提取所有的匹配
            //Regex.Replace();//替换
            //Regex.Split();//分割






            ////1.写正则表达式第一步，观察字符串找规律，根据规律写正则
    
            //while (true)
            //{
            //    Console.WriteLine("请输入");
            //    string msg = Console.ReadLine();
            //    bool b = Regex.IsMatch(msg, "^(1[1-9]|20)$");
            //    Console.WriteLine(b);
            //}


            //===========================================================
            //while (true)
            //{
            //    Console.WriteLine("请输入");
            //    int n = Convert.ToInt32(Console.ReadLine());
            //    if (n>10 && n<=20)
            //    {
            //        Console.WriteLine("ok");
            //    }
            //    else
            //    {
            //        Console.WriteLine("输入不合法！！");
            //    }
            //}
    
            #endregion


            #region 正则案例
    
            //while (true)
            //{
            //    Console.WriteLine("请输入：");
            //    string msg = Console.ReadLine();
    
            //    //IsMatch()这个函数验证指定的字符串是否匹配指定的正则表达式，但是注意：
            //    //默认情况下，如果在整个字符串中只要有一部分匹配给定的字符串则返回true
    
            //    //要想验证完全匹配6位数字，则必须在正则表达式两端加开始^和结束$
            //    //一般情况下，当调用IsMatch()函数的时候都希望是完全匹配，所以在写正则的时候两边都要加^与$
            //    bool b=Regex.IsMatch(msg, "^[0-9]{6}$");
    
            //    // bool b=Regex.IsMatch(msg, "[0-9]{6}");
            //    Console.WriteLine(b);
            //}




            //while (true)
            //{
            //    Console.WriteLine("请输入");
            //    string msg = Console.ReadLine();
            //    //1. 这个正则表达式本身只匹配z或food
            //    //2. 但是由于正则两端没有加^和$，所以整个给定字符串中只要有任何一个部分与字符串z或food匹配，则返回true
            //    bool b = Regex.IsMatch(msg, "z|food");
            //    Console.WriteLine(b);
            //}




            //while (true)
            //{
            //    Console.WriteLine("请输入");
            //    string msg = Console.ReadLine();
    
            //    //因为这个正则表达式使用了|字符，所以该正则表达式
            //    //的意思是：^z  或  food$
            //    //即：所有以z开头的字符串  或者  所有以 food结尾的字符串。
            //    bool b = Regex.IsMatch(msg, "^z|food$");
            //    Console.WriteLine(b);
            //}





            //while (true)
            //{
            //    Console.WriteLine("请输入");
            //    string msg = Console.ReadLine();
    
            //    //  ^z$
    
            //    //  ^food$
            //    //所以这个正则表达式只能匹配 一个字母  z  或者一个单词 food
            //    bool b = Regex.IsMatch(msg, "^(z|food)$");
            //    Console.WriteLine(b);
            //}




            //while (true)
            //{
            //    Console.WriteLine("请输入");
            //    string msg = Console.ReadLine();
            //    //zood   food
            //    bool b = Regex.IsMatch(msg, "^(z|f)ood$");
            //    Console.WriteLine(b);
            //}


            //while (true)
            //{
            //    Console.WriteLine("请输入");
            //    string msg = Console.ReadLine();
            //    //这个只能匹配  字母 z 或者 单词 food
            //    //这个正则表达式等价于  ^(z|food)$
            //    bool b = Regex.IsMatch(msg, "(^z$)|(^food$)");
            //    Console.WriteLine(b);
            //}




            //while (true)
            //{
            //    Console.WriteLine("请输入邮政编码");
            //    string postcode = Console.ReadLine();
            //    //bool b = Regex.IsMatch(postcode, "^[0-9]{6}$");
            //    //bool b = Regex.IsMatch(postcode, "^\\d{6}$");
    
            //    //默认.net采用的是unicode字符匹配，所以中文的１２３４５６也是匹配的。
            //    //bool b = Regex.IsMatch(postcode, @"^\d{6}$");
    
            //    //指定RegexOptions.ECMAScript选项后，\d就只表示普通的数字，不在包含全角的字符了。
            //    bool b = Regex.IsMatch(postcode, @"^\d{6}$", RegexOptions.ECMAScript);
    
            //    Console.WriteLine(b);
            //}




            #endregion
    
            #region 验证身份证号码的正则表达式
    
            //while (true)
            //{
            //    Console.WriteLine("请输入");
            //    string id = Console.ReadLine();
            //    //bool b = Regex.IsMatch(id, "^([0-9]{15}|[0-9]{17}[0-9xX])$");
            //    //bool b = Regex.IsMatch(id, "^[0-9]{15}$|^[0-9]{17}[0-9xX]$");
            //    bool b = Regex.IsMatch(id, "^[0-9]{15}([0-9]{2}[0-9xX])?$");
            //    Console.WriteLine(b);
            //}
            #endregion


            #region 电话号码
    
            //while (true)
            //{
            //    Console.WriteLine("请输入");
            //    string phoneNumber = Console.ReadLine();
            //    bool b = Regex.IsMatch(phoneNumber, @"^(\d{3,4}\-?\d{7,8}|\d{5})$", RegexOptions.ECMAScript); 
            //    Console.WriteLine(b);
            //}
            #endregion



            #region 验证是否是合法的邮件地址
            //Console.WriteLine(Regex.IsMatch(email, "^a[x.]b$"));
            while (true)
            {
    
                //www.china-open.com
                //    .com.cn
                //    .com
                //    .163.com



                Console.WriteLine("请输入邮箱:");
                string email = Console.ReadLine();
                //fdsa    @   fdsafd  .com.cn
                //.出现在[]中，就表示一个普通的.可以不转义。
                //-出现在[]中的第一个位置的时候可以不转义。
                //bool b = Regex.IsMatch(email, @"^[-a-zA-Z0-9._]+@[-0-9a-zA-Z]+(\.[a-zA-Z0-9]+){1,}$");
    
                bool b = Regex.IsMatch(email, @"^\w+@\w+(\.\w+){1,}$", RegexOptions.ECMAScript);
                Console.WriteLine(b);
            }


            #endregion
        }
    }
}
```


```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using System.IO;

namespace _02正则表达式提取
{
    class Program
    {
        static void Main(string[] args)
        {

            #region 提取数字符串中的所有的数字
    
            string msg = "大家好呀，hello,2010年10月10日是个好日子。恩，9494.吼吼！886.";
            Match match1 = Regex.Match(msg, @"\d", RegexOptions.ECMAScript); //提取得到 2  （逐个提取）
            Match match2 = Regex.Match(msg, @"\d+", RegexOptions.ECMAScript); //提取得到2010
            Console.WriteLine(match.Value);
            Console.ReadLine();
    
            ////一般在调用IsMatch()的时候要判断完全匹配，所以要加^$
            ////但是一个在调用Match()或Matches()的时候，是要从一个大字符串中提取某一部分匹配的内容，所以这个时候是不能加^与$的，如果加了，则必须整个大字符串与给定正则表达式完全匹配
            ////创建一个对象实现逐个提取
            //Regex regex = new Regex(@"\d+", RegexOptions.ECMAScript);
    
            //Match match = regex.Match(msg);
            //while (match.Value.Length != 0)
            //{
            //    Console.WriteLine(match.Value);
            //    match = regex.Match(msg, match.Index + match.Value.Length);
            //}


            ////Console.WriteLine(match.Value);
    
            ////match = regex.Match(msg, match.Index + match.Value.Length);
            ////Console.WriteLine(match.Value);
    
            ////match = regex.Match(msg, match.Index + match.Value.Length);
            ////Console.WriteLine(match.Value);
    
            ////match = regex.Match(msg, match.Index + match.Value.Length);
            ////Console.WriteLine(match.Value);
    
            ////match = regex.Match(msg, match.Index + match.Value.Length);
            ////Console.WriteLine(match.Value);
    
            ////match = regex.Match(msg, match.Index + match.Value.Length);
            ////Console.WriteLine(match.Value);
    
            ////match = regex.Match(msg, match.Index + match.Value.Length);
            ////Console.WriteLine(match.Value);
    
            ////match = regex.Match(msg, match.Index + match.Value.Length);
            ////Console.WriteLine(match.Value);
    
            //Console.ReadLine();



            ////Match match1 = Regex.Match(msg, @"\d+", RegexOptions.ECMAScript);
            ////Console.WriteLine(match1.Value);


            ////Match match2 = Regex.Match(msg, @"\d+", RegexOptions.ECMAScript);
            ////Console.WriteLine(match2.Value);
    
            ////Match match3 = Regex.Match(msg, @"\d+", RegexOptions.ECMAScript);
            ////Console.WriteLine(match3.Value);
    
            //Console.ReadLine();


            //提取所有的匹配的字符串
            MatchCollection matches = Regex.Matches(msg, @"\d+", RegexOptions.ECMAScript);
            for (int i = 0; i < matches.Count; i++)
            {
                Console.WriteLine(matches[i].Value);
            }
            Console.ReadLine();


            ////判断是否匹配
            ////Regex.IsMatch();判断是否匹配
            #endregion


            #region 从文件中提取Email地址
    
            ////提取文件  提取Email.htm  中的所有Email地址
            ////1.读取文件中的内容到字符串
            //string html = File.ReadAllText("提取Email.htm");
            ////2.创建正则表达式
            //MatchCollection matches = Regex.Matches(html, @"[-a-zA-Z0-9_.]+@[-a-zA-Z0-9]+(\.[a-zA-Z0-9]+){1,}");
    
            ////3.进行提取


            ////4.输出
            //foreach (Match item in matches)
            //{
            //    Console.WriteLine(item.Value);
            //}
            //Console.WriteLine(matches.Count);
            //Console.ReadKey();
    
            #endregion


            #region MyRegion
    
            //string msg = "大夫，我咳嗽得很重。”     大夫：“你多大年记？”     患者：“七十五岁。”     大夫：“二十岁咳嗽吗”患者：“不咳嗽。”     大夫：“四十岁时咳嗽吗？”     患者：“也不咳嗽。”     大夫：“那现在不咳嗽，还要等到什么时咳嗽？";
    
            //MatchCollection matches = Regex.Matches(msg, "咳嗽");
            //foreach (Match item in matches)
            //{
            //    Console.WriteLine(item.Index);
            //}
            //Console.WriteLine("一共出现了{0}", matches.Count);
            //Console.ReadKey();
    
            #endregion
    
            #region 字符串提取
    
            //           //1.从一个大字符串中提取 某一部分子字符串


            //           //2.在提取到的字符串中，再提取其中的某部分
            //           //【提取组】


            //           ////提取文件  提取Email.htm  中的所有Email地址
            //           //1.读取文件中的内容到字符串
            //           string html = File.ReadAllText("提取Email.htm");
            //           //2.创建正则表达式
            //MatchCollection matches = Regex.Matches(html, @"[-a-zA-Z0-9_.]+@([-a-zA-Z0-9]+((\.[a-zA-Z0-9]+){1,}))");
            //           //()((()()))(()())()
    
            //           //(   ()()  )
            //           //3.进行提取


            //           //4.输出
            //           foreach (Match item in matches)
            //           {
            //               Console.WriteLine(item.Value + "===============" + item.Groups[1].Value + "=======" + item.Groups[2].Value);
            //           }
            //           Console.WriteLine(matches.Count);
            //           Console.ReadKey();


            #endregion


            #region 提取组2


            //Match match = Regex.Match("age=30", @"^(.+)=(.+)$");
            //Console.WriteLine(match.Groups[1].Value + "============" + match.Groups[2].Value);
            //Console.ReadKey();
    
            #endregion


            //普通的字符串提取：Match() Matches(),思路是从整个字符串中找出所有那些匹配指定正则表达式的字符串
    
            //提取组的思路：先写一个能满足整个字符串的正则表达式，然后在正则表达式中用括号将那些你想要提取的内容括起来，这样就可以提取你想要的组了。
    
            #region 从文件路径中提取出文件名(包含后缀) @“^.+\(.+)$”。比如从c:\windows\testb.txt中提取出testb.txt这个文件名出来。项目中用Path.GetFileName更好。贪婪模式。注意：\在c#中与在正则表达式中的转义符问题。
    
            //string path = @"C:\Documents and Settings\Administrator\桌面\资料\讲课素材\正则表达式工具\Regulator20Bin\The Regulator 2.0\_FusionLic.dll";
            ////提取组：1>编写一个能满足整个路径的正则表达式
            //Match match = Regex.Match(path, @".+\\(.+)");
            //Console.WriteLine(match.Groups[1].Value);
            //Console.ReadKey();
            #endregion


            #region 从“June         26       ,        1951    ”中提取出月份June、26、1951来。@"^([a-zA-Z]+)\s*(\d{1,2})\s*,\s*(\d{4})\s*$"进行匹配。月份和日之间是必须要有空格分割的，所以使用空白符号“\s”匹配所有的空白字符，此处的空格是必须有的，所以使用“+”标识为匹配1至多个空格。之后的“，”与年份之间的空格是可有可无的，所以使用“*”表示为匹配0至多个
    
            //string msg = "June26,1951    ";
            //Match match = Regex.Match(msg, @"^([a-zA-Z]+)\s*(\d{1,2})\s*,\s*(\d{4})\s*$");
            //Console.WriteLine(match.Groups[1].Value);
            //Console.WriteLine(match.Groups[2].Value);
            //Console.WriteLine(match.Groups[3].Value);
            //Console.ReadKey();



            #endregion


            #region 从Email中提取出用户名和域名，比如从test@163.com中提取出test和163.com。
    
            //string email = "test@163.com";
            //Match match = Regex.Match(email, @"(^\w+)@(\w+\.\w+)$");
            //Console.WriteLine(match.Groups[1].Value + "         " + match.Groups[2].Value);
            //Console.ReadKey();
    
            #endregion



            #region “192.168.10.5[port=21,type=ftp]”，这个字符串表示IP地址为192.168.10.5的服务器的21端口提供的是ftp服务，其中如果“,type=ftp”部分被省略，则默认为http服务。请用程序解析此字符串，然后打印出“IP地址为***的服务器的***端口提供的服务为***”
    
            string s1 = "192.168.10.5[port=21,type=ftp]";
            //string s1 = "192.168.10.5[port=21]";
    
            Match match = Regex.Match(s1, @"^((\d{1,3}\.){3}\d{1,3})\[port=(\d{1,2})(,type=([a-zA-Z0-9]+))?\]$", RegexOptions.ECMAScript);
            Console.WriteLine("ip:{0}", match.Groups[1].Value);
            Console.WriteLine("port:{0}", match.Groups[3]);
            Console.WriteLine("service:{0}", match.Groups[5].Value.Length == 0 ? "http" : match.Groups[5].Value);
            Console.ReadKey();




            #endregion




        }
    }
}
```

仅支持输入字母、数字、汉字、-和下划线  

```c#
if (!System.Text.RegularExpressions.Regex.IsMatch(dto.Name, @"^[\u4e00-\u9fa5|\w]{0,}$"))
    throw JAPXResponseException.BadRequest("仅支持输入字母、数字、汉字、-和下划线");
```







 扩展：贪婪模式与非贪婪模式

http://www.blogdaren.com/post-2081.html


JS正则

以/开始  /结束  

1，/g 所有可能的匹配，不加/g最多只匹配一个

2，/i  不区分大小写

3，/m 表示多行匹配，什么是多行匹配呢？就是匹配换行符两端的潜在匹配。影响正则中的^$符号

```
var reg = /\/RE[0-9]{0,20}/;
resultUrl=resultUrl.replace(reg, "")
```
