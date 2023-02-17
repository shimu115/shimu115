# journals

# 第一篇    集合

学习集合，集合也是Java中重要的一节，学的不好，对开发会有重大的阻碍。血迹总结如下几点：

1. Collection接口的集合有两大类：List和Set

    1. List
          * List的特点是有序，有索引，可重复。
               1. LinkedList: 接口实现类， 链表， 插入删除， 没有同步， 线程不安全
               2. ArrayList: 接口实现类， 数组， 随机访问， 没有同步， 线程不安全
               3. Vector: 接口实现类 数组， 同步， 线程安全; Stack 是Vector类的实现类（已淘汰）
               4. list里一般情况下用ArrayList，特定情况下用LinkedList
    2. Set
        * HashSet 使用hash表（数组）存储元素
        * LinkedHashSet 链表维护元素的插入次序
            Set 底层实现为二叉树，元素排好序
            一般情况下默认用HashSet，如果对查询需求较高的情况下就可以使用LinkedHashSet

2. Map接口是键值对的集合 （双列集合）

    1. Hashtable: 接口实现类， 同步， 线程安全
    2. HashMap: 接口实现类 ，没有同步， 线程不安全
    3. LinkedHashMap: 双向链表和哈希表实现
    4. TreeMap: 红黑树对所有的key进行排序

    在对数据有排序需求的时候可以使用TreeMap, Hashtable是线程安全的，所以他的效率较低, 一般情况下使用HashMap就行了，
    HashMap虽然是线程不安全的，但它的效率高，而且HashMap的key和value能为null，而Hashtable是不能狗存入null的

# 第二篇    collections 工具类

学习完集合后，对于集合的一些工具类也必须有所了解


1. sort(List list): 对集合中的元素排序;
2. void sort(List list, Comparator c)：根据指定 Comparator 产生的顺序对 List 集合元素进行排序。
3. reverse: 反转集合中的元素;
4. shuffle: 打乱元素中的元素;
5. fill: 用任意元素替换掉集合中的所有元素
6. copy: 复制并覆盖相应索引的元素
7. addAll: 一次性向集合里添加多条数据
8. replaceAll: 使用一个新值 newVal 替换 List 对象的所有旧值 oldVal

还有一些不是很常用的方法，一般情况下这几个就差不多够用了，需要其他的方法是可以上网查下其他方法的应用方式

# 第三篇    Stream


Stream流是链式编程，可以在简洁的代码里写出比较优质的代码，也能让代码的效率提升

   1. Stream流有两种方法种类

      * 终结方法和函数拼接

        1. 生成流

           通过数据源（集合、数组等）生成流。

           例：list.stream()

        2. 函数拼接

           在方法后面可以继续拼接其他方法，直到遇到终结方法后才不能继续拼接，例：
          
           ```java
               List<string> list = new ArrayList<>();
               Collections.addAll(list, "张老三", "张小三", "李四", "赵五", "张六", "王八");
               list.stream().skip(3).forEach(s -> System.out.println("s = " + s));
           
               /*
           		结果：
           			s = 赵五
           			s = 张六
           			s = 王八
               */
           ```
        
           
        
        3. 终结流
        
           一个流只能有一个终结操作，当这个操作执行后，流就被用“光”了，无法再被操作。所以这必定是流的最后一个操作，例：
           
           ```java
               List<String> list = new ArrayList<>();
               Collections.addAll(list, "张老三", "张小三", "李四", "赵五", "张六", "王八");
           
               list.stream().forEach(s -> System.out.println("s = " + s));
               /*
                   结果：
                       s = 张老三
                       s = 张小三
                       s = 李四
                       s = 赵五
                       s = 张六
                       s = 王八
               */
           ```
           
           
        
      
   2. Stream流的常用方法
   
      1. count: 统计个数
      2. forEach: 逐一处理
      3. filter: 过率
      4. limit: 取用前几个
      5. skip: 跳过前几个
      6. map: 映射
      7. concat: 组合

# 第四篇    异常

异常也是Java中必要重要的一节，在必要时候要知道这个异常是什么问题，这个异常该怎么处理。

异常分为两种：编译时异常和运行时异常

