---
title: Windows/Linux权限提升前奏
top: true
cover: true
toc: true
mathjax: true
date: 2020-10-10 09:18:32
password: xxxxxx
summary: 
tags: 权限提升
categories: 权限提升
---

---

#### **一、权限提升概述**

俗话说 “不想当将军的士兵不是好士兵”，在渗透测试中，同样亦是如此，我们所有的渗透测试环节都是为了能获取最高权限，不以获取最高权限为目的的渗透测试都是耍流氓！只有获取了最高权限，我们才能更好的去进行下一步的渗透，比如横向渗透、域渗透等。

当然，这个过程也可能是曲折并艰难的，一句话概述，痛并快乐着o_o



#### **二、用户权限/用户组划分**

知彼知己，百战不殆，想要更好的去进行好权限提升，我们也需要对系统的用户及组权限有个大致的了解，这里主要以Windows为例。

用户权限（权限排序递减）：

```
TrustedInstaller权限				版本须大于Windows7版本，用来管理系统核心文件的一种权限
System权限						系统权限，理论上是实际上的的最高权限
Administrator权限					管理员权限，可以管理其他用户	
User权限							普通用户权限
Guest权限							来宾/访客权限，权限很小
```



**组：**

所谓的组，可以理解为一系列用户组成的集合，当用户加入后，会自动继承该组所拥有的权限，一个用户可以同时存在与多个组中。

用户组又分为基本用户组和内置特殊组。



**基本用户组**

```
Administrators组	：
管理员组，最高权限组，如果存在域环境，Domain Admins也自动加入该组，也会继承该组的权限。

Users组：
普通用户组，该组员只拥有一些基本的权利，例如运行应用程序，但是他们不能修改操作系统的设置、不能更改其它用户的数据、不能关闭服务器级的计算机，所有添加的本地用户帐户者自动属于该组。如果这台计算机已经加入域，则域的Domain Users会自动地被加入到该计算机的Users组中。

Guests组	：						
来宾用户组，拥有一些基本的权限，无法永久改变桌面环境。

Backup OPerators组：				
备份组，无论有无访问权限，都可以通过“开始”－“所有程序”－“附件”－“系统工具”－“备份”的途径，备份与还原这些文件夹与文件。

Network Configuration Operators组：
组内的用户可以在客户端执行一般的网络设置任务，例如更改IP地址，但是不可以安装/删除驱动程序与服务，也不可以执行与网络服务器设置有关的任务，例如DNS服务器、DHCP服务器的设置。

Power Users组：					
电源组，该组内的用户具备比Users组更多的权利，但是比Administrators组拥有的权利更少一些：
创建、删除、更改本地用户帐户
创建、删除、管理本地计算机内的共享文件夹与共享打印机
自定义系统设置，例如更改计算机时间、关闭计算机等
但是不可以更改Administrators与Backup Operators、无法夺取文件的所有权、无法备份与还原文件、无法安装删除与删除设备驱动程序、无法管理安全与审核日志

Remote Desktop Users组：			
远程桌面组，该组的成员可以通过远程计算机登录，例如，利用终端服务器从远程计算机登录。

Print Users组：					
打印机组
```



**内置特殊组：**

```

Everone：
任何一个用户都属于这个组。注意，如果Guest帐号被启用时，则给Everone这个组指派权限时必须小心，因为当一个没有帐户的用户连接计算机时，他被允许自动利用Guest帐户连接，但是因为Guest也是属于Everone组，所以他将具备Everyone所拥有的权限。

Authenticated Users：
任何一个利用有效的用户帐户连接的用户都属于这个组。建议在设置权限时，尽量针对Authenticated Users组进行设置，而不要针对Everone进行设置。

Interactive：
任何在本地登录的用户都属于这个组。

Network：
任何通过网络连接此计算机的用户都属于这个组。

Creator Owner：
文件夹、文件或打印文件等资源的创建者，就是该资源的Creator Owner（创建所有者）。不过，如果创建者是属于Administrators组内的成员，则其Creator Owner为Administrators组。

Anonymous Logon：
任何未利用有效的Windows Server 2003帐户连接的用户，都属于这个组。注意，在windows 2003内，Everone 组内并不包含“Anonymous Logon”组
```



#### 三、提权前奏-准备工作

**目标信息收集：**

**相关基础命令：**

