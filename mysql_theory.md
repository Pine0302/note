### mysql原理
> 1.事务的特性（ACID）
+ 原子性(Atomicity)：事务是数据库的逻辑工作单位，事务中包含的各操作要么都做，要么都不做
+ 一致性（Consistency）：事务执行的结果必须是使数据库从一个一致性状态变到另一个一致性状态。数据库事物的一致性就规定了事物提交前后，永远只可能存在事物提交前的状态和事物提交后的状态，从一个一致性的状态到另一个一致性状态，而不可能出现中间的过程态。也就是说事物的执行结果是量子化状态，而不是线性状态
+ 隔离性（Isolation）：一个事务的执行不能其它事务干扰。即一个事务内部的操作及使用的数据对其它并发事务是隔离的，并发执行的各个事务之间不能互相干扰。
+ 持续性（Durability）：也称永久性，指一个事务一旦提交，它对数据库中的数据的改变就应该是永久性的。接下来的其它操作或故障不应该对其执行结果有任何影响。
+ C是应用层特性，AID是为C服务的，目的就是要保证逻辑能正确的执行。比如：A、B账面共计100W，A向B转账，加上事务控制，转成功后，他们账户总额应还是100W，事务应保持这种应用逻辑正确一致。还有，转账（事务成功）前后，数据库内部的数据结构--比如账户表的主键、外键、列必须大于0、Btree、双向链表等约束需要是正确的，和原来一致的

> 2.事务的隔离级别
>> 相关定义 
> + 脏读：当A事务读取B事务未提交的内容，之后由于B事务出现了异常回滚了事务，结果造成A读取的数据不一致（A读取了B执行过程中（后来回滚了）的脏数据）
> + 不可重复读： 就是A事务读取数据，B事务改了这个数据，也提交成功了，A再读取就是B修改后的数据，再也不能重复读到最开始的那个数据值了，此为不可重复读
> + 可重复读：A事务读取数据，B事务改了这个数据（update），也提交成功了，A再读这个数据，SQL机制强行让A仍然读之前读到的数据值
> + 幻读：可重复读机制对Insert操作无效，A事务在可重复读的机制下，读取数据，B事务insert一条数据，提交成功，A再读这个数据，会显示B插入的数据，此为幻读。
> + 不可重复读和幻读的区别是：前者是指读到了已经提交的事务的更改数据(修改或删除)，后者是指读到了其他已经提交事务的新增数据。对于这两种问题解决采用不同的办法，防止读到更改数据，只需对操作的数据添加行级锁，防止操作中的数据发生变化;二防止读到新增数据，往往需要添加表级锁，将整张表锁定，防止新增数据(oracle采用多版本数据的方式实现)。
>> 级别展示
> + 未提交读（read uncommitted）：务之间完全不隔离 
> + 提交读（read committed）：事务读取的数据，都是别的事务已经提交了的 -->脏读
> + 可重复读（repeatable read）：即是在上一个级别的基础上，保证不会在一个事务内两次select同一条数据会出现变化，即是别的事务对你select的对象进行update操作不会影响。-->脏读/不可重复读
> + 序列化读（serializable）这也就是最高级别，保证事务之间不会有任何踩踏，每个事务都可以认为只有它自己在操作数据库--> 脏读/不可重复读/幻读

> 3.隔离性的实现
+ 1.加锁（lock） 
+ 2.多版本并发控制（MVCC：Multi-Version Concurrency Control）
>> MVCC:是行级锁的变种，实现了非阻塞的读操作，写操作也只锁定必要的行，因此开销较低
> + MVCC会保存某个时间点上的数据快照。这意味着事务可以看到一个一致的数据视图,这同时也意味着不同的事务在同一个时间点看到的同一个表的数据可能是不同的

 

