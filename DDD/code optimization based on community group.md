**▍Domain Primitive 的定义**

让我们重新来定义一下 Domain Primitive ：Domain Primitive 是一个在特定领域里，拥有精准定义的、可自我验证的、拥有行为的 Value Object 。

-   DP是一个传统意义上的Value Object，拥有Immutable的特性
-   DP是一个完整的概念整体，拥有精准定义
-   DP使用业务域中的原生语言
-   DP可以是业务域的最小组成部分、也可以构建复杂组合


**▍常见的 DP 的使用场景包括：**
-   有格式限制的 String：比如Name，PhoneNumber，OrderNumber，ZipCode，Address等
-   有限制的Integer：比如OrderId（>0），Percentage（0-100%），Quantity（>=0）等
-   可枚举的 int ：比如 Status（一般不用Enum因为反序列化问题）
-   Double 或 BigDecimal：一般用到的 Double 或 BigDecimal 都是有业务含义的，比如 Temperature、Money、Amount、ExchangeRate、Rating 等
-   复杂的数据结构：比如 <Map> 等，尽量能把 Map 的所有操作包装掉，仅暴露必要行为

**▍使用 Domain Primitive 的三原则**

-   让隐性的概念显性化
-   让隐性的上下文显性化
-   封装多对象行为

**▍Domain Primitive 和 DDD 里 Value Object 的区别**

在 DDD 中， Value Object 这个概念其实已经存在：

-   在 Evans 的 DDD 蓝皮书中，Value Object 更多的是一个非 Entity 的值对象
-   在Vernon的IDDD红皮书中，作者更多的关注了Value Object的Immutability、Equals方法、Factory方法等

Domain Primitive 是 Value Object 的进阶版，在原始 VO 的基础上要求每个 DP 拥有概念的整体，而不仅仅是值对象。在 VO 的 Immutable 基础上增加了 Validity 和行为。当然同样的要求无副作用（side-effect free）。
	
**▍引入mapstruct 处理 DTO DO ENTITY VO 之间的转化问题**
	
< https://mapstruct.org/   >
	
**▍项目结构方向的思考**	
-   **Data Object （DO、数据对象）：**实际上是我们在日常工作中最常见的数据模型。但是在DDD的规范里，DO应该仅仅作为数据库物理表格的映射，不能参与到业务逻辑中。为了简单明了，DO的字段类型和名称应该和数据库物理表格的字段类型和名称一一对应，这样我们不需要去跑到数据库上去查一个字段的类型和名称。（当然，实际上也没必要一摸一样，只要你在Mapper那一层做到字段映射）
    
-   **Entity（实体对象）：**实体对象是我们正常业务应该用的业务模型，它的字段和方法应该和业务语言保持一致，和持久化方式无关。也就是说，Entity和DO很可能有着完全不一样的字段命名和字段类型，甚至嵌套关系。Entity的生命周期应该仅存在于内存中，不需要可序列化和可持久化。
    
-   **DTO（传输对象）：**主要作为Application层的入参和出参，比如CQRS里的Command、Query、Event，以及Request、Response等都属于DTO的范畴。DTO的价值在于适配不同的业务场景的入参和出参，避免让业务对象变成一个万能大对象。
	
Repository 层：Data Model只存在于数据层，而Domain Model在领域层，而链接了这两层的关键对象，就是Repository

1.  **接口名称不应该使用底层实现的语法：**我们常见的insert、select、update、delete都属于SQL语法，使用这几个词相当于和DB底层实现做了绑定。相反，我们应该把 Repository 当成一个中性的类 似Collection 的接口，使用语法如 find、save、remove。在这里特别需要指出的是区分 insert/add 和 update 本身也是一种和底层强绑定的逻辑，一些储存如缓存实际上不存在insert和update的差异，在这个 case 里，使用中性的 save 接口，然后在具体实现上根据情况调用 DAO 的 insert 或 update 接口。
    
2.  **出参入参不应该使用底层数据格式：**需要记得的是 Repository 操作的是  Entity 对象（实际上应该是Aggregate Root），而不应该直接操作底层的 DO 。更近一步，Repository 接口实际上应该存在于Domain层，根本看不到 DO 的实现。这个也是为了避免底层实现逻辑渗透到业务代码中的强保障。
    
3.  **应该避免所谓的“通用”Repository模式：**很多 ORM 框架都提供一个“通用”的Repository接口，然后框架通过注解自动实现接口，比较典型的例子是Spring Data、Entity Framework等，这种框架的好处是在简单场景下很容易通过配置实现，但是坏处是基本上无扩展的可能性（比如加定制缓存逻辑），在未来有可能还是会被推翻重做。
	
4.  **RepositoryImpl在Infrastructure层，在本项目中，可以进行DAO，或者rediTemplete的调用
	
	
	
	
	
	