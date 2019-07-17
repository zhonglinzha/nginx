### nginx

* nginx 编译安装
* nginx 信号量
* nginx 日志切割
* nginx 重定向
* nginx 反向代理
* nginx gzip
* nginx 设置缓存
* nginx 负载均衡 限制访问量，漏桶算法
* nginx 路径匹配
* nginx https->http2  这里有很多优化的地方 暂时搞不清楚，以后再说
* nginx 配置优化
* nginx 惊群
[Nginx HTTPS 网站优化篇](https://blog.csdn.net/u010391029/article/details/56666943)
[Nginx 配置HTTPS性能优化方案一则](https://www.cnblogs.com/wsjhk/p/8487311.html)
[nginx 优化系列](https://blog.51cto.com/13673885/category1.html)

```powershell
./configure --help //查看帮助

objs/ngx_modules.c //这个文件里面是被编译的模块
```

* goaccess 日志监控 第三方工具
* accept_mutex on; #优化同一时刻只有一个请求而避免多个睡眠进程被唤醒的设置，on为防止被同时唤醒，默认为off，因此nginx刚安装完以后要进行适当的优化。
* server_name_in_redirect off; 不知道作用是什么


### 证书和私钥的生成

注意：一般生成的目录,应该放在nginx/conf/ssl目录
1. 创建服务器证书密钥文件 server.key 输入密码，确认密码，自己随便定义，但是要记住，后面会用到。
```powershell
openssl genrsa -des3 -out server.key 1024
```
2. 创建服务器证书的申请文件 server.csr
```powershell
openssl req -new -key server.key -out server.csr
```
输出内容为：
```powershell
Enter pass phrase for root.key: ← 输入前面创建的密码 
Country Name (2 letter code) [AU]:CN ← 国家代号，中国输入CN 
State or Province Name (full name) [Some-State]:BeiJing ← 省的全名，拼音 
Locality Name (eg, city) []:BeiJing ← 市的全名，拼音 
Organization Name (eg, company) [Internet Widgits Pty Ltd]:MyCompany Corp. ← 公司英文名 
Organizational Unit Name (eg, section) []: ← 可以不输入 
Common Name (eg, YOUR name) []: ← 此时不输入 
Email Address []:admin@mycompany.com ← 电子邮箱，可随意填
Please enter the following ‘extra’ attributes 
to be sent with your certificate request 
A challenge password []: ← 可以不输入 
An optional company name []: ← 可以不输入
```
3. 备份一份服务器密钥文件
```powershell
cp server.key server.key.org
```
4. 去除文件口令
```powershell
openssl rsa -in server.key.org -out server.key
```
5. 生成证书文件server.crt
```powershell
openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
```

