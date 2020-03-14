- 安装gitlab, apt-get install gitlab
- 安装Nginx 依赖 
下载 Nginx 源码wget link
源码安装 ./configure --help

apt-get install -yqq libpcre3-dev
apt-get install -yqq zlib1g-dev
apt-get install -yqq libssl-dev

make build
make install 

设计：用nginx把url请求路由给多个web服务。

比如[http://0.0.0.0/url1/](http://0.0.0.0/url1/) -> service1([127.0.0.1](127.0.0.1):3000/)

[http://0.0.0.0/url2/](http://0.0.0.0/url2/) -> service2([127.0.0.1](127.0.0.1):8000/)
问题1:
使用/
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4OTAwNjQyNDgsLTk4MjgzMjg0LDE1OD
g1MTAzMTZdfQ==
-->