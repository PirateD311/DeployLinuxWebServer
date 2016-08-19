# DeployLinuxWebServer
Summary fo Deploy Node.js+NginX on Linux .
#前言
### 本文档主要记录在全新Linux环境上搭建基本Node服务器的过程，主要内容包括：
1.基本linux环境的构建（用户，环境变量等）</br>
2.NginX服务器的安装</br>
3.Node.js部署</br>
4.数据库安装</br>
5.基本Node服务</br>
6.服务器可靠性、可维护性的提升</br>
####版权所有，禁止转载
#修订记录
</br>
#1.1基本linux环境的构建
本次安装使用的是CentOS的云linux环境，无任何预装应用。</br>
为安全考虑，应使用单独用户来部署服务器，不应用root用户直接进行操作。
##1.1.1用户创建
#####使用root用户远程登录云单板:ssh root@xxx.xxx.xxx.xxx 
#####创建新用户：使用useradd命令，再使用passwd命令修改用户密码。详见<http://blog.csdn.net/beitiandijun/article/details/41678251> </br>
#####创建用户后最好检查环境变量的path是否包含所有bash目录，否则会有部分命令找不到，可对比root用户的path.修改方法参见：<http://sunnylocus.iteye.com/blog/323354>
#2.1 NginX服务器的安装
##2.1.1 检查安装环境  
#####1.下载nginx安装包并使用ftp传至linux环境，解压。  
初始申请的linux只装有基本的linux系统软件，甚至连基本的gcc程序都没有，所以在正式安装Nginx前还需安装配置好nginx依赖的软件。  
Nginx依赖：gcc,pcre,openssl,zlib。  
若不确定依赖是否ok，可使用which命令查看PATH下是否有对应可执行文件，或先执行nginx的configure检查环境配置。
#####2.安装依赖  
本次安装使用CentOS的yum方便快捷的安装nginx的所有依赖，命令如下(使用root执行)： 
yum -y install gcc;   
yum -y install gcc-c++；   
yum -y install pcre-devel；  
yum -y install openssl;    
yum -y install zlib-devel；  
*也可自行下载所有依赖的安装包，自行安装部署在linux上，难度较大，另开文章进行记录*  
#####3.Nginx解压包的./configure成功生成makefile后表示环境配置ok，建议在configure是带上--prefix=path 来手动配置nginx的安装路径。执行make&make install安装即可。
#####4.进入nginx的安装目录后可执行sbin/nginx 启动nginx，启动成功后使用浏览器输入linux服务器ip既可看到nginx的默认index页面，至此基本的nginx服务器搭建成功。  
#####5.初始环境常用配置
###### ·在基本环境配置时，可根据使用喜欢进行一些常用命令的别名设置，长久生效方法为在用户home目录下的.bashrc文件中加入alias命令  
#3.1 Node.js部署
#####node.js的部署较为简单，主要步骤如下：   
1.从node官网获取对应操作系统的最新安装包，本次部署使用linux-64位的安装包，不使用源码包进行编译安装。
2.将安装包传至linux并解压既已完成初步安装，最新node包采用.xz的压缩方式，解压方式为：xz - d **.xz ;tar -xvf **.tar    
3.在node解压目录的bin目录下执行./node -v 检查node是否运行ok。出现node版本号说明安装成功，此时建议在用户的PATH下添加node/bin目录，或在PATH下创建node和npm命令的软连接，方便node及npm命令的使用。  
#4.1 数据库安装
######由于oracle收购mysql后可能会在后续将mysql闭源，社区为规避mysql闭源的风险，在mysql基础上拉了分支，既mariaDB，完全兼容mysql，目前google、Facebook等都已切换至mariadb。本次所用linux环境为Centos7，此版本yum的软件库已移除mysql-server，故推荐直接安装mariadb。
使用yum进行mariadb安装非常方便，步骤如下：  
1.检查环境上是否已安装了mysql或mariadb:rpm -qa |grep mysql*  ;rpm -qa |grep mariadb;  
  若已安装则可跳过安装，进行数据库启动。若想卸载则使用:yum -remove 包名 进行删除。   
2.安装mariadb的服务端和客户端：yum install MariaDB-server MariaDB-client ;   
3.启动mariadb：service mariadb start /service mysql start   
4.mariadb兼容mysql所有命令，数据库启动后可使用mysql命令进行操作  
##### 数据库安装成功后应进行一些基本配置。
###### 修改root用户密码:使用mysqladmin -u root -p 命令为root用户设置密码。
###### 添加用户：1.使用root用户进入mysql数据库，在user表中添加用户信息，用户名、密码、权限等。
######    2.使用GRANT命令。
`root@host# mysql -u root -p password;
Enter password:*******
mysql> use mysql;
mysql> GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP
    -> ON TUTORIALS.*
    -> TO 'zara'@'localhost'
    -> IDENTIFIED BY 'zara123';`   

参考链接：[Centos7安装mariadb](http://blog.csdn.net/heatheryun/article/details/51015354)   
[mysql管理](http://www.runoob.com/mysql/mysql-administration.html)


