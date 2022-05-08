# 中介者模式

用来降低多个对象和类之间的通信复杂性。这种模式提供了一个中介类，该类通常处理不同类之间的通信，并支持松耦合，使代码易于维护。

## 样例

```swift
class ChatUser {
    let name: String
    weak var mediator: ChatMediator?
    
    init(name: String, mediator: ChatMediator) {
        self.name = name
        self.mediator = mediator
    }
    
    func send(msg: String) {
        print("\(name):发送消息 = \(msg)")
        mediator?.sendMessage(msg: msg, user: self)
    }
    
    func receive(msg: String) {
        print("\(name):收到消息：\(msg)")
    }
}

class ChatMediator {
    
    private var users: [ChatUser] = []
    
    func sendMessage(msg: String, user: ChatUser) {
        users.filter { (it) -> Bool in
            return it.name != user.name
        }.forEach { (it) in
            it.receive(msg: msg)
        }
    }
    
    func add(user: ChatUser) {
        self.users.append(user)
    }
}

let mediator = ChatMediator()
var john = ChatUser(name: "John", mediator: mediator)

mediator.add(user: ChatUser(name: "Alice", mediator: mediator))
mediator.add(user: ChatUser(name: "Bob", mediator: mediator))
mediator.add(user: john)

john.send(msg: "Hi everyone!")
```

结果显示：

```sh
John:发送消息 = Hi everyone!
Alice:收到消息：Hi everyone!
Bob:收到消息：Hi everyone!
```