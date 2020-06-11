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
![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/11%20SSH%E5%8D%8F%E8%AE%AE%E5%8D%8F%E5%95%86.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/11%20SSH%E5%8D%8F%E8%AE%AE%E5%8D%8F%E5%95%86.png)

如果该过程中，客户端或服务端发送SSH版本无法兼容，任何一方都可以断开连接。

## 服务端认证——建立加密通道

完成协议协商阶段后，客户端与服务端已经建立**明文**的通信通道，之后进入服务端认证阶段。

1.  服务端向客户端发送Public Key, 8字节随机数（防止IP地址欺诈）， 加密算法、压缩方式和认证方式，  
2.  客户端检查自己的knows host数据库（一般为~/.ssh/know_hosts文件），如果没有包含当前服务端的Public Key, 则需要**用户决定**是否信任该服务端
3.  客户端生成**Session Key**，然后使用服务端的Public Key对Session Key进行加密, 然后发送给服务端。
4.  服务端收到数据后，用Private Key进行解密，得到Session Key。

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/12%20SSH%E6%9C%8D%E5%8A%A1%E7%AB%AF%E8%AE%A4%E8%AF%81.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/12%20SSH%E6%9C%8D%E5%8A%A1%E7%AB%AF%E8%AE%A4%E8%AF%81.png)
注意：客户端和服务器会默认此次会话的session id为：服务端Public Key和8字节的随机数生成一个128位的MD5值。  

## 客户端认证——加密通道

### 密码认证
![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/13%20SSH%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%AF%86%E7%A0%81%E8%AE%A4%E8%AF%81.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/13%20SSH%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%AF%86%E7%A0%81%E8%AE%A4%E8%AF%81.png)
优点：简单，无需任何其他配置

**缺点**: 每次登录都要输入密码很麻烦，密码容易忘记，过于简单的密码容易被暴力破解。

### Public Key认证