```
systeminfo						获取详细的系统信息，包括操作系统版本、位数、类型。
ipconfig /all					获取详细的ip、DNS、计算机名称等信息。
net user						获取当前机器用户的信息。
net localgroup administrators	 获取当前的用户组
whoami							当前登录用户权限。
netstat -an						端口开放情况。
tasklist /svc					当前运行进程信息。
quser							当前登录用户信息。
```

**①操作系统名称及版本信息收集：**

```
英文版：	systeminfo | findstr /B /C:"OS Name" /C:"OS Version"				
中文版：	systeminfo | findstr /B /C:"OS 名称" /C:"OS 版本"

版本信息：	ver
5.1   		win-xp
5.2   		win2003
6.1   		win-7 、2008
6.3   		win8、2012
10.0  		win2016、win10
```



**②主机名称和所有环境变量**

```
主机名称：				hostname
环境变量：				set
```



**③用户信息收集**

```
所有用户：				net user 或 net1 user
管理员用户组信息：		  net localgroup administrators
当前在线用户信息：		  query user 或 quser
管理员用户信息：		   net user administrator    注意版本注释、描述等信息，大型的环境下有可能会存在密码
```



**④端口信息收集**

```
端口开放状况：						netstat -an

注册表查看远程桌面端口：			
REG query HKLM\SYSTEM\CurrentControlSet\Control\Terminal" "Server\WinStations\RDP-Tcp /v PortNumber

命令行查看：
获取进程PID：tasklist /svc | find "TermService"
根据PID查询：netstat -ano | find "PID号"

```



**⑤网络状况信息收集**

```
网络配置情况：				ipconfig /all
路由信息收集：				route print
网络连接状况：				netstat -ano
ARP缓存表：				  arp -a

防火墙状况：
netsh firewall show config
netsh firewall show state

关闭防火墙：
netsh firewall set opmode disable（2003及以下默认关闭）
netsh advfirewall set allprofile state off 
建议最好不关闭，做到最小化伤害。

```



**⑥应用程序及服务信息收集**

```
判断服务&程序&杀软情况：			   tasklist /svc 	结合在线杀软对比 http://tools.mo60.cn/
驱动信息收集：						 DRIVERQUERY
启动的Windows服务：				   net start
服务启动权限：						 sc qc 服务名
查看程序安装列表：					wmic product list brief
查看服务列表：						 wmic service list brief
查看进程列表：						 wmic process list brief
查看已启动的程序：					wmic startup list brief
查看已安装更新及日期：				   wmic qfe get Caption,Description,HotFixID,InstalledOn
搜索特定补丁编号：					wmic qfe get Caption,Description,HotFixID,InstalledOn | findstr /C:"KBxxxxxx"
结束特定程序：						 wmic process where name="xx.exe" call terminate
```



**⑦敏感文件收集**

```
dir /b /s password.txt
dir /b /s *.doc
dir /b /s *.ppt
dir /b /s *.xls
dir /b /s *.docx
dir /b /s *.xlsx
dir /b /s config.* filesystem
findstr /si password *.xml *.ini *.txt
findstr /si login *.xml *.ini *.txt

在大型网络中查找无人值守的安装日志文件，这些文件中常包含一些Base64编码的密码，其共同安装位置如下：
C:\sysprep.inf
C:\sysprep\sysprep.xml
C:\Windows\Panther\Unattend\Unattend.xml
C:\Windows\Panther\Unattend.xml
```



 **⑧目录文件操作**

```
列出d:\www下的所有目录：
for /d %i in (d:\wwww) do @echo %i

列出名字长度1-3位的目录：
for /d %i in (？？？) do @echo %i

列出当前路径下的所有特定格式的文件
for /r %i in (*.exe) do @echo %i


强制读取某文件内容
for /f %i in (1.txt) do @echo %i

```



**⑨系统权限配置信息**

```
查看C盘权限配置：		cacls c:\
查看某文件权限配置：		cacls c:\q.txt
```



⑩注册表关键字信息收集

```
  查找关键字password相关注册表：		reg query  HKLM /f password /t REG_SZ /s
  								  reg query  HKCU /f password /t REG_SZ /s
```



**其他相关辅助工具：**

https://github.com/AonCyberLabs/Windows-Exploit-Suggester

https://github.com/Ruiruigo/WinEXP



**提权辅助网站：**

https://bugs.hacking8.com/tiquan/

https://github.com/Al1ex/Heptagram/tree/master/Windows/Elevation



***本文内容部分参考书籍，如有错误，欢迎斧正！***

---







