---
title: "不可变对象减少对新生代垃圾回收的影响？"
date: 2018-04-28T17:18:35+08:00
draft: false
categories:
- 他山之石，可以攻玉
---

Java Concurrency In Priactice 中有这样一句话：

>Many developers fear that this approach will create performance problems, but these fears are usually unwarranted. Allocation is cheaper than you might think, and immutable objects offer additional performance advantages such as reduced need for locking or defensive copies and reduced impact on generational garbage collection.

对于不可变对象可以减少新生代的垃圾回收，感到迷惑。上网查了相关资料，原因如下。

一个不可变对象的属性在对象被创建后就不能被修改(在这里的例子使用的是引用数据类型的属性)，比如:

	public class ObjectPair {
		 
	    private final Object first;
	    private final Object second;
	 
	    public ObjectPair(Object first, Object second) {
	        this.first = first;
	        this.second = second;
	    }
	 
	    public Object getFirst() {
	        return first;
	    }
	 
	    public Object getSecond() {
	        return second;
	    }
	 
	}

将上面的类实例化后会产生一个不可变对象—它的所有属性用 final 修饰，构造完成后就不能改变了。

不可变性意味着所有被一个不可变容器所引用的对象，在容器构造完成前对象就已经被创建。就 GC 而言：这个容器年轻程度至少和其所持有的最年轻的引用一样。这意味着当在年轻代执行垃圾回收的过程中，GC 因为不可变对象处于老年代而跳过它们，直到确定这些不可变对象在老年代中不被任何对象所引用时，才完成对它们的回收。

更少的扫描对象意味着对内存页更少的扫描，越少的扫描内存页就意味着更短的 GC 生命周期，也意味着更短的 GC 暂停和更好的总吞吐量。

~~over~~