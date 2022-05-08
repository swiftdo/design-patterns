# 组合模式

组合模式是一种具有层次关系的树形结构，不能再分的叶子节点是具体的组件，也就是最小的逻辑单元；具有子节点（由多个子组件组成）的组件称为复合组件，也就是组合对象。

组合模式定义了如何将容器对象和叶子对象进行递归组合，使得客户在使用的过程中无须进行区分，可以对他们进行一致的处理————叶子对象和组合对象实现相同的接口。

## 案例

![](http://blog.oldbird.run/mweb/16168350543656.png)

```swift
import Foundation

protocol File {
    var name: String {get set}
    func open()
}

final class eBook: File {
    var name: String
    var author: String

    init(name: String, author: String) {
        self.name = name
        self.author = author
    }

    func open() {
         print("在 iBooks 中打开 \(author) 编写的 \(name)")
    }
}

final class Music: File {

    var name: String
    var artist: String

    init(name: String, artist: String) {
        self.name = name
        self.artist = artist
    }

    func open() {
        print("在 iTunes 打开 \(artist) 唱的 \(name)")
    }
}

final class Folder: File {

    var name: String
    lazy var files: [File] = []

    init(name: String) {
        self.name = name
    }

    func addFile(file: File) {
        self.files.append(file)
    }

    func open() {
        print("\(name) 下的文件有：")
        files.forEach { (file) in
            print(file.name)
        }

        print("\n")
    }
}

let psychoKiller = Music(name: "Psycho Killer", artist: "The Talking Heads")
let rebelRebel = Music(name: "Reble Rebel", artist: "David Bowie")
let blisterInTheSun = Music(name: "Blister in the Sun", artist: "Violent Femmes")

let justKids = eBook(name: "Just Kids", author: "Patti Smith")
let documents = Folder(name: "Documents")
let musicFolder = Folder(name: "最喜欢的70首音乐")
documents.addFile(file: musicFolder)
documents.addFile(file: justKids)
musicFolder.addFile(file: psychoKiller)
musicFolder.addFile(file: rebelRebel)

blisterInTheSun.open()
justKids.open()

documents.open()
musicFolder.open()

```

输出的结果：

```sh
在 iTunes 打开 Violent Femmes 唱的 Blister in the Sun
在 iBooks 中打开 Patti Smith 编写的 Just Kids
Documents 下的文件有：
最喜欢的70首音乐
Just Kids


最喜欢的70首音乐 下的文件有：
Psycho Killer
Reble Rebel

```
