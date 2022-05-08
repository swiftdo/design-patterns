# 备忘录模式

在不破坏封装的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，这样可以在以后将对象恢复到原先保存的状态。

## 备忘录模式的结构

- 发起人：记录当前时刻的内部状态，负责定义哪些属于备份范围的状态，负责创建和恢复备忘录数据。
- 备忘录：负责存储发起人对象的内部状态，在需要的时候提供发起人需要的内部状态。
- 管理角色：对备忘录进行管理，保存和提供备忘录。

## 样例

![](http://blog.oldbird.run/mweb/16176017312597.jpg)

```swift
/// 文章类
struct Article : CustomDebugStringConvertible{
    var title: String
    var content: String
    var cover: String


    func saveToMemento() -> ArticleMemento {
        return ArticleMemento(article: self)
    }

    mutating func undoFromMemento(_ memento: ArticleMemento) {
        self.title = memento.title
        self.content = memento.content
        self.cover = memento.cover
    }

    var debugDescription: String {
        return "Article: \(title) - \(content) - \(cover)"
    }
}

/// 备忘录类
class ArticleMemento : CustomDebugStringConvertible {
    var title: String
    var content: String
    var cover: String

    init(article: Article) {
        self.title = article.title
        self.content = article.content
        self.cover = article.cover
    }

    var debugDescription: String {
        return "ArticleMemento: \(title) - \(content) - \(cover)"
    }

}

/// 备忘录管理类
class ArticleMementoManager {
    private var mementos = [ArticleMemento]()

    func getMemento() -> ArticleMemento? {
        return self.mementos.popLast()
    }

    func addMemento(_ memento: ArticleMemento) {
        self.mementos.append(memento)
    }
}


let articleMememtoma = ArticleMementoManager()

var article = Article(title: "设计模式", content: "内容", cover: "https://test.png")
articleMememtoma.addMemento(article.saveToMemento())
print(article)

article.title = "设计模式A"
article.content = "内容A"
article.cover = "https://test.a.png"
articleMememtoma.addMemento(article.saveToMemento())
print(article)


article.title = "设计模式B"
article.content = "内容B"
article.cover = "https://test.b.png"
articleMememtoma.addMemento(article.saveToMemento())
print(article)


print("\n回退到上一步")
if let mememto = articleMememtoma.getMemento() {
    article.undoFromMemento(mememto)
}
print(article)

print("\n回退到上一步")
if let mememto = articleMememtoma.getMemento() {
    article.undoFromMemento(mememto)
}
print(article)


print("\n回退到上一步")
if let mememto = articleMememtoma.getMemento() {
    article.undoFromMemento(mememto)
}
print(article)
```

结果显示：

```sh
Article: 设计模式 - 内容 - https://test.png
Article: 设计模式A - 内容A - https://test.a.png
Article: 设计模式B - 内容B - https://test.b.png

回退到上一步
Article: 设计模式B - 内容B - https://test.b.png

回退到上一步
Article: 设计模式A - 内容A - https://test.a.png

回退到上一步
Article: 设计模式 - 内容 - https://test.png
```
