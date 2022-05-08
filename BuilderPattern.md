# 建造者模式

将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

## 案例

我们假设一个快餐店的商业案例，其中，一个典型的套餐可以是一个汉堡（Burger）和一杯冷饮（Cold drink）。

汉堡（Burger）可以是素食汉堡（Veg Burger）或鸡肉汉堡（Chicken Burger），它们是包在纸盒中。

冷饮（Cold drink）可以是可口可乐（coke）或百事可乐（pepsi），它们是装在瓶子中。

我们将创建一个表示食物条目（比如汉堡和冷饮）的 Item 接口和实现 Item 的接口的实体类，以及一个表示食物包装的 Packing 接口和实现接口的实体类。汉堡是包在纸盒中，冷饮装在瓶子里。

然后我们创建 Meal 类，和一个创建不同类型的 Meal 对象的 MealBuilder。

```swift
/// 食物包装
protocol Packing {
    var pack: String {get}
}

class Box: Packing {
    var pack: String {"纸盒"}
}
class Bottle: Packing {
    var pack: String {"瓶子"}
}

/// 食物条目
protocol Item {
    var name: String {get}
    var packing: Packing { get }
    var price: Double { get }
}

class Burger: Item {
    var packing: Packing { Box() }
    var price: Double { 10 }
    var name: String {"汉堡"}
}

class VegBurger: Burger {
    override var price: Double { 25 }
    override var name: String {"蔬菜汉堡"}
}

class ChickenBurger: Burger {
    override var price: Double { 50 }
    override var name: String { "鸡肉汉堡"}
}

class ColdDrink: Item {
    var packing: Packing { Bottle() }
    var price: Double { 15 }
    var name: String { "冷饮" }
}

class Coke: ColdDrink {
    override var price: Double { 30 }
    override var name: String { "可口可乐" }
}

class PepSi : ColdDrink {
    override var price: Double { 35 }
    override var name: String { "百事可乐"}
}

/// 套餐
class Meal {
    
    var items: [Item] = []
    
    func add(item: Item) {
        items.append(item)
    }
    
    func getCost() -> Double {
        return items.reduce(0) { (total, item) -> Double in
            return total + item.price
        }
    }
    
    func showItems() {
        items.forEach { (item) in
            print("条目：\(item.name), 打包：\(item.packing.pack), 价格：\(item.price)")
        }
    }
}

/// 创建 Meal 对象
class MealBuilder {
    func makeVegMeal() -> Meal {
        let meal = Meal()
        meal.add(item: VegBurger())
        meal.add(item: Coke())
        return meal
    }
    
    func makeNonVegMeal() -> Meal {
        let meal = Meal()
        meal.add(item: ChickenBurger())
        meal.add(item: PepSi())
        return meal
    }
}


let mealBuilder = MealBuilder()

print("素食套餐")
let vegMal = mealBuilder.makeVegMeal()
vegMal.showItems()
print("总计：\(vegMal.getCost())\n")


print("非素食套餐")
let noVegMal = mealBuilder.makeNonVegMeal()
noVegMal.showItems()
print("总计：\(noVegMal.getCost())")
```

最终输出：

```sh
素食套餐
条目：蔬菜汉堡, 打包：纸盒, 价格：25.0
条目：可口可乐, 打包：瓶子, 价格：30.0
总计：55.0

非素食套餐
条目：鸡肉汉堡, 打包：纸盒, 价格：50.0
条目：百事可乐, 打包：瓶子, 价格：35.0
总计：85.0

```



