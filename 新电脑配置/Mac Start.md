# Mac Start
## Config Environment

1.  Install HomeBrew: **/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"**
2.  Install pip: **sudo easy_install pip**
3.  Install virtualenv: **sudo pip install virtualenv**  or **pip install virtualenv --user**
4. Config on-my-zsh. Copy zsh related files to /Users/own_user/ path:  **.zshrc**  and  **.zsh_history**  and  **.oh-my-zsh**

## FAQ
## MacOS 声音故障
Refer to [https://www.sysgeek.cn/macos-fix-sound/](https://www.sysgeek.cn/macos-fix-sound/)
-   sudo killall coreaudiod  
-   重启电脑
## Mac zsh terminal字体问题
Refer to https://blog.csdn.net/liuchuo/article/details/79967960
关于字体有背景色问题，打开terminal的偏好设置，点击描述文件（profiles），把“显示ANSI颜色”选项取消即可

关于波浪线两边的两个问号问题，是因为配置中有非ascii字符编码，这两个问号本来是好看的箭头，但是箭头在当前字体中是不会被显示的……所以解决方法是重新下载一个支持非ascii编码的字体：
github上有一个字体：yizhen20133868/fonts
在terminal中执行以下代码：
```
# clone
> git clone https:``//github.com/powerline/fonts.git
# install
> cd fonts
> ./install.sh
```

然后打开terminal的偏好设置Preferences->描述文件Profiles->Text->Change Font，在Family中更改字体为刚刚导入的那个字体，我选择的是 Meslo LG S DZ Regular for Powerline 字体

## Install homebrew failed with curl connection refused
Here's the work around way to install home brew
1.  open the home page of brew  [https://brew.sh/](https://brew.sh/)
2.  copy the URL from the install cmd and open it on your browser  [https://raw.githubusercontent.com/Homebrew/install/master/install.sh](https://raw.githubusercontent.com/Homebrew/install/master/install.sh)
3.  right-click and save it to your computer
4.  open a terminal and run it with: /bin/bash path-to/install.sh
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY3MDQ3NDU0NiwxNjgxNjUzNzM3LC0xNz
M1NzE3NzM0XX0=
-->