1. 编译时异常：

   编译时异常指的是在编译时不能通过的异常，在ide编译器里通常会划横线指出来，在dom面板里用javac进行编译时通常会报错。

   Java里有个Exception类，这个类是所有异常类的父类，某些方法会对外抛出一个异常，让你进行处理，也可以自定义异常，但是必须得继承Exception类

   * 异常的处理

     处理异常的方法有两种，一种是`throws`进行向上抛异常，一种是try...catch

     1. throws

        使用这个方法的时候编译器会给你指出错误，你必须要进行处理，当然到最外层也可以继续抛异常，例：

        ```java
            public static void main(String[] args) throws Exception {
        
                InputStream inputStream = new FileInputStream("myvessel\\src\\com\\shimu\\data.txt");
                byte[] bytes = new byte[3];
                int read = inputStream.read(bytes);
                System.out.println(read);
                String s = new String(bytes);
                System.out.println(s);
        
            }
        
            /*
                输出结果：
                3
                abc
            */
        ```

        如果不使用throws就是下面的结果

        ![Snipaste_2023-02-04_13-21-34](img/Snipaste_2023-02-04_13-21-34.png)

     2. try...catch

        使用这个方法可以进行捕获异常，在try里面进行捕获异常，捕捉到异常之后会进入到catch里面，可以做对应的逻辑操作，当触发异常之后也必须要执行的操作就可以在catch后面加上finally，finally里面的语句是必须会操作的，例：

        ```java
            public static void main(String[] args) {
        
                InputStream inputStream = null;
                OutputStream outputStream = null;
                try {
                    inputStream = new FileInputStream("C:\\Users\\shimu\\OneDrive\\图片\\屏幕快照\\2022-10-14.png");
                    outputStream = new FileOutputStream("F:\\new.png");
                    byte[] buffer = new byte[1024];
                    int len;
                    while ((len = inputStream.read(buffer)) != -1) {
                        outputStream.write(buffer, 0, len);
                    }
                    System.out.println("success!");
                } catch (Exception e) {
                    e.printStackTrace();
                }finally {
                    try {
                        outputStream.close();
                        inputStream.close();
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
        ```
        
        运行结果：
        
        ![Snipaste_2023-02-04_13-29-50](img/Snipaste_2023-02-04_13-29-50.png)
   
2. 运行时异常

   运行时异常就是指的在运行的时候才会给你报错，例如：

   ```jvava
       @Test
       public void runtimeExceptionDemo2() {
           int[] arr = {1, 2, 3, 4, 5};
           System.out.println("arr[10] = " + arr[10]);
       }
   
       /*
           运行结果：
           java.lang.ArrayIndexOutOfBoundsException: 10
   
               at com.shimu.test.test.runtimeExceptionDemo2 <27 internal lines>
           此时报了个数组越界异常
       */
   ```

   虽然报错了，但依旧可以使用try..catch进行捕获异常

   ```java
       @Test
       public void runtimeExceptionDemo2() {
           try {
               int[] arr = {1, 2, 3, 4, 5};
               System.out.println("arr[10] = " + arr[10]);
           } catch (Exception e) {
               System.out.println("RuntimeException");
               throw new RuntimeException("数组越界异常");
           }
       }
       /*
           运行结果：
           RuntimeException
   
           java.lang.RuntimeException: 数组越界异常
   
               at com.shimu.test.test.runtimeExceptionDemo2 <27 internal lines>
       */
   ```


# 第五篇   io流

在使用电脑的过程或者使用手机的过程都是会用到io流的，当你电脑上插入一个U盘，可以把一个视频，拷贝到你的电脑硬盘里。那么数据都是在哪些设备上的呢？键盘、内存、硬盘、外接设备等等。

我们把这种数据的传输，可以看做是一种数据的流动，按照流动的方向，以内存为基准，分为`输入input` 和`输出output` ，即流向内存是输入流，流出内存的输出流。

1. 根据数据的流向分为：**输入流**和**输出流**。

   * **输入流**

     把数据从其他设备上读取到内存中的流。 

   * **输出流**

     把数据从内存中写出到`其他设备`上的流。

2. 根据数据的类型分为：**字节流**和**字符流**。

   * **字节流**

     以字节为单位，读写数据的流。

     * InputStzream: 字节输入流的顶级父类
     * OutputStream: 字节输出流的顶级父类

   * **字符流**

     以字符为单位，读写数据的流。

     * Reader: 字符输入流的顶级父类
     * Writer: 字符输出流的顶级父类

# 第六篇   io的其他流

上一篇的io流属于是比较基础的io流，效率比较低，这篇带来的是更加强大，效率更高的流。比如能够高效读写的缓冲流，能够转换编码的转换流，能够持久化存储对象的序列化流等等。这些功能更为强大的流，都是在基本的流对象基础之上创建而来的，就像穿上铠甲的武士一样，相当于是对基本流对象的一种增强。

缓冲流,也叫高效流，是对4个基本的`FileXxx` 流的增强，所以也是4个流，按照数据类型分类：

* **字节缓冲流**：`BufferedInputStream`，`BufferedOutputStream` 
* **字符缓冲流**：`BufferedReader`，`BufferedWriter`

缓冲流的基本原理，是在创建流对象时，会创建一个内置的默认大小的缓冲区数组，通过缓冲区读写，减少系统IO次数，从而提高读写的效率。

