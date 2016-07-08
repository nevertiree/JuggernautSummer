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

爬虫进阶之路by知乎

1.先从urllib2 入手，写一个最简陋的get；
2.接触requests；
3.知道了proxy、user-agent，知道了如何post；
4.学习Regex；
5.放下Regex，开始用xpath和BeautifulSoup；
6.攻克了移动端weibo，卡在网页版的weibo，于是知道了selenium，开始用PhantomJS；
7.把数据写在CSV中；
8.把数据写在mysql或者mongoDB中；
9.发现从头开始写太麻烦，于是用scrapy；
10.爬虫最有价值的一环是数据的清洗和重塑，从千万数据中提取出有价值的部分并且为自己所用；
11.更好的分析方法还有机器学习、推荐系统和聚类分析


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
