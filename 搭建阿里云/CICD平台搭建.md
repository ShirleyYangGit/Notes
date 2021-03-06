
# 安装Gitlab
- 安装gitlab, apt-get install gitlab
# 安装Nginx
- 下载 Nginx 源码wget link
源码安装Nginx 依赖 
apt-get install -yqq libpcre3-dev
apt-get install -yqq zlib1g-dev
apt-get install -yqq libssl-dev
源码安装 ./configure --help
make build
make install 

设计：用nginx把url请求路由给多个web服务。比如[http://x.x.x.x/gitlab/] -> gitlab_service[http://127.0.0.1]
[http://x.x.x.x/url2/] -> service2[127.0.0.1:8000/]
当前使用location中的proxy_pass来实现，配置如下：
```
server {
    listen       80;
    server_name  47.101.136.57;
    location /gitlab/ {
        proxy_pass http://127.0.0.1:8000/;
        proxy_redirect http://$host/ http://$host:$server_port/gitlab/;
        proxy_set_header HOST $host;
    }
    # error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }
}
```
__问题__：从gitlab获取的html文件中，还需要加载一些静态文件，html中的配置`css <link stylesheet="text/css" href="/assets/abc.css">`
获取静态文件的url自动会更新成[http://x.x.x.x/assets/abc.css]，从而导致获取失败。我们期望是[http://x.x.x.x/gitlab/assets/abc.css]，这样Nginx就能正确转给gitlab。但是css的访问路径是由html自动发起的，路径拼接规则是定义在html中的，所以无法加上我们期望的`/gitlab`字符。
__解决方案__：
* 设置`/assets/`的重定向，将其重定向到`/gitlab/assets/`中去。缺点是：有可能第二个service中也用到`/assets/`，那就没办法处理了
* 使用域名，这个需要先购买域名。然后可以为每个service配置子域名。
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDQzMTAyODY2LC05ODI4MzI4NCwxNTg4NT
EwMzE2XX0=
-->