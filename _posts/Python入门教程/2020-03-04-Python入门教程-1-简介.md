---
title: Python入门教程-1-简介
date: 2020-03-04 17:43:50 +0800
categories: [Python入门教程]
tags: []
---

# 课程说明
在 [廖雪峰Python3教程](https://www.liaoxuefeng.com/wiki/1016959663602400) 和 [Python3.5官方文档](https://docs.python.org/3.5/tutorial/index.html) 的基础上选取部分内容进行适当讲解，预计每节课10~20分钟左右，只是进行一个简单入门讲解，所以每个小节最后附上讲解内容对应章节的链接，各位同学可在课后进行深入的学习。

# Python简介
Python是由荷兰人Guido van Rossum (“龟叔”)于1989年圣诞节期间，为了打发时间而编写。Python这个名字取自作者很喜欢的BBC电视剧"Monty Python’s Flying Circus"。

Python是在另一种编程语言 `ABC` 的基础上发展而来，ABC是“龟叔”参与设计的一种教学语言，他认为ABC非常优美和强大，但是并没有取得成功，他认为是没有开放造成的，所以Python进行了开源。Python还结合了很多C语言的使用习惯，比如Python中的open函数和C语言的open函数非常类似，Python里同样也有文件描述符等概念。

Python是一种 [解释型](https://zh.wikipedia.org/wiki/%E7%9B%B4%E8%AD%AF%E8%AA%9E%E8%A8%80)、[面向对象](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1) 的语言，相比较而言C则是 [编译型](https://zh.wikipedia.org/wiki/%E7%B7%A8%E8%AD%AF%E8%AA%9E%E8%A8%80)、[面向过程](https://zh.wikipedia.org/wiki/%E8%BF%87%E7%A8%8B%E5%BC%8F%E7%BC%96%E7%A8%8B) 的语言。

解释型语言使用解释器在运行期间动态的逐条将语句解释为计算机可识别的机器代码，编译型语言需要提前把源代码编译为机器代码，然后运行。  

解释型语言相比编译型语言的主要缺点是运行速度慢，但是由于很多的应用不需要追求很高的性能，所以使用解释型语言已经完全满足运行速度的要求。  

解释型语言相比编译型语言的主要优点是开发速度快，因为开发过程中可以不用编译就立即运行而得到反馈，另外像Python这样的高级语言做了更高程度的抽象和封装，并且把很多常用操作封装为了标准库，所以在代码量上会比C语言少很多。而且Python可以使用即时编译([JIT](https://zh.wikipedia.org/zh-hans/%E5%8D%B3%E6%99%82%E7%B7%A8%E8%AD%AF))技术提高运行速度  

面向对象和面向过程语言的主要区别是面向对象可以定义类(class)，而面向过程语言只能定义函数，Python既支持面向过程，也支持面向对象。关于面向对象和面向过程的更多区别请点击对应的链接进行深入了解。

# Python应用
近些年来Python越来越火，应用也越来越广泛，根据编程语言排行榜 [TIOBE](https://www.tiobe.com/tiobe-index/) 2020年2月最新的数据显示，目前Python的流行程度排行第3，居于Java和C之后。

很多我们所熟知的网站也是使用Python开发，比如国外的Youtube、Instgram，国内的知乎、豆瓣，还有像Google、Yahoo这样的大公司内部都在大量的使用Python。

Python在人工智能领域也非常流行，很多人工智能框架都选择使用Python语言，比如大名鼎鼎的Google人工智能框架TensorFlow就支持Python。

另外Python在自动化测试方面也是应用广泛，比如开源的自动化测试框架RobotFramework就是使用Python编写的，很多公司在自己开发自动化测试框架时也大多选择使用Python，比如华为就是在大量的使用Python来进行自动化测试。

由于Python简单、易学、易用的特点，所以在非编程相关的工作上也可以使用，比如日常办公中需要在大量的文本文件中搜索并替换某些内容，或者需要批量的整理操作大量文件，这个时候可以使用Python快速的编写一些小脚本来提升工作效率。相比shell或者bat等操作系统专用的脚本，Python的跨平台特性使得使用Python编写的脚本使用更方便。而且目前的大多数Linux发行版和OSX等系统上都内置了Python。

可以在Python官网 https://www.python.org/about/apps/ 上看到Python的很多应用列表。

# Python版本
Python自诞生以来经历了2个主要的大版本，一个是发布于2000年的Python2，另一个是发布于2008年的Python3。Python3相比Python2由了很大的变更，所以Python2下编写的代码是不能直接在Python3上运行的。而且官方已经于2020.1.1停止了对Python2的维护，也就是说如果Python2出现了重大漏洞也不会再有更新的修复版本发布了，所以建议如果是新写的项目都使用Python3。

如果由老的Python2的应用需要迁移到Python3，可以参考官方迁移指南：https://docs.python.org/3/howto/pyporting.html  
更多关于Python2和Python3的区别可以参考：http://python-future.org/compatible_idioms.html

# Python解释器
通常我们说Python是用C语言编写的，是因为官方下载的解释器CPython是使用C语言编写的，其实除了CPython之外还有很多其他的解释器。

> *以下内容引用自：https://www.liaoxuefeng.com/wiki/1016959663602400/1016966024263840*
>  
> **IPython**  
> IPython是基于CPython之上的一个交互式解释器，也就是说，IPython只是在交互方式上有所增强，但是执行Python代码的功能和CPython是完全一样的。好比很多国产浏览器虽然外观不同，但内核其实都是调用了IE。  
> CPython用>>>作为提示符，而IPython用In [序号]:作为提示符。
> 
> **PyPy**  
> PyPy是另一个Python解释器，它的目标是执行速度。PyPy采用JIT技术，对Python代码进行动态编译（注意不是解释），所以可以显著提高Python代码的执行速度。  
> 绝大部分Python代码都可以在PyPy下运行，但是PyPy和CPython有一些是不同的，这就导致相同的Python代码在两种解释器下执行可能会有不同的结果。如果你的代码要放到PyPy下执行，就需要了解PyPy和CPython的不同点。
> 
> **Jython**  
> Jython是运行在Java平台上的Python解释器，可以直接把Python代码编译成Java字节码执行。
> 
> **IronPython**  
> IronPython和Jython类似，只不过IronPython是运行在微软.Net平台上的Python解释器，可以直接把Python代码编译成.Net的字节码。

# Python编辑器(IDE)
由很多的编辑器支持编写和调试Python程序，以下列出几个比较流行的，可以根据个人喜好进行选择：
* IDLE：这个Python官方的IDE，Windows上安装Python后就会有，功能较简单，可以用于学习和测试一些语法。
* PyCharm：这个是由JetBrains开发的Python IDE，功能强大，有专业版和社区版，个人使用可以选择免费的社区版。
* VSCode：这个是微软的Visual Studio的精简版，开源且支持大量的插件，通过安装Python插件可以用于Python开发。
* Vim：如果是Linux下也可以使用这Vim加一些插件进行Python开发。

# 引用资料
1. [Python简介]：https://www.liaoxuefeng.com/wiki/1016959663602400/1016959735620448
2. [Whetting Your Appetite]：https://docs.python.org/3.5/tutorial/appetite.html
3. [Python-wikipedia]：https://zh.wikipedia.org/wiki/Python#cite_note-python_history-3
