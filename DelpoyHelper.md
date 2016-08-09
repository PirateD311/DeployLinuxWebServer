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
本次安装使用的是suse12的云linux环境，无任何预装应用。</br>
为安全考虑，应使用单独用户来部署服务器，不应用root用户直接进行操作。
##1.1.1用户创建
#####使用root用户远程登录云单板:ssh root@xxx.xxx.xxx.xxx 
#####创建新用户：使用adduser命令，详见<http://blog.csdn.net/beitiandijun/article/details/41678251> </br>
#####创建用户后最好检查环境变量的path是否包含所有bash目录，否则会有部分命令找不到，可对比root用户的path.修改方法参见：<http://sunnylocus.iteye.com/blog/323354>
