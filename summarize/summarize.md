# 总结

## 一、工作心得

在近几个月的工作中，也学到不少关于Java的知识，从九月份到现在，也能渐渐加入项目组开始真正意义上的开发，学到的内容也不少。

从刚开始的经验与知识储备不足，到现在能够渐渐开始进入开发状态，学了一些Java的框架：springboot、springcloud、中间件（redis、rabbitmq），包括还有关于mysql的进阶的知识，例如sql优化、sql的索引以及索引的结构。

## 二、学习过程

### 1. 基础

比如以前没接触过的lanbda表达式，方法引用，stream流等等。在学习了这些知识以后，代码可以做到非常简洁。

1. 方法引用

   在使用mq的时候，使用 `LambdaQueryWrapper<T>` 写 `where` 条件的时候，在比较复杂的情况下可以直接用方法引用，就可以不用先去 `new` 一个对象出来，一般情况下实体类例的变量都是成员变量( `private` )， 可以直接使用 `User::getUsername` 的形式进行书写，在看代码的时候也比较清晰明了。

2. lanbda表达式

   lanbda表达式最大的一个特点是能让代码变得非常简洁，可读性也非常好，在写的时候也不需要写参数的声明，也不需要写方法体，只有一个参数可以不写括号，多个参数需要写括号，没有参数的时候可以直接打个括号就行，如果主体只有一个表达式，返回值则编译器会自动返回值，大括号需要指定表达式返回了一个数值。

3. stream流

   stream流与上面有一个共同的特点，就是简洁，常用的方法有七种：count()、filter()、forEach()、limit()、map()、skip()、concat()。

   七种可有不同的用法：

   1. count()：对流中的元素进行计数

      ~~~java
      List<String> list = new ArrayList<>();
      Collections.addAll(list, "张老三", "张小三", "李四");
      long count = list.stream().count();
      System.out.println("count：" + count);
      /*
      结果：count：3
      */
      ~~~

   2. filter()：对流中的元素进行过

      ~~~java
      Stream<String> stream = Stream.of("张老三", "赵子龙", "赵五", "王七");
      stream.filter((name)->name.startsWith("赵"))
          .forEach((name)-> System.out.println("流中的元素" + name));
      /*
      输出结果：
      	流中的元素赵子龙
      	流中的元素赵五
      */
      ~~~
      
   3. forEach()：逐一处理流中的数据
   
         ~~~java
          List<String> list = new ArrayList<>();
          Collections.addAll(list, "张老三", "张小三", "李四");
          list.stream()
              .forEach(name -> System.out.println("name = " + name));
          /*
          输出结果：
            name = 张老三
              name = 张小三
              name = 李四
          */
         ~~~
   
   4. limit()：取流中的前几个元素
   
      ~~~java
      Stream<String> stream = Stream.of("张老三", "赵子龙", "赵五", "王七");
      stream.limit(2)
          .forEach(name-> System.out.println("name = " + name));
      /*
      输出结果：
      	name = 张老三
   	name = 赵子龙
      */
      ~~~
   
   5. map()：映射流中的数据，转换接口类型，函数式接口
   
      ~~~java
      Stream<String> stream = Stream.of("11","22","33","44","55");
      stream.map(s -> Integer.parseInt(s))
          .forEach(s -> System.out.println(s + ": " + s.getClass()));
      /*
      输出结果：
      	11: class java.lang.Integer
          22: class java.lang.Integer
          33: class java.lang.Integer
          44: class java.lang.Integer
          55: class java.lang.Integer
      */
      ~~~
   
   6. skip()：跳过前几个元素
   
      ~~~java
      Stream<String> stream = Stream.of("张老三", "赵子龙", "赵五", "王七");
      stream.skip(2)
          .forEach(s -> System.out.println("s = " + s));
      /*
      输出结果：
          s = 赵五
          s = 王七
      */
      ~~~
   
   7. concat()：合并两个流
   
      ~~~java
      Stream<String> streamInt = Stream.of("11","22","33","44","55");
      Stream.concat(streamInt, stream)
          .forEach(s -> System.out.println("s = " + s));
      /*
      输出结果：
          s = 11
          s = 22
          s = 33
          s = 44
          s = 55
          s = 张老三
          s = 赵子龙
          s = 赵五
          s = 王七
      */
      ~~~

### 2. mysql的进阶及高级用法

