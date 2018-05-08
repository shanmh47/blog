---
title: "Private Constructor capture idiom"
date: 2018-05-08T11:31:12+08:00
draft: false
categories:
- 他山之石，可以攻玉
---

Private Constructor capture idiom
---

The following code fails to compile, tips: Cannot refer to an instance field arg while explicitly invoking a constructor

	package practice;

	/**
	* @author 2232 shanmh
	*/

	public class MyThing extends Thing {

		final private int arg;
		
		public MyThing() {
			super(arg = Math.round(1f));
		}

	}

	class Thing {
		public Thing(int i) {
			
		}
	}

Suppose Thing is a library class, only a constructor argument, did not provide any access device, you do not have permission to access internal and therefore can not modify it.

At this time, want to write a subclass, the constructor through the bar with SomtOtherClass.func () method to calculate the super class constructor parameter. Return value of this method call can return again and again different values, you want to pass this value is stored in the parent class of a final sub-class instance. So with the above code, but can not compile.

modify

	public class MyThing extends Thing {

		final private int arg;
		
		public MyThing() {
			this(SomeOtherClass.func());
		}

		private MyThing(int i) {
			super(i);
			arg = i;
		}
	}

	class Thing {
		public Thing(int i) {
			
		}
	}

	class SomeOtherClass {
	    static int func() {
	    	return Math.round(1f);
	    }
	}

The program uses the alternate constructor invocation mechanism (alternate constructor invocation)

In the private constructor, the expression SomeOtherClass.func () the value has been captured in the variable i, and it can be returned after the superclass constructor to store the final type of domain arg.

Private constructor to avoid the race condition
---

Java concurrency in practice > 4.3.5 Example: vehicle tracker that publishes its state(page69)

https://stackoverflow.com/questions/12028925/private-constructor-to-avoid-race-condition#

~~over~~