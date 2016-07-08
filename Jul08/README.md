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

##2.Gradle

##3.爬虫

1.Apache HttpClient <http://hc.apache.org/>
2.WebMagic <http://webmagic.io/docs/zh/>

##4.导入Jar包

InterlliJ IDEA

[File]->[Project Structure]->[Dependencies]->[+]->[JARs or directories]

也可以在gradle.build中添加以下代码

`//gradle copyJars以下载类库`
`task copyJars(type: Copy) `
`{`
`    from configurations.runtime`
`    into 'lib' // 目标位置`
`}`
