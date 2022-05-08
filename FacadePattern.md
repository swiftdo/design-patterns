# 外观模式

为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。

## 样例

假设我们有一个压缩与解压缩系统专门处理文件的压缩和解压缩，这个系统有三个模块：ZIPModel、RARModel、ZModel，分别处理 ZIP、RAR、7Z 三种文件格式的压缩与解压缩。现在这一系统要提供给上层应用程序使用。

![](http://blog.oldbird.run/mweb/16169034808432.jpg)

```swift
import Foundation

typealias ModelZipPath = (srcFilePath: String, dstFilePath: String)

protocol Model {
    func compress(path: ModelZipPath)
    func decompress(path: ModelZipPath)
}

class ZIPModel: Model {
    func compress(path: ModelZipPath) {
        print("ZIP 模块正在进行\(path.srcFilePath)文件的压缩...")
        print("文件压缩成功,已保存至\(path.dstFilePath)")
    }

    func decompress(path: ModelZipPath) {
        print("ZIP 模块正在进行\(path.srcFilePath)文件的解压缩...")
        print("文件压解缩成功,已保存至\(path.dstFilePath)")
    }
}

class RARModel: Model {
    func compress(path: ModelZipPath) {
        print("RAR 模块正在进行\(path.srcFilePath)文件的压缩...")
        print("文件压缩成功,已保存至\(path.dstFilePath)")
    }

    func decompress(path: ModelZipPath) {
        print("RAR 模块正在进行\(path.srcFilePath)文件的解压缩...")
        print("文件压解缩成功,已保存至\(path.dstFilePath)")
    }
}

class ZModel: Model {
    func compress(path: ModelZipPath) {
        print("7Z 模块正在进行\(path.srcFilePath)文件的压缩...")
        print("文件压缩成功,已保存至\(path.dstFilePath)")
    }

    func decompress(path: ModelZipPath) {
        print("7Z 模块正在进行\(path.srcFilePath)文件的解压缩...")
        print("文件压解缩成功,已保存至\(path.dstFilePath)")
    }
}


/// 压缩系统的外观模式
class CompressionFacade {
    lazy var zipModel = ZIPModel()
    lazy var rarModel = RARModel()
    lazy var zModel = ZModel()

    /// 根据不同的压缩类型，压缩成不同的格式
    func compress(path: ModelZipPath, type: String) -> Bool {

        let extName = "." + type
        let fullName = path.dstFilePath + extName

        if type.lowercased() == "zip" {
            zipModel.compress(path: (path.srcFilePath, fullName))
        } else if type.lowercased() == "rar" {
            rarModel.compress(path: (path.srcFilePath, fullName))
        } else if type.lowercased() == "7z" {
            zModel.compress(path: (path.srcFilePath, fullName))
        } else {
            print("不支持压缩类型\(type)")
            return false
        }
        return true
    }

    func decompress(path: ModelZipPath) -> Bool {
        let url = URL(fileURLWithPath: path.srcFilePath)
        let type = url.pathExtension

        if type.lowercased() == "zip" {
            zipModel.decompress(path: path)
        } else if type.lowercased() == "rar" {
            rarModel.decompress(path: path)
        } else if type.lowercased() == "7z" {
            zModel.decompress(path: path)
        } else {
            print("不支持压缩类型\(type)")
            return false
        }
        return true
    }
}

let facade = CompressionFacade()
facade.compress(path: ("./压缩外观模式.md", "./压缩外观模式"), type: "zip")
facade.decompress(path: ("./解压外观模式.zip", "./解压外观模式"))

print("\n")
facade.compress(path: ("./压缩外观模式.pdf", "./压缩外观模式"), type: "rar")
facade.decompress(path: ("./解压外观模式.rar", "./解压外观模式"))

print("\n")
facade.compress(path: ("./压缩外观模式.odc", "./压缩外观模式"), type: "7z")
facade.decompress(path: ("./解压外观模式.7z", "./解压外观模式"))
```

结果显示：

```sh
文件压缩成功,已保存至./压缩外观模式.zip
ZIP 模块正在进行./解压外观模式.zip文件的解压缩...
文件压解缩成功,已保存至./解压外观模式


RAR 模块正在进行./压缩外观模式.pdf文件的压缩...
文件压缩成功,已保存至./压缩外观模式.rar
RAR 模块正在进行./解压外观模式.rar文件的解压缩...
文件压解缩成功,已保存至./解压外观模式


7Z 模块正在进行./压缩外观模式.odc文件的压缩...
文件压缩成功,已保存至./压缩外观模式.7z
7Z 模块正在进行./解压外观模式.7z文件的解压缩...
文件压解缩成功,已保存至./解压外观模式
```
