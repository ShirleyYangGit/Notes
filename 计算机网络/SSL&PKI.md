# SSL & PKI
# SSL

SSL（Secure Socket Layer，安全套接字层） 协议位于TCP/IP协议与各种应用层协议之间，为数据通讯提供安全支持。SSL是Netscape开发的专门用户保护Web通讯的，目前版本为3.0。**TLS (Transport Layer Security，传输层安全协议)**是IETF(工程任务组)制定的一种新的协议，它建立在SSL 3.0协议规范之上，是SSL 3.0的后续版本。两者差别极小，可以理解为SSL 3.1，它是写入了RFC的。  
SSL/TLS通过互相认证、使用数字签名确保完整性、使用加密确保私密性，以实现客户端和服务器之间的安全通讯。

HTTPS是基于SSL/TLS 实现的安全通信。在客户端与服务器间传输的数据是通过使用对称算法（如 DES 或 RC4）进行加密的。公用密钥算法（通常为 RSA）是用来获得加密密钥交换和数字签名的，此算法使用服务器的SSL数字证书中的公用密钥。有了服务器的SSL数字证书，客户端也可以验证服务器的身份。SSL 协议的版本 1 和 2 只提供服务器认证。版本 3 添加了客户端认证，此认证同时需要客户端和服务器的数字证书。

## SSL 握手

SSL 连接总是由客户端启动的。在SSL 会话开始时执行 SSL 握手。此握手产生会话的密码参数。关于如何处理 SSL 握手的简单概述，如下图所示。此示例假设已在 Web 浏览器 和 Web 服务器间建立了 SSL 连接。

SSL的客户端与服务器端的认证握手图：


# Public Key Infrastructure

## PKI架构

所有与数字证书相关的各种概念和技术，统称为PKI（ Public Key Infrastructure 公钥基础设施）。

> PKI通过引入CA，数字证书，LDAP，CRL，OCSP等技术并制定相应标准，有效地解决了公钥与用户映射关系，集中服务性能瓶颈，脱机状态查询等问题。同时为促进并提高证书应用的规范性，还制定了很多与证书应用相关的各种标准。

公钥在网络传输过程中，无法保证可信度，容易被窃取或者伪装，所以我们就需要一个受信任的第三方机构CA来保证公钥信息的安全分发。有了根CA之后，证书颁发机构信任根CA，不同证书颁发机构只要被根CA信任，那么颁发的证书都是受信任的。  
注意：浏览器默认已经内置第三方的受信任的根证书的颁发机构

使用证书保护Web访问的安全实现SSL的基本原理如下：


## 证书签发

1.  服务方S向第三方机构CA提交公钥、组织信息、个人信息(域名)等信息并申请认证;
    
2.  CA通过线上、线下等多种手段验证申请者提供信息的真实性，如组织是否存在、企业是否合法，是否拥有域名的所有权等;
    
3.  如信息审核通过，CA会向申请者签发认证文件-证书。 证书包含以下信息：申请者公钥、申请者的组织信息和个人信息、签发机构 CA的信息、有效时间、证书序列号等信息的明文，同时包含一个签名。  
    签名的产生算法：首先，使用散列函数计算公开的明文信息的信息摘要，然后，采用 CA的私钥对信息摘要进行加密，密文即签名；
    
4.  客户端 C 向服务器 S 发出请求时，S 返回证书文件;
    
5.  客户端 C读取证书中的相关的明文信息，采用相同的散列函数计算得到信息摘要，然后，利用对应 CA的公钥解密签名数据，对比证书的信息摘要，如果一致，则可以确认证书的合法性，即公钥合法;
    
6.  客户端然后验证证书相关的域名信息、有效时间等信息;
    
7.  客户端会内置信任CA的证书信息(包含公钥)，如果CA不被信任，则找不到对应 CA的证书，证书也会被判定非法。
    

## CA (Certificate Authority)

全球有两种类型的证书颁发机构，主要分为两个部门：区域供应商和全球供应商，因为证书验证通常可以通过当地国家的法律法规，但这些证书并不被全球所有浏览器承认，只能在自己国家的浏览器承认，然而持有跨国公司CA机构屈指可数，全球市场上约50个ssl证书颁发机构，中国之前有四家，后来沃通由于滥发证书被取消了CA机构资格（不一定准），国内目前三家，这些CA机构签发的证书基本被全球所有的浏览器承认。

