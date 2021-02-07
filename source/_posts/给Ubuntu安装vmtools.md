---
title: 给Ubuntu安装vmtools
categories: 学习笔记
tags:
- Ubuntu
---
<escape><!-- more --></escape> 

之前安装ubuntu的时候没有安装vmtools，本周补安装
 打开ubuntu发现安装VMtools的小条变成了灰色点不了（应该是因为用的汉化版VNware）
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020100111135071.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsaWVuRW93eW5XYW4=,size_16,color_FFFFFF,t_70#pic_center)
解决办法：
1.关闭虚拟机；
2.在虚拟机设置分别设置CD/DVD、CD/DVD2、软盘为自动检测；
3.再重启虚拟机，灰色字即点亮。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020100111145969.png#pic_center)
可能会造成打开的时候有问题，没关系，重复开几次一直OK就可以了
然后再打开就发现可以
点击虚拟机菜单栏中的【虚拟机】-->【安装VMware Tools】
然后就出现了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201001111556538.png#pic_center)
接下来把VMwareTools…tar.gz文件提取（不要点成复制了）到某个新建的目录下，比如myfile目录下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201001111714526.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsaWVuRW93eW5XYW4=,size_16,color_FFFFFF,t_70#pic_center)
大概是这样，找的网图。
启动终端，然后切换到root用户（切换命令为：sudo su，回车然后会提示你输入当前登录用户的密码，输入成功后即可进入root用户）。
以root用户进入到刚刚提取到的vmware-tools-distrib文件夹下，然后输入命令：./vmware-install.pl，然后回车。
可能会出现报错：Unable to excute "/usr/bin/vmware-uninstall-tools.pl
主要原因是：usr/bin目录下没有vmware-uninstall-tools.pl
解决方案：
进入vmtools的文件解压目录，然后进入bin目录下将vmware-uninstall-tools.pl复制到usr/bin目录下
需要输入的命令分别有：
cd /opt
cd vmware-tools-distrib/
cd bin
cp vmware-uninstall-tools.pl /usr/bin
这样就没问题了，可以继续执行这部分的操作，附上网图，自己的终端被关了…
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201001111811453.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsaWVuRW93eW5XYW4=,size_16,color_FFFFFF,t_70#pic_center)
上面的操作后就开始安装VMware Tools了，根据其提示输入yes/no，直到出现Enjoy, –the VMware team如下图，就表示安装成功了，然后手动重启虚拟机
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201001111836262.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsaWVuRW93eW5XYW4=,size_16,color_FFFFFF,t_70#pic_center)
在这一步中也可能会出现问题，那是因为之前可能安装过vwtools但是没有成功，一般不用管重复这一步的命令然后按照它[]中提示的操作就可以了，非常傻瓜式的操作但是我喜欢。
重启虚拟机之后可以看到变成了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201001111859849.png#pic_center)

到此
完成！！
