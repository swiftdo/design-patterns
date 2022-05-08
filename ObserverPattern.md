# 观察者模式

定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。

## 样例

![](http://blog.oldbird.run/mweb/16176017312597.jpg)

```swift
class Subject {

    private var observes: [Observer] = []

    private var _state: Int = 0

    var state: Int {
        set {
            _state = newValue
            notify()
        }
        get {
            return _state
        }
    }

    func attach(observer: Observer) {
        observes.append(observer)
    }

    func deattach(observer: Observer) {
        guard let index = observes.firstIndex(where: {$0.id == observer.id}) else {return}
        observes.remove(at: index)
    }

    func notify() {
        observes.forEach { (obs) in
            obs.update()
        }
    }
}

protocol Observer {
    var id : String {get set}
    var subject: Subject { get set }
    func update()
}

class BinaryObserver: Observer {
    var subject: Subject
    var id: String

    init(id: String, subject: Subject) {
        self.id = id
        self.subject = subject
        self.subject.attach(observer: self)
    }

    func update() {
        let val = String(subject.state, radix: 2, uppercase: true)
        print("二进制: \(val)")
    }
}

class OctalObserver: Observer {
    var subject: Subject
    var id: String

    init(id: String, subject: Subject) {
        self.id = id
        self.subject = subject
        self.subject.attach(observer: self)
    }

    func update() {
        let val = String(subject.state, radix: 8, uppercase: true)
        print("八进制: \(val)")
    }
}

class HexaObserver: Observer {
    var subject: Subject
    var id: String

    init(id: String, subject: Subject) {
        self.id = id
        self.subject = subject
        self.subject.attach(observer: self)
    }

    func update() {
        let val = String(subject.state, radix: 16, uppercase: true)
        print("十六进制: \(val)")
    }
}

let subject = Subject()

HexaObserver(id: "hexa", subject: subject)
OctalObserver(id: "octal", subject: subject)
BinaryObserver(id: "binary", subject: subject)


print("输入5：")
subject.state = 5


print("\n输入10")
subject.state = 10



```

结果显示：

```swift
输入5：
十六进制: 5
八进制: 5
二进制: 101

输入10
十六进制: A
八进制: 12
二进制: 1010
```
