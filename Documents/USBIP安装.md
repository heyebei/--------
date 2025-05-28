usbip-win下载地址  
[https://github.com/cezanne/usbip-win/releases](https://github.com/cezanne/usbip-win/releases)

### 更新：

`vhci(wdm or ude) drivers have been certified via MS attestation sign.`  
usbip-win 0.3.6版本，UDE驱动虚拟的ROOT-HUB已经有微软的签名(没有USBIP的交叉签名，文件属性只能看到微软的证书)，通过命令行可以直接安装，后续使用也无需进测试模式。  
![image](https://img2023.cnblogs.com/blog/1928719/202308/1928719-20230819102748340-1554096346.png)

目前最新的发行版是0.3.5，需要下载预编译可执行文件`usbip-win-0.3.5.zip`和源码`Source code (zip)`  
![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514122507796-1119326161.png)

操作过程和源码压缩包内的`README`文件一致，这里给出win10系统下的安装流程演示。

0.  在高级重启选项里，禁用windows驱动签名验证重启，具体步骤请从网上搜索。  
    ![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514123718534-962399029.png)
    
1.  解压`usbip-win-0.3.5.zip`，得到以下文件  
    ![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514122801729-1504699291.png)
    
2.  安装驱动测试证书，双击`usbip_test.pfx`, 安装路径选择`Local Machine`, 证书的密码是`usbip`, 证书存储目录选择`Trusted Root Certification Authority`, 点击`Finish`完成证书导入。  
    ![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514122952657-1551624453.png)  
    ![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514123238570-739324901.png)  
    ![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514123302183-2047885817.png)  
    ![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514123419277-1304943082.png)  
    win+R运行`certlm.msc`,查看证书安装情况。  
    ![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514131341327-1940318330.png)
    
3.  在usbip目录命令行运行`usbip install`安装内核态驱动(我这里已经安装过，会提示驱动已经存在)。  
    ![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514124028904-1861378497.png)
    
4.  进入windows设备管理器查看，会出现两个Root Hub设备，一个是UBS2.0另一个是USB3.0  
    ![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514124226663-635719580.png)  
    ![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514124230045-392720618.png)
    

使用USB Device Tree Viewer也可以看到新安装的Root Hub设备。  
![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514124438042-1634387131.png)

5.  由于关闭驱动签名验证只在本次启动有效，在下次重启时会自动打开，导致`usb-win VHCI`设备报52号错误，提示信息为驱动签名验证失败，这里还需要打开windows测试模式。管理员模式打开命令行，输入`bcdedit /set TESTSIGNING ON`进入测试模式。  
    ![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514124916317-984374219.png)  
    下次重启时，未签名驱动被正确加载，桌面右下角会提示`Test Mode`。  
    ![image](https://img2023.cnblogs.com/blog/1928719/202305/1928719-20230514125035174-692915910.png)

  

本文转自 [https://www.cnblogs.com/yanye0xcc/p/17399102.html#:~:text=usbip-win%200.3.x%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97%201%20%E5%9C%A8%E9%AB%98%E7%BA%A7%E9%87%8D%E5%90%AF%E9%80%89%E9%A1%B9%E9%87%8C%EF%BC%8C%E7%A6%81%E7%94%A8windows%E9%A9%B1%E5%8A%A8%E7%AD%BE%E5%90%8D%E9%AA%8C%E8%AF%81%E9%87%8D%E5%90%AF%EF%BC%8C%E5%85%B7%E4%BD%93%E6%AD%A5%E9%AA%A4%E8%AF%B7%E4%BB%8E%E7%BD%91%E4%B8%8A%E6%90%9C%E7%B4%A2%E3%80%82%202%20%E8%A7%A3%E5%8E%8B%20usbip-win-0.3.5.zip%EF%BC%8C%E5%BE%97%E5%88%B0%E4%BB%A5%E4%B8%8B%E6%96%87%E4%BB%B6%203,%E5%9C%A8usbip%E7%9B%AE%E5%BD%95%E5%91%BD%E4%BB%A4%E8%A1%8C%E8%BF%90%E8%A1%8C%20usbip%20install%20%E5%AE%89%E8%A3%85%E5%86%85%E6%A0%B8%E6%80%81%E9%A9%B1%E5%8A%A8%20%28%E6%88%91%E8%BF%99%E9%87%8C%E5%B7%B2%E7%BB%8F%E5%AE%89%E8%A3%85%E8%BF%87%EF%BC%8C%E4%BC%9A%E6%8F%90%E7%A4%BA%E9%A9%B1%E5%8A%A8%E5%B7%B2%E7%BB%8F%E5%AD%98%E5%9C%A8%29%E3%80%82%205%20%E8%BF%9B%E5%85%A5windows%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86%E5%99%A8%E6%9F%A5%E7%9C%8B%EF%BC%8C%E4%BC%9A%E5%87%BA%E7%8E%B0%E4%B8%A4%E4%B8%AARoot%20Hub%E8%AE%BE%E5%A4%87%EF%BC%8C%E4%B8%80%E4%B8%AA%E6%98%AFUBS2.0%E5%8F%A6%E4%B8%80%E4%B8%AA%E6%98%AFUSB3.0](https://www.cnblogs.com/yanye0xcc/p/17399102.html#:~:text=usbip-win%200.3.x%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97%201%20%E5%9C%A8%E9%AB%98%E7%BA%A7%E9%87%8D%E5%90%AF%E9%80%89%E9%A1%B9%E9%87%8C%EF%BC%8C%E7%A6%81%E7%94%A8windows%E9%A9%B1%E5%8A%A8%E7%AD%BE%E5%90%8D%E9%AA%8C%E8%AF%81%E9%87%8D%E5%90%AF%EF%BC%8C%E5%85%B7%E4%BD%93%E6%AD%A5%E9%AA%A4%E8%AF%B7%E4%BB%8E%E7%BD%91%E4%B8%8A%E6%90%9C%E7%B4%A2%E3%80%82%202%20%E8%A7%A3%E5%8E%8B%20usbip-win-0.3.5.zip%EF%BC%8C%E5%BE%97%E5%88%B0%E4%BB%A5%E4%B8%8B%E6%96%87%E4%BB%B6%203,%E5%9C%A8usbip%E7%9B%AE%E5%BD%95%E5%91%BD%E4%BB%A4%E8%A1%8C%E8%BF%90%E8%A1%8C%20usbip%20install%20%E5%AE%89%E8%A3%85%E5%86%85%E6%A0%B8%E6%80%81%E9%A9%B1%E5%8A%A8%20%28%E6%88%91%E8%BF%99%E9%87%8C%E5%B7%B2%E7%BB%8F%E5%AE%89%E8%A3%85%E8%BF%87%EF%BC%8C%E4%BC%9A%E6%8F%90%E7%A4%BA%E9%A9%B1%E5%8A%A8%E5%B7%B2%E7%BB%8F%E5%AD%98%E5%9C%A8%29%E3%80%82%205%20%E8%BF%9B%E5%85%A5windows%E8%AE%BE%E5%A4%87%E7%AE%A1%E7%90%86%E5%99%A8%E6%9F%A5%E7%9C%8B%EF%BC%8C%E4%BC%9A%E5%87%BA%E7%8E%B0%E4%B8%A4%E4%B8%AARoot%20Hub%E8%AE%BE%E5%A4%87%EF%BC%8C%E4%B8%80%E4%B8%AA%E6%98%AFUBS2.0%E5%8F%A6%E4%B8%80%E4%B8%AA%E6%98%AFUSB3.0)，如有侵权，请联系删除。