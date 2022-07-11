# 开闭原则

开闭原则（Open Closed Principle，OCP）的定义是：一个软件实体如类、模块和函数应该对扩展开放，对修改关闭。模块应尽量在不修改原（是"原"，指原来的代码）代码的情况下进行扩展。

## 开闭原则的意义

在软件的生命周期内，因为变化、升级和维护等原因需要对软件原有代码进行修改时，可能会给旧代码中引入错误，也可能会使我们不得不对整个功能进行重构，并且需要原有代码经过重新测试。当软件需要变化时，尽量通过扩展软件实体的行为来实现变化，而不是通过修改已有的代码来实现变化。

## 如何实现对扩展开放，对修改关闭

要实现对扩展开放，对修改关闭，即遵循开闭原则，需要对系统进行抽象化设计，抽象可以基于抽象类或者接口。一般来说需要做到几点：

1. 通过接口或者抽象类约束扩展，对扩展进行边界限定，不允许出现在接口或抽象类中不存在的 public 方法，也就是扩展必须添加具体实现而不是改变具体的方法。
2. 参数类型、引用对象尽量使用接口或者抽象类，而不是实现类，这样就能尽量保证抽象层是稳定的。
3. 一般抽象模块设计完成(例如接口的方法已经敲定)，不允许修改接口或者抽象方法的定义。

下面通过一个例子遵循开闭原则进行设计，场景是这样：某系统的后台需要监测业务数据展示图表，如柱状图、折线图等，在未来需要支持图表的着色操作。在开始设计的时候，代码可能是这样的：

```swift
class BarChart {
    func draw() {
        print("Draw bar chart...")
    }
}

class LineChart {
    func draw() {
        print("Draw line chart...")
    }
}


class App {
    func drawChart(type: String) {
        if type == "line" {
            LineChart().draw()
        } else if type == "bar" {
            BarChart().draw()
        }
    }
}
```

这样做在初期是能满足业务需要的，开发效率也十分高，但是当后面需要新增一个饼状图的时候，既要添加一个饼状图的类，原来的客户端 App 类的 drawChart() 方法也要新增一个 else if 分支，这样做就是修改了原有客户端类库的方法，是十分不合理的。如果这个时候，在图中加入一个颜色属性，复杂性也大大提高。基于此，需要引入一个抽象 Chart 接口 IChart，App 类在画图的时候总是把相关的操作委托到具体的 IChart 的实现类实例，这样的话 App 类的代码就不用修改：

```swift

protocol IChart {
    func draw()
}

class BarChart: IChart {
    func draw() {
        print("Draw bar chart...")
    }
}

class LineChart: IChart {
    func draw() {
        print("Draw line chart...")
    }
}


class App {
    func drawChart(_ chart: IChart) {
        chart.draw()
    }
}

App().drawChart(BarChart())
```

如果新加一种图，只需要新增一个 IChart 的实现类即可。客户端类 App 不需要改变原来的逻辑。修改后的设计符合开闭原则，因为整个系统在扩展时原有的代码没有做任何修改。
