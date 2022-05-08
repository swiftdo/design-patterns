# 装饰模式

动态地给一个对象增加一些额外的职责，就拓展对象功能来说，装饰模式比生成子类的方式更为灵活。

一般的，我们为了扩展一个类经常使用继承方式实现，由于继承为类引入静态特征，并且随着扩展功能的增多，子类会很膨胀。

## 样例

![](http://blog.oldbird.run/mweb/16169011855587.jpg)

```swift
protocol Shape {
    func draw()
}

class Rectangle: Shape {
    func draw() {
        print("形状: 矩形")
    }
}

class Circle: Shape {
    func draw() {
        print("形状：圆")
    }
}

class ShapeDecorator: Shape {
    let decoratedShape: Shape

    init(shape: Shape) {
        self.decoratedShape = shape
    }

    func draw() {
        decoratedShape.draw()
    }
}

class RedShapeDecorator: ShapeDecorator {
    override func draw() {
        super.draw()
        setRedBorder()
    }

    func setRedBorder() {
        print("边框的颜色：红色")
    }
}

let cirle = Circle()

let redCircle = RedShapeDecorator(shape: Circle())
let redRectangle = RedShapeDecorator(shape: Rectangle())

print("无边框的圆")
cirle.draw()

print("\n红色边框的圆")
redCircle.draw()

print("\n红色边框的矩形")
redRectangle.draw()
```

结果输出：

```sh
无边框的圆
形状：圆

红色边框的圆
形状：圆
边框的颜色：红色

红色边框的矩形
形状: 矩形
边框的颜色：红色
```