1. **字节缓冲流**

   1. 普通流效率如下：
   
      ~~~java
      	@Test
          void BufferedDemo() {
              // 记录开始时间
              long start = System.currentTimeMillis();
              // 创建流对象
              try (
                      FileInputStream fis = new FileInputStream("E:\\3.开发环境\\JDK\\jdk-8u211-windows-x64.exe");
                      FileOutputStream fos = new FileOutputStream("E:\\jdk8.exe")
              ){
                  // 读写数据
                  int b;
                  while ((b = fis.read()) != -1) {
                      fos.write(b);
                  }
              } catch (IOException e) {
                  e.printStackTrace();
              }
              // 记录结束时间
              long end = System.currentTimeMillis();
              System.out.println("普通流复制时间:"+(end - start)+" 毫秒");
          }
      	/*
              输出结果：普通流复制时间:469308 毫秒
          */
      ~~~
   
   2. 缓冲流效率如下：
   
      ~~~java
      	@Test
          void BufferedDemo() {
              // 记录开始时间
              long start = System.currentTimeMillis();
              // 创建流对象
              try (
                      BufferedInputStream bis = new BufferedInputStream(new FileInputStream("E:\\3.开发环境\\JDK\\jdk-8u211-windows-x64.exe"));
                      BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("E:\\jdk8.exe"));
              ){
                  // 读写数据
                  int b;
                  while ((b = bis.read()) != -1) {
                      bos.write(b);
                  }
              } catch (IOException e) {
                  e.printStackTrace();
              }
              // 记录结束时间
              long end = System.currentTimeMillis();
              System.out.println("缓冲流复制时间:"+(end - start)+" 毫秒");
          }
          /*
              输出结果：缓冲流复制时间:5665 毫秒
           */
      ~~~


2. **字符缓冲流**


   1. `public BufferedReader(Reader in)` ：创建一个 新的缓冲输入流。

      * BufferedReader：`public String readLine()`: 读一行文字。 

        ~~~java
            @Test
            void BufferedReaderDemo() throws IOException {
                // 创建流对象
                BufferedReader br = new BufferedReader(new FileReader("in.txt"));
                // 定义字符串,保存读取的一行文字
                String line  = null;
                // 循环读取,读取到最后返回null
                while ((line = br.readLine())!=null) {
                    System.out.print(line);
                    System.out.println("------");
                }
                // 释放资源
                br.close();
            }
        ~~~

      * BufferedWriter： `public void newLine()` : 写一行行分隔符,由系统属性定义符号。 

        ~~~java
        	@Test
            void BufferedWriterDemo() throws IOException {
                // 创建流对象
                BufferedWriter bw = new BufferedWriter(new FileWriter("out.txt"));
                // 写出数据
                bw.write("xx");
                // 写出换行
                bw.newLine();
                bw.write("c");
                bw.newLine();
                bw.write("b");
                bw.newLine();
                // 释放资源
                bw.close();
            }
        ~~~

        输出结果：

        ![Snipaste_2023-02-13_10-50-36](img/Snipaste_2023-02-13_10-50-36.png)

        ![Snipaste_2023-02-13_10-51-21](img/Snipaste_2023-02-13_10-51-21.png)

# 第七篇   git 版本控制

* Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目，是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件，Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。

* 可以通过一些命令来控制项目的版本，可以做到迭代版本与回溯版本，在做出更新的时候进行更新版本，出现错误也可以回到以前的某个版本。

## 基本概念

- **工作区：**就是你在电脑里能看到的目录。
- **暂存区：**英文叫 stage 或 index。一般存放在 **.git** 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- **版本库：**工作区有一个隐藏目录 **.git**，这个不算工作区，而是 Git 的版本库。

下面这个图展示了工作区、版本库中的暂存区和版本库之间的关系：

![Snipaste_2023-02-16_13-17-19](img\Snipaste_2023-02-16_13-17-19.png)

## Git 基本操作

Git 的工作就是创建和保存你项目的快照及与之后的快照进行对比。

本章将对有关创建与提交你的项目快照的命令作介绍。

![Snipaste_2023-02-16_13-18-47](img\Snipaste_2023-02-16_13-18-47.png)

Git 常用的是以下 6 个命令：

* **git clone** ：将远程仓库的代码克隆到本地仓库
* **git push** ：将本地仓库里的代码推送到远程仓库并进行合并
* **git add** ：将新创建或修改后的代码添加到暂存库
* **git commit** ：将暂存库里的代码提交到本地仓库
* **git checkout** ：切换远程仓库的分支
* **git pull** ：拉取远程代码并合并



 ***可以查看我的远程仓库，周记都会搭建成在线文档，每周写的都会上传*** 

[github在线文档](https://shimu115.github.io/wuhanxxcb/) (github可能有点慢，gitee搭建在线文档需要实名就先用github搭建了)

[github仓库](https://github.com/shimu115/wuhanxxcb) 

***周记与总结都以md文档的格式编写的，上传到定岗实习平台后看起来可能有些怪*** 
