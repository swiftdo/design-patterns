# 空对象模式

通过实现一个默认的无意义对象来避免 null 值出现，简单地说，就是为了避免在程序中出现 null 值判断而诞生的一种常用设计方法。

在大多数面向对象的编程语言中，我们会考虑不进行 null 引用，为此我们经常被迫写空检查：

```swift
let cmd: Command = getCommand()
if let cmd = cmd {
    cmd.execute()
}
```

有时，如果此类 if 语句的数量变多，则代码的可读性会变得很差，难以阅读且容易出错。这时 Null 对象模式可以派上用场。

## 样例

![](http://blog.oldbird.run/mweb/16172006654866.jpg)

```swift
protocol ICustomer {
    var name: String {get}
    func isNil() -> Bool
}

class RealCustomer: ICustomer {
    var name: String

    init(name: String) {
        self.name = name
    }

    func isNil() -> Bool {
        false
    }
}

class NullCustomer: ICustomer {
    var name: String {
        return "Not Available in Customer Database"
    }

    func isNil() -> Bool {
        true
    }
}


class CustomerFactory {

    final let names: [String] = ["Rob", "Joe", "Julie"]

    func getCustomer(_ name: String) ->ICustomer {
        for n in names where n == name {
            return RealCustomer(name: name)
        }
        return NullCustomer()
    }
}

let factory = CustomerFactory()

let c1 = factory.getCustomer("Rob")
let c2 = factory.getCustomer("Bob")
let c3 = factory.getCustomer("Julie")
let c4 = factory.getCustomer("Laura")

print("Customers")
print(c1.name)
print(c2.name)
print(c3.name)
print(c4.name)
```

结果显示：

```sh
Customers
Rob
Not Available in Customer Database
Julie
Not Available in Customer Database
```
