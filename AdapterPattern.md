
# 适配器模式

把一个类的接口变换成客户端所期待的另一种接口，从而使原本接口不匹配而无法一起工作的两个类能够在一起工作。

假如我的手机充电口是 type-c 类型的，但我只有一根传统的 Micro USB 接口的数据线，怎么办?

type-c 类型的接口就是需求接口，Micro USB 类型的接口是供应接口，想让需求匹配供应，就需要有一个适配器。

适配器的三种实现方式：

## 类适配器

当需要把一个已经实现了接口A的类A转换成能实现接口B的类时，可使用类适配器模式。

> 注意：类A是已经设计好的，不能改变。

创建一个新类，实现接口，继承原有的类即可。

```swift
/// 供应接口
protocol MicroUSB {
    func microUSB()
}

/// 需求接口
protocol TypeC {
    func typeC()
}

class Phone : TypeC {
    func typeC() {
        print("Type-C 接口")
    }
}

/// 适配器
class Adapter : Phone, MicroUSB {
    func microUSB() {
        typeC()
    }
}

let microUSB = Adapter()
microUSB.microUSB() // Type-C 接口

```

## 对象适配器

当希望将接口A的实现类A的对象转换成满足接口B的对象时，可使用对象适配器的方式。

通过组合方式实现适配器功能。

```swift
/// 供应接口
protocol MicroUSB {
    func microUSB()
}

/// 需求接口
protocol TypeC {
    func typeC()
}

class Phone : TypeC {
    func typeC() {
        print("Type-C 接口")
    }
}

/// 适配器
class Adapter: MicroUSB {
    
    private var typeC: TypeC
    
    init(typeC: TypeC) {
        self.typeC = typeC
    }
    
    func microUSB() {
        self.typeC.typeC()
    }
}

let typeC: TypeC = Phone()
let microUSB = Adapter(typeC: typeC)
microUSB.microUSB() // Type-C 接口
```

## 接口适配器

有一个很大的接口，包含很多方法，当不希望实现接口中的所有方法时，可创建一个抽象类继承接口，但方法实现都是空的，实现类可继承这个抽象类实现了适配器的功能。

日常用的转换器有很多种类型的接口，比如 type-C，VGA，Hdmi，可以把这些类型都放在一个大的接口中，当我只需type-C 和 VGA 类型的转换器时，继承相关的类即可。

```swift
/// 接口类
protocol AllPorts {
    func typeC()
    func vga()
    func hdmi()
}

/// 适配器
class Adapter : AllPorts {
    func typeC() { }
    func vga() { }
    func hdmi() { }
}

/// 适配器类
class TypeCToVGA : Adapter {
    override func typeC() {
        print("信号从TypeC接口进入")
    }
    override func vga() {
        print("信号从VGA接口出来")
    }
}

let typecToVGA: TypeCToVGA = TypeCToVGA()
typecToVGA.typeC()
typecToVGA.vga()
```

