---
title: 如何优雅的在centos上使用nginx
tags:
  - centos
  - nginx
categories:
  - 前端
abbrlink: 27f7e408
date: 2022-01-12 01:08:19
---
 ## nginx基本配置
 ### 安装nginx
用yum进行安装必要程序
```sh
yum -y install gcc gcc-c++ autoconf pcre-devel make automake
yum -y install wget httpd-tools vim
```
编辑安装环境
```sh
vim /etc/yum.repos.d/nginx.repo
```
复制下面的代码
```sh
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
```
配置完成，然后执行安装
```sh
yum install nginx
```
查看nginx版本
```sh
nginx -v
```
 ### 查看nginx安装目录
```sh
rpm -ql nginx
```
<!--more-->
rpm 是linux的rpm包管理工具，-q 代表询问模式，-l 代表返回列表，这样我们就可以找到nginx的所有安装位置了。
### nginx.conf文件解读
nginx.conf 文件是Nginx总配置文件，在我们搭建服务器时经常调整的文件。
进入etc/nginx目录下，然后用vim进行打开
```sh
cd /etc/nginx
vim nginx.conf
```
下面是文件的详细注释
```sh
#运行用户，默认即是nginx，可以不进行设置
user  nginx;
#Nginx进程，一般设置为和CPU核数一样
worker_processes  1;   
#错误日志存放目录
error_log  /var/log/nginx/error.log warn;
#进程pid存放位置
pid        /var/run/nginx.pid;

events {
    worker_connections  1024; # 单个后台进程的最大并发数
}

http {
    include       /etc/nginx/mime.types;   #文件扩展名与类型映射表
    default_type  application/octet-stream;  #默认文件类型
    #设置日志模式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;   #nginx访问日志存放位置

    sendfile        on;   #开启高效传输模式
    #tcp_nopush     on;    #减少网络报文段的数量

    keepalive_timeout  65;  #保持连接的时间，也叫超时时间

    #gzip  on;  #开启gzip压缩

    include /etc/nginx/conf.d/*.conf; #包含的子配置项位置和文件
```
### default.conf 配置
进入conf.d目录，然后使用`vim default.conf`进行查看。
```sh
server {
    listen       80;   #配置监听端口
    server_name  localhost;  //配置域名

    #charset koi8-r;     
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;     #服务默认启动目录
        index  index.html index.htm;    #默认访问文件
    }

    #error_page  404              /404.html;   # 配置404页面

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;   #错误状态码的显示页面，配置后需要重启
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```
明白了这些配置项，我们知道我们的服务目录放在了/usr/share/nginx/html下，可以使用命令进入看一下目录下的文件。
```sh
cd /usr/share/nginx/html
ls
```
可以看到目录下面有两个文件，50x.html 和 index.html。我们可以使用vim进行编辑。
### 安全组开启
阿里云的安全组配置
步骤如下：
- 进入阿里云控制台，并找到ECS实例。
- 点击实例后边的“更多”
- 点击“网络和安全组” ，再点击“安全组配置”
- 右上角添加“安全组配置”
- 进行80端口的设置。
## nginx服务命令
### 启动nginx服务
#### nginx直接启动
在CentOS7.4版本里（低版本是不行的），是可以直接直接使用nginx启动服务的。
```sh
nginx
```
#### 使用systemctl命令启动
还可以使用个Linux的命令进行启动，我一般都是采用这种方法进行使用。因为这种方法无论启动什么服务，都是一样的，只是换一下服务的名字（不用增加额外的记忆点）。
```sh
systemctl start nginx.service
```
输入命令后，没有任何提示，那我们如何知道Nginx服务已经启动了哪？可以使用Linux的组合命令，进行查询服务的运行状况。
```sh
ps aux | grep nginx
```
如果有三条记录，说明Nginx被正常开启了。
### 停止nginx服务的四种方法
停止Nginx 方法有很多种，可以根据需求采用不一样的方法，一个一个说明。
#### 立即停止服务
```sh
nginx -s stop
```
这种方法比较强硬，无论进程是否在工作，都直接停止进程。
#### 从容停止服务
```sh
nginx -s quit
```
这种方法较stop相比就比较温和一些了，需要进程完成当前工作后再停止。
#### killall方法杀死进程
```sh
killall nginx
```
这种方法也是比较野蛮的，我们直接杀死进程，但是在上面使用没有效果时，我们用这种方法还是比较好的。
#### systemctl停止服务
```sh
systemctl stop nginx.service
```
### 重启nginx服务
有时候要重启nginx服务，可以使用下面的命令
```sh
systemctl restart nginx.service
```
### 重新载入配置文件
在重新编写或者修改Nginx的配置文件后，都需要作一下重新载入，这时候可以用Nginx给的命令。
```sh
nginx -s reload
```
### 查看端口号
在默认情况下，Nginx启动后会监听80端口，从而提供HTTP访问，如果80端口已经被占用则会启动失败。我么可以使用netstat -tlnp命令查看端口号的占用情况。
## 自定义错误页和访问设置
一个好的网站会武装到牙齿，任何错误都有给用户友好的提示。比如当网站遇到页面没有找到的时候，我们要提示页面没有找到，并给用户可返回性。错误的种类有很多，所以真正的好产品会给顾客不同的返回结果。
### 多错误指向一个页面
在/etc/nginx/conf.d/default.conf 是可以看到下面这句话的。
```sh
error_page   500 502 503 504  /50x.html;
```
error_page指令用于自定义错误页面，500，502，503，504 这些就是HTTP中最常见的错误代码，/50.html 用于表示当发生上述指定的任意一个错误的时候，都是用网站根目录下的/50.html文件进行处理。
### 单独为错误置顶处理方法
有些时候是要把这些错误页面单独的表现出来，给用户更好的体验。所以就要为每个错误码设置不同的页面。设置方法如下：
```sh
error_page 404  /404_error.html;
```
然后到网站目录下新建一个404_error.html 文件，并写入一些信息。
```html
<html>
<meta charset="UTF-8">
<body>
<h1>404页面没有找到!</h1>
</body>
</html>
```
然后重启我们的服务，再进行访问，你会发现404页面发生了变化。
### 错误码换成一个地址
处理错误的时候，不仅可以只使用本服务器的资源，还可以使用外部的资源。比如我们将配置文件设置成这样。
```sh
error_page  404 https://i100.xyz;
```
### 简单服务控制
有时候我们的服务器只允许特定主机访问，比如内部OA系统，或者应用的管理后台系统，更或者是某些应用接口，这时候我们就需要控制一些IP访问，我们可以直接在location里进行配置。
```sh
 location / {
        deny   123.9.51.42;
        allow  45.76.202.231;
    }
```
配置完成后，重启一下服务器就可以实现限制和允许访问了。
## nginx访问权限
简单接触了Nginx访问简单用法，简单的知道了，deny是禁止访问，allow是允许访问。但Nginx的访问控制还是比较复杂的。
### 指令优先级
```sh
 location / {
        allow  45.76.202.231;
        deny   all;
    }
```
上面的配置表示只允许45.76.202.231进行访问，其他的IP是禁止访问的。但是如果我们把deny all指令，移动到 allow 45.76.202.231之前，会发生什么那？会发现所有的IP都不允许访问了。这说明了一个问题：就是在同一个块下的两个权限指令，先出现的设置会覆盖后出现的设置（也就是谁先触发，谁起作用）。
### 复杂访问控制权限匹配
在工作中，访问权限的控制需求更加复杂，例如，对于网站下的img（图片目录）是运行所有用户访问，但对于网站下的admin目录则只允许公司内部固定IP访问。这时候仅靠deny和allow这两个指令，是无法实现的。我们需要location块来完成相关的需求匹配。
```sh
    location =/img{
        allow all;
    }
    location =/admin{
        deny all;
    }
```
=号代表精确匹配，使用了=后是根据其后的模式进行精确匹配。这个直接关系到我们网站的安全，一定要学会。
### 使用正则表达式设置访问权限
只有精确匹配有时是完不成我们的工作任务的，比如现在我们要禁止访问所有php的页面，php的页面大多是后台的管理或者接口代码，所以为了安全我们经常要禁止所有用户访问，而只开放公司内部访问的。
```sh
 location ~\.php$ {
        deny all;
    }
```
这样我们再访问的时候就不能访问以php结尾的文件了。是不是让网站变的安全很多了那？
## nginx反向代理设置
现在的web模式基本的都是标准的CS结构，即Client端到Server端。那代理就是在Client端和Server端之间增加一个提供特定功能的服务器，这个服务器就是我们说的代理服务器。
### 正向代理
如果你觉的反向代理不好理解，那先来了解一下正向代理。我相信作为一个手速远超正常人的程序员来说，你一定用过翻墙工具（我这里说的不是物理梯子），它就是一个典型的正向代理工具。它会把我们不让访问的服务器的网页请求，代理到一个可以访问该网站的代理服务器上来，一般叫做proxy服务器，再转发给客户。
简单来说就是你想访问目标服务器的权限，但是没有权限。这时候代理服务器有权限访问服务器，并且你有访问代理服务器的权限，这时候你就可以通过访问代理服务器，代理服务器访问真实服务器，把内容给你呈现出来。
### 反向代理
反向代理跟代理正好相反（需要说明的是，现在基本所有的大型网站的页面都是用了反向代理），客户端发送的请求，想要访问server服务器上的内容。发送的内容被发送到代理服务器上，这个代理服务器再把请求发送到自己设置好的内部服务器上，而用户真实想获得的内容就在这些设置好的服务器上。
proxy服务器代理的并不是客户端，而是服务器,即向外部客户端提供了一个统一的代理入口，客户端的请求都要先经过这个proxy服务器。具体访问那个服务器server是由Nginx来控制的。再简单点来讲，一般代理指代理的客户端，反向代理是代理的服务器
### 反向代理的用途和好处
- 安全性：正向代理的客户端能够在隐藏自身信息的同时访问任意网站，这个给网络安全代理了极大的威胁。因此，我们必须把服务器保护起来，使用反向代理客户端用户只能通过外来网来访问代理服务器，并且用户并不知道自己访问的真实服务器是那一台，可以很好的提供安全保护。
- 功能性：反向代理的主要用途是为多个服务器提供负债均衡、缓存等功能。负载均衡就是一个网站的内容被部署在若干服务器上，可以把这些机子看成一个集群，那Nginx可以将接收到的客户端请求“均匀地”分配到这个集群中所有的服务器上，从而实现服务器压力的平均分配，也叫负载均衡。
### 最简单的反向代理
现在我们要访问http://nginx2.i100.com然后反向代理到i00.com这个网站。我们直接到etc/nginx/con.d/8001.conf进行修改。
```sh
server{
        listen 80;
        server_name nginx2.i100.com;
        location / {
               proxy_pass http://i100.com;
        }
}
```
一般我们反向代理的都是一个IP，但是我这里代理了一个域名也是可以的。其实这时候我们反向代理就算成功了
### 其它反向代理指令
proxy_set_header :在将客户端请求发送给后端服务器之前，更改来自客户端的请求头信息。
proxy_connect_timeout:配置Nginx与后端代理服务器尝试建立连接的超时时间。
proxy_read_timeout : 配置Nginx向后端服务器组发出read请求后，等待相应的超时时间。
proxy_send_timeout：配置Nginx向后端服务器组发出write请求后，等待相应的超时时间。
proxy_redirect :用于修改后端服务器返回的响应头中的Location和Refresh。
## nginx适配PC或移动设备
现在很多网站都是有了PC端和H5站点的，因为这样就可以根据客户设备的不同，显示出体验更好的，不同的页面了。
这样的需求有人说拿自适应就可以搞定，比如我们常说的bootstrap和24格布局法，这些确实是非常好的方案，但是无论是复杂性和易用性上面还是不如分开编写的好，比如我们常见的淘宝、京东......这些大型网站就都没有采用自适应，而是用分开制作的方式。
### $http_user_agent的使用
Nginx通过内置变量$http_user_agent，可以获取到请求客户端的userAgent，就可以用户目前处于移动端还是PC端，进而展示不同的页面给用户。
操作步骤如下：
- 1.在/usr/share/nginx/目录下新建两个文件夹，分别为：pc和mobile目录
```sh
cd /usr/share/nginx
mkdir pc
mkdir mobile
```
- 在pc和miblic目录下，新建两个index.html文件，文件里下面内容
```sh
<h1>I am pc!</h1>
```
```sh
<h1>I am mobile!</h1>
```
- 进入etc/nginx/conf.d目录下，修改8001.conf文件，改为下面的形式:
```sh
server{
     listen 80;
     server_name nginx2.jspang.com;
     location / {
      root /usr/share/nginx/pc;
      if ($http_user_agent ~* '(Android|webOS|iPhone|iPod|BlackBerry)') {
         root /usr/share/nginx/mobile;
      }
      index index.html;
     }
}
```
## nginx的Gzip压缩配置
Gzip是网页的一种网页压缩技术，经过gzip压缩后，页面大小可以变为原来的30%甚至更小。更小的网页会让用户浏览的体验更好，速度更快。gzip网页压缩的实现需要浏览器和服务器的支持。
gzip是需要服务器和浏览器同事支持的。当浏览器支持gzip压缩时，会在请求消息中包含Accept-Encoding:gzip,这样Nginx就会向浏览器发送听过gzip后的内容，同时在相应信息头中加入Content-Encoding:gzip，声明这是gzip后的内容，告知浏览器要先解压后才能解析输出。
### gzip的配置项
Nginx提供了专门的gzip模块，并且模块中的指令非常丰富。
- gzip : 该指令用于开启或 关闭gzip模块。
- gzip_buffers : 设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流。
- gzip_comp_level : gzip压缩比，压缩级别是1-9，1的压缩级别最低，9的压缩级别最高。压缩级别越高压缩率越大，压缩时间越长。
- gzip_disable : 可以通过该指令对一些特定的User-Agent不使用压缩功能。
- gzip_min_length:设置允许压缩的页面最小字节数，页面字节数从相应消息头的Content-length中进行获取。
- gzip_http_version：识别HTTP协议版本，其值可以是1.1.或1.0.
- gzip_proxied : 用于设置启用或禁用从代理服务器上收到相应内容gzip压缩。
- gzip_vary : 用于在响应消息头中添加Vary：Accept-Encoding,使代理服务器根据请求头中的Accept-Encoding识别是否启用gzip压缩。
### Gzip最简单配置
```sh
http {
   .....
    gzip on;
    gzip_types text/plain application/javascript text/css;
   .....
}
```
gzip on是启用gizp模块，下面的一行是用于在客户端访问网页时，对文本、JavaScript 和CSS文件进行压缩输出。
配置好后，我们就可以重启Nginx服务，让我们的gizp生效了。
如果你是windows操作系统，你可以按F12键打开开发者工具，单机当前的请求，在标签中选择Headers，查看HTTP响应头信息。你可以清楚的看见Content-Encoding为gzip类型。
