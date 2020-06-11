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

grant_type=authorization_code&code=SplxlOBeZQQYbYS6WxSbIA 
&redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb
```
E步骤中，认证服务器发送的HTTP回复，包含以下参数：

-   access_token：表示访问令牌，必选项。
-   token_type：表示令牌类型，该值大小写不敏感，必选项，可以是bearer类型或mac类型。
-   expires_in：表示过期时间，单位为秒。如果省略该参数，必须其他方式设置过期时间。
-   refresh_token：表示更新令牌，用来获取下一次的访问令牌，可选项。
-   scope：表示权限范围，如果与客户端申请的范围一致，此项可省略。

具体格式如下：
```
HTTP/1.1 200 OK 
Content-Type: application/json;charset=UTF-8 
Cache-Control: no-store 
Pragma: no-cache 
{ 
"access_token":"2YotnFZFEjr1zCsicMWpAA", 
"token_type":"example_value", "expires_in":3600, 
"refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA", 
"example_parameter":"example_value" 
}
```
## 简化模式（implicit）

简化模式（implicit grant type）不通过第三方应用程序的服务器，直接在浏览器中向认证服务器申请令牌，跳过了"授权码"这个步骤，因此得名。所有步骤在浏览器中完成，令牌对访问者是可见的，且客户端不需要认证。

应用场景: 适用于所有无Server端配合的应用(由于应用往往位于一个User Agent里，如浏览器里面，因此这类应用在某些平台下又被称为`Client-Side Application`), 如手机/桌面客户端程序、浏览器插件等，以及基于JavaScript等脚本客户端脚本语言实现的应用，他们的一个共同特点是，无服务端,无法监听端口直接收到回调token, 并且应用无法妥善保管其应用密钥(App Secret Key), 如果采取Authorization Code模式，则会存在泄漏其应用密钥(api_scret)的可能性

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/5%20implicit%20grant%20type.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/5%20implicit%20grant%20type.png)

（A）客户端将用户导向认证服务器。
（B）用户决定是否给于客户端授权。
（C）假设用户给予授权，认证服务器将用户导向客户端指定的"重定向URI"，并在URI的Hash部分包含了访问令牌。
（D）浏览器向资源服务器发出请求，其中不包括上一步收到的Hash值。
（E）资源服务器返回一个网页，其中包含的代码可以获取Hash值中的令牌。
（F）浏览器执行上一步获得的脚本，提取出令牌。
（G）浏览器将令牌发给客户端。

A步骤中，客户端发出的HTTP请求，包含以下参数：

-   **response_type**：表示授权类型，此处的值固定为"**token**"，必选项。
-   client_id：表示客户端的ID，必选项。
-   redirect_uri：表示重定向的URI，可选项。
-   scope：表示权限范围，可选项。
-   state：表示客户端的当前状态，可以指定任意值，认证服务器会原封不动地返回这个值。

C步骤中，认证服务器回应客户端的URI，包含以下参数：

-   access_token：表示访问令牌，必选项。
-   token_type：表示令牌类型，该值大小写不敏感，必选项。
-   expires_in：表示过期时间，单位为秒。如果省略该参数，必须其他方式设置过期时间。
-   scope：表示权限范围，如果与客户端申请的范围一致，此项可省略。
-   state：如果客户端的请求中包含这个参数，认证服务器的回应也必须一模一样包含这个参数。

下面是一个例子：
```
HTTP/1.1 302 Found 
Location: http://example.com/cb#access_token=2YotnFZFEjr1zCsicMWpAA &state=xyz&token_type=example&expires_in=3600
```
在上面的例子中，认证服务器用HTTP头信息的Location栏，指定浏览器重定向的网址。注意，在这个网址的Hash部分包含了令牌。

根据上面的D步骤，下一步浏览器会访问Location指定的网址，但是Hash部分不会发送。接下来的E步骤，服务提供商的资源服务器发送过来的代码，会提取出Hash中的令牌。

**注意**：在该模式的实现过程中, D和E步一般都会省略掉, 因为Web-Hosted Client Resource提供的这段token提取脚本, 客户端可以自己实现。

## 密码模式（resource owner password credentials）

密码模式（Resource Owner Password Credentials Grant）中，**用户向客户端提供自己的用户名和密码**。客户端使用这些信息，向"服务商提供商"索要授权。

在这种模式中，用户必须把自己的密码给客户端，但是**客户端不得储存密码**。这通常用在用户对**客户端高度信任**的情况下，比如客户端是操作系统的一部分，或者由一个著名公司出品。而认证服务器只有在其他授权模式无法执行的情况下，才能考虑使用这种模式。

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/6%20Resource%20Owner%20Password%20Credentials%20Grant.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/6%20Resource%20Owner%20Password%20Credentials%20Grant.png)

（A）用户向客户端提供用户名和密码。
（B）客户端将用户名和密码发给认证服务器，向后者请求令牌。
（C）认证服务器确认无误后，向客户端提供访问令牌。

B步骤中，客户端发出的HTTP请求，包含以下参数：

-   **grant_type**：表示授权类型，此处的值固定为"**password**"，必选项。
-   username：表示用户名，必选项。
-   password：表示用户的密码，必选项。
-   scope：表示权限范围，可选项。

## 客户端模式（client credentials）

客户端模式（Client Credentials Grant）指**客户端以自己的名义**，而不是以用户的名义，向"服务提供商"进行认证。严格地说，客户端模式并不属于OAuth框架所要解决的问题。在这种模式中，用户直接向客户端注册，客户端以自己的名义要求"服务提供商"提供服务，其实不存在授权问题。

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/7%20Client%20Credentials%20Grant.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/7%20Client%20Credentials%20Grant.png)

（A）客户端向认证服务器进行身份认证，并要求一个访问令牌。
（B）认证服务器确认无误后，向客户端提供访问令牌。

A步骤中，客户端发出的HTTP请求，包含以下参数：

-   **grant_****type**：表示授权类型，此处的值固定为"**client_credentials**"，必选项。
-   scope：表示权限范围，可选项。

# FAQ

1.  为什么授权码模式中一定要有authorization code, 不能直接根据callback url返回access token吗？  
    主要原因是为了安全。callback url有可能是http开头，不是加密传输，这会导致access token很容易被黑客窃取。相比之下，authorization code只有几分钟的有效期，且只能使用该码一次，即使被盗，黑客也需要clientID、redirect uri等信息才能冒充客户端去申请access token，这就大大降低了token泄漏的问题。  
      
    
2.  Client ID从哪里获取的？  
    这是要客户端在认证服务器中注册后，才能得到的。以Github认证服务器为例，可以在[New OAuth Application](https://github.com/settings/applications/new)中注册应用，进而获取Client ID和Client Secret。
    ![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/8%20Register%20a%20new%20application.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/OAuth%202.0/8%20Register%20a%20new%20application.png)
   
3.  资源服务器如何根据access token来确认可访问的资源的？  
    认证服务器生成的access token中，包含了scope权限信息。资源服务器正确解析token后，可以根据scope权限信息设置客户端可访问的内容。

# Demo [simple-oauth2](https://github.com/lelylan/simple-oauth2)

客户端实现认证的内容，可以参考[https://github.com/lelylan/simple-oauth2/tree/master/example](https://github.com/lelylan/simple-oauth2/tree/master/example)


Reference:

-   [http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)
-   [https://blog.yumaojun.net/2017/12/07/oauth2/](https://blog.yumaojun.net/2017/12/07/oauth2/)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM2NjMzMTI1OCwtMzIwNjcwMzUsLTE3Nz
k3ODQ1MjJdfQ==
-->