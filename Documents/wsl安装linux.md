# <span id="jump1">wsl安装linux </span> 

#### 文章目录

*   [启动子系统虚拟化](about:blank#_2)
*   [手动安装](about:blank#_4)
*   *   [安装内核更新包](about:blank#_5)
    *   [设置默认WSL版本](about:blank#WSL_12)
    *   [从官网上下载安装包](about:blank#_21)
*   [卸载WSL](about:blank#WSL_48)
*   [一些报错](about:blank#_51)

[](https://blog.csdn.net/weixin_48076899/article/details/135214749)启动子系统虚拟化
---------------------------------------------------------------------------

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/cd6b0e2ddbbb5bc8db9f91a4727912aa.png)

[](https://blog.csdn.net/weixin_48076899/article/details/135214749)手动安装
-----------------------------------------------------------------------

### [](https://blog.csdn.net/weixin_48076899/article/details/135214749)安装内核更新包

```
wsl --update

```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/23f587dd90b76bf26575147297fb0a02.png)

### [](https://blog.csdn.net/weixin_48076899/article/details/135214749)设置默认WSL版本

我们只使用wsl2，power shell 以管理员方式运行

```
# 将 WSL 默认版本设置为 WSL 2
wsl --set-default-version 2

```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/6d6504644a5c0e41dcf8440394a289e1.png)

### [](https://blog.csdn.net/weixin_48076899/article/details/135214749)从官网上下载安装包

微软提供了一个手动下载WSL发行版的网址：[手动下载适用于 Linux 的 Windows 子系统发行版包](https://learn.microsoft.com/zh-cn/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/9eaf44979c7afb461a29ff9481f6f749.png)  
选择任一版本下载，以Ubuntu 22.04为例：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/3da6bf5dc6b188c821dff3dff985968a.png)  
下载后可得到一个后缀名为.AppxBundle的文件，将后缀改成.zip，并解压  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/bd94b1fca66fe3d460c55c670731e3a7.png)  
解压后文件夹有一个后缀名为.appx的文件，将后缀改成.zip，再次解压  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/35b41077c18d67851a36ef6270e2cab5.png)  
将文件夹移动到想要安装的路径中，解压后文件夹得到一个.exe文件，双击运行；  
需要注意的是：安装目录的磁盘不能开压缩内容以便节省磁盘空间选项，否则会报错  
可以右键文件夹–>属性–>常规–>高级找到并关闭这个选项  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2feb447dd78e195ee0c1524910c55071.png)  
等待一段时间后安装完成，自行这是用户名及密码  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2e8a71418df692546d0bb58f3372221a.png)  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/ad2bcefa33169714949a224f12d94784.png)  
安装成功后 文件夹下多一个ext4.vhdx镜像，可以理解为安装的位置  
这样安装后，linux产生的文件是默认在刚刚自定义选择的路径下。WSL1的安装位置下有个rootfs文件夹就是子系统里的全部文件。**WSL2(本质虚拟机)是放在虚拟磁盘(**.vhdx)\*\*

回到PowerShell查看所有系统：

```
wsl -l -v

```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b7d1fa0e3b5378c427ba7ad85e13a23f.png)  
如图所示已安装成功

[](https://blog.csdn.net/weixin_48076899/article/details/135214749)卸载WSL
------------------------------------------------------------------------

先打开cmd,输入wsl --list，查看你安装的版本，然后输入wsl --unregister <要卸载的发行版>，之后删掉目录下的那个子系统就可以了。

[](https://blog.csdn.net/weixin_48076899/article/details/135214749)一些报错
-----------------------------------------------------------------------

重装系统后安装WSL2，出现报错：WslRegisterDistribution failed with error: 0x800701bc  
可参考该文章：[Win11安装Ubuntu子系统报错WslRegisterDistribution failed with error: 0x800701bc](https://blog.csdn.net/microsoft_mos/article/details/123627295?spm=1001.2014.3001.5506)  
解决方法：  
下载安装适用于 x64 计算机的最新 WSL2 Linux 内核更新包  
下载链接：[https://wslstorestorage.blob.core.windows.net/wslblob/wsl\_update\_x64.msi](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)  
Windows下一直next安装  
安装后再次打开Ubuntu，并无报错

参考：  
\[1\][自定义WSL的安装位置，别再装到C盘啦](https://zhuanlan.zhihu.com/p/263089007)  
\[2\][windows11 安装WSL2全流程](https://blog.csdn.net/u011119817/article/details/130745551)

 

本文转自 [https://blog.csdn.net/weixin\_48076899/article/details/135214749](https://blog.csdn.net/weixin_48076899/article/details/135214749)，如有侵权，请联系删除。