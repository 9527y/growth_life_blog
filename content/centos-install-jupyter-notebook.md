---
title: CentOS 安装 Jupyter Notebook
categories:
  - 开发
tags:
  - python
  - jupyter
  - notebook
date: 2020-10-12 16:31:40.483000
---
目录[toc]
## 安装依赖
```
yum -y groupinstall "Development Tools"
yum -y install python-devel
```
## 使用虚拟环境
```
pip install virtualenv
virtualenv pyenv
source pyenv/bin/activate
```

## Jupyter 安装 & 配置
- 安装
```
pip install jupyter
```
- 目录准备

我们先为 Jupyter 相关文件准备一个目录：
```
mkdir /data/jupytercd /data/jupyter
```
再建立一个目录作为 Jupyter 运行的根目录：
```
mkdir /data/jupyter/root
```
- 密码准备

执行以下命令:

Python2.x
```
python -c "import IPython;print IPython.lib.passwd()"
```

Python3.x
```
python -c "import IPython;print(IPython.lib.passwd())"
```
- 生成配置
```
jupyter notebook --generate-config --allow-root
```
- 修改配置
```
vi /root/.jupyter/jupyter_notebook_config.py
```
增加如下配置
```
c.NotebookApp.allow_root = True
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888
c.NotebookApp.ip = "*"
c.NotebookApp.password = u'sha1:363dfc3e77be:e214195be210befc7cb0b6cf141f9ef7a2d970c8’
c.ContentsManager.root_dir = '/data/jupyter/root'
```
`c.NotebookApp.password`的内容换成刚才生成的内容。

## 启动 jupyter
```
jupyter notebook
```

- 指定ip可以使用
```
jupyter notebook --ip='172.16.8.110'
```
如果遇见编码问题，可以使用
```
LANG=zn jupyter notebook --ip='172.16.8.110'
```

## 附
如果报错`Python3报错ImportError: No module named pysqlite2`：则需要安装 `sqlite-devel`:

```shell
yum install sqlite-devel -y
```
然后重新编译Python
```
tar -zxvf  Python-3.6.5.tgz
cd Python-3.6.5
./configure --prefix=/usr/local/python3
make && make install
```