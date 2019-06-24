# nginx

nginx 编译安装
nginx 信号量
nginx 日志切割
nginx 重定向
nginx 反向代理
nginx gzip
nginx 设置缓存
nginx 负载均衡
nginx 路径匹配
nginx 配置优化 https://blog.csdn.net/u010391029/article/details/56666943 https://www.cnblogs.com/wsjhk/p/8487311.html

./configure --help 查看帮助

查看 objs/ngx_modules.c 文件 里面是被编译的模块

goaccess 日志监控

accept_mutex on; #优化同一时刻只有一个请求而避免多个睡眠进程被唤醒的设置，on为防止被同时唤醒，默认为off，因此nginx刚安装完以后要进行适当的优化。

multi_accept on; #打开同时接受多个新网络连接请求的功能。

use epoll; #使用epoll事件驱动，因为epoll的性能相比其他事件驱动要好很多

server_tokens off; #在http 模块当中配置

gzip_proxied any; # default is "off" (no compression on proxied requests)

server_name_in_redirect off; 不知道作用是什么

max_fails=3 fail_timout=30s;   #如果在30s内连续三次产生3次失败，就认为这个服务器在之后的30s是无效(down)状态

