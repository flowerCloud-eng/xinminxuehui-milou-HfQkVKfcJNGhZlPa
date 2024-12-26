
# 02\.单一职责原则详解


#### 目录介绍


* 01\.问题思考分析
* 02\.单一职责原则介绍
* 03\.如何理解单一指责
* 04\.用例子理解单一职责
* 05\.为何遵守单一原则
* 06\.方法层面单一职责
* 07\.接口层面单一职责
* 08\.类层面单一职责
* 09\.单一职责判断模糊
* 10\.单一职责判断原则
* 11\.最后总结一下
* 12\.更多内容推荐


## 推荐一个好玩网站


一个最纯粹的技术分享网站，打造精品技术编程专栏！[编程进阶网](https://github.com)


[https://yccoding.com/](https://github.com)


设计模式Git项目地址：[https://github.com/yangchong211/YCDesignBlog](https://github.com)


单一职责原则（SRP）是面向对象设计的重要原则，强调一个类或模块应仅负责完成一个特定的职责或功能。通过将复杂的功能分解为多个粒度小、功能单一的类，可以提高系统的灵活性、可维护性和可扩展性。


本文详细介绍了如何理解单一职责原则，包括方法、接口和类层面的应用，并通过具体例子解释了其优势和判断标准。此外，还探讨了在实际开发中如何平衡类的设计，避免过度拆分导致的复杂性增加。


## 01\.问题思考分析


1. 如何理解类的单一指责，单一指责中这个单一是如何评判的？
2. 懂了，但是会用么，或者实际开发中有哪些运用，能否举例说明单一职责优势？
3. 单一指责是否设计越单一，越好呢？说出你的缘由和论证的思路想法？
4. 单一职责原则，除了应用到类的设计上，还能延伸到哪些其他设计方面吗？


## 02\.单一职责原则介绍


单一责任原则（Single Responsibility Principle，SRP）是面向对象设计中的一条重要原则。


这个原则的英文描述是这样的：A class or module should have a single responsibility。


如果我们把它翻译成中文，那就是：**一个类或者模块只负责完成一个职责（或者功能）**。


也就是说，一个模块或类应该只负责一个特定的职责或功能，通过将功能分解到不同的模块或类中，可以使系统更加灵活、可维护和可扩展。


从字面上理解，不难。你一看就感觉懂了，一看就感觉掌握了，但真的用到项目中的时候，你会发现，“看懂”和“会用”是两回事，而“用好”更是难上加难。


从工作经历来看，很多同事因为对这些原则理解得不够透彻，导致在使用的时候过于教条主义，拿原则当真理，生搬硬套，适得其反。


## 03\.如何理解单一指责


单一原则描述的对象包含两个，一个是类（class），一个是模块（module）。


关于这两个概念，在专栏中，有两种理解方式。一种理解是：把模块看作比类更加抽象的概念，类也可以看作模块。另一种理解是：把模块看作比类更加粗粒度的代码块，模块中包含多个类，多个类组成一个模块。


不管哪种理解方式，单一职责原则在应用到这两个描述对象的时候，道理都是相通的。为了方便你理解，接下来我只从“类”设计的角度，来讲解如何应用这个设计原则。对于“模块”来说，你可以自行引申。


## 04\.用例子理解单一职责


单一职责原则的定义描述非常简单，也不难理解。**一个类只负责完成一个职责或者功能。也就是说，不要设计大而全的类，要设计粒度小、功能单一的类。换个角度来讲就是，一个类包含了两个或者两个以上业务不相干的功能，那我们就说它职责不够单一，应该将它拆分成多个功能更加单一、粒度更细的类**。


举一个例子来解释一下。比如，一个类里既包含订单的一些操作，又包含用户的一些操作。而订单和用户是两个独立的业务领域模型，我们将两个不相干的功能放到同一个类中，那就违反了单一职责原则。


为了满足单一职责原则，我们需要将这个类拆分成两个粒度更细、功能更加单一的两个类：订单类和用户类。


## 05\.为何遵守单一原则


通常 ，我们做事情都要知道为什么要这么做，才回去做。做的也有底气，那么为什么我们要使用单一职责原则呢?


1. 提高类的可维护性和可读写性。一个类的职责少了，复杂度降低了，代码就少了，可读性也就好了，可维护性自然就高了。
2. 提高系统的可维护性。系统是由类组成的，每个类的可维护性高，相对来讲整个系统的可维护性就高。
3. 降低变更的风险。一个类的职责越多，变更的可能性就越大，变更带来的风险也就越大


## 06\.方法层面单一职责


现在有一个场景，需要修改用户的用户名和密码，就针对这个功能我们可以有多种实现。


先看一下第一种实现方式：



```
public enum OperateEnum {
    UPDATE_USERNAME，
    UPDATE_PASSWORD;
}

public interface UserOperate {
    void updateUserInfo(OperateEnum type，UserInfo userInfo);
}

public class UserOperateImpl implements UserOperate{
    @Override
    public void updateUserInfo(OperateEnum type，UserInfo userInfo) {
        if (type == OperateEnum。UPDATE_PASSWORD) {
            // 修改密码
        } else if(type == OperateEnum。UPDATE_USERNAME) {
            // 修改用户名
        }
    }
}

```

然后看一下第二种实现方式：



```
public interface UserOperate {

    void updateUserName(UserInfo userInfo);

    void updateUserPassword(UserInfo userInfo);
}

public class UserOperateImpl implements UserOperate {
    @Override
    public void updateUserName(UserInfo userInfo) {
        // 修改用户名逻辑
    }

    @Override
    public void updateUserPassword(UserInfo userInfo) {
        // 修改密码逻辑
    }
}

```

来看看这两种实现的区别:


1. 第一种实现是根据操作类型进行区分，不同类型执行不同的逻辑。把修改用户名和修改密码这两件事耦合在一起了。如果客户端在操作的时候传错了类型，那么就会发生错误。
2. 第二种实现是我们推荐的实现方式。修改用户名和修改密码逻辑分开，各自执行各自的职责，互不干扰，功能清晰明了。


由此可见，第二种设计是符合单一职责原则的。这是在方法层面实现单一职责原则。


## 07\.接口层面单一职责


我们假设一个场景，大家一起做家务，张三扫地，李四买菜。李四买完菜回来还得做饭。这个逻辑怎么实现呢?


先看一下第一种实现方式：



```
/**
 * 做家务
 */
public interface HouseWork {
    // 扫地
    void sweepFloor();

    // 购物
    void shopping();
}

public class Zhangsan implements HouseWork{
    @Override
    public void sweepFloor() {
        // 扫地
    }

    @Override
    public void shopping() {

    }
}

public class Lisi implements HouseWork{
    @Override
    public void sweepFloor() {

    }

    @Override
    public void shopping() {
        // 购物
    }
}

```

首先定义了一个做家务的接口，定义两个方法扫地和买菜。张三扫地，就实现扫地接口。李四买菜，就实现买菜接口。然后李四买完菜回来还要做饭，于是就要在接口类中增加一个方法cooking。张三和李四都重写这个方法，但只有李四有具体实现。


这样设计本身就是不合理的。首先: 张三只扫地，但是他需要重写买菜方法，李四不需要扫地，但是李四也要重写扫地方法。第二: 这也不符合开闭原则。增加一种类型做饭，要修改3个类。这样当逻辑很复杂的时候，很容易引起意外错误。


上面这种设计不符合单一职责原则，修改一个地方，影响了其他不需要修改的地方。


然后看一下第二种实现方式：



```
/**
 * 做家务
 */
public interface Hoursework {

}

public interface Shopping extends Hoursework{
    // 购物
    void shopping();
}

public interface SweepFloor extends Hoursework{
    // 扫地
    void sweepFlooring();
}

public class Zhangsan implements SweepFloor{

    @Override
    public void sweepFlooring() {
        // 张三扫地
    }
}

public class Lisi implements Shopping{
    @Override
    public void shopping() {
        // 李四购物
    }
}

```

上面做家务不是定义成一个接口，而是将扫地和做家务分开了。张三扫地，那么张三就实现扫地的接口。 李四购物，李四就实现购物的接口。 后面李四要增加一个功能做饭。 那么就新增一个做饭接口，这次只需要李四实现做饭接口就可以了。



```
public interface Cooking extends Hoursework{ 
    void cooking();
}

public class Lisi implements Shopping, Cooking{
    @Override
    public void shopping() {
        // 李四购物
    }

    @Override
    public void cooking() {
        // 李四做饭
    }
}

```

如上, 我们看到张三没有实现多余的接口, 李四也没有. 而且当新增功能的时候, 只影响了李四, 并没有影响张三.


这就是符合单一职责原则. 一个类只做一件事. 并且他的修改不会带来其他的变化.


## 08\.类层面单一职责


从类的层面来讲, 没有办法完全按照单一职责原来来拆分. 换种说法, 类的职责可大可小, 不想接口那样可以很明确的按照单一职责原则拆分. 只要符合逻辑有道理即可.


比如, 我们在网站首页可以注册, 登录, 微信登录.注册登录等操作. 我们通常的做法是:



```
public interface UserOperate {

    void login(UserInfo userInfo);

    void register(UserInfo userInfo);

    void logout(UserInfo userInfo);
}


public class UserOperateImpl implements UserOperate{
    @Override
    public void login(UserInfo userInfo) {
        // 用户登录
    }

    @Override
    public void register(UserInfo userInfo) {
        // 用户注册
    }

    @Override
    public void logout(UserInfo userInfo) {
        // 用户登出
    }
}

```

那如果按照单一职责原则拆分, 也可以拆分为下面的形式



```
public interface Register {
    void register();
}

public interface Login {
    void login();
}

public interface Logout {
    void logout();
}


public class RegisterImpl implements Register{

    @Override
    public void register() {

    }
}

public class LoginImpl implements Login{
    @Override
    public void login() {
        // 用户登录
    }
}

public class LogoutImpl implements Logout{

    @Override
    public void logout() {

    }
}

```

## 09\.单一职责判断模糊


在真实的软件开发中，对于一个类是否职责单一的判定，是很难拿捏的。我举一个更加贴近实际的例子来给你解释一下。


在一个社交产品中，我们用下面的 UserInfo 类来记录用户的信息。你觉得，UserInfo 类的设计是否满足单一职责原则呢？



```
public class UserInfo {
  private long userId;
  private String username;
  private String email;
  private String telephone;
  private long createTime;
  private long lastLoginTime;
  private String avatarUrl;
  private String provinceOfAddress; // 省
  private String cityOfAddress; // 市
  private String regionOfAddress; // 区 
  private String detailedAddress; // 详细地址
  // 。。。省略其他属性和方法。。。
}

```

对于这个问题，有两种不同的观点。


1. 一种观点是，UserInfo 类包含的都是跟用户相关的信息，所有的属性和方法都隶属于用户这样一个业务模型，满足单一职责原则；
2. 另一种观点是，地址信息在 UserInfo 类中，所占的比重比较高，可以继续拆分成独立的 UserAddress 类，UserInfo 只保留除 Address 之外的其他信息，拆分之后的两个类的职责更加单一。


哪种观点更对呢？实际上，要从中做出选择，我们不能脱离具体的应用场景。


1. 如果在这个社交产品中，用户的地址信息跟其他信息一样，只是单纯地用来展示，那 UserInfo 现在的设计就是合理的。
2. 如果这个社交产品发展得比较好，之后又在产品中添加了电商的模块，用户的地址信息还会用在电商物流中，那我们最好将地址信息从 UserInfo 中拆分出来，独立成用户物流信息（或者叫地址信息、收货信息等）。


从刚刚这个例子，我们可以总结出，不同的应用场景、不同阶段的需求背景下，对同一个类的职责是否单一的判定，可能都是不一样的。在某种应用场景或者当下的需求背景下，一个类的设计可能已经满足单一职责原则了，但如果换个应用场景或着在未来的某个需求背景下，可能就不满足了，需要继续拆分成粒度更细的类。


有时候一个类的职责是否足够单一，我们并没有一个非常明确的、可以量化的标准，可以说，这是件非常主观、仁者见仁智者见智的事情。


## 10\.单一职责判断原则


可能会说，这个原则如此含糊不清、模棱两可，到底该如何拿捏才好啊？我这里还有一些小技巧，能够很好地帮你，从侧面上判定一个类的职责是否够单一。下面这几条判断原则，比起很主观地去思考类是否职责单一，要更有指导意义、更具有可执行性：


1. 类中的代码行数、函数或属性过多，会影响代码的可读性和可维护性，我们就需要考虑对类进行拆分；
2. 类依赖的其他类过多，或者依赖类的其他类过多，不符合高内聚、低耦合的设计思想，我们就需要考虑对类进行拆分；
3. 私有方法过多，我们就要考虑能否将私有方法独立到新的类中，设置为 public 方法，供更多的类使用，从而提高代码的复用性；
4. 比较难给类起一个合适名字，很难用一个业务名词概括，或者只能用一些笼统的 Manager、Context 之类的词语来命名，这就说明类的职责定义得可能不够清晰；
5. 类中大量的方法都是集中操作类中的某几个属性，比如，在 UserInfo 例子中，如果一半的方法都是在操作 address 信息，那就可以考虑将这几个属性和对应的方法拆分出来。


## 11\.最后总结一下


**如何理解单一职责原则（SRP）**？


一个类只负责完成一个职责或者功能。不要设计大而全的类，要设计粒度小、功能单一的类。单一职责原则是为了实现代码高内聚、低耦合，提高代码的复用性、可读性、可维护性。


**如何判断类的职责是否足够单一**？


1. 不同的应用场景、不同阶段的需求背景、不同的业务层面，对同一个类的职责是否单一，可能会有不同的判定结果。实际上，一些侧面的判断指标更具有指导意义和可执行性，比如，出现下面这些情况就有可能说明这类的设计不满足单一职责原则：
2. 类中的代码行数、函数或者属性过多；
3. 类依赖的其他类过多，或者依赖类的其他类过多；
4. 私有方法过多；
5. 比较难给类起一个合适的名字；
6. 类中大量的方法都是集中操作类中的某几个属性。


类的职责是否设计得越单一越好？


单一职责原则通过避免设计大而全的类，避免将不相关的功能耦合在一起，来提高类的内聚性。同时，类职责单一，类依赖的和被依赖的其他类也会变少，减少了代码的耦合性，以此来实现代码的高内聚、低耦合。


但是，如果拆分得过细，实际上会适得其反，反倒会降低内聚性，也会影响代码的可维护性。


## 12\.更多内容推荐




| 模块 | 描述 | 备注 |
| --- | --- | --- |
| GitHub | 多个YC系列开源项目，包含Android组件库，以及多个案例 | [GitHub](https://github.com) |
| 博客汇总 | 汇聚Java，Android，C/C\+\+，网络协议，算法，编程总结等 | [YCBlogs](https://github.com) |
| 设计模式 | 六大设计原则，23种设计模式，设计模式案例，面向对象思想 | [设计模式](https://github.com) |
| Java进阶 | 数据设计和原理，面向对象核心思想，IO，异常，线程和并发，JVM | [Java高级](https://github.com) |
| 网络协议 | 网络实际案例，网络原理和分层，Https，网络请求，故障排查 | [网络协议](https://github.com) |
| 计算机原理 | 计算机组成结构，框架，存储器，CPU设计，内存设计，指令编程原理，异常处理机制，IO操作和原理 | [计算机基础](https://github.com):[蓝猫机场](https://fenfang.org) |
| 学习C编程 | C语言入门级别系统全面的学习教程，学习三到四个综合案例 | [C编程](https://github.com) |
| C\+\+编程 | C\+\+语言入门级别系统全面的教学教程，并发编程，核心原理 | [C\+\+编程](https://github.com) |
| 算法实践 | 专栏，数组，链表，栈，队列，树，哈希，递归，查找，排序等 | [Leetcode](https://github.com) |
| Android | 基础入门，开源库解读，性能优化，Framework，方案设计 | [Android](https://github.com) |


**23种设计模式**




| 23种设计模式 \& 描述 \& 核心作用 | 包括 |
| --- | --- |
| **[创建型模式](https://github.com)**提供创建对象用例。能够将软件模块中对象的创建和对象的使用分离 | 工厂模式（Factory Pattern）抽象工厂模式（Abstract Factory Pattern）单例模式（Singleton Pattern）建造者模式（Builder Pattern）原型模式（Prototype Pattern） |
| **[结构型模式](https://github.com)**关注类和对象的组合。描述如何**将类或者对象结合在一起形成更大的结构** | 适配器模式（Adapter Pattern）桥接模式（Bridge Pattern）过滤器模式（Filter、Criteria Pattern）组合模式（Composite Pattern）装饰器模式（Decorator Pattern）外观模式（Facade Pattern）享元模式（Flyweight Pattern）代理模式（Proxy Pattern） |
| **[行为型模式](https://github.com)**特别关注对象之间的通信。主要解决的就是“类或对象之间的交互”问题 | 责任链模式（Chain of Responsibility Pattern）命令模式（Command Pattern）解释器模式（Interpreter Pattern）迭代器模式（Iterator Pattern）中介者模式（Mediator Pattern）备忘录模式（Memento Pattern）观察者模式（Observer Pattern）状态模式（State Pattern）空对象模式（Null Object Pattern）策略模式（Strategy Pattern）模板模式（Template Pattern）访问者模式（Visitor Pattern） |


