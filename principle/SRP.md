# 单一职责原则

单一职责原则（Single Responsibility Principle, SRP）的定义是：指一个类或者模块应该有且只有一个改变的原因。如果一个类承担的职责过多，就等于把这些职责耦合在一起了。一个职责的变化可能会削弱或者抑制这个类完成其他职责的能力。这种耦合会导致脆弱的设计，当发生变化时，设计会遭受到意想不到的破坏。而如果想要避免这种现象的发生，就要尽可能的遵守单一职责原则。此原则的核心就是解耦和增强内聚性。

## 单一职责原则的意义

单一职责原则告诉我们：一个类不能做太多的东西。在软件系统中，一个类(一个模块、或者一个方法)承担的职责越多，那么其被复用的可能性就会越低。一个很典型的例子就是万能类。其实可以说一句大实话：任何一个常规的 MVC 项目，在极端的情况下，可以用一个类(甚至一个方法)完成所有的功能。但是这样做就会严重耦合，甚至牵一发动全身。一个类承(一个模块、或者一个方法)担的职责过多，就相当于将这些职责耦合在一起，当其中一个职责变化时，可能会影响其他职责的运作，**因此要将这些职责进行分离，将不同的职责封装在不同的类中，即将不同的变化原因封装在不同的类中，如果多个职责总是同时发生改变则可将它们封装在同一类中**。

不过说实话，其实有的时候很难去衡量一个类的职责，主要是很难确定职责的粒度。这一点不仅仅体现在一个类或者一个模块中，也体现在采用微服务的分布式系统中。这也就是为什么我们在实施微服务拆分的时候经常会撕逼："这个功能不应该发在 A 服务中，它不做这个领域的东西，应该放在 B 服务中"诸如此类的争论。存在争论是合理的，不过最好不要不了了之，而应该按照领域定义好每个服务的职责(职责的粒度最好找业务和架构专家咨询)，得出相对合理的职责分配。

下面通过一个很简单的实例说明一下单一职责原则：

在一个项目系统代码编写的时候，由于历史原因和人为的不规范，导致项目没有分层，一个 Service 类的伪代码是这样的：

```swift
class Service {
    func findUser(name: String) -> UserDTO {
        let connection: Connection = getConnection();
        let preparedStatement: PreparedStatement = connection.prepareStatement("SELECT * FROM t_user WHERE name = ?");
        preparedStatement.setObject(1, name);
        let user: User = //处理结果
        let dto: UserDTO = UserDTO();
        //entity值拷贝到dto
        return dto;
    }
}
```

这里出现一个问题，Service 做了太多东西，包括数据库连接的管理，Sql 的执行这些业务层不应该接触到的逻辑，更可怕的是，例如到时候如果数据库换成了 Oracle，这个方法将会大改。因此，拆分出新的 DataBaseUtils 类用于专门管理数据库资源，Dao 类用于专门执行查询和查询结果封装，改造后 Service 类的伪代码如下：

```swift
class Service {

    private var dao: Dao;

	findUser(String name) -> UserDTO {
       let user: User = dao.findUserByName(name);
       let dto: UserDTO = UserDTO();
        //entity值拷贝到dto
       return dto;
    }
}


class Dao {
    func findUserByName(name: String) -> User{
       let connection: Connection = DataBaseUtils.getConnnection();
       let preparedStatement: PreparedStatement = connection.prepareStatement("SELECT * FROM t_user WHERE name = ?");
		preparedStatement.setObject(1, name);
        let user: User = //处理结果
        return user;
    }
}
```

现在，如果有查询封装的变动只需要修改 Dao 类，数据库相关变动只需要修改 DataBaseUtils 类，每个类的职责分明。这个时候，如果我们要把底层的存储结构缓成 Redis 或者 MongoDB 怎么办，这样显然要重建整个 Dao 类，这种情况下，需要进行接口隔离，下面分析接口隔离原则的时候再详细分析。
