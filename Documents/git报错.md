# fatal....Recv failure Connection was reset报错



在使用 Git 进行代码管理的过程中，经常会遇到各种各样的问题，其中之一就是在执行 `git clone` 或 `git pull` 等操作时出现 “fatal: unable to access ‘https://github.com/…/.git’: Recv failure Connection was reset” 的报错。
#### 文章目录

*   [方法一：取消代理设置](about:blank#_18)
*   [方法二：设置系统代理](about:blank#_30)

#### [](https://blog.csdn.net/qq_43546721/article/details/139506583)方法一：取消代理设置

这是最常见的解决方法之一，通过在终端执行以下命令，可以取消 Git 的代理设置：

```
git config --global --unset http.proxy 
git config --global --unset https.proxy

```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/44ba7a47fce6e309aa74ac61b05bb120.png)

这样就可以清除 Git 的代理设置，让其直接连接网络进行操作。

#### [](https://blog.csdn.net/qq_43546721/article/details/139506583)方法二：设置系统代理

有时候取消代理设置仍然会出现报错，这时可以通过设置系统代理来解决。具体步骤如下：

1.  打开系统设置，搜索代理设置，并点击编辑按钮。  
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/0bca1de058a6575d02df7cfaac2add77.png)
    
2.  在代理服务器中，将端口设置为7890（这个端口不会影响正常上网，可以放心设置），然后点击保存。  
    ![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d771d4ba75008d4b72cc612a4e4afbdf.png)
    
3.  在终端输入以下命令，设置 Git 使用本地代理：
    

```
git config --global http.proxy http://127.0.0.1:7890

```

设置完成后，可以通过以下命令检验是否设置成功：

```
git config --global -l

```