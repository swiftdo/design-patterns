# 迭代器模式

提供一种方法顺序访问一个聚合对象中各个元素, 而又无须暴露该对象的内部表示。

分离了集合对象的遍历行为，抽象出一个迭代器类来负责，这样既可以做到不暴露集合的内部结构，又可让外部代码透明地访问集合内部的数据。

## 样例

![](http://blog.oldbird.run/mweb/16171939142879.jpg)

```swift
struct Book {
    let name: String
    let author: String
}

struct BookList: Sequence {

    private let bookList: [Book]

    init(books: [Book]) {
        self.bookList = books
    }

    func makeIterator() -> BookListInterator {
        return BookListInterator(books: bookList)
    }
}

struct BookListInterator: IteratorProtocol {
    typealias Element = Book

    private var index = 0

    private let books: [Element]

    init(books: [Element]) {
        self.books = books
    }

    mutating func next() -> Book? {
        defer {
            self.index += 1
        }

        return books.count > index ? books[index] : nil
    }
}

let books = BookList(books: [
    Book(name: "西游记", author: "吴承恩"),
    Book(name: "三国演义", author: "罗贯中")
])

for book in books {
    print("书籍：\(book.name) - \(book.author)")
}
```

结果显示：

```swift
书籍：西游记 - 吴承恩
书籍：三国演义 - 罗贯中
```
