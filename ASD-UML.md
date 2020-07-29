UML（UnifiedModelingLanguage）入门--课程系统
#### 系统要求
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-0.png" height="810" width="702" >
</div>

#### 参与者/主角
> 和系统交互，位于系统之外的实体（系统的用户或其他系统）
+  谁对系统有明确的目标和要求并且主动发出动作
+  系统是为谁服务的？
+ 参与者的选定是相对于系统边界的

+ 登记者:       Enroller
+ 登记处工作人员: Enrollment Clerk 
+ 学生: student

>Q：一个商城里面，有哪些参与者？
Customenr  Merchant  Administer  

> 用例1：查看课程清单
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-1.png" height="200" width="300" >
</div>
> 用例2:登记一门课程
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-2.png" height="400" width="300" >
</div>
> 用例3:通过电子邮件向学生发送消息
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-4.png" height="300" width="300" >
</div>
> 用例4:设置出席状态
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-5.png" height="200" width="300" >
</div>
> 用例5:获取学生状态
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-6.png" height="300" width="300" >
</div>

+ 系统边界图
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-7.png" height="400" width="400" >
</div>


> 扩展和包含的区别：
+ 包含用例会引用被包含的用例    独立，原子  管理变更 消除冗余
+ 扩展用例不会引用			 扩展  可变部分（扩展用例）和不可变部分（被扩展的用例）
   
*本例的可变部分是支付方式是可变的，不可变部分是用户必须完成支付流程*

> 用例图/系统边界图 都不是软件结构图，这些图用来人与人交流，有助于按照不同的类型的系统用户来组织系统功能，即所有不同类型的用户都能看到他们所关心的用例子集
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-3.png" height="400" width="400" >
</div>

<br>

#### 领域模型
+ 是一组图，这组图有助于定义出现在用例中的术语。显示了问题中的关键对象以及它们之间的关系，对于分析师和设计者来书，领域模型是一种描述工具，用来帮助人们记录他们的决策以及相互之间的交流，领域模型重的对象未对应于软件的面向对象设计（why ？）
> type 领域模型中的用于问题描述的概念实体
+ 代表了对象可以担当的角色
+ 可以具有操作和属性
+ 可以具有和其他type的关联

>课程目录
+ CourseCatalog
+ Course
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-8.png" height="200" width="500" >
</div>
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-9.png" height="200" width="500" >
</div>

>UML类表示的语法和语意概述
+ 名字 用一个构造型（stereotype）以及一些特性(property)修饰.构造型代表UML种类，特性是结构化注释，即名值对。
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-10.png" height="200" width="200" >
</div>

> Session:表示一门特定课程的时间，地点，教授者

>把实体连接起来的线叫做关联
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-11.png" height="400" width="400" >
</div>

+ CourseCatalog提供了很多Course，Session Schedule安排了许多Session的时间表，许多学生可以登记到一个Session中
+ 关联默认双向，除非带有箭头。
+ 双向关联允许两个实体互相知晓，箭头限定了知晓的限制，如 Session知道Course，Course不知道Session
  
#### 用例迭代
+ 错误点：在讨论课程目录和课程的地方，我们应该谈论上课时间表和课室
+ 遗漏点：CourseCatalog和SessionSchedule需要被维护
  
### 构架
> 构架是指构成应用程序的骨架(skeleton)，构架中的类及关系和代码有紧密的映射关系

<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-12.png" height="140" width="330" >
</div>

+ 依赖关系指明了哪个具有对其他组件的知识
+ 本例中Session Menu Generator 程序创建了Session Menu Web页面，所以具有对他的知识。反之不成立

> 箭头的两种情况下的不同含义
+ 数据的流向 实线箭头 表示单向关联  关联类知道被关联类的公共属性及操作，但被关联类 并不知道关联类的公共属性及操作， 比如用例1，数据从系统流向登记者 箭头的指向就是信息的流向
+ 类的依赖 虚线箭头 A-->B 表明 A依赖于B，A类使用到了B，比如某人要过河，需要借用一条船，此时人与船之间的关系就是依赖；表现在代码层面，就是类B作为参数在某个方法里面被类A使用。
+ 依赖是单向的，避免双向依赖

>  组件流程
<div align="center" height="415" width="100%">
<img align="left" src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-13.png" height="410" width="450" >
<img align="right" src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-14.png" height="410" width="450" >
</div>

<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
>规格说明/实例二分法
+ 实例：名字有下划线，根据规格说明生成的某个过程的产品，只存在于运行时。

> HTML模版
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-15.png" height="110" width="380" >
</div>

+ 组合：HTML Templete 负责map的生存期
  
> 属性和操作
+ <code> + </code> 公有的
+ <code> - </code> 私有的
+ <code> # </code> 受保护的

> 包和子系统 
<div align="center" >
<img align="left" src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-17.png" height="110" width="110" >

<img align="center" src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-18.png" height="110" width="110" >
</div>


+ 输入依赖 虚线箭头 输入包指向被输入的包 被输入的包中所有公有元素对输入包是可见的
+ 泛化关系 实线箭头 实现包指向抽象包 抽象包中所有的公有和受保护的元素对实现包可见
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-19.png" height="180" width="300" >
</div>

