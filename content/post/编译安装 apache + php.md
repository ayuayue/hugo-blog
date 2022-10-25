---
cssclass:
title: 编译安装 apache + php
tags: [IT/Apache, IT/PHP, IT/Docker, IT/Centos, IT/Linux]
image-auto-upload: true

date: 2022-10-03 20:32:46
lastmod: 2022-10-25 00:59:51
---
# 编译安装 apache + php
使用 apache + php 来作为服务器解析 php 脚本的话,需要 `libph5` 的一个库文件, 这个库文件需要编译安装 php 的时候 `--with-apxs2=/usr/local/apache2/bin/apxs` 将会在 apache 的 modules 目录中生成 libphp 的库文件并在 httpd.conf 文件中开启配置

### 安装前

编译安装 apache 需要 先 安装 `APR APR_Util PCRE GCC GCC-C++` 等包.

```bash
yum install expat-devel -y
```

#### 编译安装 APR

```bash
wget http://archive.apache.org/dist/apr/apr-1.5.2.tar.gz
tar -zxvf  apr-1.5.2.tar.gz
cd apr-1.5.2/
./configure --prefix=/usr/local/apr
make && make install

```

#### 编译安装 APR-Util

```bash
wget http://archive.apache.org/dist/apr/apr-util-1.5.4.tar.gz
tar -zxvf apr-util-1.5.4.tar.gz
cd apr-util-1.5.4/

./configure --prefix=/usr/local/apr-util \
-with-apr=/usr/local/apr/bin/apr-1-config

make && make install

```

#### 编译安装 pcre-conf

```bash
wget https://sourceforge.net/projects/pcre/files/pcre/8.39/pcre-8.39.tar.gz
tar -zxvf pcre-8.39.tar.gz
cd pcre-8.39
./configure --prefix=/usr/local/pcre
make && make install

```

### 进入正题,编译 apache

```bash
wget http://archive.apache.org/dist/httpd/httpd-2.4.23.tar.gz
tar -zxvf httpd-2.4.23.tar.gz
cd httpd-2.4.23

./configure --prefix=/usr/local/apache2 \
--with-apr=/usr/local/apr \
--with-apr-util=/usr/local/apr-util/ \
--with-pcre=/usr/local/pcre

```

#### 配置环境变量,及软连接,适应多版本 apache

1.  修改 .bashrc 加入

```Bash
export APACHE="/usr/local/apache"
export PHP="/usr/local/php"
export PATH="${HOME}/bin:${HOME}/.g/go/bin:$PATH:$APACHE/bin:$PHP/bin"
```

可以看到 编译时配置的路径是 apache2 ,而环境变量是 apache, php

所以我们在环境中保留一个版本的软件,名称并不带版本号.

2.  创建软连接

```Bash
ln -s /usr/local/apache2 /usr/local/apache
```

这样,系统的apache环境就变成了 2, 如果编译安装了 1 版本的,那么用 1 的生成软连接即可.

同理, PHP及其他软件相同

3.  刷新 `source ~/.bashrc`
    
4.  检查 `apchectl httpd` 命令 的 -v 参数如果输出了版本信息那么就 OK了 😎
    

#### 备份并修改配置文件

备份

```bash
cp /usr/local/apache2/conf/httpd.conf /usr/local/apache2/conf/httpd.conf.bak
```

修改

```bash
vim /usr/local/apache2/conf/httpd.conf

#ServerName www.example.com:80
ServerName 0.0.0.0:80

```

#### 启动暂停重启

```bash
apacheclt stop | start | restart | status

httpd
```

#### 检测配置文件是否正确

```bash
httpd -t
apachectl -t
```

启动后访问虚拟机的 80 端口返回 it works

### 编译安装 php

