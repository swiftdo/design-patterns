# 里氏代换原则

里氏代换原则（Liskov Substitution Principle，LSP）的定义是：所有引用基类的地方必须能透明地使用其子类的对象，也可以简单理解为任何基类可以出现的地方，子类一定可以出现。

## 里氏代换原则的意义

只有当衍生类可以替换掉基类，软件单位的功能不受到影响时，基类才能真正被复用，而衍生类也能够在基类的基础上增加新的行为。里氏代换原则是对"开-闭"原则的补充。实现"开-闭"原则的关键步骤就是抽象化。而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。当然，如果反过来，软件单位使用的是一个子类对象的话，那么它不一定能够使用基类对象。举个很简单的例子说明这个问题：如果一个方法接收 Map 类型参数，那么它一定可以接收 Map 的子类参数例如 HashMap、LinkedHashMap、ConcurrentHashMap 类型的参数；但是反过来，如果另一个方法只接收 HashMap 类型的参数，那么它一定不能接收所有 Map 类型的参数，否则它可以接收 LinkedHashMap、ConcurrentHashMap 类型的参数。

> 这里的 Map, HashMap、LinkedHashMap、ConcurrentHashMap，是 JAVA 的相关类。

## 子类为什么可以替换基类的位置

其实原因很简单，只要存在继承关系，基类的所有非私有属性或者方法，子类都可以通过继承获得(白箱复用)，反过来不成立，因为子类很有可能扩充自身的非私有属性或者方法，这个时候不能用基类获取子类新增的这些属性或者方法。

里氏代换原则是实现开闭原则的基础，它告诉我们在设计程序的时候进可能使用基类进行对象的定义和引用，在运行时再决定基类的具体子类型。

举个简单的例子，假设一种会呼吸的动物作为父类，子类猪和鸟也有自身的呼吸方式：

```swift
protocol Animal {
    func breathe()
}

class Bird: Animal {
    func breathe() {
        print("Bird breathes...")
    }
}

class Pig: Animal {
    func breathe() {
        print("Pig breathes...")
    }
}

let bird: Animal = Bird()
bird.breathe()

let pig: Animal = Pig()
pig.breathe()
```
