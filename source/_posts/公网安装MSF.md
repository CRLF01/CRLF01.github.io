---
title: 公网安装MSF
top: true
cover: true
toc: true
mathjax: true
date: 2020-10-23 11:35:18
password:
summary:
tags: MSF
categories: MSF
---



CentOS7安装MSF

<!-- more -->

一键安装

```
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && chmod 755 msfinstall && ./msfinstall 

adduser msf           #添加msf用户

su msf                #切换到msf用户

cd  /opt/metasploit-framework/bin   #切换到msf所在的目录 

./msfconsole          #以后启动msfconsole，都切换到msf用户下启动，这样会同步数据库。如果使用root用户启动的话，不会同步数据库  

```

也可以将msfconsole加入到执行目录下，这样在任何目录直接msfconsole就可以了
ln -s /opt/metasploit-framework/bin/msfconsole /usr/bin/msfconsole

#备注：
#初次运行msf会创建数据库，但是msf默认使用的PostgreSQL数据库不能与root用户关联，这也这也就是需要新建用户msf来运行metasploit的原因所在。如果你一不小心手一抖，初次运行是在root用户下，请使用 msfdb reinit 命令，然后使用非root用户初始化数据库。        

MSF后期的升级：msfupdate



转载自：https://www.cnblogs.com/qi-yuan/p/13780876.html

侵权请联系删除！

---