1. 存储引擎

   * **连接层** ：最上层是一些客户端和链接服务，主要完成一些类似于连接处理、授权认证、及相关的安全方案。服务器也会为安全接入的每个客户端验证它所具有的操作权限
   * **服务层** ：第二层架构主要完成大多数的核心服务功能，如sql接口，并完成缓存的查询，sql的分析和优化，部分内置函数的执行。所有跨存储引擎的功能也在这层实现，如：过程、函数等
   * **引擎层** ：存储引擎真正的负责了MySQL中数据的存储和提取，服务器通过api和存储引擎进行通信。不同的存储引擎具有不同的功能，这样我们可以根据自己的需要，来选取合适的存储引擎
   * **存储层** ：主要是将数据存储在文件系统之上，并完成与存储引擎的交互

   存储引擎就是存储数据、建立索引、更新/查询数据能技术的实现方式。存储引擎是基于表现而不是基于库的，所以存储引擎也可以被成为表引擎，默认存储引擎是InnoDB。

   1. InnoDB

      InnoDB 是一种兼顾高可靠性和高性能的通用存储引擎，在 MySQL 5.5 之后，InnoDB 是默认的 MySQL 引擎。

      特点：

      - DML 操作遵循 ACID 模型，支持**事务**
      - **行级锁**，提高并发访问性能
      - 支持**外键**约束，保证数据的完整性和正确性

   2. MyISAM

      MyISAM 是 MySQL 早期的默认存储引擎。

      特点：

      - 不支持事务，不支持外键
      - 支持表锁，不支持行锁
      - 访问速度快

   3. Memory

      Memory 引擎的表数据是存储在内存中的，受硬件问题、断电问题的影响，只能将这些表作为临时表或缓存使用。

      特点：

      - 存放在内存中，速度快
      - hash索引（默认）

2. 性能分析

   **常用的是explain** 。

   语法：直接在select语句前加上explain。

   ![Snipaste_2023-03-01_09-55-42](img\Snipaste_2023-03-01_09-55-42.png)

   ![Snipaste_2023-03-01_09-56-31](img\Snipaste_2023-03-01_09-56-31.png)

   EXPLAIN 各字段含义：

   * id：select 查询的序列号，表示查询中执行 select 子句或者操作表的顺序（id相同，执行顺序从上到下；id不同，值越大越先执行）

   * select_type：表示 SELECT 的类型，常见取值有 SIMPLE（简单表，即不适用表连接或者子查询）、PRIMARY（主查询，即外层的查询）、UNION（UNION中的第二个或者后面的查询语句）、SUBQUERY（SELECT/WHERE之后包含了子查询）等

   * type：表示连接类型，性能由好到差的连接类型为 NULL、system、const、eq_ref、ref、range、index、all

   * possible_key：可能应用在这张表上的索引，一个或多个

   * Key：实际使用的索引，如果为 NULL，则没有使用索引

   * Key_len：表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下，长度越短越好

   * rows：MySQL认为必须要执行的行数，在InnoDB引擎的表中，是一个估计值，可能并不总是准确的

   * filtered：表示返回结果的行数占需读取行数的百分比，filtered的值越大越好

3. 索引

   索引是可以给查询语句提高效率的，用于高校获取数据的数据结构。除数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用数据，这样就可以在这些数据结构上实现高级查询算法，这种数据结构就是索引。

   * **优点** ：提高数据检索效率，降低数据库的读写成本；通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗。
   * **缺点** ：索引是需要占用空间的；索引大大提高查询效率，但降低了更新速度，比如insert、update、delete。

4. 索引结构：

   * **B+Tree** ：最常见的索引类型，大部分引擎都支持B+树索引

     InnoDB、MyISAM、Memory都支持

   * **Hash** ：底层数据结构是用哈希表实现，只有精确匹配索引列的查询才有效，不支持范围查询

     只有Memory支持

   * **R-Tree(空间索引)** ：空间索引是 MyISAM 引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少

     只有MyISAM支持

   * **Full-Text(全文索引)** ：是一种通过建立倒排索引，快速匹配文档的方式，类似于 Lucene, Solr, ES

     InnoDB在5.6版本之后才支持，MyISAM支持

### 3. mybatis-plus

mybatis-plus简称mp，是mybatis的增强版，在mybatis的基础上进行增强，其目的是简化代码的开发，提高效率。

在学校里学过ssm这三大框架，其中第三种就是mybatis，但在实际开发中，没有那么复杂的sql语句直接就使用mybatis进行开发了，不用过多的写配置，就算直接在原本现有的工程中进行引入mp的依赖，也不会对mybatis的工程有任何影响，启动即会注入CRUD语句，基本没有性能的损耗，通过内置通用的Mapper和	Service，仅仅只需要一些少量的配置即可时间单表CRUD的大部分操作，它也支持Lanbda形式调用，能让代码变得简洁易懂。它还有代码生成器，分页插件等等一些工具，不需要另找插件了，使用起来特别方便。

