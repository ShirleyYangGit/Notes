# Mac Start
## Config Environment

1.  Install HomeBrew: **/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"**
2.  Install pip: **sudo easy_install pip**
3.  Install virtualenv: **sudo pip install virtualenv**  or **pip install virtualenv --user**
4. Config on-my-zsh. Copy zsh related files to /Users/own_user/ path:  **.zshrc**  and  **.zsh_history**  and  **.oh-my-zsh**

## FAQ
## MacOS 声音故障
refer to [https://www.sysgeek.cn/macos-fix-sound/](https://www.sysgeek.cn/macos-fix-sound/)
-   sudo killall coreaudiod  
-   重启电脑
## Mac zsh terminal字体问题
https://blog.csdn.net/liuchuo/article/details/79967960
关于字体有背景色问题，打开terminal的偏好设置，点击描述文件（profiles），把“显示ANSI颜色”选项取消即可

关于波浪线两边的两个问号问题，是因为配置中有非ascii字符编码，这两个问号本来是好看的箭头，但是箭头在当前字体中是不会被显示的……所以解决方法是重新下载一个支持非ascii编码的字体：

github上有一个字体：yizhen20133868/fonts

在terminal中执行以下代码：
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyNzE4MTU4NTUsLTE3MzU3MTc3MzRdfQ
==
-->