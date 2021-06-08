# 工厂模式

工厂模式根据抽象程度的不同分为三种：简单工厂模式（也叫静态工厂模式）、工厂方法模式、抽象工厂模式。

## 简单工厂模式

可以根据参数的不同返回不同类的实例。

简单工厂模式专门定义一个类来负责创建其他类的实例，被创建的实例通常都具有共同的父类。

### Button 样例

```swift
enum Style {case mac, win}

protocol Button {}
class MacButton: Button {}
class WinButton: Button {}

class ButtonFactory {
    func createButton(style: Style) -> Button {
        switch style {
        case .mac:
            return MacButton()
        default:
            return WinButton()
        }
    }
}

var style: Style = .mac
var fac = ButtonFactory()
fac.createButton(style: style)
```

### Shape 样例

```swift
protocol Shape {
    func draw()
}

class Rectangle : Shape {
    func draw() {
        print("Draw Rectangle")
    }
}

class Square: Shape {
    func draw() {
        print("Draw Square")
    }
}

class Circle: Square {
    override func draw() {
        print("Draw Circle")
    }
}

class ShapeFactory {
    func getShape(shapeType: String) -> Shape? {
        if shapeType == "CIRCLE" {
            return Circle()
        } else if shapeType == "RECTANGLE" {
            return Rectangle()
        } else if  shapeType == "SQUARE" {
            return Square()
        }
        return nil
    }
}

let facctory = ShapeFactory()
let circle = facctory.getShape(shapeType: "CIRCLE")
circle?.draw()
```

## 工厂方法模式

工厂方法模式是简单工厂的进一步深化，在工厂方法模式中，不再提供一个统一的工厂类来创建所有的对象，而是针对不同的对象提供不同的工厂。也就是说每个对象都有一个与之对应的工厂。

定义一个创建对象的接口，但让实现这个接口的类来决定实例化哪个类。工厂方法让类的实例化推迟到子类中进行

### Reader 样例

例如，有一个Reader类表示图片读取，另有它的三个子类 JPGReader， PNGReader和 GIFReader 分别代表 jpg、png 和 gif 读取。

```swift
protocol Reader {
    func read()
}

class JPGReader : Reader {
    func read() {
        print("read jpg")
    }
}


class PNGReader: Reader {
    func read() {
        print("read png")
    }
}

class GIFReader: Reader {
    func read() {
        print("read gif")
    }
}

protocol ReaderFactory {
    func getReader() -> Reader
}

class JPGReaderFactory : ReaderFactory {
    func getReader() -> Reader {
        JPGReader()
    }
}

class PNGReaderFactory: ReaderFactory {
    func getReader() -> Reader {
        PNGReader()
    }
}

class GIFReaderFactory: ReaderFactory {
    func getReader() -> Reader {
        GIFReader()
    }
}

var factory: ReaderFactory = GIFReaderFactory()
var reader: Reader = factory.getReader()
reader.read() // read gif

factory = PNGReaderFactory()
reader = factory.getReader()
reader.read() // read png

factory = JPGReaderFactory()
reader = factory.getReader()
reader.read() // read jpg
```

### Button 样例

例如，有一个Button类表示按钮，另有它的两个子类WinButton和MacButton分别代表Windows和Mac风格的按钮。

```swift
protocol Button {}

class MacButton: Button {}
class WinButton: Button {}

protocol ButtonFactory {
    func createButton() -> Button
}

class WinButtonFactory : ButtonFactory {
    func createButton() -> Button { WinButton() }
}
class MacButtonFactory: ButtonFactory {
    func createButton() -> Button { MacButton() }
}

enum Style {case mac, win}

var style: Style = .mac
var fac: ButtonFactory
switch style {
case .mac:
    fac =  MacButtonFactory()
default:
    fac = WinButtonFactory()
}

fac.createButton()
```


## 抽象工厂模式

为创建一组相关或相互依赖的对象提供一个接口，而且无需指定他们的具体类。

### 汽车样例

假设我们有两种产品接口 BenzCar、AudiCar 和 BmwCar ，每一种产品都支持多种系列，比如 Sport 系列和 Business 系列。这样每个系列的产品分别是 BenzSportCar, BenzBusinessCar, BmwSportCar, BmwBusinessCar，AudiSportCar，AudiBusinessCar 。为了可以在运行时刻创建一个系列的产品族，我们可以为每个系列的产品族创建一个工厂 SupportDriver 和 BusinessDriver 。每个工厂都有三个方法 createBenzCar ，createBmwCar 和 createAudiCar 并返回对应的产品，可以将这三个方法抽象成一个接口 Driver 。这样在运行时刻我们可以选择创建需要的产品系列

