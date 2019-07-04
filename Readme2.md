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

make -j4
make install
5、查看nginx所在位置，本文将配置指向了/usr/local/nginx目录


./configure \
--prefix=/Users/ex-zhazhonglin001/Desktop/ngs \
--with-openssl=/Users/ex-zhazhonglin001/Desktop/nginx-1.16.0/openssl-1.0.2s \
--with-http_ssl_module \
--with-http_stub_status_module \
--with-http_v2_module \
--with-http_gzip_static_module

make -j4

make install


ld: symbol(s) not found for architecture x86_64

今天在mac os 上编译安装Nginx时候，报错：ld: symbol(s) not found for architecture x86_64， 经过一番折腾之后发现，由于Nginx依赖openssl库，查看openssl的./config 文件发现，这个问题应该是 openssl/config脚本猜对你的系统是64位，但是 会根据$KERNEL_BITS来判断是否开启x86_64编译，默认不开启，他会给你5秒时间确认是否停止编译，手动设置x86_64编译，所以默认你生成的openssl库文件是32位的，最后静态链接到nginx会出错。目前看来没有很好的方法把x86_64的参数传到openssl配置文件中 (openssl/config 猜测os架构，设置编译的参数是32位还是64位，默认是32位，然后调用openssl/Configure生成Makefile)，

解决办法就是：

先运行nginx源码目录下运行
$ ./configure

然后在objs里，打开Makefile，

找到： ./config --prefix=xxx.openssl no-shared        (注释：XXX是已存在的openssl源码路径)

把该段的 ./config 改成 ./Configure darwin64-x86_64-cc 其他后面参数不变，保存

然后再make就编译通过了



ps aux | grep nginx



https://www.cnblogs.com/whych/p/9521597.html
平滑升级  kill –USR2 旧版本的nginx主进程号
         kill -QUIT旧版本的nginx主进程号