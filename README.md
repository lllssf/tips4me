# 备忘
<!-- TOC -->

- [服务器](#%e6%9c%8d%e5%8a%a1%e5%99%a8)
  - [在远程服务器搭建jupyter notebook](#%e5%9c%a8%e8%bf%9c%e7%a8%8b%e6%9c%8d%e5%8a%a1%e5%99%a8%e6%90%ad%e5%bb%bajupyter-notebook)
- [Python](#python)
  - [图像](#%e5%9b%be%e5%83%8f)
- [PyTorch](#pytorch)
  - [制作/读取自己的数据集](#%e5%88%b6%e4%bd%9c%e8%af%bb%e5%8f%96%e8%87%aa%e5%b7%b1%e7%9a%84%e6%95%b0%e6%8d%ae%e9%9b%86)
- [Linux](#linux)
- [VScode](#vscode)

<!-- /TOC -->
## 服务器
1. 指定端口连接服务器: `ssh -p 端口 root@ip`
2. 指定端口传输压缩文件：`scp -P 端口 -p file.tar.gz root@ip:files`
### 在远程服务器搭建jupyter notebook
1. 安装jupyter notebook
```
pip install jupyter notebook
```
2. 创建配置文件
```
jupyter notebook --generate-config
```
3. 生成密码
```
jupyter notebook password
```
打开密码的json文件，复制密钥

4. 修改配置文件
```
vim ~/.jupyter/jupyter_notebook_config.py
```
通过`/`查找字符串找到并修改以下内容
```
c.NotebookApp.ip='*' #允许访问的IP地址，设置为*代表允许任何客户端访问
c.NotebookApp.password = u'sha1:8d...刚才生成密码时复制的密文'
c.NotebookApp.open_browser = False
c.NotebookApp.port =8888 #可自行指定一个端口, 访问时使用该端口
c.NotebookApp.allow_remote_access = True
```
5. 服务器端启动jupyter notebook
```
jupyter notebook --allow-root
```
6. 解决无法远程访问的问题，通过xshell工具修改会话`属性-ssh-隧道`，点击`添加`，将侦听端口设为8000，服务器端口默认为8888，在本地输入`localhost:8000`就可以访问服务器jupyter notebook了。
7. 将jupyter notebook代码转为`.py`脚本
```
jupyter nbconvert --to script name.ipynb
```
8. 在服务器创建会话后台运行持续运行jupyter notebook
```
screen -S name
```
通过快捷键`Ctrl+a+d`detach这个会话回到之前的窗口，也可以创建会话来跑程序。
```
# 恢复会话
screen -r name
```
通过`Ctrl+c`停止程序。
```
# 清除后台
screen -X -S name quit
```
## Python
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
   
## PyTorch
### 制作/读取自己的数据集
1. 制作存储了图片的路径和标签信息的txt
2. 将这些信息转化为list，该list每个元素对应一个样本
3. 构建Dataset子类，通过getitem函数，读取数据和标签，并返回数据和标签

## Linux 
参见[Linux基本](https://github.com/lllssf/tips4me/blob/master/Linux%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C.md)

## VScode
参见[VScode 快捷键](https://github.com/lllssf/tips4me/blob/master/VScode%E5%BF%AB%E6%8D%B7%E9%94%AE.md)
