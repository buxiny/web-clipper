---
title: "甲骨文实例控制台连接CloudShell连接(及vnc连接)及进入bios的教程(救砖前置教程)"
source: "https://telegra.ph/OracleCloudInstancesConsoleConnection-10-02"
author:
  - "[[金箍棒]]"
published: 2024-10-02
created: 2026-05-07
description:
tags:
  - "clippings"
---
[金箍棒](https://t.me/jingubang_sub)

#vnc #VNC #CloudShell #控制台 #救机 #救鸡 #bios #vncview

[https://t.me/jingubang\_sub/83](https://t.me/jingubang_sub/83)

### 什么是控制台

控制台连接是云服务实例都会有的一个功能

就是让你像本地的电脑一样,通过一个界面查看云服务器;

最主要的功能,就是可以进入bios

这是救机的基操步骤

> 注:本教程参考教程(就是抄的):[龘龘的博客 - 甲骨文使用VNC远程链接 (3dm.pw)](https://3dm.pw/archives/oraclevnc)

### 操作方式的类型

①串行控制台CloudShell连接

②vnc网络连接(windows通过linux跳板连接)控制台,如果需要查看图形界面的windows/图形界面的ubuntu,必须使用这个方式;

③vnc网络连接(windows直接连接)控制台

### 具体操作方式如下:

### ①串行控制台CloudShell连接

1.直接登录甲骨文oci->进入\[实例\]列表->打开\[实例\]详情->菜单找到\[操作系统管理\]->找到\[控制台连接\]->点击按钮\[启动CloudShell连接\]或\[Launch Cloud Shell connection\]

![](https://s2.loli.net/2025/07/07/NhzPMiYnrcsjlWR.png)

启动CloudShell连接

2.这个时候,页面的下半部就拉起一个黑色的界面,就是\[控制台\]了

![](https://s2.loli.net/2025/07/07/JabRAm1Ty2o3gHO.png)

控制台

2.1.如果你出现了下面的截图

![](https://s2.loli.net/2025/05/09/E2gVLGIYypAXDRw.png)

说明你是一个0权的账号了，你可以继续使用②vnc网络连接(windows通过linux跳板连接)控制台-这个方式继续进行救机；

3.进入bios的方式,

3.1.点击实例中的\[重新引导\]按钮->选择一个复选框,选择\[强制重新引导实例\]->点击黑色按钮\[强制重新引导实例\]

![](https://s2.loli.net/2025/07/07/C2rDOkIFovKNYAL.png)

重新引导

3.2.动作快,非常快,点击一下页面下半部的黑色的\[控制台\],狂连续不停的狂按\[esc\]键,就会进入bios了,一个灰白色的页面;

![](https://s2.loli.net/2024/10/08/Of3IthY6wu2pL7H.png)

3.3.后面的操作,使用键盘操作,上下左右按钮可以操作;

4.进入控制台之后,有一定的概率无法操作,这个原因不清楚怎么修复,毕竟龟壳的bug比比皆是,这个时候就需要使用②的跳板机方式连接控制台了

### ②vnc网络连接(windows通过linux跳板机连接)控制台

1.这里先说一下原理

![](https://s2.loli.net/2024/10/05/8FcC1azurwLAoIK.png)

image.png

1.1.这个过程,我们一共有3个机器

1.1.1.机器A,就是被救的主机

1.1.2.机器B,是一个跳板机,就是一个普通的VPS,ubuntu或者debian都可以,amd或者arm都可以,可以是A机同局域网/也可以不是同网的,都是可以的

1.1.4.机器C,就是你自己的主控端,windows或者macos都可以;

1.2.连接的关系是->机器C->通过vnc协议(5900端口)连接,无任何加密机制->机器B->通过ssh协议(甲骨文的控制台连接方式)连接,通过密钥方式加密->机器A的控制台

2.在机器B的ssh进行操作

2.1.在SSH中执行ssh-keygen，生成当前系统的公钥及私钥；

2.2.所有的选项默认即可，这里为了方便用不着设置密码，毕竟不是作为公开使用的。

2.3.在生成完成后找到/root/.ssh/目录下id\_rsa.pub文件，

2.4.cat一下，保存下来备用，

2.5.我们下文会叫他\[id\_rsa.pub.serverB\]

![](https://3dm.pw/upload/2024/03/1.webp)

3.在机器A的甲骨文OCI操作,

3.1.登录甲骨文oci->进入\[实例\]列表->打开\[实例\]详情->左侧菜单找到\[控制台连接\]或\[ConsoleConnection\]->点击灰色的按钮\[创建本地连接\]

![](https://s2.loli.net/2025/07/07/5EMJ3tQWXoy7Rzh.png)

创建本地连接

3.2.在界面中,选择\[粘贴SSH密钥\]

3.3.在下方的\[SSH密钥\]中,将刚才备用的\[id\_rsa.pub.serverB\]以文本方式粘贴进去,开始创建。

3.4.创建完成后,点击创建出来的一行,最右侧的\[三个点\],选择\[复制Linux/Mac的VNC连接\],放到记事本中;。

![](https://s2.loli.net/2025/07/07/QOP6FLoZ5HYawWc.png)

复制VNC连接

3.5.编辑这个命令的文本,找到localhost:5900修改为0.0.0.0:5900

![](https://3dm.pw/upload/2024/03/2.webp)

3.6.这个命令的文本,下文会叫他\[vnccommand.serverA\]

4.在机器B的ssh进行操作

4.1.这里在ssh里,直接运行刚才的\[vnccommand.serverA\]

![](https://3dm.pw/upload/2024/03/image-1711855468123.png)

4.2.执行后连接两次确认的询问直接输入yes即可

4.3.在ssh里,会停在一个光标闪动的情况,就是成功了

4.4.这里需要注意一下,需要在防火墙放行tcp5900端口->防火墙的教程参考-> [甲骨文云OCI的防火墙（虚拟云网络-安全列表）怎么关闭(放行tcp或udp端口)](https://telegra.ph/OracleCloudNetworkingVirtualcloudnetworksSecurityList-10-02)

4.5.这里可能会出现如果出现

```
Unable to negotiate with 130.61.0.255 port 443: no matching host key type found. Their offer: ssh-rsa
kex_exchange_identification: Connection closed by remote host
Connection closed by UNKNOWN port 65535
```

4.5.1.需要使用文本编辑器打开配置文件，

```
# 系统级配置文件
sudo nano /etc/ssh/ssh_config
# 或用户级配置文件
nano ~/.ssh/config
```

4.5.2.在配置文件中，您需要添加或编辑以下参数：

```
Host *
    HostKeyAlgorithms +ssh-rsa
    PubkeyAcceptedKeyTypes +ssh-rsa
```

4.5.3.重启SSH服务：

```
sudo systemctl restart sshd
```

5.在机器C操作,就是你本地的电脑

5.1.你本地的电脑连接B机器开放的vnc

5.2.前往RealVNC的网站上下载VNC Connect这个软件（ [点击前往](https://www.realvnc.com/en/connect/download/viewer/windows/) ），支持的平台很多，或者有其他的你喜欢的VNC工具都是可以的;

![](https://3dm.pw/upload/2024/03/image-1711855574968.png)

image-1711855574968

5.3.在软件中直接输入B机器的公网IP:5900，回车即可开始连接。

![](https://3dm.pw/upload/2024/03/4.webp)

4

5.4.其中的不安全报错直接忽略即可，随后可以看到VNC的窗口已正常打开，随后该怎么处理问题就怎么处理。

![](https://3dm.pw/upload/2024/03/5.webp)

5

5.5.如果出现了断联的情况,请回到上面的4步骤,再次运行即可;

6.通过vnc进入bios的方式,

6.1在vnc里是有reboot重启按钮的，如图

![](https://s2.loli.net/2025/05/09/UowMVG41vFNZ2QO.png)

重启按钮

如果重启按钮不好用的话，可以使用6.1.1的重启方法

6.1.1点击实例中的\[重新引导\]按钮->选择一个复选框,选择\[强制重新引导实例\]->点击黑色按钮\[强制重新引导实例\]

![](https://s2.loli.net/2025/07/07/C2rDOkIFovKNYAL.png)

重新引导实例

6.2.动作快,非常快,点击一下vnc,狂连续不停的狂按\[F2\]键,就会进入bios了,一个灰白色的页面;

![](https://3dm.pw/upload/2024/03/image-1711846505215.png)

6.3.后面的操作,使用键盘操作,上下左右按钮可以操作;

### ③vnc网络连接(windows直接连接)控制台

这个方式特别复杂,建议你不要尝试,请选择②的跳板机方式连接控制台了

### 更多的DD或救机教程

看完这篇,大概率你需要进行下一篇教程-> [甲骨文云实例通过控制台(VNC)连接引导到Netboot的方法(救砖前置教程)(重置密码前置教程)](https://telegra.ph/OracleCloudInstancesConsoleConnectionVNCBootNetboot-10-02)

请查看-> [甲骨文实例dd/救砖/vnc/教程](https://telegra.ph/oracleddindex-04-08)

### 如果dd之后变砖了怎么办?

请查看-> [甲骨文实例dd/救砖/vnc/教程](https://telegra.ph/oracleddindex-04-08)

### 相关阅读

1.[如果我想购买甲骨文账号有哪些渠道呢?](https://telegra.ph/BuyOracleAccount-04-17)

2.[甲骨文怎么选区域?怎么测试不同区域的速度?](https://telegra.ph/%E7%94%B2%E9%AA%A8%E6%96%87%E6%80%8E%E4%B9%88%E9%80%89%E5%8C%BA%E5%9F%9F%E6%80%8E%E4%B9%88%E6%B5%8B%E8%AF%95%E4%B8%8D%E5%90%8C%E5%8C%BA%E5%9F%9F%E7%9A%84%E9%80%9F%E5%BA%A6-02-27)

3.[甲骨文哪些区域可以手开arm机型？](https://telegra.ph/oraclearm-03-22)

### ↓↓↓↓相关阅读-点击下方的链接查看更多↓↓↓↓

**※※** [**甲骨文遇到的一些问题的解决方案和处理办法**](https://telegra.ph/oraclehelp-03-07) **※※**

**本系列的订阅频道->** [**https://t.me/jingubang\_sub**](https://t.me/jingubang_sub)