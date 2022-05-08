# 解释器模式

定义一个语言的文法，并且建立一个解释器来解释该语言中的句子，这里的“语言”是指使用规定格式和语法的代码。

## 样例

![](http://blog.oldbird.run/mweb/16171114899773.jpg)

如下示例简单模拟了加减乘除整数算术运算操作，使用了解释器模式：

```swift
/// 环境角色
class Context {

    private var valueMap: [Character: Int] = [:]

    func lookupValue(_ name: Character) -> Int {
        return self.valueMap[name]!
    }

    func assign(variable: Variable, value: Int) {
        self.valueMap[variable.name] = value
    }

}

/// 抽象表达式
protocol Expression {
    func interpret(context: Context) -> Int
}

/// 终结表达式
class Constant: Expression {

    private var value: Int

    init(_ value: Int) {
        self.value = value
    }

    func interpret(context: Context) -> Int {
        return value
    }
}

class Variable:Expression {

    let name: Character

    init(name: Character) {
        self.name = name
    }
    func interpret(context: Context) -> Int {
        return context.lookupValue(self.name)
    }
}

/// 非终结表达式
class Add: Expression {

    private var left: Expression
    private var right:Expression

    init(left: Expression, right: Expression) {
        self.left = left
        self.right = right
    }

    func interpret(context: Context) -> Int {
        return left.interpret(context: context) + right.interpret(context: context)
    }
}

class Sub: Expression {

    private var left: Expression
    private var right:Expression

    init(left: Expression, right: Expression) {
        self.left = left
        self.right = right
    }

    func interpret(context: Context) -> Int {
        return left.interpret(context: context) - right.interpret(context: context)
    }
}

class Mul: Expression {

    private var left: Expression
    private var right:Expression

    init(left: Expression, right: Expression) {
        self.left = left
        self.right = right
    }

    func interpret(context: Context) -> Int {
        return left.interpret(context: context) * right.interpret(context: context)
    }
}

class Div: Expression {

    private var left: Expression
    private var right:Expression

    init(left: Expression, right: Expression) {
        self.left = left
        self.right = right
    }

    func interpret(context: Context) -> Int {
        return left.interpret(context: context) / right.interpret(context: context)
    }
}

var a = Variable(name: "A")
var b = Variable(name: "B")
var c = Variable(name: "C")

// 创建一个表达式
var expression: Expression = Sub(left: a, right: Add(left: b, right: c)) // a - (b + c)

var context = Context()
context.assign(variable: a, value: 10)
context.assign(variable: b, value: 5)
context.assign(variable: c, value: 1)

print("结果：\(expression.interpret(context: context))")
```

结果显示：

```sh
结果：4
```
