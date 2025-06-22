 

本文介绍了python虚拟环境管理工具uv，包括uv的作用、uv的常用命令等等。

> 参考：[UV - 管理Python 版本、环境、第三方包](https://zhuanlan.zhihu.com/p/27452300746)

### [](https://blog.csdn.net/muxuen/article/details/147544307)1\. 介绍uv

官网：[https://docs.astral.sh/uv/](https://docs.astral.sh/uv/)

uv是一个python虚拟环境管理工具，可以用来替代pip、pyenv、virtualenv等等工具。根据官网的介绍，使用uv来管理虚拟环境，相比于pip能得到至少10倍以上的性能提升。

uv工具有如下功能：

*   管理python版本；
*   管理第三方库（Python packages）的版本；
*   拥有全局的第三方库的缓存，能减少磁盘空间占用；
*   安装uv不需要python环境，可以通过curl或pip安装；
*   多平台支持：macOS、Linux、Windows;

试用过后，感觉uv还是很不错的，于是编写本文，推荐给大家。

![image.png](https://i-blog.csdnimg.cn/img_convert/79595d46feb81f029709c3158c851469.webp?x-oss-process=image/format,png)

### [](https://blog.csdn.net/muxuen/article/details/147544307)2\. 安装uv

文档：[https://docs.astral.sh/uv/getting-started/installation/](https://docs.astral.sh/uv/getting-started/installation/)

在linux和mac上的安装直接使用curl或者wget命令即可

```
curl -LsSf https://astral.sh/uv/install.sh | sh
# 或者wget（效果一样）
wget -qO- https://astral.sh/uv/install.sh | sh

```

在windows上的安装命令如下

```
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

```

在mac上安装之后，终端输出如下

```
❯ curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.6.14 aarch64-apple-darwin
no checksums to verify
installing to /Users/mothra/.local/bin
  uv
  uvx
everything's installed!

To add $HOME/.local/bin to your PATH, either restart your shell or run:

    source $HOME/.local/bin/env (sh, bash, zsh)
    source $HOME/.local/bin/env.fish (fish)

```

这里给出了提示，必须把`$HOME/.local/bin`这个路径加到环境变量PATH里面才可以正常使用uv工具。设置之后，使用`uv --version`命令确认安装成功。

```
❯ uv --version
uv 0.6.14 (a4cec56dc 2025-04-09)

```

#### [](https://blog.csdn.net/muxuen/article/details/147544307)2.1. 卸载uv

卸载uv之前，先执行如下命令删除所有本地缓存文件

```
uv cache clean
rm -r "$(uv python dir)"
rm -r "$(uv tool dir)"

```

然后再删除uv的二进制文件

```
# mac/linux
rm ~/.local/bin/uv ~/.local/bin/uvx
# windows
rm $HOME.local\bin\uv.exe
rm $HOME.local\bin\uvx.exe

```

### [](https://blog.csdn.net/muxuen/article/details/147544307)3\. 基本使用

#### [](https://blog.csdn.net/muxuen/article/details/147544307)3.1. 管理python版本

使用如下命令，显示出当前环境中所有可用的python版本（包括已经安装的和可以安装的）

```
uv python list

```

在我的电脑上，输出如下，我的电脑上安装了python 3.9.6（xcode开发者工具安装的）、python 3.10.11、python 3.13.1版本，这几个版本都可以用uv来调用。

```
❯ uv python list
cpython-3.14.0a6-macos-aarch64-none                 <download available>
cpython-3.14.0a6+freethreaded-macos-aarch64-none    <download available>
cpython-3.13.3-macos-aarch64-none                   <download available>
cpython-3.13.3+freethreaded-macos-aarch64-none      <download available>
cpython-3.13.1-macos-aarch64-none                   /opt/homebrew/bin/python3.13 -> ../Cellar/python@3.13/3.13.1/bin/python3.13
cpython-3.13.1-macos-aarch64-none                   /opt/homebrew/bin/python3 -> ../Cellar/python@3.13/3.13.1/bin/python3
cpython-3.13.0a3-macos-aarch64-none                 /usr/local/bin/python3.13 -> ../../../Library/Frameworks/Python.framework/Versions/3.13/bin/python3.13
cpython-3.12.10-macos-aarch64-none                  <download available>
cpython-3.11.12-macos-aarch64-none                  <download available>
cpython-3.10.17-macos-aarch64-none                  <download available>
cpython-3.10.11-macos-aarch64-none                  /usr/local/bin/python3.10 -> ../../../Library/Frameworks/Python.framework/Versions/3.10/bin/python3.10
cpython-3.10.11-macos-aarch64-none                  /usr/local/bin/python3 -> ../../../Library/Frameworks/Python.framework/Versions/3.10/bin/python3
cpython-3.10.11-macos-aarch64-none                  /Library/Frameworks/Python.framework/Versions/3.10/bin/python3.10
cpython-3.10.11-macos-aarch64-none                  /Library/Frameworks/Python.framework/Versions/3.10/bin/python3 -> python3.10
cpython-3.9.22-macos-aarch64-none                   <download available>
cpython-3.9.6-macos-aarch64-none                    /usr/bin/python3
cpython-3.8.20-macos-aarch64-none                   <download available>

```

如果需要安装其他版本的python，使用如下命令

```
uv python install 3.12

```

除了标准python之外，还可以安装其他的Python实现，比如PyPy实现的python

```
uv python install pypy@3.10

```

查找某个python版本的路径

```
❯ uv python find 3.10
/Library/Frameworks/Python.framework/Versions/3.10/bin/python3.10

```

查看已经安装的python版本

```
uv python list

```

#### [](https://blog.csdn.net/muxuen/article/details/147544307)3.2. 选用python版本

在具体的某个项目中，进入项目目录，使用如下命令指定选用的python版本

```
uv python pin 版本号

```

这个命令会在指定目录下创建一个`.python-version`文件，内容如下

```
❯ uv python pin 3.10                                    
Pinned `.python-version` to `3.10`

❯ cat .python-version  
3.10

```

注意，这里选用的python版本只和uv管理的虚拟环境有关系，和我们全局的python、python3命令都没有关系。

#### [](https://blog.csdn.net/muxuen/article/details/147544307)3.3. 创建虚拟环境（项目）

创建项目有两种方式，第一种方式，先创建好项目目录，然后设置python版本并初始化uv虚拟环境

```
uv python pin 3.10
uv init # 初始化

```

执行了uv init之后，会在当前目录下创建几个文件，同时也会在当前目录下执行git init创建出一个新的git仓库来

```
~/data/code/python/test_code                                                                                                      
❯ uv python pin 3.13
Pinned `.python-version` to `3.13`

~/data/code/python/test_code                                                                                                      
❯ uv init           
Initialized project `test-code`

~/data/code/python/test_code main ?6                                                                                              
❯ ls
README.md      main.py        pyproject.toml

```

另外一个方式是在init之后添加一个项目名，会自动创建项目文件夹

```
uv init 项目名

```

如果需要指定特定python版本，建议使用第一种方式来创建项目，否则还需要手动修改pyproject.toml配置文件里面需要的python版本。

#### [](https://blog.csdn.net/muxuen/article/details/147544307)3.4. 添加依赖

```
uv add 依赖项

```

比如添加requests库

```
uv add requests

```

还可以指定具体版本

```
uv add requests==版本号

```

执行了这个命令后，会在当前目录下创建`.venv`虚拟环境目录（在vscode里面可以选择这个目录作为虚拟环境，否则代码解析会有问题），并添加我们要的依赖项，同时会新增一个uv.lock文件，用于存放依赖项版本相关的信息。pyproject.toml文件中的dependencies字段也会包含需要的依赖项。

```
~/data/code/python/test_code main ?6                                                                                              
❯ uv add requests
Using CPython 3.13.1 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Resolved 6 packages in 13.85s
Prepared 5 packages in 5.55s
Installed 5 packages in 13ms
 + certifi==2025.1.31
 + charset-normalizer==3.4.1
 + idna==3.10
 + requests==2.32.3
 + urllib3==2.4.0

```

而且，从这个输出中也能看到，它自动使用了`.python-version`指定的3.13版本的python，和当前我们全局目录下的python3指向什么版本没有关系（我的全局python3指向的是3.10版本）

#### [](https://blog.csdn.net/muxuen/article/details/147544307)3.5. 运行程序

依赖添加好后，就可以使用uv来运行python程序了

```
uv run 程序文件名 [命令行参数]

```

uv会自动按照我们的配置来运行程序，无序我们手动维护依赖项，也不需要手动去source各式各样的虚拟环境了。

#### [](https://blog.csdn.net/muxuen/article/details/147544307)3.6. uvx命令

随着uv下载的还有一个uvx命令，uvx命令本质上是uv tool run命令的别名，

```
uvx python main.py
# 等价于
uv run main.py
# 等价于
uv tool run main.py

```

实际例子，如下这两个命令是等价的

```
❯ uvx --directory ~/data/code/python/test_code python main.py
Hello from test-code!
    
❯ uv tool run --directory ~/data/code/python/test_code python main.py
Hello from test-code!

```

#### [](https://blog.csdn.net/muxuen/article/details/147544307)3.7. 小结

基本操作就是这些了，更多复杂的操作详见uv的官网。

### [](https://blog.csdn.net/muxuen/article/details/147544307)4\. 设置下载包的镜像源

> 参考：[https://blog.csdn.net/qq\_41472205/article/details/145686414](https://blog.csdn.net/qq_41472205/article/details/145686414)

uv下载第三方库本质上也是通过pypi源下载的，所以在国内网络环境中默认链接速度会很慢，可以在项目目录的pyproject.toml中添加如下内容来使用清华源

```
[[tool.uv.index]]
url = "https://pypi.tuna.tsinghua.edu.cn/simple"
default = true

```

运行uv add命令的时候也可以指定镜像源

```
uv add --default-index https://pypi.tuna.tsinghua.edu.cn/simple requests

```

uv也提供了全局的配置项，可以通过环境变量`UV_DEFAULT_INDEX`配置镜像源

```
export UV_DEFAULT_INDEX=https://pypi.tuna.tsinghua.edu.cn/simple

```

全局的配置项优先级低于pyproject.toml中配置的镜像源。如果pyproject.toml里面配置了镜像源，则会使用pyproject.toml的配置。

### [](https://blog.csdn.net/muxuen/article/details/147544307)5\. 大模型mcp协议和uv

对于mcp而言，mcp server的开发可以使用python来编写，此时python环境的管理就非常重要了。

以常见的mcp客户端配置举例，示例配置如下，其中test.py是我们编写的一个mcp服务器。

```
{
	"mcpServers": {
		"工具名称": {
			"command": "uv",
			"args": {
				"--directory",
				"工作路径",
				"run",
				"test.py"
			}
		}
	}
}

```

下图是mcp server和mcp client交互的简要逻辑（这只是个简要的流程）

```
graph TD;
	A[ai工具加载mcp配置] --> |启动mcp服务器|B
	B[mcp client] --> |链接服务器，调用工具|C[mcp server] 
	C -->|返回工具调用结果|B

```

其中第一步，ai工具加载mcp配置的时候，就需要去根据我们填写的mcpSever的配置来通过uv启动我们的服务端了，此时如果还是用python自带的venv来管理虚拟环境就不够用了，因为这里没有办法指定虚拟环境的路径，也没有人去`source venv/bin/activate`那个虚拟环境，所以mcp需要一个解决方案来更好的管理python的虚拟环境，此时uv就登场了。

使用了uv，直接执行uv run就可以了，uv工具自动帮我们维护了虚拟环境，并使用了指定环境来运行我们的服务端代码，一切问题都解决啦！

而且，mcp强制使用uv，也进一步规范了使用python编写mcp server的格式，再也不用担心某些开源python项目的根目录下连requirements.txt都没有的尴尬情况了。


本文转自 [https://blog.csdn.net/muxuen/article/details/147544307](https://blog.csdn.net/muxuen/article/details/147544307)，如有侵权，请联系删除。