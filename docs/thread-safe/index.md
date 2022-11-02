---
title: 线程安全性
date: "2020-10-21"
spoiler: thread safe.
---

首先使代码正确运行，然后再提高代码速度。【正确编写并发程序的方法】

## 知识点

1. 竞态条件。
   当某个计算的正确性取决于多个线程的交替执行时序。最常见的竞态条件类型
1. 先检查后执行（Check-Then-Act）
1. 对象的状态。
   存储在状态变量（例如实例或静态域）中的数据。
1. 共享。
   变量可以由多个线程同时访问；
1. 可变。
   变量的值在其生命周期内可以发生变化；
1. 线程安全性。
   当多个线程访问某个类时，不管运行时环境采用何种调度方式或者这些线程将如何交替执行，并且在主调代码不需要任何交替执行，并且在主调代码中不需要任何额外的同步或协同，这个类都能表现出正确的行为。无状态对象一定是线程安全的

---

## 怎么做到线程安全

- `synchronized`
  在多个线程访问某个状态变量并且其中有一个线程执行写入时，**协同**这些线程对变量的访问。

```java
public synchronized int getNext(){return value++;}
```

### 避免竞态条件

在某个线程修改变量时，以原子方式执行。

1. 使用现有的线程安全类。例如 AtomicLong、AtomicInteger 等在 java.util.concurrent.atomic 包中的原子类。
1. 对于每个包含多个变量的不变性条件，其中涉及的所有变量都需要由同一个锁来保护。
1. 加锁【**互斥锁**】机制（java 内置的锁机制【同步代码块】）.

```java
synchronized(object){// 访问或修改由锁保护的共享状态}
```

每个 java 对象都可以用做一个实现同步的锁（可重入）。

### 加锁约定

1. 将所有的可变状态都封装在对象内部，并通过对象的内置锁对所有访问可变状态的代码路径进行同步。
1. 当执行时间较长的计算或者可能无法快速完成的操作时，**一定不要持有锁**。
1. 尽量将

```java
synchronized(lock)
```

控制在合理的大小。