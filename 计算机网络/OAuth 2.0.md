# OAuth 2.0
OAuth的作用就是让"客户端"安全可控地获取"用户"的授权，与"服务商提供商"进行互动。

- [传统认证的问题](#传统认证的问题)
- [OAuth2的解决思路](#OAuth2的解决思路)
- [OAuth2简介](#OAuth2简介)
    - [名词定义](#名词定义)
 
# 传统认证的问题 
举个简单一点的例子来说明传统身份认证的问题:

> 你有一座别墅, 这个别墅是你的资产, 清洁公司提供别墅清洁服务(第三方服务商), 园艺公司提供修剪花园的服务(第三方服务商), 而进入你别墅的凭证就是别墅的钥匙, 如果你直接将钥匙(资产访问凭证)给这2个服务商, 这是很危险的, 因为他们也可以复制你的钥匙, 此时你的别墅的安全体系就崩塌了, 假如你真的很信任他们, 但是如果园艺公司的人有问题, 他可能盗走了你给他的钥匙, 此时咋办, 唯有换锁, 但是锁换了 别墅的清洁服务也会受到影响, 因此你会发现 把钥匙直接给第三方是很不安全的, 会引发很多安全隐患问题。

# OAuth2的解决思路
对于上面的栗子, 当有多个第三方服务商要访问我们的资源时(进入别墅), 仅通过1把锁的方式来保证资产的安全访问是很成问题的, 因此我们需要一层中间层来管理这些人的访问权限, 常见的做法是招一个管家, 每次访问时由管家询问别墅主人进行授权。

OAuth的实现方式与此类似, OAuth在”客户端”(第三方服务)与”服务提供商”(用户自己的资产服务)之间，设置了一个授权层(类似于上面的管家)。”客户端”不能直接登录”服务提供商”(第三方无法直接进入别墅), 只能登录授权层(询问管家)，以此将用户(资产的主人)与客户端(第三方)区分开来。

“客户端”登录授权层所用的令牌(token), 与用户的密码不同。用户可以在登录的时候，指定授权层令牌的权限范围和有效期。”客户端”登录授权层以后，”服务提供商”根据令牌的权限范围和有效期，向”客户端”开放用户资产访问的权限。

# OAuth2简介

## 名词定义
（1）Third-party application：第三方应用程序，本文中又称"客户端"（client），即上一节例子中的园艺公司、清洁公司。
（2）HTTP service：HTTP服务提供商，本文中简称"服务提供商"，即上一节例子中的别墅。
（3）Resource Owner：资源所有者，本文中又称"用户"（user）。
（4）User Agent：用户代理，本文中就是指浏览器。
（5）Authorization server：认证服务器，即服务提供商专门用来处理认证的服务器。
（6）Resource server：资源服务器，即服务提供商存放用户生成的资源的服务器。它与认证服务器，可以是同一台服务器，也可以是不同的服务器。

## 运行流程

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/1%20OAuth%202%20flow.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/1%20OAuth%202%20flow.png)
（A）用户打开客户端以后，客户端要求用户给予授权。
（B）用户同意给予客户端授权。
（C）客户端使用上一步获得的授权，向认证服务器申请令牌。
（D）认证服务器对客户端进行认证以后，确认无误，同意发放令牌。
（E）客户端使用令牌，向资源服务器申请获取资源。
（F）资源服务器确认令牌无误，同意向客户端开放资源。

不难看出来，上面六个步骤之中，B是关键，即用户怎样才能给于客户端授权。有了这个授权以后，客户端就可以获取令牌，进而凭令牌获取资源

# OAuth2客户端的授权模式

客户端必须得到用户的授权（authorization grant），才能获得令牌（access token）。OAuth 2.0定义了四种授权方式。

-   授权码模式（authorization code）
-   简化模式（implicit）
-   密码模式（resource owner password credentials）
-   客户端模式（client credentials）

## 授权码模式（authorization code）

授权码模式（authorization code）是功能最完整、流程最严密的授权模式。它的特点就是通过客户端的后台服务器，与"服务提供商"的认证服务器进行互动。

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/2%20authorization%20code.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/2%20authorization%20code.png)

（A）用户访问客户端，后者将前者导向认证服务器。
（B）用户选择是否给予客户端授权。
（C）假设用户给予授权，认证服务器将用户导向客户端事先指定的"重定向URI"（redirection URI），同时附上一个授权码。
（D）客户端收到授权码，附上早先的"重定向URI"，向认证服务器申请令牌。这一步是在客户端的后台的服务器上完成的，对用户不可见。
（E）认证服务器核对了授权码和重定向URI，确认无误后，向客户端发送访问令牌（access token）和更新令牌（refresh token）。
 
A步骤中，客户端申请认证的URI，包含以下参数：

-   **response_type**：表示授权类型，必选项，此处的值固定为"**code**"
-   client_id：表示客户端的ID，必选项
-   redirect_uri：表示重定向URI，可选项
-   scope：表示申请的权限范围，可选项
-   state：表示客户端的当前状态，可以指定任意值，认证服务器会原封不动地返回这个值。

具体授权格式如下：
```
https://github.com/login/oauth/authorize?response_type=code&client_id=99a27aea912519cfeb7d&redirect_uri=http%3A%2F%2F10.66.4.95%3A3000%2Fcallback&scope=notifications&state=3(%230%2F!~
```
B步骤中，用户登陆授权：

在用户第一次登陆“客户端”并通过“认证服务器”（Github）认证的时候，会有一个登陆以及确认授权的步骤，如下图。之后再次登陆，这个右图将自动跳过。（猜想：对于该用户，也许scope改变的时候，它会再次出现）
![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/3%20user%20login.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/3%20user%20login.png)

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/4%20user%20authorize.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/4%20user%20authorize.png)

C步骤中，服务器回应客户端的URI，包含以下参数：

-   code：表示授权码，必选项。该码的有效期应该很短，通常设为10分钟，客户端只能使用该码一次，否则会被授权服务器拒绝。该码与客户端ID和重定向URI，是一一对应关系。
-   state：如果客户端的请求中包含这个参数，认证服务器的回应也必须一模一样包含这个参数。

具体格式如下：
```
HTTP/1.1 302 Found Location: 
http://10.66.4.95:3000/callback?code=45ca6e53dab3a6cd744f &state=3(#0/!~
```
D步骤中，客户端向认证服务器申请令牌的HTTP请求，包含以下参数：

-   **grant_type**：表示使用的授权模式，必选项，此处的值固定为"**authorization_code**"。
-   code：表示上一步获得的授权码，必选项。
-   redirect_uri：表示重定向URI，必选项，且必须与A步骤中的该参数值保持一致。
-   client_id：表示客户端ID，必选项。
-   测试发现request header中，需要**Authorization: Basic token。**该token是由client_id和client_secret组合的base64编码

具体格式如下：
```
POST /token HTTP/1.1 Host: server.example.com 
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW 
Content-Type: application/x-www-form-urlencoded 

grant_type=authorization_code&code=SplxlOBeZQQYbYS6WxSbIA &redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMzE1MjYxODAsLTMyMDY3MDM1LC0xNz
c5Nzg0NTIyXX0=
-->