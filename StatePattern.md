# 状态模式

允许对象在内部状态发生改变时改变它的行为，对象看起来好像修改了它的类。

## 样例

![](http://blog.oldbird.run/mweb/16176017312597.jpg)

```swift
class Context : CustomDebugStringConvertible {
    private var state: State = UnauthorizedState()

    var isAuthorized: Bool {
        get {
            return state.isAuthorized(context: self)
        }
    }

    var userId: String? {
        get {
            return state.userId(context: self)
        }
    }

    func changeStateToAuthorized(userId: String) {
        state = AuthorizedState(userId: userId)
    }

    func changeStateToUnauthorized() {
        state = UnauthorizedState()
    }

    var debugDescription: String {
        return "isAuthorized: \(isAuthorized), userId:\(userId ?? "")"
    }
}

protocol State {
    func isAuthorized(context: Context) -> Bool
    func userId(context: Context) -> String?
}

class UnauthorizedState: State {
    func isAuthorized(context: Context) -> Bool {
        false
    }
    func userId(context: Context) -> String? {
        nil
    }
}

class AuthorizedState: State {
    let userId: String

    init(userId: String) {
        self.userId = userId
    }

    func isAuthorized(context: Context) -> Bool {
        true
    }

    func userId(context: Context) -> String? {
        userId
    }
}

let userContext = Context()

print(userContext)
userContext.changeStateToAuthorized(userId: "oldbird")
print(userContext)

userContext.changeStateToUnauthorized()
print(userContext)
```

结果显示：

```swift
isAuthorized: false, userId:
isAuthorized: true, userId:oldbird
isAuthorized: false, userId:
```