>包的《subsystem》构造型 （这一部分不理解。。。）
+ 表示一个逻辑元素
+ 包含后模型元素和它们的行为
+ 子系统可以被赋予操作
+ 包内部的协作或者用例必须要支持这些操作，
+ 子系统表示了一个系统或者应用程序在行为上的划分

<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-20.png" height="100" width="350" >
</div>

+ 这两种包相互之间是正交的
+ 可开发性和可发布性划分通常被软件工程师用作配置管理和版本控制的单元
+ 基本行为的划分通常被分析师使用，目的是以一种直观的方式描述系统，在特性改变或增加时进行影响分析

>数据库接口层
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-16.png" height="128" width="400" >
</div>

<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-21.png" height="345" width="520" >
</div>

\*\* 图A 10 这个图到底是什么图，是领域模型图吗？为啥感觉可以直接映射到代码里？ \*\*

+ 斜体书写表示抽象类和操作
+ 空心三角虚线表示实线 implements
+ 接口是实体结构，在C++ or Java 中，有源代码对等物。
+ 类型不是实体的，没有源码对等物
+ 本例实现关系展示了实体设计结构和领域模型之间的对应关系 **** 

> UML把类之间的关系分为以下六种：
+ 关联（Association） 类A与类B的实例之间存在特定的对应关系  A --——> B 
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COMMON-1.png" height="130" width="480" >
</div>

+ 依赖（Dependency） 类A访问类B提供的服务   A — - —> B 
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COMMON-2.png" height="130" width="480" >
</div>

+ 泛化（Generalization）表示一个更泛化的元素和一个更具体的元素之间的关系  java中使用extends关键字 
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COMMON-3.png" height="130" width="480" >
</div>

+ 实现(Realization) 一个实体定义合同，另一个实体履行合同  java中implements
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COMMON-4.png" height="130" width="480" >
</div>

+ 聚合（Aggregation）是关联关系的特例，是强的关联关系，聚合是整个与个体的关系，即has-a关系，此时整体和部分是可以分离的，他们具有各自的生命周期，部分可以属于多个对象，也可以被多个对象共享；比如计算机和CPU，公司与员工的关系；在代码层面聚合与关联是一致的，只能从语义上来区分。
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COMMON-5.png" height="130" width="480" >
</div>

+ 组合（Composition） 组合（compostion）也是关联关系的一种特例，体现的是一种contain-a关系，比聚合更强，是一种强聚合关系。它同样体现整体与部分的关系，但此时整体与部分是不可分的，整体生命周期的结束也意味着部分生命周期的结束，反之亦然。如大脑和人类。
+ 组合与聚合几乎完全相同，唯一区别就是对于组合，“部分”不同脱离“整体”单独存在，其生命周期应该是一致的。

Q：订单和订单详情属于什么关系，订单商品和商品是什么关系

<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COMMON-6.png" height="130" width="480" >
</div>

#### 顺序图
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-22.png" height="418" width="588" >
</div>

+ 下划线的名字表明是对象不是类
+ 对象是由冒号隔开的两个元素组成，名字+::+这个对象实现的类或者接口名字 ，前面的第一个名字可以省略
+ lifeline：对象下面悬挂的虚线，时间自顶到底流逝
+ 连接lifiline的是消息，尾端带有圆箭头的的短线叫做数据表示”data token“
+ data token 若指向消息传递的方向，则是入参，如果和消息传递方向相反，则是返回值
+ 粗体矩形框定义了一个循环，循环的结束条件被包含在了矩形框的底部，本例中 被包围的消息一直重复执行，知道检查完了iterator<Session>中所有的Session对象
+ 顺序图中对象的类名不必是对象的实际类型的名字，只要对象符合被命名的类的接口就行了。

> Session Menu Generator的静态模型
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-23.png" height="220" width="492" >
</div>

#### CGI程序的框架
<div align="center">
<img src="https://pinecc-static.oss-cn-hangzhou.aliyuncs.com/book/ASD-UML/UML-COURSE-24.png" height="360" width="415" >
</div>

### UML 核心视图
>静态图 表达事物的静态结构，不描述行为
+ 用例图 以参与者和用例作为基本元素，以不同的视角展现系统的功能性需求
+ 类图 用于展示系统中的类和及其相互之间的关系，类图是现实世界问题领域的抽象对象的结构化、概念化、逻辑化描述
+ 包图 一般用来展示高层次的观点
> 动态视图 描述事物的动态行为，必须特指一个静态视图或者UML元素，说明在静态视图规定的事物结构下他们的动态行为  
+ 活动图 描述了为了完成某一个目标需要做的活动以及这些活动的执行顺序，分用例场景的描述和对象的描述
+ 状态图 显示一个状态机 通常用来描述实体类对象整个生命周期内的状态变迁以或得对这个实体对象的理解，同时获得系统和实体对象相互影响的关系
+ 时序图 用户以描述按照时间顺序排列的对象之间的交互模式；按照参与交互的对象所具有的生命线和它们相互发送的消息来展示这些对象
+ 通讯图 描述了对象间交互的一种模式，它通过对象之间的连接和它们互相发送的消息来显示参与交互的对象