至于下载,可以到官网下载 常用的版本 `5.6 7.0 7.4` [http://www.php.net/downloads.php](http://www.php.net/downloads.php)

```Bash
cd php-5.6.40/
yum install libpng libpng-devel libxml2 \
libxml2-devel freetype freetype-devel \
libmcrypt libmcrypt-devel -y

./configure --prefix=/usr/local/php56 \
                                --with-apxs2=/usr/local/apache2/bin/apxs \
                                --with-mysql=mysqlnd \
                                --with-gd \
                                --without-pear \
                                --disable-phar \
                                --with-config-file-path=/usr/local/php56/etc
 make && make install
 ./configure --prefix=/usr/local/php70 \
                                --with-apxs2=/usr/local/apache2/bin/apxs \
                                --with-mysql=mysqlnd \
                                --with-gd \
                                --without-pear \
                                --disable-phar \
                                --with-config-file-path=/usr/local/php70/etc
 ./configure --prefix=/usr/local/php74 \
                                --with-apxs2=/usr/local/apache2/bin/apxs \
                                --with-mysql=mysqlnd \
                                --with-gd \
                                --without-pear \
                                --disable-phar \
                                --with-config-file-path=/usr/local/php74/etc

```

#### 报错 configure: error: xml2-config not found. Please check your libxml2 installation.

系统中未安装 libxml 库

```bash
yum install libxml2 libxml2-devel -y


```

#### 报错 configure: error: png.h not found.

```bash
yum install libpng libpng-devel

```

直到提示 php 信息 后可以使用 `make && make install`

![](https://cdn.jsdelivr.net/gh/ayuayue/cdn/img/202109121245345.png)

#### 重新编译

如果要重新编译, 在更改了 .configure 命令或者配置的情况下,最好清空一下上次编译好的文件

```Bash
make clean # 清空编译产生的文件

make distclean #清空编译产生的文件及生成的 configure 文件
```

安装完成后,会在 /usr/local/apache2/modules/ 生成一个 libphp5 的文件,并且会自动更改 http.conf 增加

`LoadModule php5_module modules/libphp5.so`

#### 配置环境变量及软链接

同上 apache的配置

使用 php -v 检测安装结果

### 配置 apache 解析 php

首先 新建一个 php 脚本用来测试

```php
# vim /usr/local/apache2/htdocs/index.php
<?php
echo phpinfo();

```

```bash
# vim /usr/local/apache2/conf/http.conf

<IfModule dir_module>
    DirectoryIndex index.html index.htm index.php
</IfModule>

AddType application/x-compress .Z
AddType application/x-gzip .gz .tgz

AddType application/x-httpd-php .php
AddType application/x-httpd-php-source .php5

<Directory />
    Options FollowSymLinks
    AllowOverride none
    #Require all denied
    Require all granted
    Satisfy all
</Directory>



```

### 配置虚拟主机

修改 httpd.conf 文件,引入 虚拟主机文件

```conf
# Virtual hosts
Include conf/extra/httpd-vhosts.conf
```

然后更改 httpd-vhosts.conf 文件, 在 httpd.conf 文件中配置的 默认站点配置会失效

```bash
# vim conf/extra/httpd-vhosts.conf
<VirtualHost *:80>
    #ServerAdmin webmaster@dummy-host.example.com
    DocumentRoot "/mnt/hgfs/thinks"
    ServerName dev.com
    <Directory /mnt/hgfs/thinks/>
        AllowOverride All
        Options MultiViews Indexes Includes FollowSymLinks
        Require all granted
    </Directory>
    #ServerAlias www.dummy-host.example.com
    ErrorLog "logs/hugo.com.error_log"
    CustomLog "logs/hugo.com.access_log" common
</VirtualHost>

<VirtualHost *:80>
    #ServerAdmin webmaster@vm.com
    DocumentRoot "/usr/local/apache2/htdocs/"
    ServerName vm.com
    <Directory /mnt/hgfs/thinks/>
        AllowOverride All
        Options MultiViews Indexes Includes FollowSymLinks
        Require all granted
    </Directory>
    ErrorLog "logs/vm.com-error_log"
    CustomLog "logs/vm.com-access_log" common
</VirtualHost>

```

这里添加了两个站点.相应的在 Windows 下也要配置 hosts 文件映射这两个站点

```bash
# vim /c/Windows//System32/drivers/etc/hosts

192.168.190.128 vm.com hugo.com
```

### 注意事项

1.  如果使用浏览器跟命令行都打不开,可以清除下 Windows 的 dns ,使用 cmd

```bash
ipconfig /flushdns
```

2.  如果只是 浏览器打不开
    1.  可能是缓存,清下浏览器缓存
    2.  可能是浏览器默认打开为 https ,但是并没有配置 https,换成 http打开即可

### PHP 7

如果是配置 php7 及以上的php跟apache,流程一样,编译php时同样要带上 —with-apxs2 会生成 [libphp7.so](http://libphp7.so) 文件到 apache的modules 里.其他都是一样

#### 问题记录

#### No package 'sqlite3' found

```bash
yum install sqlite-devel
```

### 站点配置不同版本的php

#### 安装 mod_fcgid 模块

```SQL
wget http://www.apache.org/dist/httpd/mod_fcgid/mod_fcgid-2.3.9.tar.gz
tar -zxvf mod_fcgid-2.3.9.tar.gz
APXS=/usr/local/apache2/bin/apxs ./configure.apxs
make && make install

```

编译安装完会自动加入到 http.conf配置中

```SQL
#添加映射
AddHandler fcgid-script .fcgi .php
# 设置PHP_FCGI_MAX_REQUESTS大于或等于FcgidMaxRequestsPerProcess，防止php-cgi进程在处理完所有请求前退出
FcgidInitialEnv PHP_FCGI_MAX_REQUESTS 1000
#php-cgi每个进程的最大请求数
FcgidMaxRequestsPerProcess 1000
#php-cgi最大的进程数
FcgidMaxProcesses 3
#最大执行时间
FcgidIOTimeout 120
FcgidIdleTimeout 120
#限制最大请求字节 (单位b)
FcgidMaxRequestLen 2097152

AddType application/x-httpd-php .php
#------这里是默认虚拟主机配置
#php.ini的存放目录
FcgidInitialEnv PHPRC "/usr/local/php56/"
#php-cgi的路径
FcgidWrapper "/usr/local/php56/bin/php-cgi" .php
```

,修改 extra/http-vhost.conf 文件

```SQL
<VirtualHost *:80>
  DocumentRoot "/www/"
  ServerName "php70"
  DirectoryIndex index.html index.php
  #php.ini的存放目录,Linux下通常不需要
  #FcgidInitialEnv PHPRC "D:/php"
  # 设置PHP_FCGI_MAX_REQUESTS大于或等于FcgidMaxRequestsPerProcess，防止php-cgi进程在处理完所有请求前退出
  FcgidInitialEnv PHP_FCGI_MAX_REQUESTS 1000
  #php-cgi每个进程的最大请求数
  FcgidMaxRequestsPerProcess 1000
  #php-cgi最大的进程数
  FcgidMaxProcesses 3
  #最大执行时间
  FcgidIOTimeout 600
  FcgidIdleTimeout 600
  #php-cgi的路径
  FcgidWrapper /usr/local/php70/bin/php-cgi .php
  AddHandler fcgid-script .php
  FcgidInitialEnv PHP_FCGI_MAX_REQUESTS 1000
  <Directory "/www/">
    Options +ExecCGI
  </Directory>
</VirtualHost>
  <IfModule mod_fcgid.c>
      FcgidInitialEnv PHPRC "/usr/local/php70/" 
      AddHandler fcgid-script .fcgi .php
      FcgidMaxRequestsPerProcess 1000
      FcgidMaxProcesses 5
      FcgidIOTimeout 120
      FcgidIdleTimeout 120
      FcgidWrapper /usr/local/php70/bin/php-cgi .php
      
     AddType application/x-httpd-php .php
     AddType application/x-httpd-php .phtml
  </IfModule>

```

### 站点配置 https