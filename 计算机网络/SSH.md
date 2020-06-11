# SSH
# 加密

在解释SSH协议之前我先介绍一下对称和非对称加密协议。

## 对称加密

在1976年以前，所有的加密都采用对称加密，既A使用某种加密规则对信息加密，B收到信息后逆向加密规则解密数据。

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/1%20%E5%AF%B9%E7%A7%B0%E5%8A%A0%E5%AF%86.gif](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/1%20%E5%AF%B9%E7%A7%B0%E5%8A%A0%E5%AF%86.gif)

常用的对称加密算法有DES(Data Encryption Standard)、AES(Advanced Encryption Standard)等。

但是这种通信方式产生了一个难以解决的问题：**A如何安全的把加密规则通知B？**

## 非对称加密

在1976年有两位数学家提出了一个崭新的非对称加密的概念：

> 1.A生成一对两把密钥（公钥和私钥）。公钥是公开的，任何人都可以获得，私钥则是保密的。
> 2.B获取A生成的公钥，然后用它对信息加密。
> 3.A得到加密后的信息，用私钥解密。

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/2%20%E9%9D%9E%E5%AF%B9%E7%A7%B0%E5%8A%A0%E5%AF%86.gif](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/2%20%E9%9D%9E%E5%AF%B9%E7%A7%B0%E5%8A%A0%E5%AF%86.gif)

受这个思路的启发，三位数学家Rivest、Shamir 和 Adleman 设计了一种具体实现上面描述的非对称加密的算法，以他们三个人的名字命名，就是目前在计算机领域应用非常广泛的非对称加密算法[RSA加密算法](https://links.jianshu.com/go?to=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2FRSA%25E5%258A%25A0%25E5%25AF%2586%25E6%25BC%2594%25E7%25AE%2597%25E6%25B3%2595)。

这样网络上传输的数据都经过公钥加密，然后用私钥解密，就算被第三方截获也无法解密出原始数据。想深入理解非对称加密解密的原理可以看[这里](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.ruanyifeng.com%2Fblog%2F2013%2F06%2Frsa_algorithm_part_one.html)

### 数字签名

除了上面描述的加密数据的应用，非对称加密算法还有另一种应用——**数字签名。**它是为了**确保数据的完整性**和**来源可靠性（即认证）**。

应用背景：A发表了一些言论，并且希望小伙伴B拿到的数据确实是自己发表的，没有被其他人更改过的。为此，A需要给数据添加一个数字签名。

数字签名的流程如下：

1.  A编辑好数据后，先使用Hash函数，生成数据的摘要（Digest）
2.  A使用私钥对Digest进行加密，得到Signature。
3.  将Signature附在数据后面发送出去。

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/3%20digest.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/3%20digest.png)
![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/4%20signature.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/4%20signature.png)
![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/5%20%E6%95%B0%E5%AD%97%E7%AD%BE%E5%90%8D%E5%90%8E%E7%9A%84%E6%B6%88%E6%81%AF.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/5%20%E6%95%B0%E5%AD%97%E7%AD%BE%E5%90%8D%E5%90%8E%E7%9A%84%E6%B6%88%E6%81%AF.png)

对于收到数据的人来说，可以通过数字签名来完成认证和验证数据的完整性。
-   认证：A用私钥加密数据，该数据只能用A的公钥解密。如果signature能够用A的公钥解密得到Digest，就可以证明该数据是A发出的
-   完整性：对数据本身使用Hash函数得到Digest，对比解密得到的Digest，如果两者一致，就证明数据没有被修改过。

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/6%20%E8%A7%A3%E5%AF%86.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/6%20%E8%A7%A3%E5%AF%86.png)
![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/7%20%E5%AF%B9%E6%AF%94Digest.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/7%20%E5%AF%B9%E6%AF%94Digest.png)

## 应用

虽然非对称加密很安全很强大，但是它也有缺点，相对于对称加密它计算量更大，计算时间更长。

所以在大规模数据的安全通信场景中，**普遍采用非对称加密技术来相互验证，交换对称加密密钥。**

之后会建立session key（比如128位AES key），后续交互的信息都是用session key和对称加密算法（比如AES）来加解密的。

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/8%20%E5%8A%A0%E5%AF%86%E4%BC%A0%E8%BE%93SessionKey.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/8%20%E5%8A%A0%E5%AF%86%E4%BC%A0%E8%BE%93SessionKey.png)

# SSH protocol

SSH协议是在1995年，由芬兰学者Tatu Ylonen设计开发的。起因是，他们在芬兰大学服务器的主干网上发现了一个“密码嗅探器”，上面有大量的用户名密码，甚至有Tatu Ylonen所在公司的很多账号密码。这给他的公司带了很多损失。

这件事令他意识到加密传输的必要性。为此，他开发了SSH协议，将**登录信息安全加密**，并向IETF申请了22端口。目前SSH已经成为Linux系统的标准配置。

## What is SSH protocol

> The SSH protocol (also referred to as **Secure Shell**) is a method for secure remote login from one computer to another. It provides several alternative options for  **strong authentication**, and it protects the communications security and integrity with strong encryption. It is a secure alternative to the non-protected login protocols (such as [telnet](https://www.ssh.com/ssh/telnet), rlogin) and insecure file transfer methods (such as [FTP](https://www.ssh.com/ssh/ftp/)).

简单说，SSH协议是创建在**应用层**上的安全协议，为计算机上的Shell提供安全的认证和数据传输环境。

## How work

> The protocol works in the  **client-server model**, which means that the connection is established by the SSH client connecting to the SSH server.

SSH是一个Client-Server model。也就是说，它有SSH client 和 SSH server。

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/9%20client_server_connection.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/9%20client_server_connection.png)

**实际上，SSH协议使用非对称加密进行传输session key。在数据传输阶段，使用session key对称加密的方式进行传输。**

需要指出的是，SSH协议存在多种实现，既有商业实现，也有开源实现。本文针对的实现是**OpenSSH**，它是自由软件，应用非常广泛。

此外，本文只讨论SSH在Linux Shell中的用法。如果要在Windows系统中使用SSH，会用到另一种软件PuTTY，在此不做介绍。

# SSH连接建立流程

ssh-client与ssh-server连接建立的主要阶段可以划分为：
![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/10%20SSH%E6%B5%81%E7%A8%8B.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/10%20SSH%E6%B5%81%E7%A8%8B.png)
每个阶段均涉及到客户端与服务端的多次交互，通过这些交互过程完成包括**证书传输**、**算法协商**、**通道加密**等过程。

## 协议协商——明文通道

1.  服务端打开服务端口（默认为22），等待客户端连接
2.  客户端发起TCP连接请求，服务端接收到该请求后，向客户端发送包括SSH协议版本信息
3.  客户端接根据该版本信息与自己的版本，决定将要使用的SSH版本，并向服务端发送选用的SSH版本信息
4.  服务端检查是否支持客户端的决定使用的SSH版本

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc1Mjg5MDI3Miw3MzA5OTgxMTZdfQ==
-->