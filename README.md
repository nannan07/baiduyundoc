# baiduyundoc
Skip to content
 
Search or jump to…

Pull requests
Issues
Marketplace
Explore
 @nannan0713 Sign out
21
265 63 stomakun/BaiduOldDriver
 Code  Issues 4  Pull requests 0  Projects 0  Wiki  Insights
You’re editing a file in a project you don’t have write access to. We’ve created a fork of this project for you to commit your proposed changes to. Submitting a change to this file will write it to a new branch in your fork, so you can send a pull request.
BaiduOldDriver/BaiduOldDriver/ 
API.md
  or cancel
    
 
1
# API 概述
2
​
3
NetDisk 是实现百度网盘 API 的 C# 工程。使用者可以直接引用编译好的 NetDisk.dll。
4
​
5
目前版本的目标框架是 .NET Framework 4.5，引用它的工程最好也选用 .NET 4.5。
6
​
7
库中几乎所有函数的返回值以 Result 为基类。约定 bool success 表示是否成功完成调用流程（但不意味着结果为成功）。如果 success = false，Exception exception 保存了相关的异常。
8
​
9
*很多情况下，返回对象中给出的的 errno 记录了百度返回的操作结果，errno = 0 表示操作实际成功。*
10
​
11
网络传输使用 WebClient，它绝大多数情况下能自动选择合适的网络和（如果设置了的）代理服务器。
12
​
13
解决方案 BaiduOldDrive 中有若干使用 NetDisk 库的示例程序，可供参考。
14
​
15
# 身份验证
16
​
17
调用 Operations 类当中的操作之前，先要用这个类当中的函数获得登录信息。请将最终得到的 Credential 对象传入此后的操作函数。
18
​
19
它通过模拟网页版百度网盘的登录，获得 baiduid、bduss、stoken 这三个必需的 Cookies。
20
​
21
下面是命令行登录流程的例子：
22
​
23
```cs
24
var checkResult = Authentication.LoginCheck("用户名");
25
CheckSuccess(checkResult);
26
if (checkResult.needVCode)
27
{
28
 File.WriteAllBytes("vcode.png", checkResult.image);
29
 Console.WriteLine("输入 vcode.png 中的验证码：");
30
 checkResult.verifyCode = Console.ReadLine();
31
}
32
var loginResult = Authentication.Login("用户名", "密码", checkResult);
33
CheckSuccess(loginResult);
34
var credential = loginResult.credential;
35
// credential 变量以后操作使用
36
```
37
​
38
对于有 GUI 的程序，建议在用户名框更改时，就检查是否需要验证码，并对应显示。
39
​
40
## 登录检查
41
​
42
```cs
43
Authentication.LoginCheck(string username) : LoginCheckResult
44
```
45
​
46
在实际登录之前，需要先用这个函数，检查一下是否需要输入验证码。
47
​
48
### 参数
49
​
50
string username: 用户名
51
​
52
### 返回值
53
​
54
bool needVCode: 是否需要输入验证码
55
​
56
byte[] image: 如果需要验证码，这个数组存储了验证码图片
57
​
58
## 登录
59
​
60
```cs
61
Authentication.Login(string username, string password, LoginCheckResult checkResult) : LoginResult
62
```
63
​
64
对于同一个用户名，在调用 LoginCheck后，实际进行登录。如果 checkResult.needVCode == true，需将验证码文字存入 checkResult.verifyCode。
65
​
66
### 参数
67
​
68
string username: 用户名
69
​
70
string password: 密码
@nannan0713
Propose file change

Update API.md

Add an optional extended description…
 
© 2018 GitHub, Inc.
Terms
Privacy
Security
Status
Help
Contact GitHub
Pricing
API
Training
Blog
About
Press h to open a hovercard with more details.