目前全球主流的CA机构有[Comodo](https://ssl.idcspy.net/comodo/ "Comodo证书")、[Symantec](https://ssl.idcspy.net/symantec/ "Symantec证书")、[GeoTrust](https://ssl.idcspy.net/geotrust/ "GeoTrust证书")、DigiCert、[Thawte](https://ssl.idcspy.net/thawte/ "Thawte证书")、GlobalSign、[RapidSSL](https://ssl.idcspy.net/rapidssl/ "RapidSSL证书")等，其中Symantec、GeoTrust都是DigiCert机构的子公司，目前市场上主流的ssl证书品牌是Comodo证书、Symantec证书、GeoTrust证书、Thawte证书和RapidSSL证书，还有一些不知名的证书机构也是可以颁发数字证书的。

## 文件后缀

-   .csr stands for Certificate Signing Request.  
    编码有可能是 PEM，也可能是 DER  
    
-   .crt and .cer stand simply for certificate.  
    应该是 Certification 的缩写。  
    .crt 常见于 Unix 系统，编码有可能是 PEM，也可能是 DER，大多数应该是 PEM 编码。  
    .cer 常见于 Windows 系统，编码有可能是 PEM，也可能是 DER，大多数应该是 DER 编码。

-   .key can be any kind of key, but usually it's the private key.  
    通常用来存放一个公钥或者私钥，并非 X.509 证书，编码同样可能是 PEM 或 DER。
-   .pem stands for PEM, Privacy Enhanced Mail.  
    PEM格式文件。  `.pem, .crt, .cer, .key` 这样的扩展名的文件都可以是PEM格式。

[https://www.jianshu.com/p/96df7de54375](https://www.jianshu.com/p/96df7de54375)  

# openssl

OpenSSL是一个强大的安全套接字层密码库。作为一个基于密码学的安全开发包，OpenSSL提供的功能相当强大和全面，囊括了主要的密码算法、常用的密钥和证书封装管理功能以及SSL协议，并提供了丰富的应用程序供测试或其它目的使用。

## RSA加解密

-   生成一个私钥
    ```
    [root@hunterfu ~]# openssl genrsa -out private.key 1024
    ```
    
    注意: 需要注意的是这个文件包含了公钥和密钥两部分，也就是说这个文件即可用来加密也可以用来解密, 后面的1024是生成密钥的长度.
    
-   通过密钥文件private.key 提取公钥
    ```
    [root@hunterfu ~]# openssl rsa -in private.key -pubout -out pub.key
    ```
    
-   使用公钥加密信息
    ```
    [root@hunterfu ~]# echo -n "123456" | openssl rsautl -encrypt -inkey pub.key -pubin >encode.result
    ```
    
-   使用私钥解密信息
    ```
    [root@hunterfu ~]#cat encode.result | openssl rsautl -decrypt -inkey private.key 
    123456
    ```
    
至此，一次RSA加密解密的过程已经完成！

## DSA签名与认证

和RSA加密解密过程相反，在DSA数字签名和认证中，发送者使用自己的私钥对文件或消息进行签名，接受者收到消息后使用发送者的公钥来验证签名的真实性

DSA只是一种算法，和RSA不同之处在于它不能用作加密和解密，也不能进行密钥交换，只用于签名,它比RSA要快很多.

-   生成一个密钥(私钥)
    ```
    [root@hunterfu ~]# openssl dsaparam -out dsaparam.pem 1024
    [root@hunterfu ~]# openssl gendsa -out privkey.pem dsaparam.pem
    ```
    
-   生成公钥
    ```
    [root@hunterfu ~]# openssl dsa -in privkey.pem -out pubkey.pem -pubout
    [root@hunterfu ~]# rm -fr dsaparam.pem
    ```
    
-   使用私钥签名
    ```
    [root@hunterfu ~]# echo -n "123456" | openssl dgst -dss1 -sign privkey.pem > sign.result
    ```
    
-   使用公钥验证
    ```
    [root@hunterfu ~]# echo -n "123456" | openssl dgst -dss1 -verify pubkey.pem -signature sign.result
    Verified OK
    ```

至此，一次DSA签名与验证过程完成！

## 生成自己的CA (Certificate Authority)

1.  生成CA的key
    ```
    openssl genrsa -des3 -out ca.key 4096
    ```
    
2.  生成CA的证书
    ```
    openssl req -new -x509 -days 365 -key ca.key -out ca.crt
    ```
    
3.  生成我们的key和CSR
    ```
    openssl genrsa -des3 -out myserver.key 4096
    openssl req -new -key myserver.key -out myserver.csr
    ```
    
4.  使用ca的证书和key，生成我们的证书
    ```
    openssl x509 -req -days 365 -in myserver.csr -CA ca.crt -CAkey ca.key -set_serial 01 -out myserver.crt
    ```
    注意: 这里的set_serial指明了证书的序号，如果证书过期了(365天后)， 或者证书key泄漏了，需要重新发证的时候，就要加1

## 查看证书

1.  查看KEY信息
    `openssl rsa -noout -text -in myserver.key`
    
2.  查看CSR信息
    `openssl req -noout -text -in myserver.csr`
    
3.  查看证书信息
    `openssl x509 -noout -text -in ca.crt`
    
4.  验证签发的证书
    `openssl verify -CAfile ca.crt myserver.crt`
    
      
    

# Client Certificates with Passphrases

# Related Links

[https://www.51know.info/docs/security/other/openssl/](https://www.51know.info/docs/security/other/openssl/)  
[https://www.cnblogs.com/littlehann/p/3738141.html](https://www.cnblogs.com/littlehann/p/3738141.html)  
[https://www.wosign.com/basic/howsslwork.htm](https://www.wosign.com/basic/howsslwork.htm)
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjQxNDg0MzE5LDczMDk5ODExNl19
-->