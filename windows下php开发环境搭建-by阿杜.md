# windows下php开发环境搭建 #

说在前面的话
> windows下开发环境 如果熟悉php apache的运行原理之后，可以独立安装 当然也可以用[phpstudy](http://www.phpstudy.net/)，[xampp](https://www.apachefriends.org/zh_cn/index.html), [wamp](http://www.wampserver.com/),[upupw](http://www.upupw.net/)，[appserv](http://www.appservnetwork.com/en/)等集成环境进行搭建 
> 但是今天要写的是

### 1准备 ###
    搭建php开发环境。主要有需要安装apache，php，和mysql 这三个经典组合 他们的下载地址分别是
> php： http://httpd.apache.org/docs/current/platform/windows.html#down

> apache http://httpd.apache.org/docs/current/platform/windows.html#down

> http://dev.mysql.com/downloads/mysql/5.0.html#win32

    一般安装的顺序是：Apache-->PHP-->MySQL

### 2.下载确认版本 ###

今天安装的版本分别是
	
	 php 64位 安全线程 版本 http://windows.php.net/downloads/releases/php-5.6.25-Win32-VC11-x64.zip

	apache Apache 2.4.23 x64  http://www.apachehaus.com/downloads/httpd-2.4.23-x64-vc14.zip

	mysql5.6.33 x64 http://cdn.mysql.com//Downloads/MySQL-5.6/mysql-5.6.33-winx64.zip 

### 3.安装 ###
#### 3.1安装apache ####

	在D盘（或者C盘 本文以D盘为例） 创建文件夹wamp 将下载好的httpd-2.4.23-x64-vc14.zip 解压 你会得到一个Apache24 将此文件夹复制到 wamp文件夹下
> win+x打开命令提示符（管理员），定位到D:\wamp\Apache24\bin文件目录下，输入命令：httpd -k install

如果出现错误的话 找到 D:\wamp\Apache24\conf下的httpd.conf，（38行左右）修改如下内容，让serverroot指向你的安装位置：

	Define SRVROOT "D:/wamp/Apache24" 
	ServerRoot "${SRVROOT}"

然后在执行 `httpd -k uninstall `卸载服务，并再次执行安装命令 `httpd -k install`，出现如图的提示表示安装成功，启动Apache：`httpd -k -start`
 
	然后可以在浏览器中输入http://localhost来测试时候成功，如果不成功，

	说明本地80端口被占用，你可以到 httpd.conf中将所有80的端口改成8080，再次输入，如果出现如下图提示，表示安装成功。

	

![](https://sfault-image.b0.upaiyun.com/163/083/1630839188-56fdf93a2c54b_articlex)

#### 3.2安装php ####

解压下载的php-5.6.25-Win32-VC11-x64.zip 重命名为php56 并将他移动到  D:\wamp\下

> 将 D:\wamp\php56\ 将php.ini-production文件重命名为php.ini，  并修改php.ini文件 （732行左右）

	; extension_dir = "./"
修改为

	extension_dir = "D:/wamp/php56/ext"	

其他的修改  (主要是部分扩展需要打开。把前面的;号去掉)

下面主要是支持数据库之类的扩展

![](https://sfault-image.b0.upaiyun.com/322/562/3225623586-56fdf94b3728c_articlex)


#### 3.3配置php和apache ####

>  修改D:\wamp\Apache24\conf下的httpd.conf， 在181行左右 添加如下配置 

	# 如下为PHP环境添加模块
	LoadModule php5_module "D:/Program Files/wamp/php-5.6.12/php5apache2_4.dll"
	PHPIniDir "D:/Program Files/wamp/php-5.6.12/php.ini"

	# 添加PHP支持
	AddType application/x-httpd-php .php


> 然后找到 279行  把

	<IfModule dir_module>
    	DirectoryIndex index.html
	</IfModule>
改成

	<IfModule dir_module>
   		DirectoryIndex index.html index.htm index.php
	</IfModule>

win+x打开命令提示符 进入D:\wamp\Apache24\bin文件目录下，输入命令：`httpd -k restart`

然后在 D:\wamp\Apache24\htdocs 创建phpinfo.php文件 
代码为

<?php phpinfo();?>

然后在浏览器里访问 http://localhost/phpinfo.php
如果出现phpinfo信息  即表示安装成功 如下图

![](http://i1.piimg.com/567571/bb65f23636e223b0.png)


#### 3.4安装mysql ####

	下载之后，MySQL为安装版，按照提示走就可以了。选择custom自定义安装，将安装位置放到D:/wamp/mysql下，方便管理。
	确认好密码


> 附加


### 3.5安装phpmyadmin ###

	下载phpmyadmin https://files.phpmyadmin.net/phpMyAdmin/4.6.4/phpMyAdmin-4.6.4-all-languages.zip
	
	下载完成后 解压 并重命名 phpmyadmin ，复制到D:\wamp\Apache24\htdocs下，不需做其他的配置。在浏览器输入：http://localhost/phpmyadmin，打开控制台，输入你安装MySQL时设置的账号密码，账号默认为root。
	
![](http://i1.piimg.com/567571/4caa5bbc82799166.png)


### 总结 ###

	自此所有php相关环境已安装完成，新的项目 代码可以放到 D:\wamp\Apache24\htdocs 下
	 
	项目中所用到的git工具，可以在 https://git-scm.com/download/win 下载并安装

	