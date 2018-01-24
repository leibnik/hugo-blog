+++
title = "观察者模式"
date = "2016-03-06"
draft = true
tags = ["设计模式"]
categories = ["设计模式"]
+++
#### 定义
观察者模式中有两个角色，被观察者与观察者，它们之间是一种一对多的依赖关系，当被观察者的状态发生改变时，观察者也发生相应的行为。在现实生活中很容易找出这样的例子，比如教室里的教师和学生听到下课铃声响了之后下课休息，教师和学生是观察者，下课铃是被观察者，下课铃响了也就是被观察者的状态发生改变，教师和学生这些观察者做出相应的行动。另外，观察者模式也可以称为订阅-发布模式，因为观察者与被观察者的一系列活动可以简单概括为订阅和发布。使用类图表示观察者模式，如下图

<!-- more -->

![](http://ww4.sinaimg.cn/mw690/006nezkigw1f1mzq41atkj30e20aqq3c.jpg)

`subject` 可以是一个抽象类或普通类， `concreteSubject` 是其子类，是具体的被观察者， `observer` 是一个只包含 `update(Object)` 方法的接口，实现该接口的 `ConcreteObserver` 为具体的观察者。在 jdk 中就有内置的类型 `Observable` ， `Observer` 方便开发者使用观察者模式。被观察者需要继承 `Observable` 类，而被观察者则需要实现 `Observer` 接口。

再举个例子，学生听到广播后做出行为。代码如下：

观察者 Student

```java
//Student.java

public class Student implements Observer {

    private String name;

    public Student(String name){
        this.name = name;
    }

    @Override
    public void update(Observable o, Object arg) {
        System.out.println(name +","+ arg);
    }
}
```

被观察者 Broadcast

```java
// Brodcast.java

public class Broadcast extends Observable {

    public void  sendBroadcast(String content){

        // 标识状态或内容发生改变
        setChanged();

        // 通知所有观察者
        notifyObservers(content);
    }

}
```

测试类

```java
// Test.java

public class Test {
    public static void main(String[] args) {

        Broadcast broadcast = new Broadcast();

        Student student1 = new Student("黄同学");
        Student student2 = new Student("王同学");
        Student student3 = new Student("杨同学");

        brocast.addObserver(student1);
        brocast.addObserver(student2);
        brocast.addObserver(student3);

        brocast.sendBroadcast("听到广播后请到办公室一趟！");
    }
}
```

结果输出

```java
杨同学,听到广播后请到办公室一趟！
王同学,听到广播后请到办公室一趟！
黄同学,听到广播后请到办公室一趟！
```

观察者无需时刻观察着被观察者，就如以上两个例子，学生无需时刻关注下课铃声以及广播，只有当被观察者状态改变，观察者才能有下一步的行动。

Observer 和 Observable 的实现也很简单，以下就是源码：

Observer只是一个接口

```java
// Observer.java

public interface Observer {
    void update(Observable o, Object arg);
}
```

Observable 需要确保相关操作是线程安全的，同时使用一个 Vector 来存放观察者对象。

```java
// Observable.java

public class Observable {

    private boolean changed = false;

    private Vector obs;

    public Observable() {
        obs = new Vector();
    }

    public synchronized void addObserver(Observer o) {
        if (o == null)
            throw new NullPointerException();
        if (!obs.contains(o)) {
            obs.addElement(o);
        }
    }

    public synchronized void deleteObserver(Observer o) {
        obs.removeElement(o);
    }
    
    public void notifyObservers() {
        notifyObservers(null);
    }

    public void notifyObservers(Object arg) {

        Object[] arrLocal;

        synchronized (this) {
            if (!changed)
                return;
            arrLocal = obs.toArray();
            clearChanged();
        }

        for (int i = arrLocal.length-1; i>=0; i--)
            ((Observer)arrLocal[i]).update(this, arg);
    }
 
    public synchronized void deleteObservers() {
        obs.removeAllElements();
    }

    protected synchronized void setChanged() {
        changed = true;
    }

    protected synchronized void clearChanged() {
        changed = false;
    }

    public synchronized boolean hasChanged() {
        return changed;
    }

    public synchronized int countObservers() {
        return obs.size();
    }
}

```

可以看到， Observable 内部使用一个 Vector 来存储观察者对象，并且提供增加,删除以及通知观察者角色的方法，还有一点需要注意的就是， `notifyObservers(Object arg)` 方法内部有一个判断标识，因此具体的被观察者角色需要先调用 `setChanged()` 以标识状态发生改变， `notifyObservers(Object arg)` 方法才能有效。

当然，我们完全可以自己实现抽象的主题角色和抽象的观察者角色，只是 jdk 已经帮我们实现了，无需我们重复造轮子而已。


