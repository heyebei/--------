 

**目录**

[查看当前User和Email配置](about:blank#%E6%9F%A5%E7%9C%8B%E5%BD%93%E5%89%8DUser%E5%92%8CEmail%E9%85%8D%E7%BD%AE)

[配置](about:blank#%E9%85%8D%E7%BD%AE)

* * *

#### **查看当前User和Email配置**

`git config --local --list` 

`git config --list` 

#### **配置**

**​​​​​​​** local   仓库级别 、global 用户级别

法一：使用命令修改git的用户名和提交的邮箱

 命令分别为：

设置用户名和邮箱

git config --global user.name "username"

git config --global user.email  useremail@qq.com

查看是否设置成功

git config user.name

git config user.email

在git中，我们使用git config 命令用来配置git的配置文件，git配置级别主要有以下3类：

1、 local   仓库级别

2、 global 用户级别，当前用户在所有仓库都用这个配置（C:\\Users\\XiaoRui\\.gitconfig）

3、 system 系统级别，整台计算机都用这个配置（D:\\Program Files\\Git\\etc\\gitconfig）

**优先级**

git config （git config  --local ） > git config --global > git config --system。

[git config和带--global、--system的区别https://www.daixiaorui.com/read/240.html](https://www.daixiaorui.com/read/240.html "git config和带--global、--system的区别https://www.daixiaorui.com/read/240.html")

       **![](https://i-blog.csdnimg.cn/blog_migrate/2e5a0e37cf22ef945d1ec933d836f222.jpeg)**

        2）修改**当前的 project**

        git 修改当前的project的用户名的命令为：**git config user.name 你的目标用户名;**

        git 修改当前的project提交邮箱的命令为：**git config user.email 你的目标邮箱名;**

法二：直接修改git的配置文件的方式来进行修改

       1） 打开**全局**的.gitconfig文件的命令为：**$  vi ~/.gitconfig;**   然后在文件中直接修改即可.

                打开文件后如下图所示：

                ![](https://i-blog.csdnimg.cn/blog_migrate/d925cc1722088af2a95772b85bf42f0a.jpeg)     

          2） 打开**当前project** 中的config文件，该文件在每个project中的.git目录下，直接进入该目录进行编辑即可。当然，**如果没有进行过修改的话，默认打开时没有对应的用户名和密码的。**只有进行过修改之后，才会在config中产生对应字段。

参考：[Git 修改用户名以及提交邮箱 - CuriousZero - 博客园](https://www.cnblogs.com/shenxiaolin/p/7896489.html "Git 修改用户名以及提交邮箱 - CuriousZero - 博客园")

#### git 的配置文件在哪

1.  **全局配置**

例如用户名，邮箱等，位于文件：

> ~/.gitconfig

例如Windows下，则是

> C:\\Users\\lanyang\\.gitconfig

[](https://blog.csdn.net/bandaoyu/article/details/112968504)

1.  **各个仓库的配置**

在仓库目录下

> .git/config

例如orange仓库目录下

> /home/lanyang/orange/.git/config

以上的配置都可以通过命令查看

```
$ git config –list 
```

  

本文转自 [https://blog.csdn.net/bandaoyu/article/details/112968504](https://blog.csdn.net/bandaoyu/article/details/112968504)，如有侵权，请联系删除。