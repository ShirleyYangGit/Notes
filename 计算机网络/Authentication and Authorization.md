# Authentication and Authorization

## HTTP Basic Auth

Basic Auth (HTTP/1.0) is an authorization type that requires a verified username and password to access a data resource.

In Request headers, the value of Authorization: Basic ***** is username:password encoded by base64.

## HTTP Digest Auth

Refer to [https://www.jianshu.com/p/78faeb3a90e6](https://www.jianshu.com/p/78faeb3a90e6)

Digest Auth (HTTP/1.1) is an application of [MD5](https://en.wikipedia.org/wiki/MD5 "MD5")  [cryptographic hashing](https://en.wikipedia.org/wiki/Cryptographic_hash "Cryptographic hash") with usage of [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce "Cryptographic nonce") values to prevent replay attacks. It uses the HTTP protocol.

In a digest authentication flow, the client sends a request to a server, which sends back  **nonce**  and  **realm**  values for the client to authenticate. The client sends back a hashed username and password with the nonce and realm. The server then sends back the requested data.

```
HA1 = MD5( "Mufasa:testrealm@host.com:Circle Of Life" )
    = 939e7578ed9e3c518a452acee763bce9`

HA2 = MD5( "GET:/dir/index.html" )
    = 39aff3a2bab6126f332b942af96d3366

Response = MD5( "939e7578ed9e3c518a452acee763bce9:\
                 dcd98b7102dd2f0e8b11d0f600bfb0c093:\
                 00000001:0a4f113b:auth:\
                 39aff3a2bab6126f332b942af96d3366" )
         = 6629fae49393a05397450978507c4ef1
```
1.  对`用户名`、`认证域(realm)`以及`密码`的合并值计算 MD5 哈希值，结果称为  `HA1`。
2.  对HTTP方法以及URI的摘要的合并值计算 MD5 哈希值，例如，`GET`  和  `/dir/index.html`，结果称为  `HA2`。
3.  对 HA1、服务器密码随机数(nonce)、请求计数(nc)、客户端密码随机数(cnonce)、保护质量(qop)以及 HA2 的合并值计算 MD5 哈希值。结果即为客户端提供的 response 值。

服务器应当记住最近所生成的服务器密码随机数nonce的值。也可以在发行每一个密码随机数nonce后，记住过一段时间让它们过期。如果客户端使用了一个过期的值，服务器应该响应“401”状态号，并且在认证头中添加`stale=TRUE`，表明客户端应当使用新提供的服务器密码随机数nonce重发请求，而不必提示用户其它用户名和口令.

# Authentication ------ OpenID

  
OpenID是以用户为中心的数字身份识别框架，它基于这样的概念：通过URI（或者URL网址）来识别一个网站。  
用户(End User)可以在OpenID服务网站(OpenID Provider)注册一个身份，相当于**身份证**，可以用来访问支持 OpenID的网站(Relying Party)。

### OpenID 相关术语

  

-   End User：终端用户，使用OP与RP的服务
-   Relying Party依赖方：简称RP，服务提供者，需要OP鉴权终端用户的身份
-   OpenID Provider：OpenID提供者，简称OP，对用户身份鉴权
-   Identifier标识符：标识符可以是一个HTTP、HTTPS或者XRI(可扩展的资源标识)
-   User-Agent：实现了HTTP1.1协议的用户浏览器
-   OP Endpoint URL：OP鉴权的URL，提供给RP使用
-   OP Identifier：OP提供给终端用户的一个URI或者XRI,RP根据OP Identifier来解析出OP Endpoint URL与OP Version
-   User-Supplied Identifier：终端用户使用的ID，可能是OP提供的OpenID，也可以是在RP注册的ID。RP可以根据User-Supplied Identifier来解析出OP Endpoint URL、OP Version与OP_Local Identifer
-   Claimed Identifier：终端用户声明自己身份的一个标志，可以是一个URI或者XRI
-   OP-Local Identifier：OP提供的局部ID


## OpenID 验证流程

1.  终端用户请求登录RP网站，用户选择了以OpenID方式来登录
2.  RP将OpenId的登录界面返回给终端用户  
3.  终端用户以OpenID登陆RP网站
4.  RP网站对用户的OpenID进行标准化，此过程非常负责。由于OpenID可能是URI，也可能是XRI，所以标准化方式各不相同。具体标准化过程是：如果OpenID以xri://、xri://$ip或者xri://$dns开头，先去掉这些符号；然后对如下的字符串进行判断，如果第一个字符是=、@、+、$、!，则视为标准的XRI，否则视为HTTP URL（若没有http,为其增加http://）。
5.  RP发现OP，如果OpenId是XRI，就采用XRI解析，如果是URL，则用Yadis协议解析，若Yadis解析失败，则用Http发现。
6.  RP跟OP建立一个关联。两者之间可以建立一个安全通道，用于传输信息并降低交互次数。
7.  OP处理RP的关联请求
8.  RP请求OP对用户身份进行鉴权
9.  OP对用户鉴权，请求用户进行登录认证
10.  用户登录OP
11.  OP将鉴权结果返回给RP
12.  RP对OP的结果进行分析
    
# Authorization ------- OAuth
Refer to [https://www.jianshu.com/p/b06944c92228](https://www.jianshu.com/p/b06944c92228)

An open protocol to allow **secure**  **authorization** in a simple and standard method from web, mobile and desktop applications.

## OAuth history

-   **2007-12**  OAuth 1.0发布并迅速成为工业标准。
-   **2008-06**  OAuth 1.0 Revision A发布，这是个稍作修改的修订版本，主要修正一个安全方面的漏洞。
-   **2010-04**，OAuth 1.0 协议发布为  [RFC 5849](https://link.jianshu.com/?t=http://www.rfcreader.com/#rfc5849)
-   **2011-05**  OAuth 2.0 草案发布
-   **2012-10**  OAuth 2.0 协议发布为  [RFC 6749](https://link.jianshu.com/?t=http://www.rfcreader.com/#rfc6749)

## OAuth 1.0

过于复杂，易用性差，没有得到普及

## OAuth 2.0

-   **资源拥有者(Resource Owner)**  
    资源拥有者其实就是用户(user)，用户将会授权一个第三方应用可以获取他们的账户资源。当然第三方应用程序对于用户账户的操作是有限制的(比如，read access, read and write access)！这个限制就是用户授权时给予的权限范围(**scope**)  
    上面场景中，微博账户就是资源拥有者。read access就比如读取微博用户名，write access就比如以你的名义发了一个微博。
    
-   **客户端(Client)**  
    客户端就是前面说的第三方应用程序，他们想要获取用户的账户资源，但在这么做之前必须经过授权  
    上面场景中，简书就是客户端
    
-   **资源服务器(Resource Server)**  
    资源服务器存放用户账户以及账户信息和资源  
    上面场景中，新浪微博就是资源服务器，同时也是授权服务器
    
-   **授权服务器(Authorization Server)**  
    授权服务器验证用户身份，并为第三方应用程序颁发授权令牌(access token)
    
资源服务器与授权服务器可以是同一台服务器，这里分开主要是便于解释清楚OAuth协议。从程序开发者的角度，这两个都是service's API会执行的事情。

1.  应用程序向用户请求给予授权，以便获取服务器资源
2.  如果用户同意授权，应用程序将获得相应授权
3.  应用程序向授权服务器提供自己的身份证明(app key和app secret)和已被授权的证明(authorization grant)，并请求访问令牌(access token)
4.  如果应用程序的身份被核实，并且授权是有效地，那么授权服务器将会发放访问令牌给应用程序。**此时，授权完成**
5.  应用程序向资源服务器出示访问令牌，并请求资源
6.  如果访问令牌是有效的(比如：是否伪造，是否越权，是否过期)，资源服务器将会为应用程序提供资源
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgxOTcxODMzOSwtMzQxMjI4NzNdfQ==
-->