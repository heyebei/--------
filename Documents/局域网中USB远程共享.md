 

USB/IP是一种基于网络的设备共享机制，可将电脑A（server端）连接的USB设备通过网络共享给远程电脑B（client端）

USB/IP有很多的实现方式（程序），除了下文中使用的免费程序，还有商用程序：

*   USB Network Gate
*   FlexiHub
*   VirtualHere
*   USB over Network  
    更多见[这篇文章](https://zh.altapps.net/soft/virtualhere)

[](https://blog.csdn.net/OTZ_2333/article/details/124073337)Server端（电脑A，连接USB设备）
--------------------------------------------------------------------------------

### [](https://blog.csdn.net/OTZ_2333/article/details/124073337)Windows

**PS**：本文选取的windows上的开源软件为[usbip-win](https://github.com/cezanne/usbip-win/releases)，安装配置方法也可以看官方README

#### [](https://blog.csdn.net/OTZ_2333/article/details/124073337)安装

*   首先下载官方编译好的[软件压缩包](https://github.com/cezanne/usbip-win/releases)，并解压。如果想要自己编译的话参考官方代码的[README](https://github.com/cezanne/usbip-win#build)
    
*   双击文件`usbip_test.pfx`准备导入证书。证书需要导入两遍，分别导入到“受信任的根证书颁发机构”和“受信任的发布者”![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a9f5ea21c247c7a4777878be754c9a79.png)![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/872361b6770b0904a72c2904fd5b683a.png)![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/9b25271c5434af91198813fbc768ac19.png)![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/febf1eb16adc61891c640bb158645fab.png)![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/554236bef833c67e28d411e4bead7d6a.png)
    
*   以**管理员身份**打开CMD或者PowerShell，进入软件所在目录，运行命令开启测试签名
    
    ```
    bcdedit.exe /set TESTSIGNING ON
    
    ```
    
*   开放usbip所需端口：默认端口是3240，到防火墙中的入站
    
    *   也可以用其他端口，后续配置的时候需要使用这个端口  
        ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/594b7e17f8e4889ec52385a54ca9c51f.png)![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/183beefedaeced4368515f67cc2d2254.png)![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/53da97846e2a9e5133e2dfcf28ba3b04.png)![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/305da6af539417a0b700b4bff4f60d49.png)![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/05529878b97e9ac50181f67d0bd7de0f.png)
*   重启电脑
    
*   （可选）下载最新的[usb.ids](http://www.linux-usb.org/usb.ids)，替换软件所在目录的usb.ids文件。
    
    *   每个USB设备都有自己ID（类似网络的IP地址），而usb.ids（类似DNS服务器）中记录了所有已知的USB设备ID对应的名称（分成供应商名称和设备名称，类似网站域名），方便后续快速识别USB设备。
    *   但是由于USB设备在不停出新，较新的USB设备的ID并没有在usb.ids中记录过，因此在后续的识别过程中会显示“unknown product”等字样，表示在usb.ids文件中没有相关记录。这不用担心，找不到记录也是可以用的，只不过不方便我们识别罢了

#### [](https://blog.csdn.net/OTZ_2333/article/details/124073337)配置

*   以**管理员身份**打开CMD或者PowerShell，进入软件所在目录
    
*   将USB设备连接到电脑，然后使用命令`.\usbip.exe list -l`列出所有识别到的USB设备
    
    *   显示“unknown product”是因为在usb.ids文件中没有相关记录，只影响你对USB设备的识别，不影响后续操作
    *   我这里显示了五个USB设备（每个“-”表示一个设备）![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/ffb3a2f7da6668536c2c9c3129292e2a.png)
*   找到自己想要远程共享的USB设备，记录下它的busid，格式为`[数字]-[数字]`
    
    *   如果识别不到的话，可以在USB设备连接前后分别运行上面的命令
    *   假设我想要远程共享的设备是第四个带SATA字样的USB设备（我的移动硬盘），它的busid为`1-181`
*   将USB设备绑定至usbip
    
    *   注意，绑定后，本地电脑就不能使用这个USB设备了
    
    ```
    .\usbip.exe bind -b [busid]
    # 例如我这儿就是
    .\usbip.exe bind -b 1-181
    
    ```
    
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a75db660392ce04d6ebe15311d29f36c.png)
    
*   启动usbipd服务
    
    ```
    .\usbipd.exe -d -4
    # 如果想用其他端口，且安装的时候配置过防火墙
    .\usbipd.exe -d -4 -tPORT [端口]
    
    ```
    
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/fa807c5f39344de726c1438cc4a6755a.png)
    
*   使用完成后，可以将USB设备跟usbip解绑
    
    ```
    .\usbip.exe unbind -b [busid]
    
    ```
    
    然后还需要到“设备和打印机”中，找到连接的USB设备，将所有“USB/IP STUB”卸载，最后重新拔插USB设备  
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/2a3aaf5f825bf9c88c45acd6eb41e8a8.png)![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/9cedcbb2d89b1564d2e3e287420dff90.png)![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/4296662853aec16bac2acefe90ab5734.png)
    
*   如果不需要usbip了，可以关闭测试签名，然后重启
    
    ```
    bcdedit.exe /set TESTSIGNING OFF
    
    ```
    

### [](https://blog.csdn.net/OTZ_2333/article/details/124073337)Linux

#### [](https://blog.csdn.net/OTZ_2333/article/details/124073337)安装

**仅在Ubuntu上测试**，其他发行版需要自己查资料（应该是差不多的）

*   以root权限运行如下命令

    ```
    apt install linux-tools-`uname -r` linux-cloud-tools-`uname -r` linux-tools-common 
    # 下面两个不确定要不要安装，但是我都装了
    apt install linux-tools-generic linux-cloud-tools-generic
    
    ```
    

#### [](https://blog.csdn.net/OTZ_2333/article/details/124073337)配置

*   加载相关的内核模块：
    
    *   不确定重启电脑后需不需要再次加载
    
    ```
    sudo modprobe usbip-core
    sudo modprobe vhci-hcd
    sudo modprobe usbip_host
    
    ```
    
*   关闭防火墙：`ufw disable`
    *   或者在防火墙中开启3240端口的TCP连接

以下操作根window很像，具体细节&图片见上面windows的配置过程

*   将USB设备连接到电脑，然后使用命令`usbip list -l`列出所有识别到的USB设备
*   找到自己想要远程共享的USB设备，记录下它的busid，格式为`[数字]-[数字]`
*   将USB设备绑定至usbip```
    usbip bind -b [busid]
    # 例如
    usbip bind -b 1-181
    
    ```
    
*   启动usbipd服务
    ```
    usbipd -d -4
    # 如果想用其他端口，且安装的时候配置过防火墙
    usbipd -d -4 -tPORT [端口]
    
    ```
    
*   使用完成后，可以将USB设备跟usbip解绑
    ```
    usbip unbind -b [busid]
    
    ```
    

[](https://blog.csdn.net/OTZ_2333/article/details/124073337)Client端（电脑B）
------------------------------------------------------------------------

### [](https://blog.csdn.net/OTZ_2333/article/details/124073337)Windows

**PS**：安装配置方法也可以看[官方README](https://github.com/cezanne/usbip-win/releases)

安装方式与上面server端几乎一样，只是多了一个步骤

*   安装VHCI驱动：以**管理员身份**打开CMD或者PowerShell，进入软件所在目录，运行如下命令
    ```
    usbip.exe install
    
    ```
    如果后续不需要了，可以把驱动卸载了
    ```
    usbip.exe ubinstall
    
    ```
    

#### [](https://blog.csdn.net/OTZ_2333/article/details/124073337)配置

*   以**管理员身份**打开CMD或者PowerShell，进入软件所在目录
    
*   查看电脑A上可用的USB设备
    
    ```
    usbip.exe list --remote [电脑A的IP]
    
    ```
    
*   USB 设备绑定至本机
    
    ```
    usbip.exe attach --remote=[电脑A的IP] --busid=[想要绑定设备的busid]
    
    ```
    
*   查看已经绑定的远程USB设备
    
    ```
    usbip.exe port
    
    ```
    
*   将已经绑定的远程USB设备解绑：端口来自命令`usbip port`输出中"Port"后面的数字
    
    ```
    usbip.exe detach -p [端口]
    
    ```
    

### [](https://blog.csdn.net/OTZ_2333/article/details/124073337)Linux

安装方式与server端一样

#### [](https://blog.csdn.net/OTZ_2333/article/details/124073337)配置

*   查看电脑A上可用的USB设备
    
    ```
    usbip list --remote [电脑A的IP]
    
    ```
    
*   USB 设备绑定至本机
    
    ```
    usbip attach --remote=[电脑A的IP] --busid=[想要绑定设备的busid]
    
    ```
    
*   查看本机USB设备，应该能看到绑定好的USB设备
    
    ```
    lsusb
    
    ```
    
    例如  
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/6a66a98df0d46561f6e69ba064080e10.png)
    
*   查看已经绑定的远程USB设备
    
    ```
    usbip port
    
    ```
    
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/3688fcc9cc9a6aee1d74bc72f1634f5c.png)
    
*   将已经绑定的远程USB设备解绑：端口来自命令`usbip port`输出中"Port"后面的数字
    
    ```
    usbip detach -p [端口]
    
    ```
    

> 参考连接  
> [Linux 系统使用 USB/IP 远程共享 USB 设备](https://intl.cloud.tencent.com/zh/document/product/213/35722)  
> [USB/IP PROJECT](http://usbip.sourceforge.net/)  
> [Linux usb 5. usbip (USB Over IP) 使用实例](https://blog.csdn.net/pwl999/article/details/121039728)  
> [USB/IP for Windows](https://github.com/cezanne/usbip-win)

 

  

本文转自 [https://blog.csdn.net/OTZ\_2333/article/details/124073337](https://blog.csdn.net/OTZ_2333/article/details/124073337)，如有侵权，请联系删除。