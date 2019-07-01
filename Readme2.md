Nginx 是一个高性能的HTTP和反向代理服务器。文本详细记录了在centos7系统上安装的步骤。

安装依赖
yum上面没有nginx的安装路径，可以使用切换yum的源进行安装，也可以下载源码进行编译安装，本文采用下载源码安装。

注：安装用户均为root

1、由于安装编译源码，需要gcc环境没有的话请执行以下代码，如果有的话请从第2步开始安装

yum -y install gcc-c++

2、安装pcre pcre-devel 执行，出现complete字段表示安装成功

yum -y install pcre pcre-devel

3、安装zlib zlib-devel 执行以下命令 静候片刻

yum -y install zlib zlib-devel

4、安装openssl openssl-devel 执行后 静候安装成功

yum -y install openssl openssl-devel

以上步骤完成后，接下来就可以安装nginx了

安装nginx
从官网安装.tar.gz包，推荐使用wget命令进行下载，如果还没有安装wget，执行yum -y install wget进行安装，本文安装的是1.8版本，也可选择其他版本。

1、下载源码，执行

wget -c http://nginx.org/download/nginx-1.8.0.tar.gz

2、下载完成后，执行解压命令

tar -zxvf nginx-1.8.0.tar.gz

3、配置nginx
执行cd nginx-1.8.0 进入解压后的nginx文件 执行

./configure –prefix=/usr/local/nginx
这里将nginx配置文件放在/usr/local/nginx路径下，也可以使用默认配置，
使用默认配置执行./configure即可

4、编译安装 执行

make
make install
5、查看nginx所在位置，本文将配置指向了/usr/local/nginx目录