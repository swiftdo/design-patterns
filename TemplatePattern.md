# 模板模式

定义一个操作中的算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。

## 样例

![](http://blog.oldbird.run/mweb/16176017312597.jpg)

```swift
protocol Game {
    func initialize()
    func start()
    func end()
}


extension Game {
    func play() {
        initialize()
        start()
        end()
    }
}

final class Cricket: Game {

    func initialize() {
        print("Cricket Game Initialized! Start playing.")
    }

    func end() {
        print("Cricket Game Finished!")
    }

    func start() {
        print("Cricket Game Started. Enjoy the game!")
    }
}


final class Football: Game {

    func initialize() {
        print("Football Game Initialized! Start playing.")
    }

    func end() {
        print("Football Game Finished!")
    }

    func start() {
        print("Football Game Started. Enjoy the game!")
    }
}


var game: Game = Cricket()
game.play()

game = Football()
game.play()
```

结果显示：

```swift
Cricket Game Initialized! Start playing.
Cricket Game Started. Enjoy the game!
Cricket Game Finished!
Football Game Initialized! Start playing.
Football Game Started. Enjoy the game!
Football Game Finished!
```
