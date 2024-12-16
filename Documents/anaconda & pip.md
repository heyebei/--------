## <span id="jump1">anaconda</span>

[Anaconda创建环境、删除环境、环境重命名、查看环境名](https://blog.csdn.net/better_boy/article/details/107240194?fromshare=blogdetail&sharetype=blogdetail&sharerId=107240194&sharerefer=PC&sharesource=qwer_221101&sharefrom=from_link)

[在指定目录下建立conda虚拟环境后发现没有环境名(激活失败)的解决办法](https://blog.csdn.net/qq_40968179/article/details/127584615?fromshare=blogdetail&sharetype=blogdetail&sharerId=127584615&sharerefer=PC&sharesource=qwer_221101&sharefrom=from_link)

### 查看环境：
```
conda info -e
conda env list
```
### 创建环境：
```
conda create -n python37 python=3.7
```
### 在指定目录下建立conda虚拟环境：
```
conda create --prefix C://Users/supery/anaconda3/envs/python37 python=3.7
```
这里我实验时候发现新建的环境是没有名称的，查询资料以及试验后发现我们需要给这个环境所在的目录的上一级目录添加到envs_dirs中具体操作如下：

查看一下当前的环境目录：
```
conda config --show envs_dirs
```
将我们的目录加进来：

这里一定要添加环境所在目录的**上一级目录！！！**
```
conda config --append envs_dirs your_path
```
最后查看一下我们的环境名称是否正确显示：
```
conda env list
```

### 激活环境：
```
conda activate python37
```
### 关闭环境：
```
conda deactivate
```
### 删除环境：
```
conda remove -n python37 --all
```

附：

*anaconda添加系统环境变量：*

```
C:\Users\supery\anaconda3\Scripts
C:\Users\supery\anaconda3\Library\bin
将上方两个文件夹添加到系统环境变量path中，即可使用cmd调用conda命令
上方路径为参考路径，请根据实际情况修改路径为你的实际路径
```


----

## <span id="jump1">pip命令</span>

### 使用requirement文件安装依赖包


```
pip install -r requirements.txt
```
### 生成requirement文件
```
pip freeze -r requirements.txt
此命令会在当前工作目录下生成requirements.txt文件
```
[生成requirements.txt文件可看此篇](https://blog.csdn.net/KingsMan666/article/details/133688711)
### 查看已安装的包

```
pip list
```
### 正常安装包命令

```
pip install package_name
安装固定版本包：
pip install package_name==version
```
### 查看包的版本

```
pip show package_name
```
### 卸载包

```
pip uninstall package_name
```
### 更新包

```
pip install --upgrade package_name
更新到固定版本：
pip install --upgrade package_name==version
```
### 查看pip版本

```
pip --version
```
### 查看python版本

```
python --version
```
### 查看python安装路径

```
where python
```
### 查看pip安装路径

```
where pip
```
----