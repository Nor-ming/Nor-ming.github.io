---
title: CTFd平台搭建（文件方式）
categories: 学习笔记
tags:
- Ubuntu
- CTF
---
<escape><!-- more --></escape> 

记录一下自己搭建过程中踩的坑
没有云服务器，在虚拟机中搭建的，使用的是Ubuntu16.04.7LTS
查看当前版本的命令：`cat /etc/issue`
然后发现自己的虚拟机连不上网了，重新弄一下吧，选的是NAT模式。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200912164424975.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsaWVuRW93eW5XYW4=,size_16,color_FFFFFF,t_70#pic_center)
注意给它管理员权限才能更改。

连上网后就可以开始了
1.升级源

```c
sudo apt-get update
```
在升级源的时候可能会报错
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020091216540780.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsaWVuRW93eW5XYW4=,size_16,color_FFFFFF,t_70#pic_center)
我自己忘记截图了，用一下网上博客的图片吧
遇到这种情况是因为连接不到 US 的服务器，所以更新失败，需要在系统设置中将源服务器设置为中国的。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200912165825384.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsaWVuRW93eW5XYW4=,size_16,color_FFFFFF,t_70#pic_center)
这样应该就可以了，如果还不巧遇到了报的错误为”Failed to fetch 404 Not Found”，这个问题参考了一下[https://www.cnblogs.com/wangshaowei/p/10994764.html](https://www.cnblogs.com/wangshaowei/p/10994764.html)的博客，说是是因为每个Ubuntu版本都有生命结束周期（EOL）时间，常规的Ubuntu发行版提供18个月的支持，而LTS（长期支持）版本则长达3年（服务器版本）和5年（桌面版本）。当某个Ubuntu版本达到生命结束周期时，其仓库就不能再访问了，你也不能再从Canonical获取任何维护更新和安全补丁。如果你所使用的Ubuntu系统已经被结束生命周期，你就会从apt-get或aptitude得到以下404错误，因为它的仓库已经被遗弃了。

解决方法为将/etc/apt/sources.list路径下的源替换为旧版本仓库的源。

2.安装git，因为CTFd的源码和部署好的题目都是要通过github传输的
```c
sudo apt install git
```
3.安装pip　　
```c
sudo apt install python-pip
```
安装pip的时候也会遇到一些问题，如果出现异常可以升级pip
```c
sudo python -m pip install --upgrade pip
```
4.安装Flask，因为CTFd是基于Flask框架建造的，所以要搭建CTFd要安装Flask
```c
sudo pip install Flask
```
5.下载CTFd
```c
sudo git clone https://github.com/isislab/CTFd.git
```
6.安装CTFd
```c
cd CTFd
```
```c
sudo ./prepare.sh
```
第二条命令执行完了之后可能会遇到警告和报错，下面逐步介绍一下如何解决
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200912171919176.png#pic_center)
这是由于Python版本的问题了，Ubuntu自带2.7和3.5版本的Python，而2的版本在2020年1月就停止维护了，未来的pip版本将放弃对Python 2.7的支持。我们需要将2的版本改成3的。

```c
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2 100
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3 150
```
我们可以使用这两个命令将2 改成3 
改完了之后可以检查一下

```c
python --version
```
这个命令可以看到当前Pyhton的版本，但是可能不准确，因为通过改变指向可以让它显示，但是并不能将环境也改变成3 ，而环境变量是和默认有关联的
可通过查看环境来确定，如下命令

```c
env python
```
如果此时显示的是3 的版本那就是成功改变了
接下来我们需要给3 安装它的pip，否则它会出现
/usr/bin/python3: No module named pip
也会出现这样的报错
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200912173314758.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsaWVuRW93eW5XYW4=,size_16,color_FFFFFF,t_70#pic_center)
下载安装pip的方法很简单，在虚拟机中的官网下载按照说明安装就行了。注意要使用sudo命令不然没有权限会安装失败。

可能还会出现这样的警告
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200912174111985.png#pic_center)
这个执行以下命令应该就可以解决了

```c
sudo chown -R root /home/$USERNAME/.cache/pip/
sudo chown -R root /home/$USERNAME/.cache/pip/http/
```

然后我们再执行安装CTFd的命令，发现它又出现了新的报错
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200912174603106.png#pic_center)
查看了pydantic的官方文档之后发现也是由于Pyhton版本的问题，他它需要3.6及以上的版本才能够支持，Python自带的版本最高只有3.5，这就需要升级
可以参考这篇博客，方法非常有效，而且有提供安装Python3.6之后怎样预防崩溃
[https://segmentfault.com/a/1190000021838605](https://segmentfault.com/a/1190000021838605)
附上截图防止失效
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200912175157339.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsaWVuRW93eW5XYW4=,size_16,color_FFFFFF,t_70#pic_center)
如果不小心真的把Ubuntu弄崩溃了导致终端框打不开，还可以参考这一篇博客
[https://blog.csdn.net/DeepWolf/article/details/88800603](https://blog.csdn.net/DeepWolf/article/details/88800603)

这篇博客也介绍了Python3.6的安装方法以及如何改变环境，一定要记得改环境，不然依然就无法成功。
[https://blog.csdn.net/qq_32216809/article/details/86347926](https://blog.csdn.net/qq_32216809/article/details/86347926)

这样再执行安装CTFd的命令应该就没有问题了

7.运行CTFd（要在打开CTFd文件的命令后执行（cd CTF））
```c
sudo python serve.py
```
虚拟机的浏览器中访问127.0.0.1：4000/就可以看待自己的CTFd平台了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200912180012276.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FsaWVuRW93eW5XYW4=,size_16,color_FFFFFF,t_70#pic_center)
不足的是它访问的速度非常慢…目前还不清楚应该如何解决，这篇博客也许能够提供帮助。
[https://blog.csdn.net/weixin_43880435/article/details/107339592?utm_source=app](https://blog.csdn.net/weixin_43880435/article/details/107339592?utm_source=app)

如果想要让它在物理机中访问需要安装gunicorn并规定映射的端口
我尝试了但是没有成功，大家可以试一试。

```c
sudo pip install gunicorn
sudo gunicorn --bind 0.0.0.0:8000 -w 1 "CTFd:create_app()"
sudo pip install gunicorn
```
关于速度慢可以尝试这个
[https://blog.csdn.net/asd413850393/article/details/98123982](https://blog.csdn.net/asd413850393/article/details/98123982)

嗯，以上就是这次搭建的全过程，遇到了很多问题，下次尝试用docker搭建一下~
