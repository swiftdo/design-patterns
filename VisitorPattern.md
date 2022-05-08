# 访问者模式

将算法与其所作用的对象隔离开来。

将作用于某种数据结构中的各元素的操作分离出来封装成独立的类，使其在不改变数据结构的前提下可以添加作用于这些元素的新的操作，为数据结构中的每个元素提供多种访问方式

## 样例

![](http://blog.oldbird.run/mweb/16176017312597.jpg)

为啥要有访问者模式呢？我们从一个很简单的表达式求值的例子说起：

```swift
protocol Expr {}

struct Add: Expr {
    var lhs: Expr
    var rhs: Expr
}

struct Sub: Expr {
    var lhs: Expr
    var rhs: Expr
}

struct Number: Expr {
    var value: Int
}
```

我们先定义了表达式的 AST，包括叶子结点 Number 表示一个整数，中间结点 Add 和 Sub 分别表示加法和减法。 然后我们希望写一个方法，给定表达式，求其值。一种很自然的写法：

```swift
func eval(expr: Expr) -> Int {
    if let ex = expr as? Add {
        return eval(expr: ex.lhs) + eval(expr: ex.rhs)
    }
    else if let ex = expr as? Sub {
        return eval(expr: ex.lhs) - eval(expr: ex.rhs)
    }
    else {
        return (expr as! Number).value
    }
}
```

这样确实能达到目的，但是反复的 `as?` 类型转换，代码太过丑陋了。

```swift

protocol Expr {
    func accept(visitor: ExprVisitor) -> Int
}

struct Add: Expr {
    var lhs: Expr
    var rhs: Expr

    func accept(visitor: ExprVisitor) -> Int {
        return visitor.visitAdd(e: self)
    }
}

struct Sub: Expr {
    var lhs: Expr
    var rhs: Expr

    func accept(visitor: ExprVisitor) -> Int {
        visitor.visitSub(e: self)
    }
}

struct Number: Expr {
    var value: Int
    func accept(visitor: ExprVisitor) -> Int {
        visitor.visitNumber(e: self)
    }
}


protocol ExprVisitor {
    func visitAdd(e: Add) -> Int
    func visitSub(e: Sub) -> Int
    func visitNumber(e: Number) -> Int
}

class EvalVisitor: ExprVisitor {
    func visitAdd(e: Add) -> Int {
        return e.lhs.accept(visitor: self) + e.rhs.accept(visitor: self)
    }

    func visitSub(e: Sub) -> Int {
        return e.lhs.accept(visitor: self) - e.rhs.accept(visitor: self)
    }

    func visitNumber(e: Number) -> Int {
        return e.value
    }
}

func eval(expr: Expr) -> Int {
    let v = EvalVisitor()
    return expr.accept(visitor: v)
}

// (2 + 4) + (5 - 4)
let expr = Add(lhs: Add(lhs: Number(value: 2), rhs: Number(value: 4)), rhs: Sub(lhs: Number(value: 5), rhs: Number(value: 4)))
print(eval(expr: expr))
```

结果显示：

```
7
```
