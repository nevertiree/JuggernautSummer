# 07月22日

原因很简单。在对数据库操作结束后关闭连接是正确的做法，没什么大问题。至于出现：No operations allowed after connection closed。这样的问题原因只有一个，你这里和数据库的连接Connection是一个Static的，程序共享这一个Connection。所以第一次对数据库操作没问题，当把Connection关闭后，第二次还想操作数据库时Connection肯定不存在了。

