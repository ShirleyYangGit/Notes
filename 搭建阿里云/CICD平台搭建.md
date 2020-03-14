- 安装gitlab, apt-get install gitlab
- 安装Nginx 依赖 
下载 Nginx 源码wget link
源码安装 ./configure --help

apt-get install -yqq libpcre3-dev
apt-get install -yqq zlib1g-dev
apt-get install -yqq libssl-dev

make build
make install 

设计：用nginx把url请求路由给多个web服务。比如[http://0.0.0.0/url1/] -> service1[http://127.0.0.1]
[http://0.0.0.0/url2/] -> service2[127.0.0.1:8000/]
当前使用location中的proxy_pass来实现
问题1:
使用/
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjc5ODU2OTk3LC05ODI4MzI4NCwxNTg4NT
EwMzE2XX0=
-->