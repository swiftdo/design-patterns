# 策略模式

定义一系列的算法,把它们一个个封装起来, 并且使它们可相互替换。

在有多种算法相似的情况下，使用 if...else 所带来的复杂和难以维护。

## 样例

![](http://blog.oldbird.run/mweb/16171987137559.jpg)

```swift
protocol Strategy {
    func operation(a: Int, b: Int) -> Int
}

class OperationAdd: Strategy {
    func operation(a: Int, b: Int) -> Int {
        a + b
    }
}

class OperationSubstract: Strategy {
    func operation(a: Int, b: Int) -> Int {
        a - b
    }
}

class OperationMultiply: Strategy {
    func operation(a: Int, b: Int) -> Int {
        a * b
    }
}

class Context {
    private var strategy: Strategy

    init(strategy: Strategy) {
        self.strategy = strategy
    }

    func executeStrategy(a: Int, b:Int) {
        strategy.operation(a: a, b: b)
    }
}

var context = Context(strategy: OperationAdd())
print("10 + 5 = \(context.executeStrategy(a: 10, b: 5))")

context = Context(strategy: OperationSubstract())
print("10 - 5 = \(context.executeStrategy(a: 10, b: 5))")

context = Context(strategy: OperationMultiply())
print("10 * 5 = \(context.executeStrategy(a: 10, b: 5))")
```

结果显示：

```sh
10 + 5 = 15
10 - 5 = 5
10 * 5 = 50
```
