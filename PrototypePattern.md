# 原型模式

原型模式（Prototype Pattern）是用于创建重复的对象，同时又能保证性能。

原型模式是通过现有对象的所有属性来复制自身，创建一个新的对象。当类的实例间差异仅是属性值的不同时，这种做法将比手工实例化更加方便。


```swift
protocol Prototype {
    func clone() -> Self
}

protocol Product : Prototype {
    func use(s: String)
}

/// 它的作用是可以给字符创添加类似方框的边界的图案。
final class MessageBox: Product {
    private var decochar: Character
    
    init(decochar: Character) {
        self.decochar = decochar
    }
    
    func use(s: String) {
        let length = s.count
        print(String(repeating: "\(decochar)", count: length + 4))
        print("\(decochar) " + s + " \(decochar)")
        print(String(repeating: "\(decochar)", count: length + 4))
    }
    
    func clone() -> MessageBox {
        return MessageBox.init(decochar: decochar)
    }
}

/// 给字符串添加下划线的类UnderlinePen
final class UnderlinePen: Product {
    private var ulchar: Character
    init(ulchar: Character) {
        self.ulchar = ulchar
    }
    
    func use(s: String) {
        let length = s.count
        print("\"" + s + "\"")
        print(" " + String(repeating: ulchar, count: length))
    }
    
    func clone() -> UnderlinePen {
        UnderlinePen(ulchar: ulchar)
    }
}

/// 最后我们新建一个管理类Manager来管理这些对象的创建，复制和使用。
class Manager {
    var showcase:[String: Product] = [:]
    
    func register(name: String, proto: Product) {
        showcase[name] = proto
    }
    
    func create(protoname: String) -> Product? {
        let product = showcase[protoname]
        return product?.clone()
    }
}


let manager = Manager()
let upen = UnderlinePen(ulchar: "~")
let mbox = MessageBox(decochar: "*")
let sbox = MessageBox(decochar: "/")

manager.register(name: "strong message", proto: upen)
manager.register(name: "warning box", proto: mbox)
manager.register(name: "slash box", proto: sbox)

let p1 = manager.create(protoname: "strong message")
p1?.use(s: "Hello, Oldbirds.")

print("")

let p2 = manager.create(protoname: "warning box")
p2?.use(s: "Hello, Oldbirds.")

print("")

let p3 = manager.create(protoname: "slash box")
p3?.use(s: "Hello, Oldbirds.")
```

最后输出：

```sh
"Hello, Oldbirds."
 ~~~~~~~~~~~~~~~~

********************
* Hello, Oldbirds. *
********************

////////////////////
/ Hello, Oldbirds. /
////////////////////
```