# 备忘
<!-- TOC -->

- [服务器](#服务器)
    - [在远程服务器搭建jupyter notebook](#在远程服务器搭建jupyter-notebook)
    - [在本地查看远程visdom](#在本地查看远程visdom)
- [Python](#python)
    - [零碎知识](#零碎知识)
    - [图像](#图像)
    - [常用函数](#常用函数)
    - [numpy](#numpy)
- [PyTorch](#pytorch)
    - [PyTorch基础教程](#pytorch基础教程)
    - [制作/读取自己的数据集](#制作读取自己的数据集)
- [Linux](#linux)
    - [基础](#基础)
    - [日常用到的命令](#日常用到的命令)
- [VScode](#vscode)
- [Markdown](#markdown)
    - [数学公式](#数学公式)
        - [戴帽符号](#戴帽符号)
        - [字体转换](#字体转换)
- [其他](#其他)

<!-- /TOC -->
## 服务器

1. 指定端口连接服务器: `ssh -p 端口 root@ip`
2. 指定端口传输压缩文件：`scp -P 端口 -p file.tar.gz root@ip:files`
3. 指定端口传输文件夹：`scp -P 端口 -r file root@ip:files`

### 在远程服务器搭建jupyter notebook

1. 安装jupyter notebook

```shell
pip install jupyter notebook
```

2. 创建配置文件

```shell
jupyter notebook --generate-config
```

3. 生成密码

```shell
jupyter notebook password
```

- 打开密码的json文件，复制密钥

4. 修改配置文件

```shell
vim ~/.jupyter/jupyter_notebook_config.py
```

- 通过`/`查找字符串找到并修改以下内容

```shell
c.NotebookApp.ip='*' #允许访问的IP地址，设置为*代表允许任何客户端访问
c.NotebookApp.password = u'sha1:8d...刚才生成密码时复制的密文'
c.NotebookApp.open_browser = False
c.NotebookApp.port =8888 #可自行指定一个端口, 访问时使用该端口
c.NotebookApp.allow_remote_access = True
```

5. 服务器端启动jupyter notebook

```shell
jupyter notebook --allow-root
```

6. 解决无法远程访问的问题，通过xshell工具修改会话`属性-ssh-隧道`，点击`添加`，将侦听端口设为8000，服务器端口默认为8888，在本地输入`localhost:8000`就可以访问服务器jupyter notebook了。
7. 将jupyter notebook代码转为`.py`脚本

```shell
jupyter nbconvert --to script name.ipynb
```

8. 在服务器创建会话后台运行持续运行jupyter notebook

```shell
screen -S name
```

- 通过快捷键`Ctrl+a+d`detach这个会话回到之前的窗口，也可以创建会话来跑程序。

```shell
# 恢复会话
screen -r name
```

- 通过`Ctrl+c`停止程序。

```shell
# 清除后台
screen -X -S name quit
```

### 在本地查看远程visdom

Visdom是支持torch和numpy的可视化工具。

1. 服务器端安装visdom
   ```shell
   pip install visdom
   ```
2. 将服务器的8097端口重定向到自己机器上来
   ```shell
   ssh -L 18097:127.0.0.1:8097 username@remote_server_ip
   ```
3. 服务器端启动visdom：
   ```shell
   python -m visdom.server
   ```
4. 通过xshell工具修改会话`属性-ssh-隧道`，点击`添加`，将侦听端口设为8097，服务器端口为8097。
5. 在本地浏览器输入地址`http://localhost:8097/`即可查看visdom

## Python

### 零碎知识

当时为面试积累的一些[python零碎知识](https://github.com/lllssf/Fight-for-offer/blob/master/%E9%9D%A2%E8%AF%95%E7%BC%96%E7%A8%8B%E7%AE%97%E6%B3%95/python%E9%9B%B6%E7%A2%8E.md)

### 图像

1. 查看图像通道数

   ```python
   print(len(img.split())
   ```

2. 图片格式转换：png图，RBGA（透明度）四通道转换为RBG三通道：

   ```python
   from PIL import Image
   img = Image.open(png_path)
   img = img.convert('RGB')
   ```

3. 利用`numpy.transpose`转换图像序列：在做图像处理后图片格式变为(3,x,y)，而`plt.imshow()`能绘制的格式为(x,y,3)，所以需要：

   ```python
   img.transpose(1,2,0)
   ```

### 常用函数

1. 格式化输出之`format`用法：

   ```python
   # 取位数
   print('{:4s}, {:.2f}'.format(string,float))
   ```

### numpy

1. 增：`np.vstack((a,b))`纵向扩展，`np.hstack((a,b))`横向扩展。


## PyTorch

### PyTorch基础教程

学习的一些[PyTorch基础教程](https://github.com/lllssf/NN-implemantation/blob/master/torch_tutor.ipynb)

### 制作/读取自己的数据集

1. 制作存储了图片的路径和标签信息的txt
2. 将这些信息转化为list，该list每个元素对应一个样本
3. 构建Dataset子类，通过getitem函数，读取数据和标签，并返回数据和标签

## Linux 

### 基础

参见[Linux基本](https://github.com/lllssf/tips4me/blob/master/Linux%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C.md)

### 日常用到的命令

1. 统计文件数目：

   ```shell
   # 当前目录下所有文件数目
   ls -l | grep '^-' | wc -l
   # 当前目录及其所有子目录下文件数目：
   ls -lR | grep '^-' | wc -l
   # 当前目录及所有子目录下所有文件夹数目：
   ls -lR | grep '^d' | wc -l
   # 当前目录及所有子目录下所有.png文件数目：
   ls -lR | grep '.png' | wc -l
   ```

2. 查看端口占用情况

   ```shell
   lsof -i:<port>
   # 杀死某个端口占用进程
   kill -s 9 <进程ID>
   ```

## VScode

参见[VScode 快捷键](https://github.com/lllssf/tips4me/blob/master/VScode%E5%BF%AB%E6%8D%B7%E9%94%AE.md)

## Markdown

Github中Markdown公式显示方法：Chrome安装 [MathJax](https://chrome.google.com/webstore/detail/mathjax-plugin-for-github/ioemnmodlmafdkllaclgeombjnmnbima/related) 插件。

### 数学公式

- `$公式$` 是行中公式
- `$$公式$$` 是独立公式，独立成行居中显示。
- `\sum_{下标表达式}^{上标表达式} {累加表达式}` 累加符号

#### 戴帽符号

格式|显示|格式|显示
:-:|:-:|:-:|:-:
\hat|$\hat{x}$|\widehat|$\widehat{xyz}$
\tilde|$\tilde{x}$|\widetilde|$\widetilde{xyz}$
\check|$\check{x}$|\widecheck|$\widecheck{xyz}$

#### 字体转换

可以使用 `${\字体{内容}}$` 格式进行字体转换，一般情况下，公式默认为意大利体。

格式|字体|样例
:-:|:-:|:-:
\rm|罗马体|${\rm{X,x}}$
\mathcal|花体|${\mathcal{X,x}}$
\it|意大利体|${\it{X,x}}$
\Bbb|黑板粗体|${\Bbb{X,x}}$
\bf|粗体|${\bf{X,x}}$
\mathit|数学斜体|${\mathit{X,x}}$
\sf|等线体|${\sf{X,x}}$
\scriptstyle|手写体|${\scriptstyle{X,x}}$
\tt|打字机体|${\tt{X,x}}$
\frak|旧德式字体|${\frak{X,x}}$
\blodsymbol|黑体|${\boldsymbol{X,x}}$

## 其他

1. 下载Github上某一仓库下的某一文件可以使用[DownGit](https://www.itsvse.com/downgit/#/home)网站
