# 命令模式

将一个请求封装成一个对象，从而使您可以用不同的请求对客户进行参数化。

在软件系统中，行为请求者与行为实现者通常是一种紧耦合的关系，但某些场合，比如需要对行为进行记录、撤销或重做、事务等处理时，这种无法抵御变化的紧耦合的设计就不太合适。

## 样例

![](http://blog.oldbird.run/mweb/16171111898819.jpg)

```swift
protocol Command {
    func execute()
    func undo()
}

class Content : CustomDebugStringConvertible{
    var msg: String

    init(_ msg: String) {
        self.msg = msg
    }

    var debugDescription: String {
        return msg
    }
}

extension String {
    subscript(_ indexs: Range<Int>) -> String {
        let beginIndex = index(startIndex, offsetBy: indexs.lowerBound)
        let endIndex = index(startIndex, offsetBy: indexs.upperBound)
        return String(self[beginIndex..<endIndex])
    }
}

class CopyCommand: Command {
    private var content: Content

    init(content: Content) {
        self.content = content
    }

    func execute() {
        content.msg += content.msg
    }

    func undo() {
        content.msg = content.msg[0..<content.msg.count/2]
    }
}

class DeleteCommand: Command {
    private var content: Content
    private var deleted: String?

    init(content: Content) {
        self.content = content
    }

    func execute() {
        deleted = content.msg[0..<5]
        content.msg = content.msg[5..<content.msg.count]
    }

    func undo() {
        if let del = deleted {
            content.msg = del + content.msg
        }
    }
}

class InsertCommand: Command {
    private var content: Content
    private var ins: String = "https://oldbird.run"

    init(content: Content) {
        self.content = content
    }

    func execute() {
        content.msg += ins
    }

    func undo() {
        content.msg = content.msg[0..<content.msg.count - ins.count]
    }
}

let content = Content("hello oldbirds.")

print("原始数据：\(content)")
let insertCommand: Command = InsertCommand(content: content)
insertCommand.execute()
insertCommand.undo()

let copyCommand: Command = CopyCommand(content: content)
copyCommand.execute()
copyCommand.undo()


let deletedCommand: Command = DeleteCommand(content: content)
deletedCommand.execute()
deletedCommand.undo()

let commands: [Command] = [InsertCommand(content: content), CopyCommand(content: content), DeleteCommand(content: content)]

commands.forEach { $0.execute()}
commands.reversed().forEach { $0.undo() }

print("最终数据：\(content)")
```

结果显示：

```sh
原始数据：hello oldbirds.
最终数据：hello oldbirds.
```
