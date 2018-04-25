---
title: "Java8 接口中的静态方法"
date: 2018-04-25T10:55:39+08:00
draft: false
categories:
- Java8
- 翻译
---

>原文 [AWP.Java.SE.8.for.the.Really.Impatient.2014](/files/AWP.Java.SE.8.for.the.Really.Impatient.2014.pdf) ，1.8 Static Methods in Interfaces

在 Java8 里，你可以向接口中添加静态方法。从来没有技术上的原因，来说明为什么不允许这样做，可能仅仅是因为它打破了接口作为抽象声明的原则。

到现在为止，把静态方法放在陪伴类（ companion class ）中已达成共识。你会经常发现成对的接口和工具类，例如， `Collection/Collections` 、 `Path/Paths` 。

让我们看一下 `Paths` 类中有什么，它仅仅有几个工厂方法。你可以通过一系列字符串创建一个 `path` 对象，例如：

	Paths.get("jdk1.8.0", "jre", "bin");

在 Java8 里，你可以添加这个方法到 `Path` 接口中：

	public interface Path {
		public static Path get(String first, String...more) {
			return FileSystems.getDefault().getPath(first, more);
		}
		...
	}

然后， `Paths` 类可以永远消失了。

当你审查 `Collections` 类时，你会发现两种方法。一种方法如下：

	public static void shuffle(List<?> list)

这种方法可以使用[默认方法](/post/java8_default_method/)替代，放在 `List` 接口中

	public default void shuffle()

然后你就可以在任何 `list` 对象上调用 `list.shuffle()` 了。

对于一个工厂方法，你就不能使用[默认方法](/post/java8_default_method/)代替了，因为你没有一个对象来调用这个工厂方法。这时候，接口中的静态方法就显身手了，例如：

	public static <T> List<T> nCopies(int n, T o)
	// Constructs a list of n instances of o

可以把这个方法放在 `List` 接口中，然后你就可以简单的调用 `List.nCopies(10, "Fred")` 而不是 `Collections.nCopies(10, "Fred")` ，并且对于代码阅读者来说这样更清晰，因为你知道它会返回一个 `List` 。

以这种方式重新构建 Java 集合类库几乎是不可能的。但是你自己创建接口的时候，再也没有必要为你的接口提供一个陪伴类来提供工具方法了。

Java8 ，已经在很多接口中加入了静态方法，例如， `Comparator` 接口有一个非常有用的静态方法 `comparing` ，接收一个 “key extractor” 函数作为参数，返回一个 comparator ，用于比较 extracted keys 。要通过姓名比较 `Person` 对象，可以这样写：

	Comparator.comparing(Person::name);

