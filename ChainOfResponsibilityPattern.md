# 责任链模式

避免请求发送者与接收者耦合在一起，让多个对象都有可能接收请求，将这些对象连接成一条链，并且沿着这条链传递请求，直到有对象处理它为止。

职责链上的处理者负责处理请求，客户只需要将请求发送到职责链上即可，无须关心请求的处理细节和请求的传递，所以职责链将请求的发送者和请求的处理者解耦了。

## 样例

![](http://blog.oldbird.run/mweb/16169408042714.jpg)

```swift

enum LogLevel: Int {
    case info = 1
    case debug = 2
    case error = 3
}

protocol Logger {
    var level: LogLevel { get set }
    var next: Logger? {get set}
    func log(level: LogLevel, message: String)
    func write(message: String)
}

extension Logger {

    func log(level: LogLevel, message: String) {
        if self.level.rawValue <= level.rawValue {
            write(message: message)
        }

        if let next = next {
            next.log(level: level, message: message)
        }
    }

}

final class ConsoleLogger: Logger {

    var level: LogLevel
    var next: Logger?

    init(level: LogLevel) {
        self.level = level
    }

    func write(message: String) {
        print("标准输出：\(message)")
    }

}

final class ErrorLogger: Logger {

    var level: LogLevel
    var next: Logger?

    init(level: LogLevel) {
        self.level = level
    }

    func write(message: String) {
        print("错误输出：\(message)")
    }
}

final class FileLogger: Logger {
    var level: LogLevel
    var next: Logger?

    init(level: LogLevel) {
        self.level = level
    }

    func write(message: String) {
        print("写入文件：\(message)")
    }
}


func getLoggersChain() -> Logger {
    let errorLogger = ErrorLogger(level: .error)
    let fileLogger = FileLogger(level: .debug)
    let consoleLogger = ConsoleLogger(level: .info)

    errorLogger.next = fileLogger
    fileLogger.next = consoleLogger
    return errorLogger
}

let loggerChain: Logger = getLoggersChain()

loggerChain.log(level: .info, message: "这是一条普通信息")
loggerChain.log(level: .debug, message: "这是一条Debug信息")
loggerChain.log(level: .error, message: "这是一条Error信息")

```

结果显示：

```sh
标准输出：这是一条普通信息
写入文件：这是一条Debug信息
标准输出：这是一条Debug信息
错误输出：这是一条Error信息
写入文件：这是一条Error信息
标准输出：这是一条Error信息
```
