---
title: "Fluent syntax"
date: 2018-05-08T15:24:43+08:00
draft: false
categories:
- 他山之石，可以攻玉
---

What is fluent syntax?
---

在 netty 中，`AbstractBootstrap` 抽象类的声明如下：

	public abstract class AbstractBootstrap
			<B extends AbstractBootstrap<B, C>, C extends Channel>

在这个签名中，子类 B 作为父类的类型参数，因此，运行时的实例可以作为方法的返回值来实现链式调用。我们称这种方式为 fluent syntax。

子类的声明如下：

	public class Bootstrap extends
			AbstractBootstrap<Bootstrap, Channel>

Application
---

	public class Cat extends Animal<Cat> {

		@Override
		public Cat walk() {
			System.out.println("I am a cat, I am walking~");
			return this;
		}

		public void bark() {
			System.out.println("miao miao~");
		}

		public static void main(String[] args) {
			Cat cat = new Cat();
			cat.walk().bark();
		}
	}

	abstract class Animal<A extends Animal<A>> {
		public abstract A walk();
	}

~~over~~