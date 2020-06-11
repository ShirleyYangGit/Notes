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
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTgxODIwODgyMyw3MzA5OTgxMTZdfQ==
-->