Public Key认证提供了一种更安全便捷的认证客户端的方式。这个技术也用到了非对称加密技术，由客户端生成公私密钥对，然后将公钥保存在服务端的密钥库（Linux中存储在~/.ssh/**authorized_keys**文件）。

例如：Github中使用Git协议push代码前要先添加SSH Key。

认证的过程大体如下：

-   客户端发起一个Public Key的认证请求，并发送RSA Key的模数作为标识符。
-   服务端检查是否存在请求帐号的公钥（Linux中存储在~/.ssh/**authorized_keys**文件中），以及其拥有的访问权限。如果没有则断开连接
-   服务端使用对应的公钥对一个随机的256位的字符串进行加密，并发送给客户端
-   客户端使用私钥对字符串进行解密，并将其结合session id生成一个MD5值发送给服务端。**结合session id的目的是为了避免攻击者采用重放攻击（replay attack）**。
-   服务端采用同样的方式生成MD5值与客户端返回的MD5值进行比较，完成对客户端的认证。

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/14%20Public%20Key%E8%AE%A4%E8%AF%81.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/14%20Public%20Key%E8%AE%A4%E8%AF%81.png)

# 中间人攻击

SSH之所以能够保证安全，主要原因在于它采用了公钥加密来传输Session Key。

过程大概是这样的：

（1）远程主机收到用户的登录请求，把自己的公钥发给用户。
（2）用户使用这个公钥，将Session Key加密后，发送回来。
（3）远程主机用自己的私钥，解密得到Session Key。

这个过程本身是安全的，但是实施的时候存在一个风险：如果有人截获了登录请求，然后冒充远程主机，将伪造的公钥发给用户，那么用户很难辨别真伪。因为SSH协议的公钥都是自己签发的。

可以设想，如果攻击者插在用户与远程主机之间（比如在公共的wifi区域），用伪造的公钥，与用户建立加密通道，进而获取用户的登录密码。再用这个密码登录远程主机，那么SSH的安全机制就荡然无存了。

这种风险就是著名的["中间人攻击"](http://en.wikipedia.org/wiki/Man-in-the-middle_attack)（Man-in-the-middle attack）。

SSH协议是如何应对的呢？

## SSH人工判断

如果你是第一次登录对方主机，系统会出现下面的提示：

> 　　$ ssh user@host
> 　　The authenticity of host 'host (12.18.429.21)' can't be established.
> 　　RSA key fingerprint is `98:2e:d7:e0:de:9f:ac:67:28:c2:42:2d:37:16:58:4d`.
> 
> 　　Are you sure you want to continue connecting (yes/no)?

这段话的意思是，无法确认host主机的真实性，只知道它的公钥指纹，问你还想继续连接吗？

所谓"公钥指纹"，是指公钥长度较长（这里采用RSA算法，长达1024位），很难比对，所以对其进行MD5计算，将它变成一个128位的指纹。上例中是`98:2e:d7:e0:de:9f:ac:67:28:c2:42:2d:37:16:58:4d`，再进行比较，就容易多了。

很自然的一个问题就是，用户怎么知道远程主机的公钥指纹应该是多少？回答是没有好办法，远程主机必须在自己的网站上贴出公钥指纹，以便用户自行核对。

假定经过风险衡量以后，用户决定接受这个远程主机的公钥。

> 　　Are you sure you want to continue connecting (yes/no)? yes

系统会出现一句提示，表示host主机已经得到认可。

> 　　Warning: Permanently added 'host,12.18.429.21' (RSA) to the list of known hosts.

然后，会要求输入密码。

> 　　Password: (enter password)

如果密码正确，就可以登录了。

当远程主机的公钥被接受以后，它就会被保存在文件$HOME/.ssh/known_hosts之中。下次再连接这台主机，系统就会认出它的公钥已经保存在本地了，从而跳过警告部分，直接提示输入密码。

每个SSH用户都有自己的known_hosts文件，此外系统也有一个这样的文件，通常是/etc/ssh/ssh_known_hosts，保存一些对所有用户都可信赖的远程主机的公钥。

## SSL & TLS CA数字认证机构

SSH其实是专门为shell设计的一种通信协议，它垮了两个网络层（传输层和应用层）。通俗点讲就是只有SSH客户端，和SSH服务器端之间的通信才能使用这个协议，其他软件服务无法使用它。但是其实我们非常需要一个通用的，建立在应用层之下的一个传输层安全协议，它的目标是建立一种对上层应用协议透明的，不管是HTTP、FTP、还是电子邮件协议或其他任何应用层协议都可以依赖的底层的可安全通信的传输层协议。

网景公司于1994年为解决上面的问题，设计了SSL（Secure Sockets Layer）协议的1.0版本，但并未发布，直到1996年发布SSL3.0之后，开始大规模应用于互联网服务。TLS（Transport Layer Security）是SSL协议的一个后续版本，他是SSL经过IETF标准化之后的产物（详细参考[传输层安全协议](https://links.jianshu.com/go?to=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F%25E5%2582%25B3%25E8%25BC%25B8%25E5%25B1%25A4%25E5%25AE%2589%25E5%2585%25A8%25E5%258D%2594%25E8%25AD%25B0)，下文中所说的SSL协议也包括TLS）。

跟SSH相比SSL所面临的问题要更复杂一些，上面我们提到，SSH协议通过人工鉴别Public Key的printfinger来判断与之通信的服务器是否可信（不是伪装的中间人）。可是SSL是为了整个互联网上的所有客户端与服务器之间通信而设计的，他们彼此之间不可能自己判断通信的对方是否可信。那么如何解决这个问题呢？答案是[CA(数字证书认证机构)](https://links.jianshu.com/go?to=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F%25E6%2595%25B0%25E5%25AD%2597%25E8%25AF%2581%25E4%25B9%25A6%25E8%25AE%25A4%25E8%25AF%2581%25E6%259C%25BA%25E6%259E%2584) 它为浏览器发行一个叫[数字证书](https://links.jianshu.com/go?to=https%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F%25E9%259B%25BB%25E5%25AD%2590%25E8%25AD%2589%25E6%259B%25B8)。比较重要的信息有：

-   服务器的公开密钥
-   数字签名 ：服务器证书内容--->Hash得到摘要Digest--->CA私钥进行加密

![https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/15%20%E6%95%B0%E5%AD%97%E7%AD%BE%E5%90%8D.png](https://raw.githubusercontent.com/ShirleyYangGit/Pictures/master/ComputerNetwork/SSH/15%20%E6%95%B0%E5%AD%97%E7%AD%BE%E5%90%8D.png)
# SSH Commands

## 配置环境

用Docker起一个ubuntu的container。安装ssh server, 它的配置文件默认位于/etc/ssh/sshd_config
```
config ssh server

$ apt update
$ apt-get install net-tools
$ apt-get install iputils-ping

安装sshd
$ apt-get install openssh-server

启动sshd
$ /etc/init.d/ssh start
or
$ service ssh start

查看
$ ps -e | grep sshd
```
## SSH
```
远程登录命令: 使用ssh连接远程主机的2222端口
$ ssh user@server -p 2222

指定密钥文件
$ ssh -i ~/.ssh/id_rsa_test user@server -p 2222

调试信息
$ ssh -v user@server -p 2222
```
## ssh-keygen

运行上面的命令以后，系统会出现一系列提示，可以一路回车。其中有一个问题是，要不要对私钥设置口令（passphrase），如果担心私钥的安全，这里可以设置一个。
```
生成密钥：指定算法RSA，添加Email，指定密钥文件名
$ ssh-keygen -t rsa -C "mytest@example.com"  -f "id_rsa_test"
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in id_rsa_test.
Your public key has been saved in id_rsa_test.pub.
The key fingerprint is:
SHA256:jNmMVIVW0MCGmBQYTdeewEwF3xV/mNECQqeewi+racA mytest@example.com
The key's randomart image is:
+---[RSA 2048]----+
|.o=+o+XoO=+.+o |
| o.+o= * = +o |
|. ..o o o o o o |
| ....o o . o .|
|. .o..O S |
|.E o+ + |
| . o |
| + |
| . |
+----[SHA256]-----+
```
运行结束以后，在$HOME/.ssh/目录下，会新生成两个文件：id_rsa_test.pub和id_rsa_test。前者是你的公钥，后者是你的私钥。

## ssh-copy-id
将公钥传送到远程主机server上面

复杂实现
```
$ ssh user@host -p 2222 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ./id_rsa_test.pub
```
命令解析：
(1) `$ ssh user@host -p 2222`，表示登录远程主机，端口2222；
(2) 单引号中的`mkdir .ssh && cat >> .ssh/authorized_keys`，表示登录后在远程shell上执行的命令；
(3) `$ mkdir -p .ssh`的作用是，如果用户主目录中的.ssh目录不存在，就创建一个；
(4) 
```
'cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa_test.pub 
```
的作用是，将本地的公钥文件`~/.ssh/id_rsa_test.pub`，重定向追加到远程文件`authorized_keys`的末尾。

简单实现
`$ ssh-copy-id user@server`

指定本地的ssh公钥文件:
```
$ ssh-copy-id -i ~/.ssh/id_rsa_test.pub yyx@127.0.0.1 -p 2222
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "./id_rsa_test.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
yyx@127.0.0.1's password:

Number of key(s) added: 1

Now try logging into the machine, with: "ssh -p '2222' 'yyx@127.0.0.1'"
and check to make sure that only the key(s) you wanted were added.
```
## scp

scp是 secure copy的缩写, scp是linux系统下基于ssh登陆进行安全的远程文件拷贝命令。
```
$ scp [可选参数] file_source file_target

从本地复制到远程 (远程端口 2222)
$ scp -P 2222 local_file remote_username@remote_ip:remote_file

从远程复制到本地（远程端口 2222）
$ scp -P 2222 remote@www.runoob.com:/usr/local/sin.sh
```

## ssh-keyscan

ssh-keyscan 批量获取集群上机器的密钥指纹。

1.  准备公钥指纹的IP或hostname的列表，保存在hostlist.txt中  
    ```
    127.0.0.1
    127.0.0.2
    ```
    
2.  执行命令
    ```
    $ ssh-keyscan -f hostlist.txt
    # 127.0.0.1 SSH-2.0-OpenSSH_6.6.1
    127.0.0.1 ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCWBZ3XrIajPmnd6R+g/wcUuOPOiRBMOYjAl4Dv8SfcZtgHqKTK6Zb1EeG3u/uzRYxqXMctG/2A4iXRDG9mvg9H9bimCWbA3xtR79NImPYg4m7BNuH9C+OXRYYJwoOGpjVMs0rGLXkq3/WVkXvQreBuhVD8NI2pEPnQsT1J5abdVbCHlwFYG6wVCJQqFY6jdntJJlxQv5EJu6w4/+Fd4LvdjysH+ngqArac6HMJUxqSxLQjzMdCRWEQKp3ySwmnRp9rHYVaJnnsXeYPfnMN1iMjdIQJPzc89Mepg4ip1q2bCMbMcx2XFO3I7YjYRdcOameFNafMGY0q5RHzhvgnNnal
    # 127.0.0.1 SSH-2.0-OpenSSH_6.6.1
    127.0.0.1 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBCPWoEQ7iCCYDrpyb5KeMmCaQ8aOnSfehqmrplZRkbqqnkS9++PdSX/eSLJ0tkFd5902/C+HTCqbDgso4mCKpMo=
    # 127.0.0.2 SSH-2.0-OpenSSH_6.6.1
    127.0.0.2 ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCWBZ3XrIajPmnd6R+g/wcUuOPOiRBMOYjAl4Dv8SfcZtgHqKTK6Zb1EeG3u/uzRYxqXMctG/2A4iXRDG9mvg9H9bimCWbA3xtR79NImPYg4m7BNuH9C+OXRYYJwoOGpjVMs0rGLXkq3/WVkXvQreBuhVD8NI2pEPnQsT1J5abdVbCHlwFYG6wVCJQqFY6jdntJJlxQv5EJu6w4/+Fd4LvdjysH+ngqArac6HMJUxqSxLQjzMdCRWEQKp3ySwmnRp9rHYVaJnnsXeYPfnMN1iMjdIQJPzc89Mepg4ip1q2bCMbMcx2XFO3I7YjYRdcOameFNafMGY0q5RHzhvgnNnal
    # 127.0.0.2 SSH-2.0-OpenSSH_6.6.1
    127.0.0.2 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBCPWoEQ7iCCYDrpyb5KeMmCaQ8aOnSfehqmrplZRkbqqnkS9++PdSX/eSLJ0tkFd5902/C+HTCqbDgso4mCKpMo=
    ```
3.  可以直接将结果重定向
    ```
    ssh-keyscan -f hostlist.txt 1>>~/.ssh/known_hosts 2>/dev/null
    ```
    

## ssh-agent & ssh-add

ssh-agent是一种控制用来保存公钥身份验证所使用的私钥的程序，启动后，可以使用ssh-add将私钥交给ssh-agent保管。

- start ssh-agent
`$ eval ssh-agent -s` 
- add id_rsa_test
`$ ssh-add ~/.ssh/id_rsa_test`
- 查看
    ```
    $ ssh-add -l
    2048 SHA256:QOtjNmMVIVMEREWdsWfQdgdwF3xV/mNsdWEQqE+racA mytest@example.com (RSA)
    $ ssh-add -k
    Identity added: /Users/yaxingy/.ssh/id_rsa (yaxingy@splunk.com)
    ```
在每台服务器上都配置，告诉ssh 允许 ssh-agent 转发
- 修改全局：
`$ echo "ForwardAgent yes" >> /etc/ssh/ssh_config`
- 修改个人
- 
`$ touch ~/.ssh/config`
`$ vim ~/.ssh/config`
`Host *`
`　　ForwardAgent yes`

  

# References

-   [http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)
-   [https://www.ssh.com/ssh/#sec-The-SSH-protocol](https://www.ssh.com/ssh/#sec-The-SSH-protocol)
-   [https://www.jianshu.com/p/5e3f9dfd2cb4](https://www.jianshu.com/p/5e3f9dfd2cb4)
-   [http://erik-2-blog.logdown.com/posts/74081-ssh-principle](http://erik-2-blog.logdown.com/posts/74081-ssh-principle)
-   [http://www.ruanyifeng.com/blog/2011/08/what_is_a_digital_signature.html](http://www.ruanyifeng.com/blog/2011/08/what_is_a_digital_signature.html)
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjg4MDY4MDU0LDczMDk5ODExNl19
-->