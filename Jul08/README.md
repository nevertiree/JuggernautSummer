# 07月08日

##关键词

快捷方式

##1.Linux下建立快捷方式

>今天下载了PythonIDE:pycharm，现在学习如何把pycharm.sh文件加为快捷方式。

1.建立desktop文件

`sudo vim /usr/share/applications/pycharm.desktop`

2.在文件中加入以下模板。注意去掉中文注释。

`[Desktop Entry]`
`Type=Application`
`Name=Pycharm`
`GenericName=Pycharm3`
`Comment=Pycharm3:The Python IDE`
`Exec=sh /opt/pycharm/bin/pycharm.sh -desktop(下划线部分是你存放的地址)`
`Icon=/opt/pycharm/bin/pycharm.png(下划线部分是Pycharm内部的图片，比较一下上下) `
`Terminal=pycharm`
`Categories=Pycharm;`

http://www.iteblog.com/idea/key.php
