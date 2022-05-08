# 代理模式

为其他对象提供一种代理以控制对这个对象的访问。

## 案例

![](http://blog.oldbird.run/mweb/16169310879998.jpg)

```swift
/// 租房接口
protocol IRentHouse {
    func rentHouse()
}

class RentHouse: IRentHouse {
    func rentHouse() {
        print("租了一间房子...")
    }
}

/// 中介
class IntermediaryProxy: IRentHouse {
    private var rHouse: IRentHouse

    init(rentHouse: IRentHouse) {
        self.rHouse = rentHouse
    }

    func rentHouse() {
        print("交中介费")
        rHouse.rentHouse()
        print("中介负责维修管理")
    }
}

let rentHouse: IRentHouse = RentHouse()
let intermediary = IntermediaryProxy(rentHouse: rentHouse)
intermediary.rentHouse()
```

结果显示：

```sh
交中介费
租了一间房子...
中介负责维修管理
```

## 与其他模式的关系

- 适配器模式能为被封装对象提供不同的接口， 代理模式能为对象提供相同的接口， 装饰模式则能为对象提供加强的接口。

- 外观模式与代理的相似之处在于它们都缓存了一个复杂实体并自行对其进行初始化。 代理与其服务对象遵循同一接口， 使得自己和服务对象可以互换， 在这一点上它与外观不同。

- 装饰和代理有着相似的结构，但是其意图却非常不同。 这两个模式的构建都基于组合原则，也就是说一个对象应该将部分工作委派给另一个对象。 两者之间的不同之处在于代理通常自行管理其服务对象的生命周期， 而装饰的生成则总是由客户端进行控制。
