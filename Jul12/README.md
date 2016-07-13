# 07月12日

##关键词

Spring JavaBean IoC DI JUnit

##1.控制反转Ioc基本概念

Inversion of Control即意味着将你设计好的对象交给容器控制，IoC有容器来创建及注入依赖对象(反转)，即由Ioc容器来控制对象的创建。有了IoC容器后，把创建和查找依赖对象的控制权交给了容器，由容器进行注入组合对象，所以对象与对象之间是松散耦合，这样也方便测试，利于功能复用，更重要的是使得程序的整个体系结构变得非常灵活。

而在传统设计程序直接在对象内部通过new主动创建依赖对象，程序在对象中主动控制去直接获取依赖对象(正转)。z这导致类与类之间高耦合，难于测试。

理解好Ioc的关键是要明确“谁控制谁？控制什么？为何是反转？哪些方面反转了？”

>1.谁控制谁？-->IoC容器控制了对象;

>2.控制什么？-->主要控制了外部资源获取（不只是对象包括比如文件等）;

>3.为何是反转？-->容器帮我们查找及注入依赖对象，对象只是被动的接受依赖对象;

>4.哪些方面反转了-->依赖对象的获取被反转了;

在IoC语境中，所有的类都在Spring容器中登记，程序告诉Spring自己是什么，自己需要什么，然后Spring会在适当的时候把东西主动给你，也把你交给其他需要你的东西。适当的时间意思是类的创建、销毁都由Spring来控制，Spring在系统运行中动态的向某个对象提供它所需要的其他对象。对象只需要知道需要什么，而不用操心什么时候使用该对象。

>比如对象A需要操作数据库，传统程序需要代码获得Connection对象，而IoC语境中只要告诉Spring需要Connection，至于这个Connection怎么构造，何时构造，程序不需要知道。在系统运行时，Spring会在适当的时候制造一个Connection，然后DI到A当中，这样就完成了对各个对象之间关系的控制。A需要依赖Connection才能正常运行，而这个Connection是由Spring注入到A中的，依赖注入(DI)的名字就这么来的。

对于某个具体的对象而言，以前是它控制其他对象，现在是所有对象都被Spring控制，所以这叫控制反转。

IoC对编程带来的最大改变不是从代码上，而是从思想上，发生了“主从换位”的变化。应用程序原本获取什么资源都是主动出击，但是在IoC/DI思想中，应用程序就变成被动的等待IoC容器来创建并注入它所需要的资源了。IOC的思想是用容器来实现这些相互依赖对象的创建、协调工作，对象只需要关系业务逻辑本身就可以了，对于某个具体的对象而言，以前是它控制其他对象，现在是所有对象都被Spring控制(控制反转),而对象如何得到他的协作对象的责任也被反转了。

参考文章

