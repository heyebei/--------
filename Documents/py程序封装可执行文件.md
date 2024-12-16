# <span id="jump1">windows封装py程序 </span>

```
pyinstaller -F-w 文件名称
```

### 生成Windows版可执行文件.exe
1. 安装pyinstaller
```
pip3 install pyinstaller
```
2. 进入.py脚本文件所在目录执行打包命令
```
pyinstaller -F ***.py
```
3. 若要指定项目的依赖包时，可以加-p参数即指定依赖包的路径。
```
pyinstaller -F ***.py -p /Users/supery/venv/lib/python3.7/site-packages
```
**执行完成后可执行文件在dist包中**

4. 上面的命令执行起来会出现cmd控制台，若要去掉需要添加参数 
```
--noconsole
```
详细参数可看pyinstaller --help：
```
D:\admin\Desktop\Vscode\python\demo>pyinstaller
usage: pyinstaller [-h] [-v] [-D] [-F] [--specpath DIR] [-n NAME] [--contents-directory CONTENTS_DIRECTORY]
                   [--add-data SOURCE:DEST] [--add-binary SOURCE:DEST] [-p DIR] [--hidden-import MODULENAME]
                   [--collect-submodules MODULENAME] [--collect-data MODULENAME] [--collect-binaries MODULENAME]
                   [--collect-all MODULENAME] [--copy-metadata PACKAGENAME] [--recursive-copy-metadata PACKAGENAME]
                   [--additional-hooks-dir HOOKSPATH] [--runtime-hook RUNTIME_HOOKS] [--exclude-module EXCLUDES]
                   [--splash IMAGE_FILE] [-d {all,imports,bootloader,noarchive}] [--python-option PYTHON_OPTION] [-s]
                   [--noupx] [--upx-exclude FILE] [-c] [-w]
                   [--hide-console {minimize-late,minimize-early,hide-early,hide-late}]
                   [-i <FILE.ico or FILE.exe,ID or FILE.icns or Image or "NONE">] [--disable-windowed-traceback]
                   [--version-file FILE] [-m <FILE or XML>] [-r RESOURCE] [--uac-admin] [--uac-uiaccess]
                   [--argv-emulation] [--osx-bundle-identifier BUNDLE_IDENTIFIER] [--target-architecture ARCH]
                   [--codesign-identity IDENTITY] [--osx-entitlements-file FILENAME] [--runtime-tmpdir PATH]
                   [--bootloader-ignore-signals] [--distpath DIR] [--workpath WORKPATH] [-y] [--upx-dir UPX_DIR]
                   [--clean] [--log-level LEVEL]
                   scriptname [scriptname ...]
pyinstaller: error: the following arguments are required: scriptname
```
### eg封装代码
```
pyinstaller -F ***.py -p /Users/supery/venv/lib/python3.7/site-packages --noconsole
```


----

# <span id="jump1">mac封装py程序</span>

1. 安装py2app
```
pip3 install py2app
```
2. 进入到需要转换的.py脚本文件所在目录执行如下命令
```
er ***.py
```
3. 执行上面的命令后会生成build和dist目录，所以为了避免缓存造成影响，在执行上面命令之前先删除这两个文件
```
rm -rf build dist
```
4. 最后执行下面命令
```
python setup.py py2app
```


----

## <span id="jump1">linux-python-for-android</span>

### Buildozer

Buildozer 是一个将整个构建过程自动化的工具。它会下载和设置 python-for-android 需要的所有依赖项目，包括 Android 的 SDK 和 NDK，然后会构建 APK ，这个 APK 可以自动推送到设备上。

目前 Buildozer 只能用在 Linux 上面，而且还不是正式版，处于测试阶段，发布的是 alpha 版本，不过目前用起来还不错，能显著简化 APK 构建的过程。

可以到 Buildozer 的项目页面 下载获取 Buildozer。
```
git clone https://github.com/kivy/buildozer.git
cd buildozer
sudo python2.7 setup.py install
```
上面的命令就会把 Buildozer 安装到你的操作系统中。接下来就是到你的项目目录然后运行如下命令：
```
buildozer init
```
这会在你的目录下创建一个名为 buildozer.spec 的文件，这个文件是控制项目构建选项的。估计你需要编辑修改一下这个文件，比如设置你应用的名字等等。在这里可以设置传递给 python-for-android 的全部或者大部分参数。

安装 Buildozer 的依赖项目。

最后一步了，连接上你的 Android 设备然后运行下面的命令：
```
buildozer android debug deploy run
```
这样就可以创建、推送 APK 到你的设备上，然后就可以自动运行了。
Buildozer 有很多可以控制的选项和工具，对你都会游泳，上面这些步骤只是创建 APK 的最简单的方法。可以到 Buildozer 的官方文档页面查看完整说明。也可以看看 Buildozer 项目页面的 README 文件。

### 示例2：
步骤 1: 安装 Buildozer 和其依赖
首先确保你的系统中已安装 Python 和 pip。接下来，我们需要安装 Buildozer 以及一些必要的系统依赖。

Linux (Ubuntu) 环境下安装:
安装 Python 和 pip（如果尚未安装）:
```
sudo apt update
sudo apt install python3 python3-pip
```
安装 Buildozer 依赖:
```
sudo apt install -y git zip unzip openjdk-8-jdk python3-kivy
```
安装 Buildozer:
```
pip3 install buildozer
```
安装 Android 的命令行工具:
```
sudo apt install -y autoconf automake libtool pkg-config
```
步骤 2: 创建你的 Python 应用
使用 Kivy 库创建一个简单的图形用户界面应用。这是一个示例 Python 脚本，显示一个按钮。

创建文件 main.py:
```
from kivy.app import App
from kivy.uix.button import Button

class MyApp(App):
    def build(self):
        return Button(text='Hello, World!')

if __name__ == '__main__':
    MyApp().run()
```
步骤 3: 配置 Buildozer
在你的项目文件夹中（包含 main.py 的文件夹），初始化 Buildozer 配置文件。

初始化 Buildozer:
```
buildozer init
```
编辑 buildozer.spec 文件，修改以下关键字段以适配你的应用:
```
title：应用名称
package.name：应用包名
package.domain：应用域名
source.include_exts：包括的文件扩展名，如 py,png,jpg,kv,atlas
requirements：指定依赖，如 python3,kivy
```
步骤 4: 打包成 APK
在项目目录下运行 Buildozer，开始构建 APK:
```
buildozer -v android debug
```
这个命令会处理很多事情：下载 Android SDK 和 NDK，创建一个新的虚拟环境，安装你的 Python 代码和依赖库，最后编译并打包成一个 APK 文件。

将 APK 安装到设备:

确保 Android 设备已连接到你的计算机并开启 USB 调试模式。
使用以下命令部署 APK：
```
buildozer android deploy run
```
总结:

遵循以上步骤，你可以将一个基于 Kivy 库的 Python 应用打包成一个 APK 文件，并在 Android 设备上运行。

----