```swift
class BenzCar {
    var name: String
    init(name: String) {
        self.name = name
    }
    
    func drive() {}
}

class BenzSportCar : BenzCar {
    override func drive() {
        print("\(name) :: BenzSportCart")
    }
}

class BenzBusinessCar : BenzCar {
    override func drive() {
        print("\(name) :: BenzBusinessCar")
    }
}


class BmwCar {
    var name: String
    
    init(name: String) {
        self.name = name
    }
    
    func drive() {}
}

class BmwSportCar : BmwCar {
    override func drive() {
        print("\(name) :: BmwCSportCart")
    }
}

class BmwBusinessCar : BmwCar {
    override func drive() {
        print("\(name) :: BmwBusinessCar")
    }
}

class AudiCar {
    var name: String
    
    init(name: String) {
        self.name = name
    }
    
    func drive() {}
}

class AudiSportCar : AudiCar {
    override func drive() {
        print("\(name) :: AudiCSportCart")
    }
}

class AudiBusinessCar : AudiCar {
    override func drive() {
        print("\(name) :: AudiBusinessCar")
    }
}


protocol Driver {
    func createBenzCar(car: String) -> BenzCar
    func createBmwCar(car: String) -> BmwCar
    func createAudiCar(car: String) -> AudiCar
}

class SupportDriver : Driver {
    
    func createBenzCar(car: String) -> BenzCar {
        BenzSportCar.init(name: car)
    }
    
    func createBmwCar(car: String) -> BmwCar {
        BmwSportCar.init(name: car)
    }
    
    func createAudiCar(car: String) -> AudiCar {
        AudiSportCar.init(name: car)
    }
}


class BusinessDriver: Driver {
    func createBenzCar(car: String) -> BenzCar {
        BenzBusinessCar.init(name: car)
    }
    
    func createBmwCar(car: String) -> BmwCar {
        BmwBusinessCar.init(name: car)
    }
    
    func createAudiCar(car: String) -> AudiCar {
        AudiBusinessCar.init(name: car)
    }
}

let driver = BusinessDriver()
let car = driver.createAudiCar(car: "BusinessDriver")
car.drive()
```

### GUI 样例

假设我们有两种产品接口 Button 和 Border ，每一种产品都支持多种系列，比如 Mac 系列和 Windows 系列。这样每个系列的产品分别是 MacButton, WinButton, MacBorder, WinBorder 。为了可以在运行时刻创建一个系列的产品族，我们可以为每个系列的产品族创建一个工厂 MacFactory 和 WinFactory 。每个工厂都有两个方法 CreateButton 和 CreateBorder 并返回对应的产品，可以将这两个方法抽象成一个接口 AbstractFactory 。这样在运行时刻我们可以选择创建需要的产品系列

```swift
protocol Button {}

class MacButton: Button {}
class WinButton: Button {}

protocol Border {}
class MacBorder: Border {}
class WinBorder: Border {}

protocol AbstractFactory {
    func createButton() -> Button
    func createBorder() -> Border
}

class MacFactory: AbstractFactory {
    func createButton() -> Button { MacButton() }
    func createBorder() -> Border { MacBorder() }
}

class WinFactory: AbstractFactory {
    func createButton() -> Button { WinButton() }
    func createBorder() -> Border { WinBorder() }
}

enum Style { case mac, win}

var fac: AbstractFactory
var style: Style = .mac

switch style {
case .mac:
    fac = MacFactory()
default:
    fac = WinFactory()
}

let button: Button = fac.createButton()
let border: Border = fac.createBorder()
```

## 三者对比

|  |简单工厂| 工厂方法| 抽象工厂|
| -- | --    |   -- |  --|
|特点|  使用静态方法通过接受参数的不同来返回不同的实例| 针对每一种产品提供一个工厂类 | 针对产品族 | 
| 扩展方式 | 在不修改代码的前提下无法扩展 | 在同一级别中可以任意扩展 | 应对产品族的概念，可以增加新的产品线，但无法增加新的产品 |
| 优点 | 在于工厂类中包含了必要的逻辑，更具客户需要的条件动态实例化相关的类，对客户端来说，去掉了与具体产品的依赖 | 创建对象的接口，让子类去决定具体的实例化的对象，把简单的内部逻辑判断移到了客户端代码。工厂方法克服了简单工厂违背开放-封闭原则的缺点，又保持了封装对象创建过程的优点。| 分离了具体的类，抽象工厂模式帮助你控制一个应用创建的对象的类，客户通过他们的抽象接口操纵实例，产品的类名也在具体工厂的实现中被分离，它们不出现在客户代码中。它使得易于交换产品系列。|
| 缺点 | 工厂类集中了所有实例的创建逻辑，违反了高内聚责任分配原则，当增加新的产品时，会违反开发-封闭原则 | 不易于维护，假如某个具体的产品类需要进行一定的修改，很可能需要修改对应的工厂类。当同时需要修改多个产品类的时候，对工厂类的修改会变得相当麻烦 | 抽象工厂模式在于难于应对“新对象”的需求变动，难以支持新种类产品，难以扩展抽象工厂以生产新种类产品 |
