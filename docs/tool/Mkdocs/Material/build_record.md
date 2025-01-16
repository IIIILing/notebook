---
title: 记录一下自己在LINUX上配置MKDOCS和MATERIAL的过程
statistics: True
---
## 安装MKDOCS

先克隆仓库，然后想要MKDOCS SERVE发现没有MKDOCS

```bash
iiiling@iiiiling-server:~/Desktop/notebook$ mkdocs serve
Command 'mkdocs' not found, but can be installed with:
sudo apt install mkdocs
iiiiling@iiiiling-server:~/Desktop/notebook$ sudo apt install mkdocs
[sudo] password for iiiiling: 
Reading package lists... Done
```
然后去安装mkdocs

再发现自己没有material
```bash
iiiiling@iiiiling-server:~/Desktop/notebook$ mkdocs serve
Error: MkDocs encountered an error parsing the configuration file: while constructing a Python object
cannot find module 'material.extensions.emoji' (No module named 'material')
  in "/home/iiiiling/Desktop/notebook/mkdocs.yml", line 174, column 20
```
安装material需要pip install

所以还需要下载python
```bash
iiiiling@iiiiling-server:~/Desktop/notebook$ pip install mkdocs-material
Command 'pip' not found, but can be installed with:
sudo apt install python3-pip
iiiiling@iiiiling-server:~/Desktop/notebook$ sudo apt install python3-pip

```
下载完成之后又pip install一下

之后发现
```bash
iiiiling@iiiiling-server:~/Desktop/notebook$ pip install mkdocs-material
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try apt install
    python3-xyz, where xyz is the package you are trying to
    install.
    
    If you wish to install a non-Debian-packaged Python package,
    create a virtual environment using python3 -m venv path/to/venv.
    Then use path/to/venv/bin/python and path/to/venv/bin/pip. Make
    sure you have python3-full installed.
    
    If you wish to install a non-Debian packaged Python application,
    it may be easiest to use pipx install xyz, which will manage a
    virtual environment for you. Make sure you have pipx installed.
    
    See /usr/share/doc/python3.12/README.venv for more information.

note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.

```

需要安装虚拟环境

我没有任何安装虚拟环境的经验，所以就求助了以下kimi

```
根据你提供的错误信息，你在尝试使用 pip 安装 Python 包时遇到了 error: externally-managed-environment 错误。这通常发生在你尝试在系统级安装 Python 包，但你的系统环境被外部管理时，比如使用 Homebrew 或者其他包管理器安装的 Python。

为了解决这个问题，你有几个选项：

    使用系统的包管理器安装：如果你的系统使用 apt（如 Ubuntu 或 Debian），你可以使用 apt install python3-xyz 来安装包，其中 xyz 是你想要安装的包名

。

创建虚拟环境：你可以使用 Python 的 venv 模块创建一个虚拟环境，然后在该环境中安装包。这样做可以避免与系统级的包管理器冲突。创建虚拟环境的命令如下：

python3 -m venv path/to/venv

然后激活虚拟环境：

source path/to/venv/bin/activate

在虚拟环境中，你可以使用 pip 安装你想要的包

。

使用 pipx：pipx 是一个工具，它可以帮助你在隔离的环境中安装和运行 Python 应用程序。安装 pipx 后，你可以使用它来安装包：

pipx install xyz

这将为每个包管理一个虚拟环境，并在 .local/bin 中创建符号链接

。

删除 EXTERNALLY-MANAGED 文件：如果你知道你在做什么，并且愿意承担风险，你可以删除 Python 安装目录下的 EXTERNALLY-MANAGED 文件来覆盖这个保护机制。例如，在 Ubuntu 上，你可以这样做：

sudo rm /usr/lib/python3/dist-packages/EXTERNALLY-MANAGED

但请注意，这样做可能会破坏你的系统包管理器和 Python 环境之间的协调

。

使用 --break-system-packages 选项：如果你想要强制 pip 忽略这个错误，可以在 pip 命令中添加 --break-system-packages 选项，但这样做可能会有风险

    。

建议的做法是使用虚拟环境，这样可以避免与系统包管理器的潜在冲突，并且使得环境管理更加清晰。如果你不确定该怎么做，或者需要更具体的帮助，请提供更多的上下文信息，例如你的操作系统和 Python 版本


```

之后安装py的venv虚拟环境

```bash
iiiiling@iiiiling-server:~/Desktop/notebook$ sudo apt-get install python3-venv
```
再重新配置一下
```bash
iiiiling@iiiiling-server:~/Desktop/notebook$ python3 -m venv mkdocs
iiiiling@iiiiling-server:~/Desktop/notebook$ source mkdocs/bin/activate
(mkdocs) iiiiling@iiiiling-server:~/Desktop/notebook$ pip install mkdocs-material
```
其中，第一步需要在想配置虚拟环境的文件夹下运行

安装完之后就可以直接mkdocs serve了
```bash
(mkdocs) iiiiling@iiiiling-server:~/Desktop/notebook$ mkdocs serve
```

