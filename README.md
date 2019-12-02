# 备忘

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