1.[谈谈对Spring IoC的理解](http://jinnianshilongnian.iteye.com/blog/1413846)

2.[透透彻彻IoC](http://stamen.iteye.com/blog/1489223/)

3.[Spring IoC原理](http://blog.csdn.net/it_man/article/details/4402245)

4.[IoC模式](http://www.cnblogs.com/qqlin/archive/2012/10/09/2707075.html)

5.[IoC框架](http://blog.csdn.net/wanghao72214/article/details/3969594)

##2.依赖注入Dependency Injection基本概念

DI和IoC其实是同一概念的不同描述，DI指组件之间依赖关系由容器在运行期决定，即由容器动态的将某个依赖关系注入到组件之中。DI的目是提升组件重用的频率。

通过DI，我们只需要通过简单的配置，而无需任何代码就可指定目标需要的资源，完成自身的业务逻辑，而不需要关心具体的资源来自何处，由谁实现。
 
理解DI的关键是：“谁依赖谁，为什么需要依赖，谁注入谁，注入了什么”，那我们来深入分析一下：
 
>1.谁依赖于谁-->程序依赖于IoC容器；

>2.为什么需要依赖-->程序需要IoC容器来提供对象需要的外部资源；

>3.谁注入谁-->IoC容器注入应用程序某个程序依赖的对象；

>4.注入了什么-->某个对象所需要的外部资源（对象、资源、常量数据）。

那么DI是如何实现的呢？

>Java 1.3之后一个重要特征是反射，它允许程序在运行的时候动态的生成对象、执行对象的方法、改变对象的属性，Spring就是通过反射来实现注入的。

参考文章

1.[Inversion of Control Containers and the Dependency Injection pattern](http://www.martinfowler.com/articles/injection.html)

##3.Spring简单参考代码

Main函数

Spring容器集中管理Bean的实例化，Bean实例可以通过BeanFactory的getBean(Stringbeanid)方法得到。BeanFactory是一个工厂，程序只需要获取BeanFactory引用，即可获得Spring容器管理全部实例的引用。程序不需要与具体实例的实现过程耦合。应用在启动时会自动创建Spring容器，组件之间直接以依赖注入的方式耦合，甚至无须主动访问Spring容器本身。

```Java

public static void main(String[] args) {
        ApplicationContext context = new FileSystemXmlApplicationContext(  
                "applicationContext.xml");  //引入Spring配置文件
        Animal animal = (Animal) context.getBean("animal");  //对象实例化
        animal.say();  } 

```

Spring配置文件applicationContext.xml

配置文件中通过<bean id=”xxxx” class=”xx.XxClass”/>方法配置一个Bean时，需要该Bean实现类中必须有无参构造器。故Spring底层相当于调用了`Xxx = new xx.XxClass()`；

```xml

<bean id="animal" class="springframework.test.Cat">  JavaBean的id和所属的class
        <property name="name" value="kitty" />   该类的属性和属性值
</bean> 

```

Spring配置文件中的class来自事先编写好的springframework.test.Cat

```Java

public class Cat implements Animal {//该类实现了一个接口，符合Spring面向接口编程的特点
    private String name;

    public void say() {//实现了接口中的抽象方法
        System.out.println("I am " + name + "!");   
    }

    public void setName(String name) {
        this.name = name;   
    }
}

```
Cat类的来源是Animal接口

```Java

public interface Animal {
    public void say();//定义了一个抽象方法
}

```

参考文章

[Bean的基本概念](http://blog.csdn.net/chenssy/article/details/8222744)

##4.Bean在Spring中的意义

Spring是一个大型的工厂，而Bean就是该工厂的产品，JavaBean是一切配置到IoC容器中的实体。Spring容器能够生产的产品取决于配置文件的配置。
对于开发人员来说，使用Spring框架所做的就是两件事：开发Bean、配置Bean。
对于Spring框架来说，它要做的就是根据配置文件来创建Bean实例，并调用Bean实例的方法完成DI。

<beans>标签是Spring配置文件的根元素，<beans>标签可以包含多个<bean>子标签，每个<bean…/>元素可以定义一个Bean实例，每一个Bean对应Spring容器里的一个Java实例定义Bean时通常需要指定两个属性。

Id：确定该Bean的唯一标识符，容器对Bean管理、访问、以及该Bean的依赖关系，都通过该属性完成。

Class：指定该Bean的具体实现类(不能使接口)。Spring会直接使用new关键字创建该Bean的实例，故必须提供Bean实现类的类名。

举例

```xml

<bean id="Cat" class="springframework.test.Cat" /> 

```

org.springframework.beans

org.springframework.context(实现保存Bean的对象)

##5.Value Object

Value Object实际上是封装哲学的一种体现，它解决了形参过于冗长、修改形参成本太高的问题。Value Object中主要有构造器、Getter和Setter三类。

##6.MyBatis

##3.Mysql安装

[MySql官网](http://dev.mysql.com/downloads/repo/yum/)

把安装的rpm安装--> 会在/etc/yum.repos.d

```shell
sudo rpm -Uvh platform-and-version-specific-package-name.rpm
```
