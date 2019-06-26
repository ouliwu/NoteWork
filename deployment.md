##网站的一些知识

- 域名: 方便人们记忆，因为不是一般人都可以记住ip
- 空间: 早期的网站，人们一般不会购买主机，而是使用别人主机上的一个虚拟空间，可以理解为一个文件夹
- 主机: 实际上就是一台远程的电脑，只是不是物理电脑
- DNS: domain name system, 用于绑定域名和ip



## 一些集成环境了解

- LNMP: Linux Nginx MySql PHP
- WAMP: Windows Apatch MySql PHP
- LAMP: Linux Apatch MySql PHP
- MAMP: Mac Apatch MySql PHP
- XAMPP: Apache+MySQL+PHP+PERL

## 后端常见语言 

- .net
- Java web
- php
- ruby
- python
- go
- ……

## 基于node的部署(deployment)

1. 安装nodejs, 由于yum的资源比较老，要安装更新的nodejs, 请看末尾

   ```bash
   $ yum install nodejs
   ```

2. 为了保证node应用不被中止，需要安装 pm2.

   ```bash
   $ npm install pm2 -g
   ```

3. 随便使用任何node服务器即可，比如http-server:

   ```bash
   $ npm install http-server -g
   ```

4. 根据pm2的文档操作

## centOS nginx部署

1. 安装nginx
```
  $ yum install nginx -y
```
2. 装好之后，在 /etc/nginx 目录下，看到有一堆文件， 其中 nginx.conf 这个文件就是默认的nginx 配置文件， 下面有一个目录叫conf.d的目录用于存储我们自己写的配置。
3. 如果报nginx: [error] open() "/run/nginx.pid" failed (2: No such file or directory)类似的错误，那就先执行:
```
  $ nginx -t 
```
将得到如下的输出
```
  nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
  nginx: configuration file /etc/nginx/nginx.conf test is successful
```
这时执行
```
  $ nginx -c /etc/nginx/nginx.conf
  $ nginx -s reload
```
如果nginx配置正确，直接访问咱们的公网ip地址，应该能得到的是welcome to nginx

4. 进入到 /etc/nginx/conf.d 新建一个 **任意名字.conf** 在里面写入server的配置

如果单页应用没有使用hash router，那么就会刷新之后页面无法访问

只需要在server配置里加上
```
location / {
  try_files $uri $uri/ /index.html;
}
```

代理设置
```
location /api/ {
  proxy_pass ip:port
}
```
server配置看起来像这样
```
server {
	listen 8000;
	root /var/www/yourfolder/build/;
	location / {
		try_files $uri $uri/ /index.html;
	}
	location /api/ {
		proxy_pass http://serverip:4444;
	}
}
```

比较全的location配置

```
location / {
        proxy_pass  http://apachephp;
 
        #Proxy Settings
        proxy_redirect     off;
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        proxy_max_temp_file_size 0;
        proxy_connect_timeout      90;
        proxy_send_timeout         90;
        proxy_read_timeout         90;
        proxy_buffer_size          4k;
        proxy_buffers              4 32k;
        proxy_busy_buffers_size    64k;
        proxy_temp_file_write_size 64k;
   }
```



## 解决nginx不能`npm run build`的问题

```bash
$ sudo /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
$ sudo /sbin/mkswap /var/swap.1
$ sudo /sbin/swapon /var/swap.1
```

## 安装更高版本的node

```bash
$ curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -
$ yum install nodejs
```

## 