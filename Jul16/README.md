# 07月16日

##关键词

MySQL正则表达式 MySQL数据导出

##1.MySQL正则表达式

[MySQL正则表达](http://www.cnblogs.com/way_testlife/archive/2010/09/17/1829567.html)式仅支持多数正则表达式实现的一个很小的子集

1. 基本字符匹配

```sql

SELECT prod_name FROM products WHERE prod_name REGEXP '1000' ORDER BY prod_name;

```

2. OR匹配

```sql

SELECT prod_name FROM products WHERE prod_name REGEXP '1000|2000' ORDER BY prod_name;

```

##2.MySQL数据导出

见github

参考文章

[prepareStatement与Statement的区别](http://blog.csdn.net/zsm653983/article/details/7296609)
[prepareStatement的用法与解释](http://www.cnblogs.com/0banana0/articles/2029863.html)

##3.MySQL列名修改

```sql

alter table tablename change column old_name new_name new_type;


```

##4.正则表达式

捕获分组group(num):num表示对应括号的编号，括号分组的编号规则是从左向右计数，从1开始。

```java

String regex = "(\\d{4})-(\\d{2})-(\\d{2})";
        String input = "2013-12-23";  
        Pattern pattern = Pattern.compile(regex);  
        Matcher matcher = pattern.matcher(input);  
        while (matcher.find()) {  
            System.out.println("matcher.group() :" + matcher.group() + " starting at index \"" + matcher.start() + "\" and ending at index \""  
                     + matcher.end()+"\""  
            );  
            System.out.println("matcher.group(1) :" + matcher.group(1)   
            );  
            System.out.println("matcher.group(2) :" + matcher.group(2)   
            );  
            System.out.println(matcher.groupCount());  
              
        }  

```

```

Exception in thread "main" java.util.regex.PatternSyntaxException: Unclosed group near index 1

```

在正则表达式中，有个“捕获组”的概念，其使用了小括号；因此分析，当正则表达式解析到左括号时，没有发现对应的右括号，从而报错。

##5.设计模式

##6.Lombok

>在写Java程序的时候经常会遇到如下情形：新建了一个Class类，然后在其中设置了几个字段，最后还需要花费很多时间来建立getter和setter方法。lombok项目的产生就是为了省去我们手动创建getter和setter方法的麻烦，它能够在我们编译源码的时候自动帮我们生成getter和setter方法。即它最终能够达到的效果是：在源码中没有getter和setter方法，但是在编译生成的字节码文件中有getter和setter方法 

[Lombok的Git仓库](https://github.com/rzwitserloot/lombok)

[Lombok的官方网站](https://projectlombok.org/)

[官方文档](http://jnb.ociweb.com/jnb/jnbJan2010.html)

##7.爬虫的性能优化问题

1.每个方法都要写try catch finally
2.每次new都要主动destory
3.每个变量都要及时null
4.把new方法写在循环外面
5.用sleep控制线程，防止服务器kill
