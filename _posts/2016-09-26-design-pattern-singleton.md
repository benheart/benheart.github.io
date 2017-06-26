---
layout: post
title: 设计模式之单例
description: 单例模式(Singleton Pattern)是常见的设计模式之一，属于创建型的设计模式，它提供一种创建对象的最佳方式。
key: design-pattern, Singleton
---

### 单例简介
**特点**:
- 单例类只能有一个实例。
- 单例类必须自己创建自己的唯一实例。
- 单例类必须给所有其他对象提供这一实例。

**目的**: 保证一个类仅有一个实例，并提供一个访问它的全局访问点。  
**主要解决**: 一个全局使用的类频繁地创建与销毁。  
**何时使用**: 想控制实例数目，节省系统资源的时候。  
**如何解决**: 判断系统是否已经有这个单例，有则返回，无则创建。

### 实现方式
#### 懒汉式，线程不安全
```java
public class Singleton {
    private static Singleton instance;
    private Singleton (){}
  
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

#### 懒汉式，线程安全
```java
public class Singleton {
    private static Singleton instance;
    private Singleton (){}
    
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

#### 饿汉式
```java
public class Singleton {
    private static Singleton instance = new Singleton();
    private Singleton (){}
    
    public static Singleton getInstance() {
        return instance;
    }
}
```

#### 双检锁/双重校验锁（DCL，即 double-checked locking）
```java
public class Singleton {
    private volatile static Singleton singleton;
    private Singleton (){}
    
    public static Singleton getSingleton() {
        if (singleton == null) {
            synchronized (Singleton.class) {
                if (singleton == null) {
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }
}
```

#### 登记式/静态内部类
```java
public class Singleton {
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    private Singleton (){}
    
    public static final Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```

#### 枚举
```java
public enum Singleton {
    INSTANCE;
}
```