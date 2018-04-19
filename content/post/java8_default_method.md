---
title: "Java8_default_method"
date: 2018-04-19T09:54:57+08:00
draft: false
categories:
- Java8
---

正文
---
很多编程语言在它们的集合库中整合了函数表达式，这通常使代码变得更简洁、更容易阅读。例如，考虑一个循环：

	for (int i = 0; i < list.size(); i++)
		System.out.println(list.get(i));

有更好的方式可以实现同样的功能。类库设计者可以提供一个 `forEach` 方法，应用一个函数表达式到集合中的每个元素。然后，代码可以简化为：

	list.forEach(System.out::println);

如果集合库从一开始就这样设计，那就好了。但是 Java 的集合库是在很多年前设计的，那么就会有一个问题。如果集合库中的接口有了新的方法，例如 `forEach` ，那么所有程序的类，只要实现了这个接口，就会遭到破坏，除非修改类实现新的方法。对于 Java 人员来说，这是不可接受的。

Java 设计者决定一次性解决这个问题，允许接口中的方法有具体实现，这些方法被称为默认方法（ default method ），这样新的方法就可以安全地添加到接口中了。

默认方法结束了一种经典的类结构模式：提供一个接口，和一个实现了接口中大部分或全部方法的抽象类。

如果一个方法，作为默认方法存在于一个接口中，又存在于另一个接口或者父类中，会发生些什么呢？规则如下：

1. 父类优先。父类中提供了具体的方法，接口中相同方法签名的默认方法会被忽略。
2. 接口之间冲突。如果一个接口提供了默认方法，另一个接口中有相同方法签名的方法（不管是不是默认方法），那么你必须自己解决冲突，通过 override 那个方法。

e.g.

	interface Person {
		long getId();
		default String getName() {return "Shan MH";}
	}

	interface Named {
		default String getName() {return getClass.getName() + "_" + hashCode();}
		//String getName();
	}

	class Student implements Person, Named {
		public String getName() {return Person.super.getName();}
	}





翻译自如下内容，摘抄自 AWP.Java.SE.8.for.the.Really.Impatient.2014
---

Many programming languages integrate function expressions with their collections library. This often leads to code that is shorter and easier to understand than the loop equivalent. For example, consider a loop

	for (int i = 0; i < list.size(); i++)
		System.out.println(list.get(i));

There is a better way. The library designers can supply a forEach method that applies a function to each element. Then you can simply call

	list.forEach(System.out::println);

That's fine if the collections library has been designed from the ground up. But the Java collections library has been designed many years ago, and there is a problem. If the Collection interface gets new methods, such as foreach, then every program that defines its own class implementing Collection will break until it, too, implements that method. That is simply unacceptable in java.

The Java Designers decided to solve this problem once and for all by allowing interface methods with concrete implementations(called default methods). Those methods can be safely added to existing interfaces.

Default methods put an end to the classic pattern of providing an interface and an abstract class that implements most or all of its methods.

What happens if the exact same method is defined as a default method in one interface and then again as method of a superclass or another interface? Here the rules are:

1. Superclasss win. If a superclass provides a concrete method, default methods with the save name and parameter types are simply ignored.
2. Interfaces clash. If a superinterface provides a default method, and another interface supplies a method with the save name and parameter types(default or not), then you must resolve the conflict by overriding that method.

e.g.

	interface Person {
		long getId();
		default String getName() {return "Shan MH";}
	}

	interface Named {
		default String getName() {return getClass.getName() + "_" + hashCode();}
		//String getName();
	}

	class Student implements Person, Named {
		public String getName() {return Person.super.getName();}
	}