# 07月07日

##1.Python环境搭建 && Linux安装包管理

>学习Python是为了写爬虫，在搭建Python环境的时候发现系统上自带的Python是2.7版本，为了安装3.4版本顺路学习了Linux的程序安装。

下面以安装Python为例，展示Linux安装过程

1.查看旧版本号

`python -V`

2.下载安装文件

>源码文件要放在/usr/local/src文件夹中并解压

`cd /usr/local/src`
`wget http://www.python.org/ftp/python/3.4.0/Python-3.7.0.tar.bz2`
`tar -jxvf Python-3.4.0.tar.bz2`

3.编译源码文件

>进入解压过的源码文件，先把文件的安装位置定位到/usr/local文件夹中，然后编译并安装

`cd /usr/local/src/Python-3.4.0/`
`./configure --prefix=/usr/local`
`make`
`make install`

4.添加快捷命令

>在路径PATH中添加新安装的Python
>进入/usr/bin/添加链接。注意不能覆盖删除以前的python链接，因为很多Linux系统自带的程序会调用2.7版本的命令。

`cd /usr/bin/`
`PATH="$PAHT"：/usr/local/Python-3.4.0/bin`
`sudo ln -s /usr/local/Python=3.4.0/bin/Python3 ./py`
