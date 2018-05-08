---
title: "更简单的方式创建 Logger 对象"
date: 2018-04-27T18:08:45+08:00
draft: false
categories:
- 他山之石，可以攻玉
---

最近在看 [Reflection Madness ppt](https://www.javaspecialists.eu/talks/oslo09/ReflectionMadness.pdf) 的时候，发现一个可以获取调用者的方法：

	public class CallerID {
		public static String whoAmI(){
			Throwable t = new Throwable();
			StackTraceElement directCaller = t.getStackTrace()[1];
			return directCaller.getClassName() + "." + directCaller.getMethodName() + "()";
		}
	}

再往下翻，发现了它的应用：

before：

	public class Application {
		private final static Logger logger = Logger.getLogger(Application.class.getName());
	}

after:

	public class LoggerFactory {
		public static Logger create() {
			Throwable t = new Throwable();
			StackTraceElement caller = t.getStackTrace()[1];
			return Logger.getLogger(caller.getClassName());
		}
	}

	// in Application
	private final static Logger logger = LoggerFactory.create();

自己也曾因创建日志对象的时候，每次都要复制一下类的名称，而感到麻烦。Happy!😃

那么，使用原来的方式创建 Logger 对象，把 Class 对象作为参数传入时，日志框架都做了些什么呢？查看源码：

	public static Logger getLogger(Class<?> clazz) {
        Logger logger = getLogger(clazz.getName());
        if (DETECT_LOGGER_NAME_MISMATCH) {
            Class<?> autoComputedCallingClass = Util.getCallingClass();
            if (autoComputedCallingClass != null && nonMatchingClasses(clazz, autoComputedCallingClass)) {
                Util.report(String.format("Detected logger name mismatch. Given name: \"%s\"; computed name: \"%s\".", logger.getName(),
                                autoComputedCallingClass.getName()));
                Util.report("See " + LOGGER_NAME_MISMATCH_URL + " for an explanation");
            }
        }
        return logger;
    }

`getLogger(Class<?> clazz)` 内部将 Logger 的创建委托给了 `getLogger(String className)` 方法。然后，如果设置了 DETECT_LOGGER_NAME_MISMATCH 为 `true` ，会去检查传入的 Class 对象是否和调用者一致。此处也有一个获取调用者的方法（`Util.getCallingClass()`），源代码如下：

	public static Class<?> getCallingClass() {
        ClassContextSecurityManager securityManager = getSecurityManager();
        if (securityManager == null)
            return null;
        Class<?>[] trace = securityManager.getClassContext();
        String thisClassName = Util.class.getName();

        // Advance until Util is found
        int i;
        for (i = 0; i < trace.length; i++) {
            if (thisClassName.equals(trace[i].getName()))
                break;
        }

        // trace[i] = Util; trace[i+1] = caller; trace[i+2] = caller's caller
        if (i >= trace.length || i + 2 >= trace.length) {
            throw new IllegalStateException("Failed to find org.slf4j.helpers.Util or its caller in the stack; " + "this should not happen");
        }

        return trace[i + 2];
    }

两种方式孰优孰劣，暂时还不清楚。个人感觉第二种方式更优雅，也更稳健，于是更改了自己创建 Logger 对象的工具类，代码如下：

	public class LoggerUtil {
		public static Logger create(){
			ClassContextSecurityManager securityManager = getSecurityManager();
	        if (securityManager == null) return null;
	        Class<?>[] trace = securityManager.getClassContext();
	        String thisClassName = LoggerUtil.class.getName();

	        int i;
	        for (i = 0; i < trace.length; i++) {
	            if (thisClassName.equals(trace[i].getName()))
	                break;
	        }

	        return LoggerFactory.getLogger(trace[i + 1].getName());
		}
		
		private static final class ClassContextSecurityManager extends SecurityManager {
	        protected Class<?>[] getClassContext() {
	            return super.getClassContext();
	        }
	    }
		
		private static ClassContextSecurityManager SECURITY_MANAGER;
	    private static boolean SECURITY_MANAGER_CREATION_ALREADY_ATTEMPTED = false;

	    private static ClassContextSecurityManager getSecurityManager() {
	        if (SECURITY_MANAGER != null)
	            return SECURITY_MANAGER;
	        else if (SECURITY_MANAGER_CREATION_ALREADY_ATTEMPTED)
	            return null;
	        else {
	            SECURITY_MANAGER = safeCreateSecurityManager();
	            SECURITY_MANAGER_CREATION_ALREADY_ATTEMPTED = true;
	            return SECURITY_MANAGER;
	        }
	    }
		
		private static ClassContextSecurityManager safeCreateSecurityManager() {
	        try {
	            return new ClassContextSecurityManager();
	        } catch (java.lang.SecurityException sm) {
	            return null;
	        }
	    }
	}

~~over~~