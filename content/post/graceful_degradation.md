---
title: "Graceful degradation（平稳退化）"
date: 2018-05-14T09:22:14+08:00
draft: false
categories:
- 他山之石，可以攻玉
---

Graceful degradation
---

>[原文](https://searchnetworking.techtarget.com/definition/graceful-degradation)发表于 2007 年，且站在 internet 的角度上，请大家阅读时注意判别。

Graceful degradation is the ability of a computer, machine, electronic system or network to maintain limited functionality even when a large portion of it has been destroyed or rendered inoperative. The purpose of graceful degradation is to prevent catastrophic failure. Ideally, even the simultaneous loss of multiple components does not cause downtime in a system with this feature. In graceful degradation, the operating efficiency or speed declines gradually as an increasing number of components fail.

平稳退化是一种计算机、机器、电子系统或者网络在系统的大部分已经被损坏或者变得不起作用，系统仍然保持有限功能性的一种能力。

平稳退化的目标是防止灾难性故障。

在理想情况下，具备平稳退化能力的系统，即使多个模块同时失去作用，也不会造成系统宕机。

在平稳退化下，随着越来越多的组件失败，操作效率或速度会逐渐下降。

Graceful degradation has been an important consideration in the design and implementation of large communications networks.

在设计和实现一个大型的交互系统时，平稳退化是一个非常重要的考虑因素。

Graceful degradation is sometimes considered equivalent to fault tolerance but there is a significant difference. Fault-tolerant systems are designed so that if a component fails or a network route becomes unusable, a backup component, procedure or route can immediately take its place with no negative impact whatsoever on individual subscribers. Graceful degradation is an outgrowth of effective fault management, which is the component of network management concerned with detecting, isolating and resolving problems.

有时候，平稳退化被认为等同于容错，但是他们有非常大的不同。容错系统是这样设计的，如果一个组件失败或者网络路由变得不可用，后备组件、过程或者路由可以立即取代它，对个人用户没有任何负面影响。平稳退化被用来进行有效地错误管理，它关注于检测、隔离和解决问题，是网络管理的一部分。

In Web site design, the term graceful degradation refers to the judicious implementation of new or sophisticated features to ensure that most Internet users can effectively interact with pages on the site. Significant milestones in Web site design and Internet use over the years have included the introduction of images, frames, online gaming, Java, JavaScript, ActiveX controls, tabbed browsing, voice over Internet Protocol (VoIP) and videoconference technology. When an updated version of a browser or operating system is released, new features are often included to keep pace with the latest enhancements to the Internet. For various reasons, many Internet users prefer to continue using their existing browsers rather than immediately upgrading to the latest version every time a new Web site technology becomes popular. When a site is designed with graceful degradation in mind, such users are not abruptly forced to upgrade their browsers unless they are using "ancient" ones!

在网站设计方面，平稳退化指的是明智地实现新的或者复杂的特性，以确保大多数互联网用户能够有效地与网站上的页面进行交互。

近年来，网站设计和互联网使用有了很多重要的里程碑，包括图像的引入、frame、在线游戏、java、javascript、activeX、多标签页、VoIP和视屏会议。当浏览器或者操作系统发布新版本时，通常会包含新的特性，以跟上互联网最新的改进步伐。由于各种原因，每次新的网站技术变得流行时，许多互联网用户喜欢继续使用他们现有的浏览器，而不是立即升级到最新版本。当一个网站在设计的时候，如果时刻把“平稳退化”记在心中，用户就不会突然地被迫升级他们的浏览器，除非他们使用非常古老的浏览器。

浅谈 Javascript 中的退化
---

>转载地址：https://www.cnblogs.com/struCoder/p/3473871.html

*   首先，什么是平稳退化
	
	通俗的说呢，就是如果正确的使用的javascript脚本，就可以让那些访问者在他们的浏览器不支持javascript的情况下任然可以顺利的浏览你的网站。这就是所谓的平稳退化，也就是说，虽然某个功能无法使用，但最基本的操作仍然顺利完成。

*   下面我们来举一个例子

	我们来看在新建窗口中打开一个链接，Javascript使用window对象的open()方法来创建新的浏览器窗口。下面这个函数是window.open()方法的一种典型应用：

		function popUp(URl){
	        window.open(URl,"baidu","width = 320,hight=600");
	    }

	当然了调用popUp函数的一个办法就是使用伪协议，下面是通过javascript伪协议调用：

		<a href = "javascript:popUP('http://www.cnblogs.com/struCoder/')">Mr.Smart</a>

	这条语句在支持“javascript:”伪协议的浏览器中运行正常，较老的浏览器会打开失败。当然了会有人问，谁关心这个啊，在这里我想说，比如那些google，百度的搜索引擎，如果你的javascript网页不能平稳退化，他们在搜索引擎上的排名可能大受损害。那么有什么办法进行改变呢？

*   为javascript代码预留出退路

		<a href="http://www.cnblogs.com/struCoder/" onClick="popUp(this.href);return false;">Click me</a>

	所以，把这个href属性设置为真实存在的URL地址后，即使javascript已经被禁用，这个链接依然是可用的，虽然这个链接在功能上大了点折扣(因为他没有打开一个新的窗口)，这个链接并没有彻底失效，这个一个经典的平稳退化的例子。

~~over~~