一些单表操作可以直接使用mp，例如： `select * from user where id = 1;` 可以写成：
~~~java
	@Test
    void select() {
        QueryWrapper<Teachplan> wrapper = new QueryWrapper<>();
        wrapper.eq("id", 1);
        Teachplan one = teachplanService.getOne(wrapper);
    }
~~~

eq就是 `where` 条件，当写多个 `eq` 就会用 `and` 进行拼接，如果有 `or` 的条件是，可以直接使用 `or` ， `update` 的sql语句自然就是用update了，有根据id进行更改 `int updateById`(@Param("et") T entity); ，也有根据条件更改多个字段 `int update(@Param("et") T entity, @Param("ew") Wrapper<T> updateWrapper);` ，删除有常用的两种方法 `int delete(@Param("ew") Wrapper<T> queryWrapper);` 和 `int deleteById(Serializable id);` ，比较简单也用的比较多的就是这些。

### 4. 注册中心与网关

1. 注册中心

   注册中心常见的有两种，第一种是euraka，第二种是nacos。

   euraka是Netflix公司开发的的一款开源的服务注册与发现组件。

   nacos是国内的阿里巴巴想springcloud提供的注册中心服务，它相对euraka来说，共能相对丰富，对国人也比较友好，都是中文。它不仅仅只有服务注册与发现组件，还有其他的一些功能，例如：分布式配置、服务健康注册等等，在国内用的比较多的就是nacos了。

   它的主要功能就是检测服务器的状态，在微服务工程下，每个人都拿着一个模块，在不同的功能下还需要调用其他模块的接口，在这种情况下如果不知其他模块接口的服务是否正常运行，那么就要一个个去问，这样会变的特别麻烦，有了注册中心，我们可以直接去这上面去看每个模块的运行状况，如果上面没有相应的服务，就标明这个服务有问题，直接去nacos上看每个服务的运行状况就行。

2. 网关(gateway)

   网关是一个微服务的架构，是一个全新的springcloud项目，它是基于spring5.0，springboot等响应式编程和事件流技术的网关。微服务项目通常都会把不同的服务都注册到nacos里，统一由nacos进行管理，每个服务之间的接口调用都会使用feign进行调用，在微服务前有个入口，它就是网关(gateway)，它的作用就是控制所有服务的请求路由，也就是所谓的请求路径，每个服务都会有它唯一的一个ip，当我们的ip改变了，那么其它服务里的接口就不能进行调用，网关的作用就在此体现了，通过网关的服务都会有一个相对路径，这个相对路径对应着一个ip，这个ip是网关从nacos里拿到的，那么就是说，我们的ip就算改变了，只要服务里的nacos配置的ip跟着进行变动了，那么其他的服务就不会受影响，也大大提高了开发的效率。
   
   gateway作为微服务的入口，它有三个核心功能：
   
   * 请求路径
   * 路由和负载均衡
   * 限流
   
   架构图：
   
   ![Snipaste_2023-03-06_016-34-42](img\Snipaste_2023-03-06_016-34-42.png)
   
   **权限控制** ：gateway作为微服务的入口，他会先校验用户是否有资格，如果没有就会进行拦截
   
   **路由和负载均衡** ：一切请求都需要经过gateway，但网关是不会去处理业务的，二十工具某种规则，把请求转发到某个微服务，这个过程叫做路由。当然路由的目标服务有多个时，还需要做负载均衡。
   
   **限流** ：当请求过高时，在网关中按照下流的微服务能够接收速度来放行请求，避免微服务压力过大。

## 三、总结

在实习的过程中，我也有了不小的变化，从学生枕边成了一名职业人，从此不再是一名学生，需要具备一些职业人应有的素养，例如不迟到不早退，按时完成自己应该完成的工作，而且还要时时刻刻要求自己严格遵守规则。在实习期间，我对项目的开发上有了比较完整的了解，比如在开发功能上，他的流程应该是怎样的，在代码上应该有什么样的规范，这是我感受比较深的。

在此次毕业实习，我学会了如何去进行开发，学会了如何跟同事进行沟通，积累了有关人际关系问题的经验方法，同事我体验到了社会工作的艰苦行。通过实习，让我在社会中磨练了自己，也锻炼了意志力，提升了自己的实践能力。在工作的同事，对自己的专业能力也有所提升，也积累了一些人际关系，为以后的工作做铺垫。实习也是为将来大号基础的阶段，珍惜每一个工作的机会，初入社会，每一份工作都能给你累计经验。