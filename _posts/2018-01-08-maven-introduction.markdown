---
layout: post
title:  "Maven简介"
date:   2018-01-08 10:33:45 +0800
categories: jekyll update
---
>这篇文章讲的是： `Maven`是什么，以及 `Maven`怎样配合IDE使用

# 背景
现在做Java项目，总有大大小小的依赖。光是一个Spring就要添加好几个jar包，还有那个Hibernate，也有好几个jar包，单元测试还要用JUnit。之前常规的做法是全部下载下来，像这样
![](/res/ge65wy3qt4warhtrjyulyi.jpg)  

#  最初的做法：手动下载，手动复制
想要把它们导入项目，就在IDE里设置一下，然后直接把jar包复制过去。  
取消一个依赖，也是在IDE里设置一下，再把复制过去的jar包删掉。  
而且这些jar包还会不断更新，经常要去下载最新版本，非常麻烦。  

# 大胆的想法：配置文件
依赖总容易出问题，程序员苦不堪言。于是有个同学就想了，能不能像Spring那样，写个配置文件表示项目之间的依赖？
>用 xml 表示项目之间的依赖关系
{% highlight xml %}
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
</dependencies>

<dependencies>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>servlet-api</artifactId>
        <version>2.5</version>
    </dependency>
</dependencies>

<dependencies>
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.2</version>
    </dependency>
</dependencies>
{% endhighlight %}
这段代码定义了3个依赖：`junit V4.12`，`servlet-api V2.5`和`jsp-api V2.2`。  
`xml`非常清晰地描述了这个关系。  
给每个项目里都放一个这样的`xml`，就表示了各个项目需要哪些jar包。  
写个程序来读取配置文件，自动把jar包复制到正确的地方。这样就方便多了，不用改IDE设置，也不用手动复制jar包，只要改改配置文件就行。  
既然xml配置这么好用，那应该再加入一些别的信息，比如使用的jdk版本，字符编码，这些设置在不同项目里可能不一样，统统加到xml里去。  
>配置一下jdk版本和字符编码
{% highlight xml %}
<build>
    <plugins>
        <plugin>
            <groupId>org.apche.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
    </plugins>
</build>
{% endhighlight %}
最后别忘了，你的项目也有可能被别的项目使用，所以要把自己的名称和版本写进去，提供给别人。
>给自己的项目起个名字
{% highlight xml %}
<groupId>java-in-summer</groupId>
<artifactId>news</artifactId>
<version>1.0</version>
{% endhighlight %}  

# 关键的问题
现在成功用一个配置文件描述了各种信息和依赖，那么问题来了：这些jar包去哪找？  
在本地找？当然可以，在硬盘上圈一块地，叫做`本地库`。读取了这些依赖，直接到`本地库`找。  
但问题并没有解决：本地库里的jar包还是要手动下载，想用新版本还要再下载一次。  
没办法了，只能从网上自动下载！于是这位同学架起了一个服务器，跟各大开源项目进行交易，把它们的代码保存到服务器上。如果本地库里找不到，就去服务器自动下载，存到本地库，如果服务器里还是没找到，就报错。  
这样一来，本地库里就会有多个版本并存，想用哪个就用哪个，只要改配置文件就行。  
![](/res/htrfdsreyjdfg.jpg) 
 
# 万事俱备，就差一个xml解析程序
思路已经清晰，写出这么一个程序当然不在话下，三下五除二解决问题。  
这套工具就是`Maven`（英[ˈmeɪvn] 美[ˈmevən]），这个服务器叫`Maven中央仓库`，配置文件叫`pom.xml`  

# 渐入佳境
后来越来越多的项目都发布到了`Maven中央仓库`里。只要搜索一下，经常能看到有关`Maven`的信息
![](/res/vbnjredzdshtrydere.jpg)  
点进去，选择一个版本，就会显示对应的`pom.xml`代码
![](/res/zsertghfdsgjhliutydr54.jpg)  
把这段代码复制，粘到自己的`pom.xml`中，刷新一下项目，就自动导入了spring-context包

Maven也跟各大IDE做了交易，让IDE们集成了Maven，比如Eclipse、MyEclipse、IDEA之类，当然也可以下载安装独立的Maven，按照网上的教程配置一下环境变量。我是推荐安装独立Maven的。  
有了集成Maven的IDE，就可以直接新建Maven项目，也可以把普通项目改成Maven项目。如果你钟爱编辑器，也可以只用Maven来新建Maven项目，只要手动创建出`src`目录和`pom.xml`就行。  
记得连网，Maven要访问中央仓库下载jar包！

# 继续深入
Maven项目结构是这样：`src`目录（项目的所有文件都放在这里），和一个配置文件`pom.xml`  
![](/res/vbnmlio8i67e5uwyeratw4354.jpg)  
没有任何jar包，只需要`src`目录和`pom.xml`就可以把项目导入各种IDE。  
进去细看一下  
![](/res/oiuyt54y6htrbzfd.jpg)
![](/res/kjhegw546hrtsd.jpg)
![](/res/iuytgzsvdfwg456e5yt.jpg)  
整理一下大概就是这样
> src  
>> main				（项目文件）  
>>> java			（存放java代码）  
>>> resources		（存放图片、文本之类的资源文件）  
>>> webapp		（web应用才会有这个目录，存放WEB-INF、js、jsp之类）  
>>
>> test				（测试用的目录）  
>>> java			（测试用的java代码）

假如别人把他的Maven项目发给你，可以直接导入而不会因为IDE设置不同报错。用Eclipse演示一遍：
>选择导入项目  

![](/res/fareetwhy5jeu6r8j.jpg)  
>弹出框里找到 `Maven - Existing Maven Project`  

![](/res/lkyj4u75etyrshdfxgjfye57.jpg)  
>在 `Root Directory`里选择 `pom.xml`所在的目录（也就是 `src`的上一级目录）  

![](/res/nye5wj4ts.jpg)  
>发现有 `pom.xml`，自动勾上了。如果没有勾，就手动把它勾上，然后点 `Finish`  

![](/res/bbewty6erthsufg.jpg)  
>Maven会去配置项目，寻找jar包。稍等一会儿，最后项目成功导入  

![](/res/hj8675w64tx.jpg)  
新建Maven项目以及把普通项目改成Maven项目的方法，网上有大量资料，这里就不赘述了。
>本地库的位置默认在`C:\User\你的用户名\.m2下`  

![](/res/fghy5j6ue44ymg.jpg)  

