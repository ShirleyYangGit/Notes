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
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk3ODUzMTQ1MCwtMzQxMjI4NzNdfQ==
-->