# 桥接模式

将抽象部分与它的实现部分分离，使它们都可以独立地变化。通过提供抽象化和实现化之间的桥接结构，来实现二者的解耦。

我们有一个作为桥接实现的 TV 接口和实现了 TV 接口的实体类 Haier、KONKA，RemoteControl 是一个接口，将使用 TV 的对象。

```swift
protocol TV {
    func on()
    func off()
    func tuneChannel(channel: Int)
}

/// 海尔电视
class Haier : TV {
    func on() {
        print("Haier on")
    }
    func off() {
        print("Haier off")
    }
    func tuneChannel(channel: Int) {
        print("Haier 切换到 \(channel) 频道")
    }
}

/// 康佳电视
class KONKA : TV {
    func on() {
        print("KONKA on")
    }
    func off() {
        print("KONKA off")
    }
    func tuneChannel(channel: Int) {
        print("KONKA 切换到 \(channel) 频道")
    }
}

protocol RemoteControl {
    var tv: TV{get set}
    
    func on()
    func off()
    func setChannel(channel: Int)
}

extension RemoteControl {
    func on() {
        tv.on()
    }
    
    func off() {
        tv.off()
    }
    
    func setChannel(channel: Int) {
        tv.tuneChannel(channel: channel)
    }
}

/// 万能遥控器
class ConcreteRemote : RemoteControl {
    
    var tv: TV
    
    var channel: Int
    
    init(tv: TV, channel: Int = 0) {
        self.tv = tv
        self.channel = channel
    }
        
    func nextChannel() {
        self.channel += 1
        setChannel(channel: channel)
    }
    
    func previousChannel() {
        self.channel -= 1
        setChannel(channel: channel)
    }
}


let haierRemote = ConcreteRemote(tv: Haier())
haierRemote.on()
haierRemote.nextChannel()
haierRemote.off()

print("\n")
let konkaRemove = ConcreteRemote(tv: KONKA(), channel: 3)
konkaRemove.on()
konkaRemove.previousChannel()
konkaRemove.off()

```

结果将输出：

```sh
Haier on
Haier 切换到 1 频道
Haier off


KONKA on
KONKA 切换到 2 频道
KONKA off
```

桥接模式中的所谓脱耦，就是指在一个软件系统的抽象化和实现化之间使用关联关系（组合或者聚合关系）而不是继承关系，从而使两者可以相对独立地变化，这就是桥接模式的用意。